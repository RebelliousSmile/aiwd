---
name: build-pdf
description: Compile LaTeX project to PDF using platform-specific build scripts
argument-hint: project="<univers>/<nom-projet>" [clean="true"] [open="true"]
---

# Build PDF

## Goal

Compile a LaTeX project to PDF using the appropriate build script for the current platform.

## Arguments

- **project**: Path to project (required) - format: `<univers>/<nom-projet>`
- **mainfile**: Main .tex file (optional, auto-detected)
- **outputname**: Custom PDF name (optional, defaults to project name)
- **clean**: Clean auxiliary files after compilation (optional)
- **open**: Open PDF after compilation (optional)

## Steps

### Step 1: Verify Project Exists

```bash
ls "$ARGUMENTS_PROJECT"
```

If not found, list available projects and ask user.

### Step 2: Build Command

**Windows (PowerShell):**
```powershell
.\.claude\scripts\build-pdf.ps1 -ProjectPath "<project>" [-MainFile "<mainfile>"] [-OutputName "<outputname>"] [-CleanAux] [-OpenPDF]
```

**Linux/macOS (Bash):**
```bash
./.claude/scripts/build-pdf.sh <project> [<mainfile>] [--output-name=<outputname>] [--clean] [--open]
```

### Step 3: Execute and Report

1. Detect platform
2. Run appropriate script
3. Report result:
   - Success: PDF path in `<project>/output/`
   - Failure: Show errors, suggest `latex-validator`

## Examples

```
/build-pdf project="archipels/carnets_de_voyage" clean="true"
/build-pdf project="ecryme/solo-ecryme" open="true"
```

## Error Handling

| Error | Action |
|-------|--------|
| pdflatex not installed | Suggest MiKTeX/TeX Live |
| Project not found | List available projects |
| Compilation failed | Suggest latex-validator |
| No .tex file | List .tex files in project |

## Notes

- Double compilation automatic (for TOC/references)
- Supports PNG, JPG, PDF images
- UTF-8 with `\usepackage[utf8]{inputenc}`
