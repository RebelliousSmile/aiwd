---
name: write-technical
description: Write technical documentation section in Markdown using output-style, docs and toc
argument-hint: <section-number-or-range> [--feedback <personas-comment-file>]
version: 1.1
changelog:
  - version: 1.1
    date: 2026-02-28
    changes:
      - "Charge output-style + personas depuis toc-chapter header (Option A)"
      - "Lecture personas.yml pour adapter ton selon profils cibles"
      - "Confirmation explicite ressources chargées (Step 1)"
  - version: 1.0
    date: 2026-02-28
    changes:
      - "Adaptation de write-novel pour documentation technique"
      - "Style objectif, précis, structuré"
      - "Support diagrammes, tableaux, code blocks"
---

# Write Technical Documentation Section

## Goal

Write a technical documentation section in **Markdown** following the TOC specifications, using the project's output-style and client documentation. The section will be converted to ICML later via `build-icml.py`.

## Context

### Required Resources (MUST load all)

**Table of Contents (chapter specifications - CHARGER EN PREMIER):**
```markdown
@.toc/toc-chapter<XX>.md
```
*Extraire depuis l'en-tête:*
- `output-style:` → Nom du fichier output-style à charger
- `persona review:` → Liste des personas pour cette section

**Bank Configuration:**
```yaml
@bank.yml
```

**Output Style (writing conventions):**
```markdown
@<client>/.output-styles/<output-style>.md  # Utiliser le nom depuis toc-chapter
```
```markdown
@.claude/output-style.md  # project override if exists
```

**Client Documentation:**
```markdown
@<client>/.docs/CLIENT.md
@<client>/.docs/glossaire.md
@<client>/.docs/brand-guidelines.md
```

**Persona Specifications (référence uniquement):**
```yaml
@<client>/.templates/personas/<persona>.yml  # Pour chaque persona listée dans toc-chapter
```
*Note:* Les personas sont utilisées au moment du `comment` — ici on les charge pour comprendre le profil des lecteurs cibles.

**Persona Feedback (mode --feedback uniquement):**
```markdown
@.wip/comments/chapitre<XX>-personas.md  # si --feedback spécifié
```

### Chapter Target

```text
$ARGUMENTS
```

## Rules

- **Langue française:** Toujours utiliser les accents et caractères spéciaux (é, è, ê, ë, à, â, ù, û, ô, î, ï, ç, œ, æ, «», —, etc.)
- **Output format:** Markdown (`.md`) — conversion ICML séparée
- Follow output-style conventions STRICTLY
- Use glossaire.md for proper technical terms
- Match TOC synopsis and key points exactly
- Save chapter to `chapitres/chapitre<XX>.md`
- One chapter per invocation unless range specified
- DO NOT write what is not in the TOC
- Style objectif, clair, structuré — pas de narration
- **Mode --feedback:** Réécriture TOTALE depuis la TOC. Ne JAMAIS lire ni patcher le chapitre existant. Le feedback personas sert de contraintes supplémentaires, pas de diff à appliquer. Le texte existant est ignoré — seule la TOC fait foi.

## Markdown Syntax for Technical Content

### Headings
```markdown
# Titre Principal (H1)
## Sous-section (H2)
### Sous-sous-section (H3)
```

### Lists
```markdown
- Liste à puces
- Élément 2

1. Liste numérotée
2. Étape suivante
```

### Tables
```markdown
| Paramètre | Type | Description |
|-----------|------|-------------|
| `param1` | String | Description du paramètre |
| `param2` | Integer | Valeur numérique |
```

### Code Blocks
```markdown
\`\`\`python
def exemple():
    return "Code Python"
\`\`\`

\`\`\`json
{
  "clé": "valeur"
}
\`\`\`
```

### Diagrams (Mermaid)
```markdown
\`\`\`mermaid
graph TD
    A[Début] --> B[Étape 1]
    B --> C[Fin]
\`\`\`
```

### Important/Warning Blocks
```markdown
> **Important**
> Contenu important à mettre en avant.

> **Attention**
> Avertissement ou précaution.
```

### Links and References
```markdown
[Voir la documentation](url)
[Section connexe](#titre-ancre)
```

## Steps

### Step 1: Load Context

1. **Load TOC chapter FIRST:** `@.toc/toc-chapter<XX>.md`
   - Extract output-style name from header: `**Output-style:** user-friendly.md`
   - Extract personas list from header: `**Persona review:** platform-admin, showroom-staff`
2. Parse bank.yml for project configuration
3. **Load output-style specified in TOC header:** `@<client>/.output-styles/<output-style>.md`
4. Load client documentation (CLIENT.md, glossaire.md)
5. **Load persona specs (for reader profile):** `@<client>/.templates/personas/<persona>.yml` for each persona
6. **IF `--feedback`:** Load persona feedback file. **Ne PAS lire la section existante.**
7. Verify all resources loaded successfully

**Confirmation Message:**
```
✓ TOC loaded: toc-chapter<XX>.md
✓ Output-style: <output-style>.md
✓ Personas target: [list]
✓ Client docs loaded
✓ Ready to write
```

