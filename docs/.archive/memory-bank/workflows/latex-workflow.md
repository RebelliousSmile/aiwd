# Workflow LaTeX - Guide Complet

## Vue d'ensemble

Ce document décrit le workflow de production des documents LaTeX dans le projet PDFLateX.

---

## Structure des Fichiers

Chaque projet utilise une séparation claire entre sources et fichiers compilés :

```
<projet>/
├── chapitres/           # Source de vérité (markdown)
│   ├── 00_introduction.md
│   ├── 01_chapitre1.md
│   └── ...
├── latex-partials/      # Fichiers .tex (inclus par main.tex)
│   ├── 00_introduction.tex
│   ├── 01_chapitre1.tex
│   └── ...
├── output/              # PDFs générés
│   └── document.pdf
├── main.tex             # Document maître
└── bank.yml             # Configuration projet
```

### Rôle de chaque dossier

| Dossier | Contenu | Éditable | Généré |
|---------|---------|----------|--------|
| `chapitres/` | Fichiers markdown (source) | Oui | Non |
| `latex-partials/` | Fichiers .tex (partials) | Oui* | Optionnel |
| `output/` | PDFs compilés | Non | Oui |

\* Les fichiers .tex peuvent être édités directement ou générés depuis les .md

---

## Flux de Travail

### Option A : Markdown → LaTeX → PDF

```
chapitres/*.md  →  latex-partials/*.tex  →  output/*.pdf
     ↓                    ↓                      ↓
  Rédaction          Conversion            Compilation
  (source)           (md-to-tex)           (pdflatex)
```

**Quand utiliser :** Projets longs avec révisions fréquentes. Le markdown facilite l'édition et le versioning.

### Option B : LaTeX Direct → PDF

```
latex-partials/*.tex  →  output/*.pdf
        ↓                      ↓
    Rédaction             Compilation
    (source)              (pdflatex)
```

**Quand utiliser :** Projets courts ou contenu technique nécessitant un contrôle fin du LaTeX.

---

## Le Fichier main.tex

Le fichier `main.tex` est le document maître qui :
1. Définit la classe de document
2. Charge le package de l'univers (`.sty`)
3. Inclut les partials dans l'ordre

### Structure Typique

```latex
\documentclass[10pt,a4paper,twoside]{article}
\usepackage[top=2cm,bottom=2cm,left=2cm,right=2cm]{geometry}

% Charge le style de l'univers
\usepackage{../.templates/<univers>}

\begin{document}

% Page de titre
\begin{titlepage}
...
\end{titlepage}

% Table des matières
\tableofcontents
\newpage

% Inclusion des partials
\input{latex-partials/00_introduction}
\input{latex-partials/01_chapitre1}
\input{latex-partials/02_chapitre2}
% ...

\end{document}
```

### Bonnes Pratiques

1. **Chemins relatifs** : Toujours utiliser `latex-partials/` dans les `\input{}`
2. **Ordre logique** : Numéroter les fichiers pour clarifier l'ordre
3. **Modularité** : Un fichier par chapitre ou section majeure
4. **Pas de `\begin{document}`** dans les partials

---

## Conversion Markdown → LaTeX

### Prompt md-to-tex

```bash
@docs/prompts/workshop/md-to-tex.prompt.md chapitres/01_chapitre1.md
```

Le prompt convertit :
- Titres markdown → `\section{}`, `\subsection{}`
- Listes → `\begin{itemize}` / `\begin{enumerate}`
- Emphase → `\textbf{}`, `\textit{}`
- Citations → environnements personnalisés
- Tables → `\begin{tabular}`

### Commandes Spécifiques Univers

Chaque univers a ses commandes dans le fichier `.sty` :

| Univers | Exemples de commandes |
|---------|----------------------|
| `wot` | `\saidin`, `\saidar`, `\terangreal`, `\aessedai` |
| `archipels` | `\archipel`, `\navigateur`, `\tempete` |
| `ecryme` | `\ecryme`, `\clameur`, `\machinerie` |

---

## Compilation PDF

### Commande Simple

```bash
python scripts/build-pdf.py --project "<univers>/<projet>" --clean
```

### Compilation Manuelle

```bash
cd <univers>/<projet>
pdflatex main.tex
pdflatex main.tex  # Deux fois pour la TOC
mv main.pdf output/
```

### Pourquoi Deux Passes ?

1. **Première passe** : Génère les références (TOC, labels)
2. **Deuxième passe** : Résout les références et numérote correctement

---

## Configuration bank.yml

La section `compilation` du `bank.yml` documente la structure :

```yaml
compilation:
  main-file: "main.tex"
  output-dir: "output/"
  partials-dir: "latex-partials/"
  chapitres-source: "chapitres/"
  partials:
    - "latex-partials/00_introduction.tex"
    - "latex-partials/01_chapitre1.tex"
    # ...
```

---

## Templates Univers (.sty)

Chaque univers a son package LaTeX dans `<univers>/.templates/<univers>.sty`.

### Contenu Typique

```latex
\ProvidesPackage{<univers>}

% Polices
\usepackage{fontspec}
\setmainfont{...}

% Couleurs
\definecolor{<univers>primary}{HTML}{...}

% Environnements personnalisés
\newenvironment{quotebox}{...}{...}
\newenvironment{statblock}{...}{...}

% Commandes spécifiques
\newcommand{\terme}[1]{\textsc{#1}}
```

### Référence Complète

Voir `docs/memory-bank/templates-reference.md` pour la liste des commandes par univers.

---

## Dépannage

### Erreur : "File not found"

Vérifier que le chemin dans `\input{}` correspond au dossier `latex-partials/`.

### Erreur : "Undefined control sequence"

Le package univers n'est pas chargé. Vérifier le chemin dans `\usepackage{../.templates/<univers>}`.

### Table des matières vide

Compiler deux fois avec `pdflatex`.

### Caractères spéciaux cassés

Utiliser UTF-8 et `\usepackage[utf8]{inputenc}` (ou `fontspec` avec XeLaTeX/LuaLaTeX).

---

## Fichiers Auxiliaires

La compilation génère des fichiers temporaires :

| Extension | Description | Garder ? |
|-----------|-------------|----------|
| `.aux` | Références croisées | Non |
| `.log` | Journal de compilation | Non |
| `.toc` | Table des matières | Non |
| `.out` | Hyperliens PDF | Non |
| `.pdf` | Document final | Oui (dans output/) |

Le flag `--clean` du script `build-pdf.py` supprime ces fichiers automatiquement.
