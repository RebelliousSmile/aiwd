# Workflow: Transcription Source → LaTeX

## Vue d'Ensemble

Ce workflow couvre la transformation de textes sources (OCR, markdown, TXT brut) en documents LaTeX structurés, en respectant les templates d'univers.

**Types de sources:**
- Fichiers OCR (scans de livres/manuels papier)
- Fichiers markdown
- Fichiers TXT bruts
- Documents Word convertis

**Objectif:** Fidélité maximale au texte source + mise en forme LaTeX professionnelle

---

## Étapes du Workflow

### 1. Préparation Source

**1.1. Placer le fichier source**

```
<univers>/<nom-projet>/sources/<fichier-source>.txt
```

**Format:**
```
<univers>/<nom-projet>/sources/<fichier-source>.txt
```

**Exemples:**
```
archipels/guerre_des_ombres/sources/chapitre1_ocr.txt
roue-du-temps/nouvelles_arad_doman/sources/nouvelle1.txt
```

**1.2. Vérifier qualité OCR (si applicable)**

**Erreurs OCR courantes à corriger manuellement AVANT transcription:**

| Erreur OCR | Correction |
|------------|------------|
| `rn` → `m` | `rnarin` → `marin` |
| `1` → `l` | `1es` → `les` |
| `0` → `o` | `n0m` → `nom` |
| Espaces manquants | `delamer` → `de la mer` |
| Tirets parasites | `na-vire` → `navire` |

**Outil recommandé:** Éditeur de texte avec recherche/remplacement (VS Code, Notepad++)

**1.3. Identifier l'univers cible**

Si le projet existe déjà:
```
Lire <univers>/<projet>/README.md
```

Si nouveau projet:
```
"Crée un nouveau projet '<nom>' dans l'univers <univers> (type: transcription)"
→ Invoque agent project-manager
```

---

### 2. Analyse Structure Source

**2.1. Identifier structure du texte**

**Éléments à détecter:**
- Titres de chapitres (`# Titre` en markdown, ou MAJUSCULES en TXT)
- Sous-titres (`## Sous-titre` ou lignes soulignées)
- Paragraphes normaux
- Listes à puces ou numérotées
- Tableaux
- Citations
- Encadrés/Apartés (souvent entre `---` ou `***`)
- Notes de bas de page

**2.2. Créer plan de conversion**

**Exemple de mapping:**

| Élément Source | Élément LaTeX | Exemple |
|----------------|---------------|---------|
| `# Chapitre 1` | `\section{Chapitre 1}` | `\section{Houlemorte}` |
| `## Section A` | `\subsection{Section A}` | `\subsection{Le Grand Port}` |
| `- item` | `\begin{itemize}\item` | Liste à puces |
| `1. item` | `\begin{enumerate}\item` | Liste numérotée |
| `> citation` | `\citationarchipels{...}{...}` | Citation avec auteur |
| `---` (encadré) | `\begin{statsbox}...\end{statsbox}` | Encadré structuré |
| `**gras**` | `\textbf{gras}` | Mise en forme |
| `*italique*` | `\textit{italique}` | Mise en forme |

---

### 3. Transcription Automatisée

**3.1. Invoquer skill latex-transcriber**

**Commande utilisateur:**
```
"Transcris le fichier sources/chapitre1.txt en LaTeX Archipels"
"Convertis sources/regles.md en LaTeX WoT avec terminologie stricte"
```

**La skill effectue automatiquement:**

1. **Détection univers:**
   - Lecture `README.md` du projet
   - Ou analyse termes typiques dans le texte

2. **Chargement documentation univers:**
   ```
   @<univers>/.docs/UNIVERS.md
   @<univers>/.docs/terminologie.md
   @docs/workflows/transcription-workflow.md (ce fichier)
   ```

3. **Conversion structure:**
   - Titres → `\section{}`, `\subsection{}`
   - Paragraphes → Texte direct (double saut de ligne entre paragraphes)
   - Listes → `itemize` ou `enumerate`
   - Tableaux → `tabular`

