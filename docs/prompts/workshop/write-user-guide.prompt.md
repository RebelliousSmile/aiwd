---
name: write-user-guide
description: Write user guide section in Markdown with step-by-step instructions
argument-hint: <section-number-or-range> [--feedback <personas-comment-file>]
version: 1.0
changelog:
  - version: 1.0
    date: 2026-02-28
    changes:
      - "Adaptation de write-roleplaying pour guides utilisateur"
      - "Style tutoriel, pas-à-pas, accessible"
      - "Support procédures, captures d'écran, troubleshooting"
---

# Write User Guide Section

## Goal

Write a user guide section in **Markdown** with step-by-step instructions, following TOC specifications and using the project's output-style. The section will be converted to ICML later via `build-icml.py`.

## Context

### Required Resources (MUST load all)

**Bank Configuration:**
```yaml
@bank.yml
```

**Output Style (writing conventions):**
```markdown
@<client>/.output-styles/<output-style>.md
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

**Table of Contents (chapter specifications):**
```markdown
@.toc/toc-chapter<XX>.md
```

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
- Style tutoriel, accessible, pas-à-pas
- Instructions numérotées pour procédures
- Suggestions de captures d'écran avec annotations
- Save chapter to `chapitres/chapitre<XX>.md`
- One chapter per invocation unless range specified
- DO NOT write what is not in the TOC
- **Mode --feedback:** Réécriture TOTALE depuis la TOC. Ne JAMAIS lire ni patcher le chapitre existant. Le feedback personas sert de contraintes supplémentaires, pas de diff à appliquer. Le texte existant est ignoré — seule la TOC fait foi.

## Markdown Syntax for User Guide Content

### Step-by-Step Procedures
```markdown
### Procédure : Nom de la tâche

1. Première étape à réaliser
   - Détail si nécessaire
   - Sous-étape optionnelle

2. Deuxième étape
   > **Astuce**
   > Conseil pour faciliter l'opération.

3. Dernière étape
```

### Screenshot Placeholders
```markdown
![Description de la capture](screenshots/section-XX-fig-01.png)
*Figure 1 : Légende explicative*

<!-- SCREENSHOT: Capture d'écran montrant [élément], avec [annotation] -->
```

### Tips and Warnings
```markdown
> **Astuce**
> Conseil pratique pour l'utilisateur.

> **Attention**
> Avertissement important à ne pas ignorer.

> **Note**
> Information complémentaire utile.
```

### Troubleshooting Sections
```markdown
## Problèmes fréquents

### Symptôme : [Description du problème]

**Cause possible :** [Explication]

**Solution :**
1. Étape de résolution 1
2. Étape de résolution 2
```

### Before/After Comparisons
```markdown
| Avant | Après |
|-------|-------|
| État initial | État final attendu |
```

### Quick Reference Tables
```markdown
| Action | Raccourci | Description |
|--------|-----------|-------------|
| Sauvegarder | Ctrl+S | Enregistre les modifications |
```

## Steps

### Step 1: Load Context

1. Parse bank.yml for project configuration
2. Load output-style (global + project override)
3. Load client documentation
4. Load TOC and extract target section(s)
5. **IF `--feedback`:** Load persona feedback file. **Ne PAS lire la section existante.**
6. Verify all resources loaded successfully

### Step 1.5: Cross-Chapter Awareness (chapitres 02+)

**SKIP pour le chapitre 01.**

Scanner les chapitres précédents dans `chapitres/` pour inventorier ce qui a déjà été présenté :

1. **Termes expliqués** : quels termes techniques ont déjà leur définition ? → utiliser uniquement le terme dans la section courante
2. **Procédures de base** : quelles procédures fondamentales sont déjà documentées ? → y faire référence au lieu de les répéter
3. **Interfaces présentées** : quels écrans/menus sont déjà décrits ? → mention courte uniquement
4. **Avertissements posés** : quels warnings importants sont déjà donnés ? → ne pas les répéter

**Compiler un inventaire rapide** (usage interne, non affiché) avant de passer au Step 2.

### Step 2: Analyze Chapter Requirements

From TOC, extract:

| Element | Value |
|---------|-------|
| Title | [from toc] |
| Synopsis | [from toc] |
| Key Points | [checklist from toc] |
| Audience | [novice/intermediate/expert] |
| Goal | [what user will accomplish] |
| Prerequisites | [required knowledge/setup] |
| Length | [from toc] |

**User Guide Elements:**

| Element | Value |
|---------|-------|
| Procedures | [list of tasks to document] |
| Screenshots | [suggested locations] |
| Tips | [helpful hints to include] |
| Troubleshooting | [common problems] |

**Ressources Visuelles (depuis section "Ressources Visuelles" du toc-chapter) :**

| Figure | Fichier | Outil | Priorité |
|--------|---------|-------|----------|
| [depuis tableau toc] | [path] | [generate-mockups.py / import-screenshot.py] | [must-have / recommandé] |

→ Insérer les refs markdown `![]()` aux positions indiquées dans le toc-chapter. Ne pas inventer de nouvelles images non spécifiées dans le toc.

### Step 2.5: Extract Feedback Constraints (mode --feedback uniquement)

**SKIP si pas de --feedback.**

Parser le fichier `.wip/comments/chapitre<XX>-personas.md` et extraire des **contraintes d'écriture** (pas des corrections) :

**2.5.1 Must-haves manquants → Contraintes structurelles**

Pour chaque persona plafonnée (score ≤ 11/20), extraire les must-haves non satisfaits et les convertir en directives d'écriture :

```
Persona: end-user (11/20, plafond must-have)
Must-have manquant: "Capture d'écran pour chaque étape critique"
→ CONTRAINTE: Chaque procédure de plus de 3 étapes DOIT avoir au moins
   une capture d'écran avec annotations visuelles.
