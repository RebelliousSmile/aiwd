# PDFLateX - Multi-Universe TTRPG Writing Project

## Project Overview

Multi-universe tabletop RPG content creation system using modular LaTeX templates and a structured writing workflow. Each universe (archipels, wot, ecryme, zombiology, demonslayer, otherscape, mina) has isolated documentation, templates, and projects.

## Build Commands

### Compile PDF
```bash
# Compile a project to PDF
python scripts/build-pdf.py --project "<universe>/<project-name>"

# With custom output name and auto-open
python scripts/build-pdf.py --project "archipels/carnets_de_voyage" --output-name "carnets-v2" --open

# Clean auxiliary files after compilation
python scripts/build-pdf.py --project "archipels/carnets_de_voyage" --clean
```

### Export to InDesign (ICML)
```bash
# Convert Markdown chapters to ICML via Pandoc
python scripts/build-icml.py --project "<universe>/<project-name>"

# Generate separate ICML per chapter
python scripts/build-icml.py --project "archipels/carnets_de_voyage" --no-concat
```

**Requirements:**
- **pdflatex** (TeX Live, MiKTeX, or MacTeX)
- **Pandoc 3.0+** (for ICML export)
- **Python 3.10+**

### Synchronize Git Repositories
```bash
# Commit, pull, push all project repositories
python scripts/git-sync.py

# Dry-run (no modifications)
python scripts/git-sync.py --dry-run
```

### Publish to itch.io
```bash
# Install Butler (once)
python scripts/install-butler.py
scripts/butler/butler login

# Publish single project
python scripts/publish-one.py --project "<universe>/<project-name>"

# Publish all ready projects
python scripts/publish-all.py
```

## Architecture

### Repository Structure

```
PDFLateX/
├── <universe>/                     # Universe containers (archipels, wot, etc.)
│   ├── .docs/                      # Universe documentation
│   │   ├── UNIVERS.md              # Universe overview (required)
│   │   └── terminologie.md         # Terminology (required)
│   ├── .templates/                 # LaTeX style packages
│   │   ├── <universe>.sty          # Main style package
│   │   └── main-template.tex       # Project template
│   ├── .output-styles/             # Writing tone guidelines
│   │   └── latex-<universe>.md
│   └── <project>/                  # Individual writing projects
│       ├── .docs/                  # Project-specific docs
│       │   ├── pj.md               # Player characters
│       │   ├── document-rules.md   # Project-specific rules
│       │   └── sources/            # Reference PDFs
│       ├── .toc/                   # Table of contents (canonical)
│       │   ├── INDEX.md
│       │   └── toc-chapter<NN>.md
│       ├── chapitres/              # Markdown source (source of truth)
│       ├── latex-partials/         # Generated .tex files (included by main.tex)
│       ├── output/                 # Compiled PDFs
│       ├── bank.yml                # Project configuration
│       ├── overview.md             # Canonical content document
│       ├── main.tex                # LaTeX entry point
│       └── .git/                   # Individual project repository
│
├── docs/                           # Global documentation
│   ├── prompts/workshop/           # Writing workflow prompts
│   ├── templates/                  # Configuration templates
│   ├── memory-bank/                # Technical documentation
│   └── rules-files/                # Game system rules
│
└── scripts/                        # Build and publishing scripts
```

### Git Architecture

**One Git repository per project** (not at root). Each `<universe>/<project>` has its own `.git/`.

### Key Directories

- **`chapitres/`** - Markdown source files (source of truth)
- **`latex-partials/`** - Generated `.tex` files (included by main.tex)
- **`.docs/`** - Documentation supporting the project (universe or project-specific)
- **`.toc/`** - Detailed table of contents (canonical, part of deliverables)
- **`output/`** - Compiled PDFs (git-ignored)

### Project Configuration: bank.yml

Every project has a `bank.yml` that configures the writing workflow:

```yaml
document:
  name: "Project Name"
  univers: "archipels"
  type: "roleplaying"  # or "novel"

output-style:
  global: "archipels/.output-styles/latex-archipels.md"

docs:
  univers: "archipels/.docs/UNIVERS.md"
  terminologie: "archipels/.docs/terminologie.md"
  projet:
    - ".docs/pj.md"
    - ".docs/document-rules.md"

toc:
  fichier: ".toc/INDEX.md"

templates:
  package: "archipels/.templates/archipels.sty"
```

## Writing Workflow (Workshop Prompts)

### Primary Flow

```
brainstorm → generate-toc → write-toc-chapter (opt.) → write-roleplaying → doctor → md-to-tex
                                                     → write-novel → doctor → md-to-tex
```

### Industrialized Pipeline

```
.toc/toc-chapter<NN>.md → write-roleplaying → doctor → md-to-tex → latex-partials/chapitre<NN>.tex
```

### Available Prompts

Located in `docs/prompts/workshop/`:

| Prompt | Purpose |
|--------|---------|
| `brainstorm.prompt.md` | Concept development and refinement |
| `generate-toc.prompt.md` | Generate table of contents from source |
| `write-toc-chapter.prompt.md` | Detailed chapter outline (optional) |
| `write-novel.prompt.md` | Write narrative fiction chapter |
| `write-roleplaying.prompt.md` | Write TTRPG scenario/adventure |
| `review-chapter.prompt.md` | Review narrative chapter |
| `review-roleplay.prompt.md` | Review TTRPG content |
| `doctor.prompt.md` | Persona analysis and consistency check |
| `md-to-tex.prompt.md` | Convert Markdown to LaTeX |
| `tone-finder.prompt.md` | Generate output-style from sources |
| `research.prompt.md` | Research and information gathering |
| `upgrade.prompt.md` | Update prompts/templates to latest version |
| `tabula-rasa.prompt.md` | Reset project to clean state |

**Usage:**
```
@docs/prompts/workshop/brainstorm.prompt.md <project-name>
@docs/prompts/workshop/write-roleplaying.prompt.md 3
```

### Autonomous Agents

Located in `docs/agents/`:

| Agent | Purpose |
|-------|---------|
| `writing-pipeline.md` | Orchestrate full writing workflow (can run parallel) |
| `memory-manager.md` | Manage context and memory across sessions |

**Example:**
```
# Write chapters 02-07 in parallel (6 agents simultaneously)
@docs/agents/writing-pipeline.md 2-7 --parallel
```

## Key Conventions

### French Language

**CRITICAL:** All generated content must use proper French characters and typography:
- Accents: é, è, ê, ë, à, â, ù, û, ô, î, ï, ç
- Ligatures: œ, æ
- Quotes: «», ""
- Dashes: —, –

### Content Source of Truth

**`chapitres/` directory** contains the Markdown source files, which are the **single source of truth** for all content. The `latex-partials/` directory contains generated `.tex` files that are included by `main.tex`.

**Workflow:**
```
chapitres/*.md (source) → md-to-tex → latex-partials/*.tex → main.tex → PDF
                        ↓
                        → build-icml.py → Pandoc → ICML → InDesign → PDF HD
```

### LaTeX Compilation

**Always run pdflatex twice** to generate correct table of contents and cross-references.

```bash
pdflatex main.tex  # First pass
pdflatex main.tex  # Second pass (resolves references)
```

The `build-pdf.py` script handles this automatically.

### Template Usage

Each universe has its own LaTeX style package in `<universe>/.templates/`:

```latex
\documentclass[10pt,a4paper,twoside]{article}
\usepackage[...]{geometry}

% Load universe style package
\usepackage{../.templates/archipels}

\begin{document}
\input{latex-partials/chapitre01}
\end{document}
```

### Script Output Format

**No emojis in scripts** - use text prefixes instead: `[OK]`, `[ERROR]`, `[INFO]`, `[WARNING]`.

### PDF Extraction Rules

When extracting content from PDFs:
- **NEVER invent content** - only extract what's in the source
- Use the `extract` prompt (`docs/prompts/workshop/extract.prompt.md`) for structured extraction
- Use `normalize-text.py` to fix encoding issues after extraction if needed

### Universe Documentation

**Always load universe documentation** when working on a project:
- `<universe>/.docs/UNIVERS.md` - Overview (required)
- `<universe>/.docs/terminologie.md` - Terminology (required)
- Additional files may exist (style-guide.md, lieux.md, etc.)

These files provide context, terminology, and conventions specific to each universe.

## Creating a New Project

```bash
# 1. Create project structure
cd <universe>
mkdir <project-name>
cd <project-name>

# 2. Copy template
cp ../.templates/main-template.tex main.tex

# 3. Create directories
mkdir chapitres latex-partials output .docs .toc

# 4. Create bank.yml from template
cp ../../docs/templates/bank.yml .
# Edit with project-specific values

# 5. Initialize git
git init && git add . && git commit -m "Initial commit"
```

## Useful Scripts

| Script | Description |
|--------|-------------|
| `export-sty-specs.py` | Export LaTeX .sty specs to YAML for InDesign template |
| `ocr-to-latex.py` | Convert scanned images to LaTeX via Tesseract OCR |
| `normalize-text.py` | Fix encoding and character issues in text files |
| `prepare-for-itch.py` | Prepare PDFs for manual itch.io upload |
| `itch-checklist.py` | Show checklist of itch.io projects |

## Documentation

- **Project structure:** `docs/memory-bank/project-structure.md`
- **LaTeX workflow:** `docs/memory-bank/latex-workflow.md`
- **InDesign/ICML workflow:** `docs/memory-bank/indesign-workflow.md`
- **Templates reference:** `docs/memory-bank/templates-reference.md`
- **Workshop system:** `docs/README.md`
- **Scripts reference:** `scripts/README.md`

## Active Universes

- **archipels** - Maritime medieval fantasy
- **wot** - Wheel of Time adaptation
- **ecryme** - Crime & investigation
- **zombiology** - Zombie survival
- **demonslayer** - Demon Slayer adaptation
- **otherscape** - Horror/mystery
- **mina** - Heavy Metal (robots post-apocalyptiques)

## Publishing Workflow

1. **Compile locally:**
   ```bash
   python scripts/build-pdf.py --project "<universe>/<project>" --clean
   ```

2. **Configure in itch-projects.json:**
   Edit `docs/templates/itch-projects.json` to add project metadata

3. **Publish:**
   ```bash
   python scripts/publish-one.py --project "<universe>/<project>"
   ```

4. **Bulk publish:**
   ```bash
   python scripts/itch-checklist.py           # Check status
   python scripts/publish-all.py              # Publish all ready projects
   ```