4. **Application terminologie:**
   - Détection termes spécifiques univers
   - Application formatage (italiques, petites caps, etc.)
   - **Exemple Archipels:** `Houlemorte` → `\textsc{Houlemorte}` (première mention)
   - **Exemple WoT:** `saidin` → `\textit{saidin}` (toujours)

5. **Correction erreurs OCR:**
   - Patterns d'erreurs connus
   - Validation orthographique française
   - Espaces manquants/excédentaires

6. **Génération fichier .tex:**
   ```
   <univers>/<projet>/chapitres/<nom-chapitre>.tex
   ```

**3.2. Résultat attendu**

**Fichier .tex structuré:**
```latex
% chapitres/chapitre1.tex
% Transcription de sources/chapitre1_ocr.txt

\section{Houlemorte}

\textsc{Houlemorte} s'étend sur trois îles reliées par d'imposants ponts
de pierre. La cité portuaire principale des Îles Connues ne dort jamais.

\subsection{Le Grand Port}

Le Grand Port grouille d'activité. Les navires affluent de toutes les
Îles Connues, chargés de marchandises exotiques.

\begin{itemize}
  \item Épices des îles du sud
  \item Soieries de Vendrest
  \item Armes forgées dans les montagnes du nord
\end{itemize}

\citationarchipels{Le Grand Port ne dort jamais.}{Proverbe marin}
```

---

### 4. Validation et Révision

**4.1. Validation syntaxe LaTeX**

**Commande:**
```
"Valide le fichier chapitres/chapitre1.tex"
```

**Skill latex-validator vérifie:**
- Syntaxe LaTeX correcte (balises fermées, commandes valides)
- Respect template univers (commandes personnalisées valides)
- Encodage UTF-8
- Chemins relatifs corrects

**4.2. Vérification fidélité source**

**Checklist manuelle:**
- [ ] Tous les titres sont présents
- [ ] Contenu textuel complet (aucune omission)
- [ ] Structure respectée (ordre des sections)
- [ ] Tableaux corrects (colonnes, lignes)
- [ ] Listes complètes
- [ ] Citations préservées

**Outil:** Comparer visuellement source TXT et PDF compilé côte à côte

**4.3. Vérification terminologie**

**Checklist par univers:**

**Archipels:**
- [ ] Lieux en petites caps à première mention
- [ ] Organisations en italique
- [ ] Citations avec commande `\citationarchipels{}{}`
- [ ] Vocabulaire nautique correct

**La Roue du Temps:**
- [ ] Termes Vieille Langue en italique (`\textit{saidin}`)
- [ ] Majuscules respectées (La Roue, Le Dessin, Le Pouvoir)
- [ ] "Aes Sedai" invariable
- [ ] Citations avec commande `\citationwot{}{}`

**Ecryme:**
- [ ] Termes techniques avec `\terme{}`
- [ ] Atmosphère sombre préservée
- [ ] Citations avec `\citationecryme{}`

**4.4. Révision qualité**

**Points d'attention:**
- Erreurs OCR résiduelles (noms propres mal reconnus)
- Espaces multiples ou manquants
- Coupures de mots incorrectes
- Symboles spéciaux (æ, œ, accents) bien encodés

---

### 5. Compilation Test

**5.1. Compiler le document**

**Commandes:**
```bash
cd <univers>/<projet>/
pdflatex main.tex
pdflatex main.tex  # Deux fois pour références
```

**5.2. Vérifier PDF généré**

**Checklist visuelle:**
- [ ] Mise en page correcte
- [ ] Couleurs template appliquées
- [ ] Polices correctes
- [ ] Encadrés bien formés
- [ ] Pas de débordements (overfull hbox)
- [ ] Numérotation pages cohérente
- [ ] Table des matières (si applicable)

**5.3. Corriger erreurs de compilation**

**Erreurs courantes:**