### Step 2: Analyze Chapter Requirements

From TOC, extract:

| Element | Value |
|---------|-------|
| Title | [from toc] |
| Synopsis | [from toc] |
| Key Points | [checklist from toc] |
| Audience | [technical/end-user/mixed] |
| Complexity | [beginner/intermediate/advanced] |
| Format | [tutorial/reference/conceptual] |
| Length | [from toc] |
| Output-style | [from toc header] |
| Personas target | [from toc header] |

**Technical Elements:**

| Element | Value |
|---------|-------|
| Diagrams | [list from toc] |
| Code Examples | [required languages] |
| Tables | [data to present] |
| Screenshots | [suggested locations] |

**Persona Profiles (pour adapter le ton):**

Pour chaque persona listée, résumer le profil depuis `.yml`:
- **Patience level:** [level]
- **Must-haves:** [liste des éléments obligatoires]
- **Deal-breakers:** [liste des éléments rédhibitoires]

*Utilisation:* Ces profils informent le style d'écriture. Les personas évalueront le texte plus tard via `comment.prompt.md`.

### Step 2.5: Extract Feedback Constraints (mode --feedback uniquement)

**SKIP si pas de --feedback.**

Parser le fichier `.wip/comments/chapitre<XX>-personas.md` et extraire des **contraintes d'écriture** (pas des corrections) :

**2.5.1 Must-haves manquants → Contraintes structurelles**

Pour chaque persona plafonnée (score ≤ 11/20), extraire les must-haves non satisfaits et les convertir en directives d'écriture :

```
Persona: technical-reviewer (11/20, plafond must-have)
Must-have manquant: "Exemples concrets pour chaque concept"
→ CONTRAINTE: Chaque concept technique DOIT inclure au moins un exemple
   réel avec code ou capture d'écran annotée.
```

**2.5.2 Patterns systémiques → Directives de ton**

Extraire les patterns Section 2 et les convertir en directives :

```
Pattern: "Jargon technique non expliqué" (3 occurrences)
→ DIRECTIVE: Premier usage de chaque terme technique = définition
   courte + lien vers glossaire.
```

**2.5.3 Points forts unanimes → Éléments à préserver**

Identifier les aspects appréciés par toutes les personas pour ne pas les perdre lors de la réécriture :

```
Unanime positif: "Structure progressive claire"
→ PRÉSERVER: Maintenir l'ordre logique (concept → syntaxe → exemple).
```

**2.5.4 Synthèse des contraintes**

Compiler une fiche de contraintes avant écriture :

```markdown
## Contraintes d'écriture (depuis feedback personas)

### Structurelles (must-haves)
- [ ] [contrainte 1]
- [ ] [contrainte 2]

### Directives de ton (patterns)
- [ ] [directive 1]
- [ ] [directive 2]

### À préserver (points forts)
- [élément 1]
- [élément 2]
```

**Présenter cette fiche à l'utilisateur avant de commencer l'écriture (Step 4).**

### Step 3: Pre-Writing Checks

Verify availability of:

- [ ] All technical terms exist in glossaire.md
- [ ] All referenced features documented in client docs
- [ ] All code examples use correct syntax/versions
- [ ] All diagrams are feasible (Mermaid/PlantUML)
- [ ] Continuity with previous sections (if not section 1)

Report any issues to user.

### Step 4: Generate Section Draft

Create a structural draft based on TOC + template + overview:

```markdown
## Draft: [Titre Section]

### Structure proposée

1. **Introduction:** [contexte, objectifs]
2. **Concepts principaux:**
   - Concept A: [explication]
   - Concept B: [explication]
3. **Détails techniques:**
   - Paramètres/Configuration
   - Exemples de code
4. **Diagrammes:** [liste]
5. **Clôture:** [résumé, liens vers sections suivantes]

### Key Points à couvrir

- [ ] [Key point 1]: prévu dans [sous-section X]
- [ ] [Key point 2]: prévu dans [sous-section Y]
- [ ] [Key point 3]: prévu dans [sous-section Z]

### Éléments techniques

| Élément | Utilisation prévue |
|---------|-------------------|
| Diagrammes | [quels schémas, où] |
| Code | [quels langages, où] |
| Tables | [quelles données, où] |
```

### Step 5: Brainstorm & Refine

**Objectif:** Affiner le draft avant l'écriture finale.

#### 5.1 Identifier les zones floues

Pour chaque élément du draft, vérifier :

| Zone | Question | Résolu ? |
|------|----------|----------|
| Complexité | Niveau de détail approprié pour l'audience ? | [ ] |
| Exemples | Exemples réalistes et complets ? | [ ] |
| Termes techniques | Définitions nécessaires ? | [ ] |
| Transitions | Liens logiques entre sections ? | [ ] |

#### 5.2 Vérifier les ressources disponibles

Docs client nécessaires pour cette section :