```

**2.5.2 Patterns systémiques → Directives de ton**

Extraire les patterns Section 2 et les convertir en directives :

```
Pattern: "Instructions trop techniques, vocabulaire inadapté" (3 occurrences)
→ DIRECTIVE: Utiliser langage courant, éviter jargon technique sauf si
   défini au premier usage.
```

**2.5.3 Points forts unanimes → Éléments à préserver**

Identifier les aspects appréciés par toutes les personas pour ne pas les perdre lors de la réécriture :

```
Unanime positif: "Astuces pratiques très utiles"
→ PRÉSERVER: Maintenir les encadrés "Astuce" avec conseils concrets.
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

- [ ] All features mentioned exist in client application
- [ ] All screenshots can be captured (or placeholders noted)
- [ ] All procedures tested and validated
- [ ] All terms defined in glossaire.md
- [ ] Continuity with previous sections (if not section 1)

Report any issues to user.

### Step 4: Generate Section Draft

Create a structural draft based on TOC + template + overview:

```markdown
## Draft: [Titre Section]

### Structure proposée

1. **Introduction:** [Objectif, ce que l'utilisateur va accomplir]
2. **Prérequis:** [Accès, configuration nécessaire]
3. **Procédure principale:**
   - Étape 1: [action]
   - Étape 2: [action]
   - Étape 3: [action]
4. **Vérification:** [Comment vérifier le succès]
5. **Problèmes fréquents:** [Troubleshooting]
6. **Prochaines étapes:** [Que faire ensuite]

### Key Points à couvrir

- [ ] [Key point 1]: prévu dans [étape X]
- [ ] [Key point 2]: prévu dans [étape Y]
- [ ] [Key point 3]: prévu dans [section troubleshooting]

### Éléments utilisateur

| Élément | Utilisation prévue |
|---------|-------------------|
| Screenshots | [quelles captures, où] |
| Tips | [quels conseils, où] |
| Warnings | [quels avertissements, où] |
```

### Step 5: Write Chapter