| Erreur | Cause | Solution |
|--------|-------|----------|
| `Undefined control sequence` | Commande inexistante | Vérifier nom commande template |
| `! LaTeX Error: Environment ... undefined` | Environnement non chargé | Vérifier `\usepackage{}` |
| `Overfull \hbox` | Texte dépassant marge | Reformuler phrase ou `\sloppy` |
| `Missing $ inserted` | Caractère spécial non échappé | Échapper `\$`, `\%`, `\&`, etc. |
| Accents incorrects | Encodage | Vérifier UTF-8 + `\usepackage[utf8]{inputenc}` |

---

### 6. Intégration et Finalisation

**6.1. Inclure chapitre dans main.tex**

**Éditer `main.tex`:**
```latex
\documentclass[a5paper,11pt]{book}

\usepackage{archipels}  % Template univers

\begin{document}

\tableofcontents

\include{chapitres/chapitre1}  % Nouveau chapitre transcrit
\include{chapitres/chapitre2}

\end{document}
```

**6.2. Recompiler document complet**

```bash
pdflatex main.tex
pdflatex main.tex
```

**6.3. Commit Git**

**Commandes:**
```bash
cd <univers>/<projet>/
git add sources/chapitre1_ocr.txt
git add chapitres/chapitre1.tex
git commit -m "Transcription chapitre 1: Houlemorte

- Source: chapitre1_ocr.txt
- Corrections OCR appliquées
- Terminologie Archipels respectée
- Compilation validée

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
git push
```

---

## Cas Particuliers

### Transcription Tableau Complexe

**Source TXT:**
```
Nom       | Rôle         | Compétences
----------|--------------|-------------
Vérane    | Capitaine    | Navigation, Combat
Elric     | Cartographe  | Cartes, Lore
```

**Conversion LaTeX:**
```latex
\begin{center}
\begin{tabular}{|l|l|l|}
\hline
\textbf{Nom} & \textbf{Rôle} & \textbf{Compétences} \\
\hline
Vérane & Capitaine & Navigation, Combat \\
Elric & Cartographe & Cartes, Lore \\
\hline
\end{tabular}
\end{center}
```

### Transcription Encadré Stats

**Source TXT:**
```
---
MAÎTRE ELRIC
Rôle: Cartographe
Compétences: Cartographie experte
Motivation: Cartographier les Îles Connues
---
```

**Conversion LaTeX:**
```latex
\begin{statsbox}{Maître Elric}
  \textbf{Rôle:} Cartographe\\
  \textbf{Compétences:} Cartographie experte\\
  \textbf{Motivation:} Cartographier les Îles Connues
\end{statsbox}
```

### Transcription Note de Bas de Page

**Source markdown:**
```
Le Dragon Réincarné[^1] est prophétisé dans le Karaethon.

[^1]: Rand al'Thor, personnage principal
```

**Conversion LaTeX:**
```latex
Le Dragon Réincarné\footnote{Rand al'Thor, personnage principal}
est prophétisé dans le Karaethon.
```

---

## Optimisations et Bonnes Pratiques

### Organisation Fichiers

**Structure recommandée:**
```
<univers>/<projet>/
├── main.tex                    # Document principal
├── README.md                   # Type projet, statut
├── sources/                    # Sources brutes (OCR, TXT)
│   ├── chapitre1_ocr.txt
│   ├── chapitre2_ocr.txt
│   └── corrections.md          # Notes corrections OCR
├── chapitres/                  # Fichiers .tex transcrits
│   ├── chapitre1.tex
│   └── chapitre2.tex
├── images/                     # Images (si applicable)
└── compiled/                   # PDFs compilés
    └── projet.pdf
```

### Workflow Itératif

**Pour gros documents (> 50 pages):**

1. Transcrire par **petits blocs** (5-10 pages)
2. Valider chaque bloc avant de continuer
3. Commit fréquents (un par chapitre/section)
4. Compilation test régulière (détecter erreurs tôt)

