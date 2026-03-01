# Snippets et Formats de Présentation LaTeX Universels

**Version:** 1.0
**Date:** 2025-11-08
**Source:** Basé sur archipels_template.tex
**Application:** Tous univers (conventions communes)

---

## Vue d'Ensemble

Ce document référence les **snippets et formats de présentation LaTeX communs** à tous les templates d'univers. Ces éléments constituent la **base universelle** que chaque template peut adapter selon son esthétique.

**Principe:** Structure commune, stylisation par univers.

---

## 1. Structure de Base Document

### 1.1 Déclaration Document

```latex
\documentclass[10pt,a4paper,twoside]{article}
```

**Variantes:**
```latex
\documentclass[11pt,a4paper,twoside]{book}      % Pour longs ouvrages
\documentclass[10pt,a4paper,oneside]{article}   % Pour documents unilatéraux
```

**Paramètres standard:**
- `10pt` ou `11pt` : Taille police de base
- `a4paper` : Format page (Europe)
- `twoside` : Recto-verso (marges alternes)
- `oneside` : Recto uniquement

### 1.2 Packages Essentiels

```latex
% Encodage et langue
\usepackage[utf8]{inputenc}              % UTF-8 obligatoire
\usepackage[T1]{fontenc}                 % Encodage police
\usepackage[french]{babel}               % Langue française (optionnel)

% Mise en page
\usepackage[top=2cm,bottom=2cm,left=2cm,right=2cm,columnsep=0.8cm]{geometry}
\usepackage{multicol}                    % Colonnes multiples
\usepackage{enumitem}                    % Listes personnalisées

% Typographie
\usepackage{palatino}                    % Police élégante (ou autre)
\usepackage{microtype}                   % Améliore espacement

% Graphiques et couleurs
\usepackage{graphicx}                    % Images
\usepackage{xcolor}                      % Couleurs

% En-têtes et pieds
\usepackage{fancyhdr}                    % En-têtes/pieds personnalisés
\usepackage{titlesec}                    % Titres personnalisés

% Encadrés
\usepackage{tcolorbox}                   % Boîtes colorées
\tcbuselibrary{skins}                    % Styles avancés
\usetikzlibrary{shadows}                 % Ombres portées

% Dessins (pour checkboxes, etc.)
\usepackage{tikz}
```

**Notes:**
- UTF-8 est **obligatoire** (caractères accentués)
- `babel` peut être commenté si problèmes système
- `palatino` peut être remplacé (voir section Polices)

---

## 2. Configuration En-têtes et Pieds de Page

### 2.1 Style Standard (fancyhdr)

```latex
\pagestyle{fancy}
\fancyhf{} % Nettoyer tout

% En-têtes
\fancyhead[LE]{\small\itshape\nouppercase{\leftmark}}      % Pages paires, gauche
\fancyhead[RO]{\small\itshape\nouppercase{\rightmark}}     % Pages impaires, droite
\fancyhead[RE,LO]{\small\scshape <Univers> --- <Titre Projet>}  % Titre général

% Pieds de page
\fancyfoot[C]{}                                             % Vider centre
\fancyfoot[LE,RO]{\large\bfseries\thepage}                  % Numéros bords extérieurs
\fancyfoot[RE,LO]{\small\itshape\today}                     % Date bords intérieurs

% Lignes décoratives
\renewcommand{\headrulewidth}{1pt}
\renewcommand{\footrulewidth}{1pt}
```

**Variables:**
- `\leftmark` : Nom du chapitre (`\section`)
- `\rightmark` : Nom de la section (`\subsection`)
- `LE` : Left Even (gauche, pages paires)
- `RO` : Right Odd (droite, pages impaires)
- `RE` : Right Even (droite, pages paires)
- `LO` : Left Odd (gauche, pages impaires)

### 2.2 Style Pages de Chapitre (plain)

```latex
\fancypagestyle{plain}{
  \fancyhf{}
  \fancyfoot[C]{\large\bfseries\thepage}  % Numéro centré
  \renewcommand{\headrulewidth}{0pt}       % Pas de ligne en-tête
  \renewcommand{\footrulewidth}{1pt}       % Ligne pied de page
}
```

