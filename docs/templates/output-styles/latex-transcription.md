# Output-Style: Transcription Pure (Universel)

**Type:** Transcription fidèle
**Univers:** Tous (universel)
**Priorité:** Fidélité maximale au texte source

## Principe Fondamental

**Transcription ≠ Rédaction**

Ce style s'applique aux projets de **transcription pure** où l'objectif est de convertir un texte source existant (OCR, manuscrit, document ancien) en LaTeX structuré **sans altérer le contenu**.

### Différences avec Rédaction

| Critère | Transcription | Rédaction |
|---------|---------------|-----------|
| **Source** | Texte existant (OCR, .txt, .md) | Création originale |
| **Fidélité** | Maximale | Créative |
| **Corrections** | OCR uniquement | Réécriture possible |
| **Structure** | Préservée | Flexible |
| **Terminologie** | Originale | Adaptable |
| **Ton** | Préservé | Libre (dans cadre univers) |

## Règles de Transcription

### 1. Fidélité au Texte Source

**TOUJOURS préserver:**
- Contenu exact du texte
- Structure (chapitres, sections, paragraphes)
- Citations originales
- Formulations de l'auteur

**JAMAIS modifier:**
- Vocabulaire (sauf erreur OCR évidente)
- Ordre des éléments
- Ton narratif
- Style de l'auteur

### 2. Corrections Autorisées

**UNIQUEMENT corrections OCR:**

```
Avant (OCR erroné):    frax → Arax (correction)
Avant (OCR erroné):    lbysses → Abysses
Avant (OCR erroné):    roleur → voleur
Avant (OCR erroné):    0bjet → Objet
```

**Vérifier contexte** avant correction (risque faux positif).

**Documenter corrections** dans commentaires LaTeX:

```latex
% OCR original: "frax navigateur"
% Corrigé en: "Arax navigateur" (contexte: nom propre récurrent)
```

### 3. Structure LaTeX

**Préserver hiérarchie source:**

```
SOURCE:
# Chapitre I
## Section A
### Sous-section 1

LATEX:
\section{Chapitre I}
\subsection{Section A}
\subsubsection{Sous-section 1}
```

**Pas de réorganisation** même si structure semble améliorable.

### 4. Formatage

**Appliquer template LaTeX de l'univers** SANS modifier contenu:

```latex
% Source: "Citation" - Auteur
% LaTeX Archipels:
\citationarchipels{Citation}{Auteur}

% Source: ENCADRÉ statistiques lieu
% LaTeX Archipels:
\begin{statsbox}{Nom Lieu}
  <contenu préservé>
\end{statsbox}
```

**Adapter forme (LaTeX), préserver fond (contenu).**

### 5. Caractères Spéciaux

**Échapper obligatoirement:**

```latex
Source:  50% de chances
LaTeX:   50\% de chances

Source:  Coût: 10$
LaTeX:   Coût: 10\$

Source:  Chapitre #3
LaTeX:   Chapitre \#3
```

**Liste complète:** `% $ & # _ { } ~ ^ \`

### 6. Guillemets et Apostrophes

**Convertir selon LaTeX:**

```latex
Source:  "Citation"
LaTeX:   ``Citation''  (ou \og Citation \fg{} si babel français)

Source:  l'univers, c'est...
LaTeX:   l'univers, c'est... (OK en UTF-8)
```

### 7. Ponctuation

**Préserver exactement:**
- Points de suspension: `...` → `\ldots` (LaTeX)
- Tirets dialogues: `- ` → `--- ` (tiret cadratin)
- Espaces insécables: Ajouter si typographie française (`M.~Dupont`, `page~42`)

### 8. Listes

**Conserver structure originale:**

```
Source:
- Item 1
- Item 2

LaTeX:
\begin{itemize}
  \item Item 1
  \item Item 2
\end{itemize}
```

**Numérotation originale préservée:**

```
Source:
1. Premier
2. Deuxième

LaTeX:
\begin{enumerate}
  \item Premier
  \item Deuxième
\end{enumerate}
```

### 9. Tableaux

**Reproduire fidèlement:**

```
Source:
| Nom    | Valeur |
|--------|--------|
| Item A | 10     |