### Automatisation Partielle

**Pour projets répétitifs:**

**Créer script de pré-traitement OCR:**
```bash
# correct_ocr.sh
sed -i 's/rn/m/g' $1
sed -i 's/1es/les/g' $1
sed -i 's/0/o/g' $1
# etc.
```

**Utiliser:**
```bash
./correct_ocr.sh sources/chapitre1_ocr.txt
```

**Puis transcription LaTeX:**
```
"Transcris sources/chapitre1_ocr.txt"
```

---

## Méthodologie de Transcription Parallèle

### Vue d'Ensemble

Pour les **fichiers volumineux avec OCR très corrompu** (> 100KB, lignes de milliers de caractères), une approche de traitement parallèle permet de réduire drastiquement le temps de nettoyage.

**Principe:** Diviser automatiquement le fichier en sections, puis lancer plusieurs agents Claude Code en parallèle, chacun nettoyant une section indépendamment.

**Cas d'usage idéal:**
- Fichiers OCR massifs (> 100KB)
- Structure claire avec marqueurs de chapitres identifiables
- Sections relativement indépendantes (peu de références croisées)
- Besoins de résultats rapides (4 chapitres en 10 minutes au lieu de 40 minutes séquentiellement)

**Avantages:**
- Gain de temps proportionnel au nombre d'agents (4x plus rapide avec 4 agents)
- Isolation des erreurs (un problème sur une section n'affecte pas les autres)
- Progression visible en temps réel (rapports parallèles)

**Inconvénients:**
- Nécessite une structure claire avec marqueurs de sections
- Requiert un script Python de division initial
- Consomme plus de tokens simultanément (mais durée totale réduite)

---

### Workflow Complet: Split-Then-Parallel-Clean

#### Phase 1: Analyse et Division du Fichier Source

**Étape 1.1: Identifier les marqueurs de sections**

Ouvrir le fichier source et identifier les marqueurs structurels:

```bash
# Exemple de commande pour repérer les titres
grep -E "^[A-Z][A-Za-z ]{5,50}$" chapitres/scenario.tex | head -20
```

**Marqueurs typiques:**
- Titres de chapitres en MAJUSCULES
- Lignes avec motifs répétitifs (`===`, `---`, `***`)
- Numérotation explicite (`Chapitre 1`, `Section II`)
- Mots-clés récurrents (`Introduction`, `Conclusion`)

**Étape 1.2: Créer le script de division**

Créer un script Python dans `.claude/scripts/<projet>/split_scenario.py`:

```python
#!/usr/bin/env python3
"""
Script pour diviser scenario.tex en fichiers par chapitre
Basé sur les marqueurs de sections identifiés dans le contenu
"""

import re
import os

# Définir les chapitres et leurs délimitations
CHAPTERS = [
    {
        "name": "01_introduction",
        "title": "Introduction",
        "start_marker": "^Introduction",  # Regex pattern
        "end_marker": "^Chapitre 1"
    },
    {
        "name": "02_chapitre_1",
        "title": "Chapitre 1",
        "start_marker": "^Chapitre 1",
        "end_marker": "^Chapitre 2"
    },
    # Ajouter autres chapitres...
    {
        "name": "07_conclusion",
        "title": "Conclusion",
        "start_marker": "^Conclusion",
        "end_marker": None  # Jusqu'à la fin du fichier
    }
]

def main():
    input_file = "chapitres/scenario.tex"
    output_dir = "chapitres/scenario_split"

    # Créer le répertoire de sortie
    os.makedirs(output_dir, exist_ok=True)

    # Lire le fichier complet
    with open(input_file, 'r', encoding='utf-8') as f:
        content = f.read()

    print(f"Fichier lu: {len(content)} caractères")
    print(f"Nombre de lignes: {len(content.splitlines())}")

    # Pour chaque chapitre, extraire le contenu
    for i, chapter in enumerate(CHAPTERS):
        print(f"\nTraitement chapitre: {chapter['name']}")

        # Trouver le début
        start_pattern = chapter['start_marker']
        start_match = re.search(start_pattern, content, re.MULTILINE | re.IGNORECASE)

        if not start_match:
            print(f"  [WARN] Marqueur de debut non trouve: {start_pattern}")
            continue

        start_pos = start_match.start()

        # Trouver la fin
        if chapter['end_marker'] is None:
            end_pos = len(content)
        else:
            end_pattern = chapter['end_marker']
            end_match = re.search(end_pattern, content[start_pos+100:], re.MULTILINE | re.IGNORECASE)
            if end_match:
                end_pos = start_pos + 100 + end_match.start()
            else:
                print(f"  [WARN] Marqueur de fin non trouve: {end_pattern}")
                end_pos = len(content)

        # Extraire le contenu
        chapter_content = content[start_pos:end_pos]

        # Créer l'en-tête LaTeX
        header = f"""% {chapter['title'].upper()}
% Extrait automatiquement de scenario.tex
% Script: split_scenario.py

"""

        # Écrire le fichier
        output_file = os.path.join(output_dir, f"{chapter['name']}.tex")
        with open(output_file, 'w', encoding='utf-8') as f:
            f.write(header + chapter_content)

        print(f"  [OK] Créé: {output_file} ({len(chapter_content)} caractères, {len(chapter_content.splitlines())} lignes)")

    print(f"\n[OK] Division terminée! Fichiers dans: {output_dir}")

if __name__ == "__main__":
    main()
```

**Étape 1.3: Exécuter la division**

```bash
cd <univers>/<projet>
python .claude/scripts/<projet>/split_scenario.py
```

**Résultat attendu:**
```
chapitres/scenario_split/
├── 01_introduction.tex        (5 KB)
├── 02_chapitre_1.tex          (12 KB)
├── 03_chapitre_2.tex          (18 KB)
├── 04_chapitre_3.tex          (15 KB)
├── 05_chapitre_4.tex          (20 KB)
├── 06_chapitre_5.tex          (14 KB)
└── 07_conclusion.tex          (3 KB)
```

---

#### Phase 2: Transcription Parallèle avec Task Agents

**Étape 2.1: Préparer la liste des fichiers à nettoyer**

Identifier les fichiers nécessitant nettoyage (ignorer les trop petits ou déjà propres):

```bash
# Lister avec tailles
ls -lh chapitres/scenario_split/*.tex
```

**Critères de sélection:**
- Fichiers > 5 KB (suffisamment volumineux pour bénéficier du parallélisme)
- Fichiers avec OCR corrompu (vérifier un échantillon)
- Exclure fichiers déjà nettoyés (si reprise après interruption)

**Étape 2.2: Lancer les agents parallèles**

**CRUCIAL: Tous les agents doivent être lancés dans UN SEUL message utilisateur.**

**Exemple de commande utilisateur (4 fichiers en parallèle):**

```
Lance 4 agents de transcription en parallèle:

Agent 1: Nettoie chapitres/scenario_split/02_chapitre_1.tex avec latex-transcriber et sauvegarde dans chapitres/02_chapitre_1_clean.tex

Agent 2: Nettoie chapitres/scenario_split/03_chapitre_2.tex avec latex-transcriber et sauvegarde dans chapitres/03_chapitre_2_clean.tex

Agent 3: Nettoie chapitres/scenario_split/04_chapitre_3.tex avec latex-transcriber et sauvegarde dans chapitres/04_chapitre_3_clean.tex

Agent 4: Nettoie chapitres/scenario_split/05_chapitre_4.tex avec latex-transcriber et sauvegarde dans chapitres/05_chapitre_4_clean.tex

Utilise la skill latex-transcriber pour corriger OCR, structurer LaTeX, et appliquer terminologie Archipels.
```

**Pattern technique (côté Claude Code):**

Claude Code invoquera 4 agents Task en parallèle dans une seule réponse. Chaque agent travaillera indépendamment sur son fichier.

**Étape 2.3: Suivre la progression**

Les agents retournent leurs rapports de manière asynchrone. Vous verrez les résultats au fur et à mesure:

```
[Agent 1] Nettoyage 02_chapitre_1.tex complété:
- 1250 lignes traitées
- 42 erreurs OCR corrigées
- 15 environnements LaTeX créés
- Sauvegardé: chapitres/02_chapitre_1_clean.tex

[Agent 3] Nettoyage 04_chapitre_3.tex complété:
- 980 lignes traitées
- 38 erreurs OCR corrigées
- 12 environnements LaTeX créés
- Sauvegardé: chapitres/04_chapitre_3_clean.tex

[Agent 2] Nettoyage 03_chapitre_2.tex complété:
...

[Agent 4] Nettoyage 05_chapitre_4.tex complété:
...
```

**Temps typiques:**
- Sequential (4 fichiers): ~40 minutes (10 min/fichier)
- Parallel (4 agents): ~10-12 minutes (tous simultanément)
- **Gain: 70-75% de temps économisé**

---

#### Phase 3: Intégration et Validation

**Étape 3.1: Vérifier les fichiers nettoyés**

```bash
# Lister les fichiers clean générés
ls -lh chapitres/*_clean.tex

# Vérifier qu'aucun n'est vide
find chapitres/ -name "*_clean.tex" -size 0
```

**Étape 3.2: Intégrer dans main.tex**

Éditer `main.tex` pour inclure les chapitres nettoyés:

```latex
\documentclass[10pt,a4paper,twoside]{article}
\usepackage{../.templates/archipels}

\begin{document}

\tableofcontents
\newpage

% Chapitres nettoyés en parallèle
\input{chapitres/01_introduction_clean}
\input{chapitres/02_chapitre_1_clean}
\input{chapitres/03_chapitre_2_clean}
\input{chapitres/04_chapitre_3_clean}
\input{chapitres/05_chapitre_4_clean}
\input{chapitres/06_chapitre_5_clean}
\input{chapitres/07_conclusion_clean}

\end{document}
```

**Étape 3.3: Compilation test**

```bash
pdflatex main.tex
pdflatex main.tex
```

**Si erreurs de compilation:**

1. **Identifier le fichier problématique:**
   - Lire les logs de compilation pour trouver le fichier .tex concerné
   - Commenter les `\input{}` un par un pour isoler

2. **Valider le fichier individuellement:**
   ```
   "Valide le fichier chapitres/03_chapitre_2_clean.tex"
   ```

3. **Corriger et re-tester:**
   - Corriger l'erreur dans le fichier problématique
   - Recompiler `main.tex`

**Étape 3.4: Commit des résultats**

```bash
git add chapitres/*_clean.tex
git add main.tex
git commit -m "Transcription parallèle scenario.tex complétée

- 7 chapitres nettoyés en parallèle (4 agents)
- ~850 lignes LaTeX structurées au total
- Erreurs OCR corrigées
- Terminologie Archipels appliquée
- Compilation testée: OK (20 pages PDF)

Méthode: split-then-parallel-clean workflow
Gain de temps: ~70% vs séquentiel

🤖 Generated with Claude Code (latex-transcriber skill)
Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Bonnes Pratiques Transcription Parallèle

#### Quand Utiliser le Parallélisme

**OUI - Parallèle recommandé:**
- Fichier source > 100 KB
- Plus de 3 sections indépendantes
- Structure claire avec marqueurs
- OCR fortement corrompu (ratio signal/bruit < 50%)
- Urgence temporelle (deadline courte)

**NON - Séquentiel préférable:**
- Fichier source < 50 KB (overhead de split non rentable)
- Sections très interdépendantes (références croisées fréquentes)
- Structure floue ou ambiguë
- Première transcription d'un nouveau type de document (apprentissage nécessaire)
- Pas de contrainte de temps

#### Nombre Optimal d'Agents

**Recommandations:**

| Taille Fichier | Nombre de Sections | Agents Recommandés | Temps Estimé |
|----------------|--------------------|--------------------|--------------|
| 50-100 KB | 3-4 sections | 2-3 agents | 15-20 min |
| 100-300 KB | 5-8 sections | 4-5 agents | 10-15 min |
| 300-500 KB | 9-15 sections | 6-8 agents | 12-18 min |
| > 500 KB | > 15 sections | 8-10 agents (max) | 15-25 min |

**Limites pratiques:**
- **Maximum recommandé:** 10 agents en parallèle (au-delà, diminution des gains)
- **Token budget:** Chaque agent consomme ~5-10k tokens
- **Durée:** Tous les agents se terminent généralement dans un délai similaire

#### Gestion des Erreurs

**Scénario 1: Un agent échoue**

Si un des 4 agents échoue, les 3 autres continuent. Relancer uniquement l'agent échoué:

```
Relance le nettoyage de chapitres/scenario_split/03_chapitre_2.tex uniquement
(les autres fichiers sont déjà traités)
```

**Scénario 2: Erreur de compilation après intégration**

1. Identifier le fichier problématique via les logs
2. Valider individuellement: `"Valide chapitres/03_chapitre_2_clean.tex"`
3. Corriger l'erreur spécifique
4. Recompiler

**Scénario 3: Division incorrecte (sections tronquées)**

1. Vérifier le script `split_scenario.py`
2. Ajuster les regex patterns `start_marker` et `end_marker`
3. Relancer la division
4. Relancer le nettoyage parallèle sur les nouveaux fichiers

---

### Cas d'Usage Réel: Projet "Eveil des Ombres"

**Contexte:**
- Fichier: `scenario.tex` (169 KB, 134 lignes avec corruption OCR massive)
- Structure: 7 sections identifiables
- Objectif: Nettoyer et compiler en PDF

**Étapes réalisées:**

**1. Division (1 minute):**
```bash
python .claude/scripts/<projet>/split_scenario.py
```
Résultat: 7 fichiers créés (5-138 KB chacun)

**2. Sélection fichiers (manuel):**
- Sélectionné 4 fichiers moyens (10-30 KB) pour premier batch
- Conservé 2 gros fichiers (138 KB, 105 KB) pour subdivision ultérieure
- Ignoré 1 petit fichier (< 5 KB, propre)

**3. Nettoyage parallèle (10 minutes):**

Commande:
```
Lance 4 agents de transcription en parallèle:

Agent 1: Nettoie chapitres/scenario_split/01_introduction_tribulations.tex → chapitres/01_introduction_tribulations_clean.tex
Agent 2: Nettoie chapitres/scenario_split/02_quartier_nefs.tex → chapitres/02_quartier_nefs_clean.tex
Agent 3: Nettoie chapitres/scenario_split/05_attentat.tex → chapitres/05_attentat_clean.tex
Agent 4: Nettoie chapitres/scenario_split/07_suite_aventure.tex → chapitres/07_suite_aventure_clean.tex
```

Résultat:
- Agent 1: 149 lignes nettoyées
- Agent 2: 220 lignes nettoyées (correction erreur `statsbox` incluse)
- Agent 3: 128 lignes nettoyées
- Agent 4: 281 lignes nettoyées

**4. Intégration (2 minutes):**
- Édité `main.tex` pour inclure les 4 chapitres
- Compilation: `pdflatex main.tex` (2 passes)
- Résultat: **PDF de 20 pages généré avec succès**

**5. Temps total: ~13 minutes**
- vs ~40 minutes en séquentiel (gain: 67%)

**Actions futures:**
- Subdiviser `03_gros_palais.tex` (138 KB) en 3-4 sous-sections
- Subdiviser `06_repaire_fraternite.tex` (105 KB) en 2-3 sous-sections
- Lancer second batch de nettoyage parallèle (4-6 agents)

---

### Checklist Transcription Parallèle

**Phase 1 - Préparation:**
- [ ] Fichier source > 100 KB (sinon, utiliser workflow séquentiel)
- [ ] Marqueurs de sections identifiés (titres, patterns, numéros)
- [ ] Script `.claude/scripts/<projet>/split_scenario.py` créé et testé
- [ ] Division exécutée: fichiers générés dans `scenario_split/`
- [ ] Fichiers cibles sélectionnés (ignorer petits/déjà propres)

**Phase 2 - Nettoyage Parallèle:**
- [ ] Commande utilisateur préparée (tous agents dans UN message)
- [ ] Nombre d'agents optimal (4-8 recommandé)
- [ ] Agents lancés simultanément
- [ ] Rapports de progression surveillés
- [ ] Fichiers `*_clean.tex` générés et vérifiés (non vides)

**Phase 3 - Intégration:**
- [ ] Fichiers clean intégrés dans `main.tex`
- [ ] Compilation test réussie (2 passes pdflatex)
- [ ] PDF généré et visuellement correct
- [ ] Erreurs de compilation corrigées (si présentes)
- [ ] Commit Git avec message descriptif

**Phase 4 - Itération (si nécessaire):**
- [ ] Gros fichiers restants identifiés (> 50 KB)
- [ ] Subdivision additionnelle planifiée
- [ ] Second batch de nettoyage lancé
- [ ] Tous fichiers finalement nettoyés et intégrés

---

## Checklist Complète Transcription

**Avant de commencer:**
- [ ] Source placée dans `sources/`
- [ ] Univers cible identifié
- [ ] Projet créé (si nouveau)
- [ ] Documentation tenant chargée

**Pendant transcription:**
- [ ] Structure détectée correctement
- [ ] Terminologie appliquée
- [ ] Commandes template utilisées
- [ ] Erreurs OCR corrigées

**Après transcription:**
- [ ] Validation syntaxe LaTeX (latex-validator)
- [ ] Vérification fidélité source
- [ ] Compilation test réussie
- [ ] PDF visuellement correct
- [ ] Chapitre inclus dans main.tex
- [ ] Commit Git effectué

**Qualité finale:**
- [ ] Aucune erreur de compilation
- [ ] Mise en page professionnelle
- [ ] Terminologie respectée à 100%
- [ ] Lisibilité optimale
- [ ] Fidélité au texte source

---

## Support et Résolution Problèmes

### Problème: Erreurs OCR Trop Nombreuses

**Solution:**
- Utiliser outil OCR plus performant (Tesseract, ABBYY FineReader)
- Pré-traiter image source (contraste, résolution)
- Correction manuelle assistée par dictionnaire

### Problème: Terminologie Inconnue

**Solution:**
```
"Ajoute le terme '<nouveau-terme>' à la terminologie <univers>"
→ Met à jour <univers>/.docs/terminologie.md
```

### Problème: Compilation Échoue

**Solution:**
```
"Valide le fichier chapitres/<nom>.tex"
→ Skill latex-validator identifie erreurs précises
```

### Problème: Template Manque Commande

**Solution:**
```
"Ajoute la commande \<nom> au template <univers>"
→ Agent latex-template-manager modifie template
```

---

**Version:** 2.0
**Dernière mise à jour:** 2025-11-08
**Maintenu par:** Claude Code + Utilisateur

**Changelog v2.0:**
- Ajout section complète "Méthodologie de Transcription Parallèle"
- Workflow Split-Then-Parallel-Clean documenté (3 phases)
- Script Python de division automatique inclus
- Bonnes pratiques: quand utiliser parallèle vs séquentiel
- Tableau nombre optimal d'agents par taille de fichier
- Gestion des erreurs (3 scénarios courants)
- Cas d'usage réel: projet "Eveil des Ombres" (gain 67% temps)
- Checklist spécifique transcription parallèle (4 phases)