**Usage:** Appliqué automatiquement première page chapitre.

### 2.3 Variantes par Univers

**Archipels:**
```latex
\fancyhead[RE,LO]{\small\scshape Archipels --- Carnets de Voyages}
```

**La Roue du Temps:**
```latex
\fancyhead[RE,LO]{\small La Roue du Temps --- <Titre Projet>}
```

**Ecryme:**
```latex
\fancyhead[RE,LO]{\small\fantasyfont Ecryme --- <Titre Projet>}
```

---

## 3. Titres de Sections

### 3.1 Configuration Standard

```latex
\usepackage{titlesec}

% Section (niveau 1)
\titleformat{\section}
  {\normalfont\Huge\bfseries\scshape\centering}
  {}{0em}{}[\vspace{0.3em}]

% Subsection (niveau 2)
\titleformat{\subsection}
  {\normalfont\LARGE\bfseries}
  {}{0em}{}[\vspace{0.2em}]

% Subsubsection (niveau 3)
\titleformat{\subsubsection}
  {\normalfont\Large\bfseries\itshape}
  {}{0em}{}
```

**Résultat:**
- **Section:** Centré, grandes capitales, gras
- **Subsection:** Gras, grand
- **Subsubsection:** Gras italique, moyen

### 3.2 Usage

```latex
\section{Houlemorte}                  % Chapitre principal
\subsection{La Geste de l'Arbre}      % Section
\subsubsection{Le Crime des Elfes}    % Sous-section
```

### 3.3 Sections Non Numérotées

```latex
\section*{Introduction}               % Pas de numéro, pas dans TOC
```

---

## 4. Palette de Couleurs

### 4.1 Définition Couleurs

```latex
\usepackage{xcolor}

% Exemple Archipels
\definecolor{archipelsblue}{RGB}{44,62,80}
\definecolor{archipelsbeige}{RGB}{236,229,216}
\definecolor{archipelsred}{RGB}{140,30,30}
\definecolor{archipelsgold}{RGB}{184,134,11}
```

**Format:** `\definecolor{nom}{RGB}{R,G,B}`

**Variantes:**
```latex
\definecolor{nom}{HTML}{RRGGBB}       % Hexadécimal
\definecolor{nom}{cmyk}{C,M,Y,K}      % CMYK pour impression
```

### 4.2 Utilisation Couleurs

```latex
\textcolor{archipelsblue}{Texte en bleu}
\colorbox{archipelsbeige}{Fond beige}
\fcolorbox{archipelsblue}{white}{Bordure bleue, fond blanc}
```

### 4.3 Palettes par Univers

**Archipels:**
- `archipelsblue` : RGB(44,62,80) - Bleu marine
- `archipelsbeige` : RGB(236,229,216) - Beige parchemin
- `archipelsred` : RGB(140,30,30) - Rouge accent
- `archipelsgold` : RGB(184,134,11) - Or détails

**La Roue du Temps (à définir):**
- `wotblue` : Bleu Aes Sedai
- `wotgold` : Or Dragon
- `wotred` : Rouge Ténèbres
- `wotparchment` : Parchemin

**Ecryme (à définir):**
- `ecrymecopper` : RGB(184,115,51) - Cuivre
- `ecrymeiron` : RGB(67,70,75) - Fer sombre
- `ecrymeshadow` : RGB(28,20,13) - Ombre
- `ecrymeparchment` : RGB(230,218,206) - Parchemin vieilli

---

## 5. Encadrés (tcolorbox)

### 5.1 Encadré Statistiques (statsbox)

**Code:**
```latex
\newtcolorbox{statsbox}[1]{
  enhanced,
  colback=<couleur-fond>,
  colframe=<couleur-bordure>,
  fonttitle=\bfseries\Large,
  title=#1,
  arc=3mm,
  boxrule=2pt,
  left=8pt,
  right=8pt,
  top=8pt,
  bottom=8pt,
  before skip=15pt,
  after skip=15pt,
  drop shadow={opacity=0.3}
}
```

**Usage:**
```latex
\begin{statsbox}{Houlemorte}
  \textbf{Population:} 8000 habitants\\
  \textbf{Gouvernement:} Oligarchie marchande\\
  \textbf{Commerce:} Épices, soieries
\end{statsbox}
```

