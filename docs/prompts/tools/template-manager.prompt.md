---
name: template-manager
description: Create and evolve LaTeX templates for game universes
argument-hint: Action and universe (e.g., "create template archipels" or "add command to ecryme")
---

# LaTeX Template Manager

## Goal

Create and maintain LaTeX templates for each game universe, ensuring all projects benefit from graphical improvements automatically.

## Responsibilities

### 1. Create New Template

**Steps:**

1. Read `<univers>/.docs/UNIVERS.md`
2. Read `<univers>/.docs/assets-graphiques.md`
3. Extract color palette
4. Create structure:

```
<univers>/.templates/
├── <univers>.sty              # Main package
├── <univers>-colors.sty       # Colors (optional, can be in main .sty)
├── <univers>-commands.sty     # Custom commands (optional)
├── main-template.tex          # Document template
└── README.md                  # Documentation
```

### 2. Add New Command

**Steps:**

1. Open `<univers>/.templates/<univers>-commands.sty` (or `<univers>.sty` if single file)
2. Add command definition
3. Update version in `\ProvidesPackage`
4. Document in README.md
5. Test compilation on existing projects:
   ```bash
   grep -r "usepackage{<univers>}" <univers>/*/main.tex
   ```

### 3. Modify Color

**Steps:**

1. Open `<univers>/.templates/<univers>-colors.sty` (or `<univers>.sty` if single file)
2. Modify `\definecolor`
3. Update version
4. Test all projects

## Template Structure

### Main Package (<univers>.sty)

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{<univers>}[YYYY/MM/DD Template <Univers>]

\RequirePackage[utf8]{inputenc}
\RequirePackage[T1]{fontenc}
\RequirePackage[french]{babel}
\RequirePackage{xcolor}
\RequirePackage{tcolorbox}

\RequirePackage{<univers>-colors}
\RequirePackage{<univers>-commands}

\endinput
```

### Colors Package (<univers>-colors.sty)

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{<univers>-colors}[YYYY/MM/DD Colors]

\RequirePackage{xcolor}

\definecolor{<univers>blue}{RGB}{44,62,80}
\definecolor{<univers>beige}{RGB}{236,229,216}

\endinput
```

### Commands Package (<univers>-commands.sty)

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesPackage{<univers>-commands}[YYYY/MM/DD Commands]

\RequirePackage{tcolorbox}
\RequirePackage{<univers>-colors}

% Quote box
\newtcolorbox{quotebox}{
  colback=<univers>beige,
  colframe=<univers>blue,
  boxrule=0.5pt,
  arc=2mm
}

\endinput
```

## Validation

After any template change:

1. **Scan affected projects**
   ```bash
   grep -r "usepackage{<univers>}" <univers>/*/main.tex
   ```

2. **Test compilation**

3. **Report impact**
   ```markdown
   # Template Modification: <univers>

   **Date:** <date>
   **Change:** <description>

   ## Impact
   - Project-1: OK
   - Project-2: OK
   ```

## Examples

```
"Create LaTeX template for universe Archipels"
"Add \encadresecret command to Archipels template"
"Change archipelsbeige color to lighter shade"
"List available commands in Ecryme template"
"Test template on all projects"
```

## Golden Rules

1. **Modularity**: Separate colors/commands/structure
2. **Backward compatibility**: Avoid breaking changes
3. **Documentation**: Every command documented with example
4. **Testing**: Always compile after modification
5. **Versioning**: Increment version in `\ProvidesPackage`
