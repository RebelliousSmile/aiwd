---
name: generate-main-tex
description: Generate main.tex from universe template and TOC chapter structure
argument-hint: (none - uses current project context)
---

# Generate Main LaTeX File

## Goal

Generate a production-ready `main.tex` file that includes all chapters from the TOC structure as latex-partials inputs.

## Context

### Bank Configuration

```yaml
@bank.yml
```

### TOC Structure

```markdown
@.toc/INDEX.md
```

### Universe Template

```latex
@<univers>/.templates/main-template.tex
```

## Rules

- **Langue française:** Toujours utiliser les accents et caractères spéciaux (é, è, ê, à, ù, ç, œ, «», —, etc.)
- Use template as base structure
- Replace ALL placeholders with values from bank.yml
- Generate `\input{latex-partials/chapitre<XX>}` for each TOC chapter
- Preserve template comments and structure
- Save to project root as `main.tex`

## Steps

### Step 1: Load Configuration

1. Parse `bank.yml` for:
   - `document.name` → {{TITRE_PROJET}}
   - `document.univers` → template path
   - `metadata.auteur` → {{AUTEUR}}
   - `metadata.date` → {{DATE}}
   - `metadata.licence` → {{LICENCE}}

2. Parse `.toc/INDEX.md` for chapter list

### Step 2: Identify Chapters

Extract from INDEX.md:

| Chapter | File |
|---------|------|
| Chapitre 01 | `latex-partials/chapitre01.tex` |
| Chapitre 02 | `latex-partials/chapitre02.tex` |
| ... | ... |

### Step 3: Generate main.tex

1. Copy template structure
2. Replace placeholders:
   - `{{TITRE_PROJET}}` → document name
   - `{{AUTEUR}}` → author
   - `{{DATE}}` → date
   - `{{SOUS_TITRE}}` → document type (Scénario, Roman, etc.)
   - `{{SYNOPSIS}}` → from INDEX.md overview
   - `{{LICENCE}}` → license text
   - `{{DUREE}}` → if applicable
   - `{{NIVEAU_JOUEURS}}` → if applicable

3. Replace chapter section with:

```latex
%----------------------------------------------------------------------------------------
%	CHAPITRES
%----------------------------------------------------------------------------------------

\input{latex-partials/chapitre01}
\newpage

\input{latex-partials/chapitre02}
\newpage

% ... continue for all chapters
```

### Step 4: Save and Report

1. Write `main.tex` to project root
2. Report:
   - Chapters included
   - Placeholders replaced
   - Any missing values

## Output Format

```markdown
# main.tex Generated

**File:** `main.tex`
**Template:** `<univers>/.templates/main-template.tex`

## Chapters Included

| # | Title | Partial File |
|---|-------|--------------|
| 01 | [title] | latex-partials/chapitre01.tex |
| 02 | [title] | latex-partials/chapitre02.tex |

## Placeholders Replaced

- {{TITRE_PROJET}} → [value]
- {{AUTEUR}} → [value]
- {{DATE}} → [value]

## Next Steps

1. Generate chapter content: `write-roleplaying.prompt.md <n>`
2. Convert to LaTeX: `md-to-tex.prompt.md chapitre<XX>.md`
3. Compile PDF: `python scripts/build-pdf.py --project "<projet>"`
```

## Validation Checklist

- [ ] All placeholders replaced (no remaining `{{...}}`)
- [ ] All TOC chapters have corresponding `\input` statements
- [ ] Template structure preserved
- [ ] LaTeX syntax valid
- [ ] Correct path to .sty package