**Variantes:**
- `statsbox` : Lieux, PNJ, organisations
- Adapter couleurs selon univers
- Titre = argument `{Nom}`

### 5.2 Encadré Citations (quotebox)

**Code:**
```latex
\newtcolorbox{quotebox}{
  enhanced,
  colback=<couleur-fond>!50,
  colframe=<couleur-accent>,
  boxrule=1pt,
  arc=2mm,
  left=8pt,
  right=8pt,
  top=8pt,
  bottom=8pt,
  before skip=10pt,
  after skip=10pt,
  borderline west={3pt}{0pt}{<couleur-accent>}
}
```

**Usage:**
```latex
\begin{quotebox}
  Les vents portent les secrets des îles.
\end{quotebox}
```

**Particularité:** Barre gauche épaisse (3pt) pour marquer citation.

### 5.3 Encadré Règles (rulebox)

**Code:**
```latex
\newtcolorbox{rulebox}{
  enhanced,
  colback=white,
  colframe=<couleur-bordure>,
  boxrule=1.5pt,
  arc=1mm,
  left=6pt,
  right=6pt,
  top=6pt,
  bottom=6pt,
  before skip=8pt,
  after skip=8pt
}
```

**Usage:**
```latex
\begin{rulebox}
  \textbf{Jet de Perception:} Lancer 2d6 + modificateur.
  Sur 10+, succès total.
\end{rulebox}
```

**Utilité:** Règles de jeu, actions spéciales, mécanismes.

### 5.4 Paramètres tcolorbox Courants

| Paramètre | Description | Exemple |
|-----------|-------------|---------|
| `colback` | Couleur fond | `archipelsbeige` |
| `colframe` | Couleur bordure | `archipelsblue` |
| `boxrule` | Épaisseur bordure | `2pt` |
| `arc` | Arrondi coins | `3mm` |
| `left`, `right`, `top`, `bottom` | Marges internes | `8pt` |
| `before skip`, `after skip` | Espaces avant/après | `15pt` |
| `drop shadow` | Ombre portée | `{opacity=0.3}` |
| `borderline west` | Ligne côté gauche | `{3pt}{0pt}{couleur}` |

---

## 6. Commandes Personnalisées

### 6.1 Citations avec Auteur

**Définition:**
```latex
\newcommand{\citation<univers>}[2]{%
  \begin{quotebox}
    \itshape\small
    ``#1''
    \begin{flushright}
      \normalfont--- \textbf{#2}
    \end{flushright}
  \end{quotebox}
}
```

**Usage:**
```latex
\citationarchipels{Les vents portent les secrets.}{Proverbe marin}
```

**Résultat:**
- Citation en italique, guillemets anglais
- Auteur en gras, aligné à droite
- Encadré via `quotebox`

**Variantes par univers:**
- `\citationarchipels{texte}{auteur}` : Archipels
- `\citationwot{texte}{source}` : La Roue du Temps
- `\citationecryme{texte}` : Ecryme (sans auteur souvent)

### 6.2 Talent (JdR)

**Définition:**
```latex
\newcommand{\talent}[3]{%
  \noindent\textbf{#1}%
  \ifx&#2&%
  \else%
    \ \textit{(#2)}%
  \fi
  \par
  \noindent #3
  \par\vspace{0.5em}
}
```

**Usage:**
```latex
\talent{Navigation Experte}{Prérequis: Niveau 3}{Le personnage peut naviguer
sans carte dans les Mers de Brume.}
```

**Résultat:**
- **Nom du talent** en gras
- *(Prérequis)* en italique (si présent)
- Description en dessous

### 6.3 Équipement

**Définition:**
```latex
\newcommand{\equipement}[3]{%
  \noindent\textbf{#1} --- #2
  \par
  \noindent\small #3
  \par\vspace{0.3em}
}
```

**Usage:**
```latex
\equipement{Corde magique}{50 PO}{Corde enchantée qui peut s'allonger
ou se rétracter sur commande.}
```

**Résultat:**
- **Nom** --- Prix
- Description (petite taille)

### 6.4 PNJ Simple

**Définition:**
```latex
\newcommand{\pnj}[2]{%
  \noindent\textbf{#1} : #2
  \par
}
```