**Mode --feedback :** Écrire le chapitre DE ZÉRO depuis la TOC, en intégrant organiquement les contraintes extraites au Step 2.5. Les éléments demandés par les personas (captures d'écran, simplification langage, astuces, troubleshooting) doivent faire partie du texte dès la conception, pas être ajoutés après coup.

Structure the chapter for user guide use:

```markdown
<!-- chapitres/chapitre<XX>.md -->
<!-- Généré par write-user-guide.prompt.md -->

# Titre de la Section

## Objectif

[Description claire de ce que l'utilisateur va accomplir dans cette section]

## Prérequis

- [ ] [Accès nécessaire]
- [ ] [Configuration préalable]
- [ ] [Connaissances requises]

## Procédure : [Nom de la tâche]

1. **[Première action]**
   
   Description détaillée de l'action.
   
   ![Description capture](screenshots/section-XX-fig-01.png)
   *Figure 1 : [Légende explicative]*
   
   > **Astuce**
   > Conseil pratique pour faciliter cette étape.

2. **[Deuxième action]**
   
   Description de l'action suivante.
   
   <!-- SCREENSHOT: Montrer [élément interface] avec [annotation] -->

3. **[Action finale]**
   
   Dernière étape de la procédure.

<!-- KEY_POINT: [description] - COVERED -->

## Vérification

Pour vérifier que la procédure a réussi :

- [ ] [Critère de succès 1]
- [ ] [Critère de succès 2]

## Problèmes fréquents

### Symptôme : [Description du problème]

**Cause possible :** [Explication simple]

**Solution :**
1. [Étape de résolution 1]
2. [Étape de résolution 2]

> **Attention**
> [Avertissement important si applicable]

<!-- KEY_POINT: [description] - COVERED -->

## Prochaines étapes

Maintenant que vous avez terminé [action], vous pouvez :

- [Action suivante 1] (voir [Section XX])
- [Action suivante 2] (voir [Section YY])
```

**Writing Guidelines from Output-Style:**
- Tone: [friendly, professional, encouraging]
- Sentence length: [short and clear]
- Instructions: [one action per step]
- Visuals: [screenshot + annotation for complex steps]

### Step 6: Validate Content

**Procedure Check:**
```
[ ] Key Point 1: [location in text]
[ ] Key Point 2: [location in text]
[ ] Key Point 3: [location in text]
```

**User Experience Check:**
```
[ ] Steps are clear and actionable
[ ] Screenshots suggested at right moments
[ ] Tips provide real value
[ ] Troubleshooting covers common issues
[ ] Language appropriate for audience
```

IF any check fails: revise content.

### Step 7: Save and Report

1. Save to `chapitres/chapitre<XX>.md`
2. Report statistics:
   - Word count
   - Key points covered
   - Procedures documented
   - Screenshots suggested
   - Tips/warnings included
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

## User Guide Elements

### Procedures
| Procédure | Steps | Screenshots |
|-----------|-------|-------------|
| [nom] | [count] | [count] |

### Support Elements
| Type | Count |
|------|-------|
| Tips | [count] |
| Warnings | [count] |
| Troubleshooting entries | [count] |

## Screenshots Suggested

1. `screenshots/section-XX-fig-01.png` : [description]
2. `screenshots/section-XX-fig-02.png` : [description]

## Feedback Constraints Applied (mode --feedback only)

- [x] [Contrainte 1]: intégrée à [procédure/section]
- [x] [Contrainte 2]: intégrée à [procédure/section]
- [x] [Directive 1]: appliquée dans [X passages]
- Préservé: [élément 1], [élément 2]

## Next Steps

1. Capture screenshots if not already available
2. Persona review: `workshop/comment.prompt.md chapitres/chapitre<XX>.md [personas]`
3. Doctor corrections (if score ≥ 14/20): `workshop/doctor.prompt.md chapitre<XX>.md`
4. Convert to ICML: `scripts/build-icml.py --project <client>/<projet>`
```

## Transition

After chapter is written, route to:

```
workshop/comment.prompt.md
Arguments: chapitres/chapitre<XX>.md [persona1 persona2...]
```

Note: `review-technical.prompt.md` peut être utilisé en complément pour une vérification technique objective, mais `comment` est l'étape principale de validation qualitative par personas.

Then, if score ≥ 14/20:

```
workshop/doctor.prompt.md
Arguments: chapitre<XX>.md
```

Then convert to ICML:

```
python scripts/build-icml.py --project <client>/<projet>
```

## Quality Checklist

- [ ] Synopsis fully covered
- [ ] All key points addressed
- [ ] Procedures tested and validated
- [ ] Steps clear and actionable (one action per step)
- [ ] Screenshots suggested at critical points
- [ ] Tips provide real value
- [ ] Troubleshooting covers common issues
- [ ] Language appropriate for target audience
- [ ] Markdown well-formed
- [ ] Glossaire terms used correctly
- [ ] Output-style conventions followed
- [ ] (sections 02+) Termes déjà expliqués non ré-définis
- [ ] (sections 02+) Procédures de base référencées, non répétées
- [ ] (--feedback) Toutes les contraintes structurelles intégrées organiquement
- [ ] (--feedback) Directives de ton appliquées dans le texte
- [ ] (--feedback) Points forts unanimes préservés
- [ ] (--feedback) Section existante NON lue — réécriture pure depuis TOC
