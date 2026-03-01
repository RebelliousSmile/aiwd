---
name: init-project
description: Initialize or verify LaTeX project structure in multi-tenant architecture
argument-hint: <univers> "<nom-projet>" [type]
---

# Initialize Project

## Goal

Initialize a new LaTeX project with proper structure, templates, and documentation for the specified universe.

## Arguments

- **univers**: archipels | roue-du-temps | ecryme (required)
- **nom-projet**: Project name (required)
- **type**: transcription | redaction (optional, default: redaction)

## Steps

### Step 1: Verify Universe Exists

Check:
- `<univers>/.docs/UNIVERS.md` exists
- `<univers>/.templates/<univers>.sty` exists

If missing: Error with explicit message

### Step 2: Create Project Structure

```
<univers>/<nom-projet>/
├── README.md           # Auto-generated
├── main.tex            # From template
├── chapitres/          # Empty directory
├── sources/            # If type=transcription
│   └── ocr_output/     # If type=transcription
└── scripts/            # If type=transcription
```

### Step 3: Generate README.md

```markdown
# <Nom du Projet>

Projet <type> pour l'univers <Univers>

## Type de projet

**<Type>**: <description>

## Compilation

```bash
pdflatex main.tex
pdflatex main.tex  # Two passes for TOC
```

## Template

`<univers>/.templates/<univers>.sty`
```

### Step 4: Copy and Customize main.tex

- Source: `<univers>/.templates/main-template.tex`
- Replace:
  - `{{TITRE_PROJET}}` → Project name
  - `{{DATE}}` → Current date (YYYY-MM-DD)

### Step 5: Initialize Git (optional)

```bash
cd "<univers>/<nom-projet>"
git init
git add .
git commit -m "Initial commit: <nom-projet>"
```

### Step 6: Report

```
Project initialized: <univers>/<nom-projet>

Documentation loaded:
- @<univers>/.docs/UNIVERS.md
- @<univers>/.docs/terminologie.md
- @docs/output-styles/latex-<univers>.md

Files created:
- README.md
- main.tex
- chapitres/

Ready to work!
```

## Examples

```
/init-project archipels "nouveau projet"
/init-project roue-du-temps "nouvelles arad doman" redaction
/init-project ecryme "solo aventure" transcription
```

## Error Handling

| Error | Action |
|-------|--------|
| Unknown universe | List available universes |
| Missing template | Show how to create template |
| Project exists | Offer verification or new name |