**Usage:**
```latex
\pnj{Ziaa, Grande Prêtresse}{Gardienne du Bois Sacré, autorité spirituelle suprême}
```

**Résultat:**
- **Nom** : Description courte

### 6.5 Checkbox (Listes Interactives)

**Définition:**
```latex
\usepackage{tikz}
\newcommand{\checkbox}{\tikz{\draw[black,thick] (0,0) rectangle (0.3,0.3);}\hspace{0.2em}}
```

**Usage:**
```latex
\checkbox \textbf{Tâche à accomplir}
\checkbox \textbf{Autre tâche}
```

**Résultat:** ☐ Case à cocher (style Dungeon World).

### 6.6 Police Fantasy (Optionnel)

**Définition:**
```latex
\newcommand{\fantasyfont}{\fontfamily{pzc}\selectfont}
```

**Usage:**
```latex
{\fantasyfont Texte stylisé fantasy}
```

**Note:** Police Zapf Chancery (pzc), peut nécessiter installation.

---

## 7. Mise en Page

### 7.1 Colonnes Multiples

**Usage standard:**
```latex
\begin{multicols}{2}
  Texte distribué automatiquement sur deux colonnes.
  Les colonnes sont équilibrées automatiquement.
\end{multicols}
```

**Dans encadré statsbox:**
```latex
\begin{statsbox}{Titre}
\begin{multicols}{2}
  \textbf{Info 1:} Valeur\\
  \textbf{Info 2:} Valeur

  \columnbreak  % Forcer saut colonne

  \textbf{Info 3:} Valeur\\
  \textbf{Info 4:} Valeur
\end{multicols}
\end{statsbox}
```

**Nombre colonnes:** `{2}`, `{3}`, etc.

**Commandes:**
- `\columnbreak` : Forcer saut à colonne suivante
- `\vfill` : Remplir espace vertical

### 7.2 Espacements

```latex
\vspace{1em}       % Espace vertical (1em = hauteur 'M')
\vspace{0.5cm}     % Espace vertical absolu
\vfill             % Remplir tout l'espace disponible

\hspace{2em}       % Espace horizontal
\hfill             % Remplir horizontalement
```

**Unités:**
- `em` : Relatif à taille police
- `cm`, `mm` : Absolu
- `pt` : Points (1pt = 1/72 pouce)

### 7.3 Alignements

```latex
\begin{center}
  Texte centré
\end{center}

\begin{flushleft}
  Texte aligné à gauche
\end{flushleft}

\begin{flushright}
  Texte aligné à droite
\end{flushright}
```

---

## 8. Listes

### 8.1 Listes à Puces (itemize)

**Standard:**
```latex
\begin{itemize}
  \item Premier élément
  \item Deuxième élément
  \item Troisième élément
\end{itemize}
```

**Personnalisée (enumitem):**
```latex
\begin{itemize}[leftmargin=*,noitemsep,topsep=0pt]
  \item Élément sans marge gauche
  \item Pas d'espace entre items
\end{itemize}
```

**Options enumitem:**
- `leftmargin=*` : Pas de retrait gauche
- `noitemsep` : Pas d'espace entre items
- `topsep=0pt` : Pas d'espace avant/après liste

### 8.2 Listes Numérotées (enumerate)

```latex
\begin{enumerate}
  \item Premier
  \item Deuxième
  \item Troisième
\end{enumerate}
```

**Personnalisée:**
```latex
\begin{enumerate}[label=\arabic*.,leftmargin=*]
  \item Numérotation: 1., 2., 3.
\end{enumerate}
```

### 8.3 Listes de Descriptions

```latex
\begin{description}
  \item[Terme 1] Définition du terme 1
  \item[Terme 2] Définition du terme 2
\end{description}
```

---

## 9. Page de Titre

### 9.1 Template Standard

```latex
\begin{document}

\begin{center}
  \vspace*{2cm}
  {\fontsize{48}{60}\selectfont\bfseries\scshape <UNIVERS>}\\[1.5cm]
  {\Huge\itshape <Titre Projet>}\\[4cm]
  {\LARGE <Sous-titre>}\\[2cm]
  \vfill
  {\normalsize Par <Auteur>}\\[0.5cm]
  {\small <Date>}
\end{center}

\newpage
```