- [ ] Spécifications: `.docs/sources/[spec].pdf`
- [ ] Captures d'écran: `.docs/sources/screenshots/`
- [ ] Exemples de code: `.docs/sources/examples/`
- [ ] Diagrammes existants: `.docs/sources/diagrams/`

**Si ressource manquante:**
```
⚠️ Ressource manquante détectée:
- [élément] → Aucun fichier trouvé dans .docs/sources/

Options:
1. Créer la ressource avant d'écrire (recommandé)
2. Utiliser placeholder [À COMPLÉTER]
3. Simplifier la section pour éviter l'élément

Choix utilisateur requis.
```

#### 5.3 Proposer des enrichissements

Suggestions basées sur la documentation client :

- **Diagramme complémentaire:** [proposition]
- **Exemple de cas d'usage:** [proposition]
- **Tableau comparatif:** [proposition]
- **Lien vers ressource externe:** [proposition]

#### 5.4 Validation utilisateur

Présenter le draft enrichi et demander confirmation :

```
Draft affiné prêt. Résumé des décisions:
- [décision 1]
- [décision 2]

Proceed with writing? [Y/n]
```

### Step 6: Write Chapter

**Mode --feedback :** Écrire le chapitre DE ZÉRO depuis la TOC, en intégrant organiquement les contraintes extraites au Step 2.5. Les éléments demandés par les personas (exemples, définitions, diagrammes, clarifications) doivent faire partie du texte dès la conception, pas être ajoutés après coup.

Structure the chapter according to output-style:

```markdown
<!-- chapitres/chapitre<XX>.md -->
<!-- Généré par write-technical.prompt.md -->

# Titre de la Section

## Introduction

[Contexte, objectifs, à quoi sert cette section]

## Concept Principal 1

[Explication claire et progressive]

### Détails techniques

| Paramètre | Type | Description |
|-----------|------|-------------|
| param1 | String | Description |

### Exemple

\`\`\`python
# Code exemple
def fonction():
    pass
\`\`\`

<!-- KEY_POINT: [description] - COVERED -->

## Diagramme d'Architecture

\`\`\`mermaid
graph TD
    A[Composant A] --> B[Composant B]
\`\`\`

## Concept Principal 2

[Suite du contenu]

> **Important**
> Note importante pour l'utilisateur.

<!-- KEY_POINT: [description] - COVERED -->

## Résumé

[Points clés à retenir, prochaines étapes]
```

**Writing Guidelines from Output-Style:**
- Tone: [objectif, précis, professionnel]
- Sentence length: [court à moyen pour clarté]
- Technical terms: [définis à la première occurrence]
- Examples: [concrets, testables, complets]

### Step 7: Validate Key Points

Check each TOC key point is covered:

```
[ ] Key Point 1: [location in text]
[ ] Key Point 2: [location in text]
[ ] Key Point 3: [location in text]
```

IF any key point missing: add or revise text.

### Step 8: Save and Report

1. Save to `chapitres/chapitre<XX>.md`
2. Report statistics:
   - Word count
   - Key points covered
   - Diagrams included
   - Code examples added
   - Tables created
3. Update TOC with completion status

## Output Format

```markdown
# Chapter Written: [Title]

**File:** `chapitres/chapitre<XX>.md`
**Word count:** [count]

## Key Points Coverage

- [x] [Point 1]: Line XX
- [x] [Point 2]: Line YY
- [x] [Point 3]: Line ZZ

## Technical Elements

**Diagrams:** [list with types]
**Code blocks:** [list with languages]
**Tables:** [count]
**Screenshots suggested:** [locations]

## Glossary Terms Used

[List of technical terms from glossaire.md]

## Quality Notes

[Any observations about the writing]

## Feedback Constraints Applied (mode --feedback only)

- [x] [Contrainte 1]: intégrée à [sous-section]
- [x] [Contrainte 2]: intégrée à [sous-section]
- [x] [Directive 1]: appliquée dans [X passages]
- Préservé: [élément 1], [élément 2]

## Next Step

Run: `workshop/review-technical.prompt.md chapitre<XX>.md`
Then: `scripts/build-icml.py --project <client>/<projet>`
```

## Transition

After chapter is written, route to:

```
workshop/review-technical.prompt.md
Arguments: chapitre<XX>.md
```

Then convert to ICML:

```
python scripts/build-icml.py --project <client>/<projet>
```

## Quality Checklist

- [ ] Synopsis fully covered
- [ ] All key points addressed
- [ ] Tone appropriate for audience
- [ ] Length appropriate
- [ ] Markdown well-formed
- [ ] Technical terms consistent with glossaire.md
- [ ] Output-style conventions followed
- [ ] Code examples tested/valid
- [ ] Diagrams render correctly
- [ ] No jargon without explanation
- [ ] (--feedback) Toutes les contraintes structurelles intégrées organiquement
- [ ] (--feedback) Directives de ton appliquées dans le texte
- [ ] (--feedback) Points forts unanimes préservés
- [ ] (--feedback) Section existante NON lue — réécriture pure depuis TOC