LaTeX:
\begin{tabular}{|l|r|}
\hline
Nom & Valeur \\
\hline
Item A & 10 \\
\hline
\end{tabular}
```

### 10. Notes et Annotations

**Distinguer source vs transcripteur:**

```latex
% Note originale de l'auteur:
\footnote{Note de l'auteur original.}

% Note du transcripteur:
% [Note transcripteur: Possible erreur dans source original, vérifier édition papier]
```

## Workflow Transcription

### Étape 1: Préparation Source

1. **Lire source complet** (comprendre structure)
2. **Identifier univers** (Archipels, WoT, Ecryme)
3. **Détecter type document** (narratif, technique, règles)
4. **Évaluer qualité OCR** (bon/moyen/mauvais)

### Étape 2: Conversion Structure

1. **Chapitres → `\section{}`**
2. **Sections → `\subsection{}`**
3. **Sous-sections → `\subsubsection{}`**
4. **Paragraphes → paragraphes LaTeX** (séparés par ligne vide)

### Étape 3: Application Template

1. **Charger template univers** (`<univers>/.templates/`)
2. **Identifier éléments spéciaux:**
   - Citations → `\citation<univers>{}{}`
   - Encadrés → `\begin{<type>box}{}`
   - Listes → `\begin{itemize}` ou `\begin{enumerate}`
3. **Appliquer commandes template** (forme LaTeX)
4. **Préserver contenu exact** (fond textuel)

### Étape 4: Corrections OCR

1. **Scanner erreurs courantes:**
   ```
   rn → m
   l → I (majuscule i)
   o → a (contexte)
   0 → O (chiffre vs lettre)
   ```

2. **Vérifier contexte** avant correction
3. **Documenter corrections** en commentaire
4. **Laisser doutes en commentaire** pour révision manuelle

### Étape 5: Validation

1. **Skill `latex-validator`:** Vérifier syntaxe LaTeX
2. **Compilation test:** `pdflatex` (détecter erreurs restantes)
3. **Relecture manuelle:** Comparer PDF vs source original
4. **Corrections finales**

## Exemples Pratiques

### Exemple 1: Texte Narratif Simple

**Source (texte brut):**
```
Chapitre 1 - La Ville

Houlemorte se dresse sur un promontoire. Les murailles grises
encerclent le port. Les marchands crient sur les quais.

"Où allons-nous ?" demanda le marin.
"Vers le sud," répondit le capitaine.
```

**Transcription LaTeX (Archipels):**
```latex
\section{La Ville}

Houlemorte se dresse sur un promontoire. Les murailles grises
encerclent le port. Les marchands crient sur les quais.

--- Où allons-nous ? demanda le marin.

--- Vers le sud, répondit le capitaine.
```

**Notes:**
- Titre chapitre préservé
- Paragraphes séparés (ligne vide)
- Dialogues avec tirets cadratins (`---`)
- Aucun ajout créatif

### Exemple 2: Texte avec Encadré

**Source (OCR):**
```
HOULEMORTE
Population: 8000 habitants
Gouvernement: Oligarchie marchande
Défense: Milice (200 hommes)
```

**Transcription LaTeX (Archipels):**
```latex
\begin{statsbox}{Houlemorte}
  \textbf{Population:} 8000 habitants\\
  \textbf{Gouvernement:} Oligarchie marchande\\
  \textbf{Défense:} Milice (200 hommes)
\end{statsbox}
```

**Notes:**
- Structure encadré détectée
- Commande template appliquée (`statsbox`)
- Contenu préservé exactement
- Formatage LaTeX (`\textbf`, `\\`)

### Exemple 3: Citation avec Erreur OCR

**Source (OCR):**
```
"Les rents portent les secrets." - Proverbe marin
```

**Transcription LaTeX (corrigée):**
```latex
% OCR original: "rents" → Corrigé: "vents" (contexte: proverbe sur navigation)
\citationarchipels{Les vents portent les secrets.}{Proverbe marin}
```

**Notes:**
- Erreur OCR évidente (`rents` → `vents`)
- Correction documentée
- Commande template appliquée

### Exemple 4: Liste avec Caractères Spéciaux

**Source:**
```
Équipement:
- Corde (10m)
- 50% de rations
- Arme & armure
```

**Transcription LaTeX:**
```latex
Équipement:
\begin{itemize}
  \item Corde (10m)
  \item 50\% de rations
  \item Arme \& armure
\end{itemize}
```

**Notes:**
- Liste détectée → `\begin{itemize}`
- Caractères spéciaux échappés (`\%`, `\&`)
- Contenu préservé

## Checklist Transcription

### Avant de Commencer

- [ ] Source disponible et lisible
- [ ] Univers identifié
- [ ] Template LaTeX disponible
- [ ] Projet initialisé (structure répertoires)

### Pendant Transcription

- [ ] Structure source préservée
- [ ] Commandes template appliquées (forme)
- [ ] Contenu préservé exactement (fond)
- [ ] Caractères spéciaux échappés
- [ ] Corrections OCR documentées
- [ ] Pas d'ajout créatif

### Après Transcription

- [ ] Validation `latex-validator`
- [ ] Compilation test réussie
- [ ] Relecture vs source original
- [ ] Corrections finales appliquées
- [ ] Commit Git avec message descriptif

## Différences selon Univers

**Template LaTeX change, principes transcription IDENTIQUES:**

### Archipels
- Commandes: `\citationarchipels`, `statsbox`, `quotebox`
- Terminologie: Préserver noms lieux originaux

### La Roue du Temps
- **ATTENTION:** Respecter italiques termes Vieille Langue (*saidin*, etc.)
- Commandes: `\motitalique`, `\citationwot`, `pouvoirbox`
- Orthographe stricte (Aes Sedai, etc.)

### Ecryme
- Commandes: `\citationecryme`, `\terme`, `mecanismebox`
- Atmosphère: Préserver ton sombre original

**Dans tous cas:** Fidélité au texte source PRIORITAIRE.

## Interaction avec Skills

### latex-transcriber

**Skill dédiée à transcription.**

Invocation:
```
"Transcris le chapitre 1 de sources/texte.txt"
"Convertis sources/ocr_guerre_ombres.txt en LaTeX"
```

**Applique automatiquement** ce output-style.

### latex-validator

**Validation après transcription.**

Invocation:
```
"Valide le fichier chapitres/01_chapitre.tex"
```

**Vérifie:**
- Syntaxe LaTeX correcte
- Conformité template univers
- Pas d'erreur compilation

### narrative-writer

**NE PAS utiliser pour transcription** (rédaction créative).

Transcription = `latex-transcriber` uniquement.

## Rapport de Transcription

**Format attendu après transcription:**

```markdown
# Transcription Complète

**Source:** guerre_des_ombres_ocr.txt (6314 lignes)
**Destination:** chapitres/01_une_vie_de_voleur.tex
**Univers:** Archipels
**Template:** archipels/.templates/

## Statistiques

- Chapitres traités: 1
- Lignes source: 412
- Paragraphes: 28
- Citations: 3
- Encadrés: 1
- Corrections OCR: 12

## Corrections OCR Appliquées

- Ligne 45: "frax" → "Arax" (nom propre récurrent)
- Ligne 67: "Velêne" → "Velène" (accent erroné)
- Ligne 102: "roleur" → "voleur" (erreur scan)
- [...]

## Doutes / Révision Manuelle

- Ligne 234: "fraxx" → Laissé tel quel (possiblement nom propre inconnu, vérifier source papier)

## Fichier Généré

📄 `chapitres/01_une_vie_de_voleur.tex` (892 lignes LaTeX)

**Prêt pour compilation:** ✅
**Validé avec latex-validator:** ✅

## Prochaines Étapes

1. Réviser corrections OCR (vérifier contexte ligne 234)
2. Compiler avec `pdflatex main.tex`
3. Comparer PDF vs source original (relecture)
4. Transcription chapitre suivant
```

## Notes Importantes

### Limite de la Transcription Automatique

**Claude Code peut:**
- Convertir structure (titres, paragraphes, listes)
- Appliquer template LaTeX
- Corriger erreurs OCR évidentes
- Échapper caractères spéciaux

**Claude Code NE PEUT PAS:**
- Deviner corrections ambiguës (nécessite contexte étendu ou source papier)
- Garantir 100% fidélité sans relecture humaine
- Interpréter mise en page complexe (tableaux multi-pages, etc.)

**Révision manuelle TOUJOURS nécessaire.**

### Documentation Corrections

**TOUJOURS documenter corrections non triviales:**

```latex
% OCR: "le Frax" → Corrigé: "le Prax" (vérifier: nom lieu ou personnage ?)
% OCR: "300 livres" → Laissé tel quel (possible, vérifier source papier si doute)
```

**Facilite:**
- Révision ultérieure
- Traçabilité modifications
- Détection erreurs de correction

### Projet README.md

**Indiquer dans README.md du projet:**

```markdown
## Source

- **Document original:** Supplément "Guerre des Ombres" (édition papier 1995)
- **Emplacement:** sources/guerre_des_ombres_ocr.txt
- **Format:** OCR via Tesseract
- **Qualité:** Moyenne (nombreuses corrections nécessaires)

## Transcription

- **Méthode:** Skill `latex-transcriber` + révision manuelle
- **Corrections OCR:** Documentées en commentaires LaTeX
- **Fidélité:** Maximale au texte original
```

---

**Version:** 1.0
**Dernière mise à jour:** 2025-11-08
**Type:** Output-style universel (transcription pure)