**Éléments:**
- Titre univers: Grandes capitales (48pt), gras, petites caps
- Titre projet: Énorme (Huge), italique
- Sous-titre: Large (LARGE)
- Auteur: Normal, en bas
- `\vfill` : Pousse auteur vers le bas

### 9.2 Variantes

**Avec image:**
```latex
\begin{center}
  \vspace*{1cm}
  \includegraphics[width=0.4\textwidth]{images/logo-univers.pdf}\\[1cm]
  {\fontsize{48}{60}\selectfont\bfseries\scshape <UNIVERS>}\\[1.5cm]
  [...]
\end{center}
```

**Avec ligne décorative:**
```latex
{\Huge\itshape <Titre>}\\[0.5cm]
\rule{0.7\textwidth}{0.8pt}\\[1cm]
{\LARGE <Sous-titre>}
```

---

## 10. Table des Matières

### 10.1 Génération

```latex
\tableofcontents
\newpage
```

**Note:** Compiler **deux fois** pour générer TOC.

### 10.2 Profondeur

```latex
\setcounter{tocdepth}{2}  % Sections et subsections seulement
```

**Niveaux:**
- `0` : Chapters uniquement (book)
- `1` : + Sections
- `2` : + Subsections
- `3` : + Subsubsections

### 10.3 Personnalisation

```latex
\renewcommand{\contentsname}{Sommaire}  % Changer titre TOC
```

---

## 11. Images

### 11.1 Insertion Simple

```latex
\includegraphics[width=0.5\textwidth]{images/carte.pdf}
```

**Options:**
- `width=0.5\textwidth` : 50% largeur texte
- `height=5cm` : Hauteur fixe
- `scale=0.8` : Échelle 80%
- `angle=90` : Rotation 90°

### 11.2 Image Centrée avec Légende

```latex
\begin{figure}[h]
  \centering
  \includegraphics[width=0.8\textwidth]{images/carte.pdf}
  \caption{Carte des Archipels}
  \label{fig:carte}
\end{figure}
```

**Positions:**
- `[h]` : Here (ici)
- `[t]` : Top (haut page)
- `[b]` : Bottom (bas page)
- `[p]` : Page dédiée

**Référence:**
```latex
Voir Figure~\ref{fig:carte}.
```

---

## 12. Tableaux

### 12.1 Tableau Simple

```latex
\begin{tabular}{|l|c|r|}
\hline
Gauche & Centré & Droite \\
\hline
A & B & C \\
D & E & F \\
\hline
\end{tabular}
```

**Alignements:**
- `l` : Left (gauche)
- `c` : Center (centré)
- `r` : Right (droite)
- `|` : Ligne verticale

**Commandes:**
- `\\` : Nouvelle ligne
- `\hline` : Ligne horizontale
- `&` : Séparateur colonnes

### 12.2 Tableau dans Encadré

```latex
\begin{statsbox}{Statistiques}
\begin{tabular}{ll}
\textbf{Population:} & 8000 habitants \\
\textbf{Gouvernement:} & Oligarchie \\
\end{tabular}
\end{statsbox}
```

---

## 13. Typographie

### 13.1 Emphases

```latex
\textit{italique}
\textbf{gras}
\textsc{petites capitales}
\underline{souligné}
```

**Combinaisons:**
```latex
\textbf{\textit{gras italique}}
\textsc{\textbf{petites caps gras}}
```

### 13.2 Tailles

```latex
{\tiny minuscule}
{\scriptsize très petit}
{\footnotesize petit}
{\small petit}
{\normalsize normal}
{\large grand}
{\Large plus grand}
{\LARGE très grand}
{\huge énorme}
{\Huge gigantesque}
```

### 13.3 Polices

**Famille:**
```latex
\textrm{Roman (serif)}
\textsf{Sans-serif}
\texttt{Monospace (code)}
```

**Personnalisées:**
```latex
\usepackage{palatino}     % Palatino
\usepackage{times}        % Times
\usepackage{helvet}       % Helvetica
\usepackage{courier}      % Courier
```

---

## 14. Caractères Spéciaux

