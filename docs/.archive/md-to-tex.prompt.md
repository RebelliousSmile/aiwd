---
name: md-to-tex
description: Convert Markdown chapters to LaTeX using universe .sty templates
argument-hint: <chapter.md | --all> [--preview | --dry-run] [--output <dir>]
---

# Markdown to LaTeX Converter

## Workflow Position

```
write-novel / write-roleplaying (Markdown)
                ↓
          output-style (ton, vocabulaire)
                ↓
        ▶ md-to-tex ◀ ← lit le .sty
                ↓
          chapitre.tex
                ↓
           build-pdf
```

## Goal

Convert Markdown chapters to LaTeX, using ONLY commands defined in the universe's `.sty` template.

## Context

Load from `bank.yml`:
- **templates.package** → `.sty` file (source of truth)
- **output-style.global** → Writing conventions (PETITES CAPS, dialogues...)

## Rules

- Use ONLY commands found in the `.sty`
- Respect conventions from output-style
- Generated `.tex` = content only (no `\documentclass`)
- Preserve section hierarchy
- Convert typography: `—` → `---`
- Escape LaTeX special characters
- Flag patterns without .sty equivalent

## Process

### Step 1: Analyze .sty

Parse the `.sty` file and list available constructs.

**Example output** *(actual analysis depends on the .sty)*:
```
=== .sty ANALYSIS: [univers].sty ===

Environments:
  - [env1]            → [description]
  - [env2][opt]{arg}  → [description]
  ...

Commands:
  - \[cmd1]{arg}      → [description]
  - \[cmd2]{a}{b}     → [description]
  ...

Standard LaTeX (always available):
  - \textsc{}, \textbf{}, \textit{}, \texttt{}
  - \section{}, \subsection{}, \subsubsection{}
  - tabular, itemize, enumerate
```

### Step 2: Convert Patterns

Map Markdown patterns to .sty constructs found in Step 1.

#### Block quotes → Read-aloud environment
```markdown
> *La brume s'accroche aux arbres morts.*
```
```latex
\begin{[read-aloud-env]}
La brume s'accroche aux arbres morts.
\end{[read-aloud-env]}
```
*→ Use environment for read-aloud text from .sty*

#### Semantic headers → Custom environments
```markdown
## PHASE DE COMBAT : L'embuscade

Contenu...
```
```latex
\begin{[phase-env]}[combat]{L'embuscade}
Contenu...
\end{[phase-env]}
```
*→ Use phase/section environment from .sty if available*

#### Tables → tabular
```markdown
| Col1 | Col2 | Col3 |
|------|------|------|
| A    | B    | C    |
```
```latex
\begin{tabular}{|c|c|c|}
\hline
\textbf{Col1} & \textbf{Col2} & \textbf{Col3} \\
\hline
A & B & C \\
\hline
\end{tabular}
```

#### Lists
```markdown
- Élément A
- Élément B
```
```latex
\begin{itemize}
  \item Élément A
  \item Élément B
\end{itemize}
```

```markdown
1. Première étape
2. Deuxième étape
```
```latex
\begin{enumerate}
  \item Première étape
  \item Deuxième étape
\end{enumerate}
```

#### Inline formatting
| Markdown | LaTeX | Détection |
|----------|-------|-----------|
| `**gras**` | `\textbf{}` | Double astérisques |
| `*italique*` | `\textit{}` | Simple astérisques |
| `` `code` `` | `\texttt{}` | Backticks |
| `— dialogue` | `--- dialogue` | Tiret cadratin en début de ligne |

#### PETITES CAPS (convention output-style)
Selon l'output-style, les noms propres de lieux à première mention sont en MAJUSCULES :
```markdown
Ils arrivèrent à GREEN VALLEY RANCH au crépuscule.
```
```latex
Ils arrivèrent à \textsc{Green Valley Ranch} au crépuscule.
```
*→ Convertir en small caps avec casse normale*

#### Escape special characters
LaTeX réserve certains caractères à échapper :

| Caractère | Échappement |
|-----------|-------------|
| `%` | `\%` |
| `&` | `\&` |
| `#` | `\#` |
| `_` | `\_` |
| `$` | `\$` |
| `{` `}` | `\{` `\}` |

*Exception : ne pas échapper dans les commandes LaTeX générées.*

### Step 3: Handle Structure

**Content file** (no preamble):
```latex
% Source: chapitres/acte1.md
% Converted: [date]

\section{Titre du chapitre}

[content]
```

**With sub-files:**
```latex
\section{Acte 1}
\input{chapitres/scene1}
\input{chapitres/scene2}
```

### Step 4: Handle Unknowns

When no .sty command matches:

```
=== WARNINGS ===

Line 45: No .sty equivalent
  Pattern: ```mermaid```
  Action: SKIP — added TODO comment

Line 78: Ambiguous
  Pattern: **Note importante**
  Fallback: \textbf{} (no notebox in .sty)
```

**Fallback hierarchy:**
1. Similar .sty environment → use it
2. Standard LaTeX → use it
3. None → skip + `% TODO:` comment

### Step 5: Validate

- [ ] All `\begin{}` have `\end{}`
- [ ] No undefined commands
- [ ] Braces balanced
- [ ] Special characters escaped
- [ ] UTF-8 preserved

## Options

| Option | Description |
|--------|-------------|
| `--preview`, `--dry-run` | Show conversion, don't write |
| `--all` | Convert all `.md` in `chapitres/` |
| `--output <dir>` | Output directory (default: same as source) |

## Output

```
=== CONVERTED: acte1.md → acte1.tex ===

.sty: [univers].sty
Environments: [env1]×4, [env2]×3
Commands: \[cmd]×12

Warnings: 1
Lines: 245

Written: chapitres/acte1.tex
```

## Ignored

Skip with note in output:

| Pattern | Raison |
|---------|--------|
| ` ```mermaid `, ` ```diagram ` | Diagrammes non supportés |
| `![alt](image.png)` | Images → gestion manuelle ou `\includegraphics` |
| `<!-- comment -->` | Commentaires HTML ignorés |

*Pour les images : ajouter manuellement `\includegraphics{path}` ou utiliser un prompt dédié.*

## Checklist

- [ ] .sty parsed dynamically
- [ ] Output-style conventions respected
- [ ] Tables converted
- [ ] Lists converted (itemize + enumerate)
- [ ] Special characters escaped
- [ ] Environments properly closed
- [ ] Unknowns have fallback + TODO