### 14.1 Échappement Obligatoire

```latex
\%    % Pourcent
\$    % Dollar
\&    % Esperluette
\#    % Dièse
\_    % Souligné
\{    % Accolade ouvrante
\}    % Accolade fermante
\textbackslash    % Backslash
\textasciitilde   % Tilde ~
\textasciicircum  % Circonflexe ^
```

### 14.2 Guillemets

```latex
``Citation''              % Guillemets anglais (LaTeX standard)
\og Citation \fg{}        % Guillemets français (avec babel)
```

### 14.3 Tirets

```latex
Trait d'union: -
Tiret demi-cadratin: --  (plages: 1--10)
Tiret cadratin: ---      (dialogues, ponctuation)
```

### 14.4 Points de Suspension

```latex
\ldots   % Points de suspension LaTeX (espacement correct)
```

---

## 15. Organisation Fichiers

### 15.1 Structure Projet Type

```
projet/
├── main.tex              % Document principal
├── chapitres/            % Chapitres séparés
│   ├── 01_intro.tex
│   ├── 02_chapitre1.tex
│   └── 03_chapitre2.tex
├── images/               % Images
│   └── carte.pdf
└── sources/              % Sources texte (si transcription)
    └── chapitre1.txt
```

### 15.2 Inclusion Fichiers

**Dans main.tex:**
```latex
\input{chapitres/01_intro}
\input{chapitres/02_chapitre1}
\input{chapitres/03_chapitre2}
```

**Note:** Pas d'extension `.tex` dans `\input{}`

### 15.3 Chemins

**Toujours chemins Unix (/):**
```latex
\input{chapitres/fichier}           % Correct
\includegraphics{images/image.pdf}   % Correct
```

**Jamais chemins Windows (\):**
```latex
\input{chapitres\fichier}           % ERREUR
```

---

## 16. Compilation

### 16.1 Commande Standard

```bash
pdflatex main.tex
pdflatex main.tex  # Deux fois pour TOC et références
```

### 16.2 Avec Index/Glossaire

```bash
pdflatex main.tex
makeindex main
pdflatex main.tex
pdflatex main.tex
```

### 16.3 Nettoyage

**Fichiers temporaires à ignorer (.gitignore):**
```
*.aux
*.log
*.toc
*.out
*.fdb_latexmk
*.fls
*.synctex.gz
*.idx
*.ilg
*.ind
```

---

## 17. Checklist Utilisation Snippets

### Pour Créer Nouveau Template Univers

- [ ] Copier structure packages de base (section 1.2)
- [ ] Définir palette couleurs univers (section 4.1)
- [ ] Créer encadrés `statsbox`, `quotebox`, `rulebox` (section 5)
- [ ] Définir commande `\citation<univers>{}{}` (section 6.1)
- [ ] Configurer en-têtes/pieds avec nom univers (section 2)
- [ ] Adapter titres sections si besoin (section 3)
- [ ] Créer page de titre personnalisée (section 9)

### Pour Utiliser Template Existant

- [ ] Charger template: `\usepackage{<univers>}`
- [ ] Utiliser commandes template: `\citation<univers>{}{}`, etc.
- [ ] Respecter palette couleurs définie
- [ ] Compiler deux fois (TOC)
- [ ] Vérifier avec `latex-validator` skill

---

## 18. Références

### 18.1 Documentation LaTeX

- **Wikibooks LaTeX:** https://en.wikibooks.org/wiki/LaTeX
- **CTAN (packages):** https://www.ctan.org/
- **tcolorbox manuel:** `texdoc tcolorbox`

### 18.2 Fichiers Templates Projet

- **Base Archipels:** `archipels/Archipels___Carnets_de_voyage/archive/archipels_template.tex`
- **Templates univers:** `templates/<univers>/<univers>.sty`

### 18.3 Documentation Projet

- **Output-styles:** `docs/output-styles/latex-<univers>.md`
- **Skills:** `.claude/skills/latex-validator/`, `latex-transcriber/`
- **Agents:** `.claude/agents/latex-template-manager.md`

---

**Version:** 1.0
**Dernière mise à jour:** 2025-11-08
**Basé sur:** archipels_template.tex
**Mainteneur:** latex-template-manager agent
