---
name: write-toc-chapter
description: Generate detailed chapter outline from INDEX.md entry and structure template
argument-hint: <chapter-number> [--feedback <personas-comment-file>]
version: 1.4
changelog:
  - version: 1.4
    date: 2026-03-01
    changes:
      - "Nouveau Step 2.7 : Inventaire Ressources Visuelles — classification mockup/screenshot/diagram, specs YAML pour mockups.yml, refs markdown à insérer dans write-*"
      - "Step 3 : ajout branche technical-doc / user-guide (sections visuelles, procédures, API)"
      - "Template toc-chapter : nouvelle section Ressources Visuelles (tableau + blocs YAML + commandes CLI)"
      - "Quality Checklist : items ressources visuelles ajoutés"
      - "Transition : mention generate-mockups.py et import-screenshot.py"
      - "Type étendu : technical-doc | user-guide | api-doc | process-doc en plus de novel | rules | scenario"
  - version: 1.3
    date: 2026-02-14
    changes:
      - "Nouveau Step 5.5 : Relecture Persona TOC — relecture en persona depuis overview.md (sélection par type de chapitre), corrections intégrées avant écriture finale"
      - "Quality Checklist : items relecture persona ajoutés"
      - "Step 6 : mention corrections persona dans la confirmation"
  - version: 1.2
    date: 2026-02-14
    changes:
      - "Nouveau Step 3.5 : Divergence/Convergence — 3 variantes condensées + tableau comparatif + synthèse hybride"
      - "Step 5 : la fiche générée est le résultat de la synthèse Step 3.5"
      - "Step 6 : affiche les éléments retenus par variante"
      - "Quality Checklist : items divergence/convergence ajoutés"
  - version: 1.1
    date: 2026-02-14
    changes:
      - "Ajout mode --feedback : régénération TOC-chapter informée par retours personas"
      - "Nouveau Step 2.6 : extraction contraintes depuis feedback personas"
      - "Règle explicite : --feedback = régénération totale depuis INDEX.md, jamais un patch du toc-chapter existant"
  - version: 1.0
    date: 2026-01-31
    changes:
      - "Version initiale : génération fiche chapitre depuis INDEX.md + template type"
---

# Write TOC Chapter

## Goal

Generer une fiche detaillee pour un chapitre specifique, en utilisant les informations de INDEX.md et un template de structure adapte au type de document.

## Context

### Configuration Projet

```yaml
@bank.yml
```

### Table des Matieres

```markdown
@.toc/INDEX.md
```

### Chapitre Cible

```text
Chapitre $ARGUMENTS
```

### Persona Feedback (mode --feedback uniquement)

```markdown
@.docs/comments/<chapitre>-personas.md  # si --feedback spécifié
```

### TOC-Chapter existant (mode --feedback uniquement)

```markdown
@.toc/toc-chapter<XX>.md  # lu uniquement pour identifier les lacunes, PAS comme base de réécriture
```

### Template de Structure (selon type)

**Si type = technical-doc:**
```markdown
@docs/templates/technical-doc-chapter.md
```

**Si type = user-guide:**
```markdown
@docs/templates/user-guide-chapter.md
```

## Rules

- Sortie en francais
- Utiliser les informations du chapitre dans INDEX.md comme base
- Adapter la structure selon le template du type de document
- Creer le fichier dans `.toc/toc-chapter<XX>.md`
- Format numero: 2 chiffres (01, 02... 10, 11)
- Ne pas inventer de contenu absent de INDEX.md ou de la documentation univers
- **Mode --feedback:** Régénération TOTALE depuis INDEX.md. Le toc-chapter existant peut être lu pour comprendre les lacunes, mais la fiche est reconstruite de zéro. Le feedback personas sert à enrichir les spécifications (détails manquants, précisions mécaniques, contraintes de ton), pas à patcher l'existant.

## Etapes

### 1. Charger le Contexte

1. Lire bank.yml pour identifier le type de document
2. Lire INDEX.md et extraire les informations du chapitre cible
3. Charger le template de structure correspondant au type
4. Charger la documentation univers pertinente
5. **IF `--feedback`:** Charger le fichier persona feedback. Lire le toc-chapter existant pour comprendre les lacunes (mais ne PAS l'utiliser comme base).

### 2. Extraire les Informations du Chapitre

Depuis INDEX.md, recuperer:
- Titre du chapitre
- Synopsis
- Points cles
- Position dans la structure (acte, partie)
- **Type de chapitre** (technical-doc, user-guide, api-doc, process-doc)

### 2.5. Sélectionner l'Output-Style

**Priorité de sélection :**

1. **Champ `Output-style:` dans INDEX.md** — si le chapitre cible déclare un output-style, l'utiliser directement (chemin résolu depuis bank.yml `output-style`)
2. **Fallback depuis bank.yml** — si le champ est absent dans INDEX.md, inférer depuis le type de chapitre :
   - Documentation technique, architecture, API → `technical-formal` ou `developer-docs`
   - Guide utilisateur, tutoriel, manuel → `user-friendly`
   - Workflow métier, SOP, procédure → `process-documentation`
   - Fallback: premier style trouvé dans `<client>/.output-styles/`

### 2.6. Extraire les Contraintes Feedback (mode --feedback uniquement)

**SKIP si pas de --feedback.**

Parser le fichier `.docs/comments/<chapitre>-personas.md` et extraire des **enrichissements pour la fiche TOC** (pas des corrections textuelles) :

**2.6.1 Must-haves manquants → Spécifications à ajouter**

Pour chaque persona plafonnée (score ≤ 11/20), identifier les lacunes du toc-chapter qui ont causé des problèmes à l'écriture :

```
Persona: clarity-expert (11/20, plafond must-have)
Must-have manquant: "Procédure complète avec critère de vérification"
→ ENRICHISSEMENT TOC: Ajouter dans chaque procédure documentée une note
  "Inclure critère de succès visible par l'utilisateur (message, état, badge)"
```

**2.6.2 Patterns systémiques → Notes d'écriture**

Convertir les patterns récurrents en instructions explicites dans "Notes d'Écriture" :

```
Pattern: "Procédures trop vagues, manque d'actions concrètes" (4 occurrences)
→ NOTE D'ÉCRITURE: "Chaque étape de procédure doit inclure 1 action précise
   (verbe + objet + résultat attendu)"
```

**2.6.3 Points forts → Éléments à conserver**

Identifier ce qui fonctionnait bien pour le préserver dans les spécifications :

```
Unanime positif: "Structure claire en scènes"
→ CONSERVER: Garder la découpe en scènes distinctes avec transitions explicites.
```

**2.6.4 Synthèse**

Compiler les enrichissements avant régénération :

```markdown
## Enrichissements TOC (depuis feedback personas)

### Spécifications à ajouter (must-haves)
- [ ] [spécification 1]
- [ ] [spécification 2]

### Notes d'écriture (patterns)
- [ ] [note 1]
- [ ] [note 2]

### À conserver (points forts)
- [élément 1]
- [élément 2]
```

**Présenter cette synthèse à l'utilisateur avant de régénérer (Step 5).**

### 2.7. Inventaire Ressources Visuelles

**Applicable à tous les types de chapitres.** SKIP si le chapitre ne contient aucun élément d'interface, workflow, procédure ou schéma.

Pour les chapitres `technical-doc` et `user-guide` décrivant une interface ou des étapes avec UI : **cette step est obligatoire.**

#### 2.7.1 — Identifier les besoins visuels

Parcourir le contenu planifié du chapitre (synopsis, sections, étapes) et lister chaque endroit où une illustration améliore la compréhension :

- Interfaces utilisateur à présenter (pages, formulaires, menus, popups)
- Procédures avec étapes visuelles (workflows, processus en plusieurs écrans)
- Schémas d'architecture, flux de données, diagrammes conceptuels
- Tableaux de référence complexes déjà couverts en texte → **pas d'image**

#### 2.7.2 — Classifier chaque ressource visuelle

Pour chaque besoin identifié, choisir le bon outil :

| Critère | Outil | Type |
|---------|-------|------|
| Interface reconstituable (login, menu, popup, dashboard) | `generate-mockups.py` | `mockup` |
| Capture de l'application réelle requise | `import-screenshot.py` | `screenshot` |
| Flux, architecture, séquence, diagramme | `mermaid-to-images.py` *(planifié)* | `diagram` |

#### 2.7.3 — Spécifier chaque ressource

Pour chaque ressource, produire une fiche de spécification :

**Mockup (generate-mockups.py) :**

```
Figure N.X — [Titre descriptif]
  type        : login | menu | popup
  éléments    : [liste des éléments annotables : email, password, user_icon, etc.]
  annotations : [(1) label, (2) label, ...]
  fichier     : chapitres/screenshots/<guide>-ch<NN>-<slug>.png
  id mockup   : ch<NN>-<slug>
  priorité    : must-have | recommandé
```

**Screenshot réel (import-screenshot.py) :**

```
Figure N.X — [Titre descriptif]
  montre      : [description précise de ce que la capture doit montrer]
  annotations : [(1) élément à pointer, (2) élément à pointer, ...]
  fichier     : chapitres/screenshots/<guide>-ch<NN>-<slug>.png
  id mockup   : ch<NN>-<slug>  (pour real_annotations dans mockups.yml)
  priorité    : must-have | recommandé
```

#### 2.7.4 — Générer les blocs de configuration

Produire les éléments prêts à l'emploi pour alimenter le projet.

**Bloc YAML à ajouter dans `mockups.yml` :**

```yaml
# Chapitre <NN> — à ajouter dans mockups.yml → section mockups:

  - id: "ch<NN>-<slug>"
    type: <login|menu|popup>
    output: "chapitres/screenshots/<guide>-ch<NN>-<slug>.png"
    <type>:
      # [champs selon type : email, password, items, message, etc.]
    annotations:
      - {id: 1, element: <element>}
      # ...
    real_annotations:
      # À compléter après avoir la capture réelle (coordonnées px dans image 900×1100)
      - {id: 1, cx: 30, cy: 0, tip_x: 100, tip_y: 0}   # [label] — coords à ajuster
```

**Commandes CLI à exécuter :**

```bash
# Générer les mockups de ce chapitre
python scripts/generate-mockups.py --project "<client>/<projet>" --id "ch<NN>-<slug>"

# Ou importer une vraie capture
python scripts/import-screenshot.py \
    --project "<client>/<projet>" \
    --input <chemin-capture.png> \
    --id "ch<NN>-<slug>"
```

**Refs markdown à insérer dans le chapitre (pour le prompt write-*) :**

```markdown
<!-- TODO: générer via generate-mockups.py --id ch<NN>-<slug> -->
![<Titre figure>](screenshots/<guide>-ch<NN>-<slug>.png)
*Figure N.X : <légende complète avec numéros d'annotation>*
```

#### 2.7.5 — Synthèse visuelle du chapitre

Produire un tableau récapitulatif à inclure dans la section `Ressources Visuelles` du toc-chapter :

```markdown
| Figure | Titre | Fichier | Outil | Priorité |
|--------|-------|---------|-------|----------|
| Fig N.1 | [titre] | `screenshots/<nom>.png` | `generate-mockups.py` (login) | Must-have |
| Fig N.2 | [titre] | `screenshots/<nom>.png` | `import-screenshot.py` | Recommandé |
```

**Ce tableau conditionne directement le prompt write-* :** l'auteur sait quelles images existent (ou seront générées) et insère les refs `![]()` aux bons endroits plutôt que d'inventer des descriptions textuelles de substitution.

### 3. Enrichir selon le Template

Selon le type de document, ajouter les sections appropriees:

**Pour un technical-doc:**
- Prérequis et dépendances
- Architecture / composants impliqués
- Spécifications techniques (paramètres, valeurs, contraintes)
- Exemples concrets (code, commandes, valeurs réelles)
- Cas d'erreur et comportements limites

**Pour un user-guide:**
- Prérequis utilisateur (accès, configuration)
- Procédures pas-à-pas avec critères de succès
- Captures d'écran aux points critiques
- Astuces et avertissements
- Troubleshooting des erreurs courantes

**Pour un process-doc:**
- Acteurs et rôles impliqués
- Déclencheur et conditions d'entrée
- Étapes séquentielles avec responsables
- Points de décision / embranchements
- Livrables et critères de sortie

### 3.5. Divergence / Convergence

Avant de produire la fiche finale, explorer **3 variantes** qui divergent sur la structure, les instructions et l'approche narrative, puis synthétiser le meilleur de chaque.

#### 3.5.1 — Générer 3 variantes condensées

Choisir **3 combinaisons pertinentes pour ce chapitre** en faisant varier simultanément :

| Axe | Ce qui varie |
|-----|-------------|
| **Structure** | Ordre et découpe des sections (linéaire, modulaire, centré personnages, etc.) |
| **Instructions paragraphes** | Degré de prescription (détaillé, évocateur, mécanique-first, etc.) |
| **Approche narrative** | Angle narratif (classique, in medias res, mystère, etc.) |

**Les axes ci-dessus sont indicatifs.** Adapter les combinaisons au chapitre : un chapitre de règles pures n'a pas les mêmes axes pertinents qu'un chapitre narratif. Choisir les 3 combinaisons qui produisent les variantes les plus distinctes et intéressantes.

Pour chaque variante, produire une fiche **condensée** (pas la fiche complète) :

```markdown
### Variante A : [Nom]
**Structure :** [H2/H3 skeleton — titres de sections + ordre]
**Angle :** [description de l'approche en 2-3 phrases]
**Exemples d'instructions :**
- Section X : [instruction spécifique illustrant cette variante]
- Section Y : [instruction spécifique illustrant cette variante]
```

#### 3.5.2 — Tableau comparatif

Présenter les 3 variantes côte-à-côte :

```markdown
| Critère | Variante A | Variante B | Variante C |
|---------|-----------|-----------|-----------|
| Structure | ... | ... | ... |
| Force principale | ... | ... | ... |
| Risque | ... | ... | ... |
| Meilleur pour | ... | ... | ... |
```

#### 3.5.3 — Synthèse

Fusionner les meilleurs éléments de chaque variante dans la fiche finale. La fiche est un **hybride**, pas un choix exclusif d'une variante. Les éléments retenus doivent être cohérents entre eux.

```markdown
## Synthèse

**Retenu de A :** [élément] — parce que [raison]
**Retenu de B :** [élément] — parce que [raison]
**Retenu de C :** [élément] — parce que [raison]

→ Fiche finale ci-dessous
```

**Interaction avec --feedback :** En mode `--feedback`, les enrichissements du Step 2.6 sont appliqués à la **fiche finale** (post-synthèse), pas aux 3 variantes. Les variantes explorent la structure et l'approche ; le feedback personas apporte les contraintes de contenu.

### 4. Verifier la Coherence

- Les personnages mentionnes existent dans personnages.md
- Les lieux mentionnes existent dans geographie.md
- Les termes utilisent la terminologie correcte
- La progression est coherente avec les chapitres adjacents

### 5. Generer le Fichier

**La fiche générée est le résultat de la synthèse Step 3.5** — elle intègre les meilleurs éléments de chaque variante dans une structure cohérente.

**Mode --feedback :** Régénérer la fiche DE ZÉRO depuis INDEX.md, en intégrant les enrichissements du Step 2.6 dans la fiche post-synthèse. Les spécifications ajoutées (exemples mécaniques, détails sensoriels, contraintes de ton) doivent être tissées naturellement dans les sections appropriées, pas ajoutées en annexe.

Creer `.toc/toc-chapter<XX>.md`:

```markdown
# Chapitre [N]: [Titre]

**Type:** [technical-doc|user-guide|api-doc|process-doc]
**Output-style:** [nom-fichier.md]
**Pages prévues:** [estimation]
**Statut:** [À écrire|En cours|OK]

---

## Synopsis

[2-3 phrases depuis INDEX.md]

## Points Cles

- [Point 1]
- [Point 2]
- [Point 3]

## [Sections selon template du type]

### Personnages

| Nom | Role dans ce chapitre |
|-----|----------------------|
| [perso] | [action/evolution] |

### Lieux

- [lieu]: [description courte]

### [Autres sections selon type]

## Ressources Visuelles

<!-- Laisser vide si aucune illustration requise -->

### Tableau des assets

| Figure | Titre | Fichier | Outil | Priorité |
|--------|-------|---------|-------|----------|
| Fig N.1 | [titre] | `screenshots/<nom>.png` | `generate-mockups.py` (type: login) | Must-have |
| Fig N.2 | [titre] | `screenshots/<nom>.png` | `import-screenshot.py` | Recommandé |

### Specs mockups.yml

```yaml
# À ajouter dans mockups.yml → section mockups:
  - id: "ch<NN>-<slug>"
    type: <login|menu|popup>
    output: "chapitres/screenshots/<guide>-ch<NN>-<slug>.png"
    <type>:
      # champs selon type
    annotations:
      - {id: 1, element: <element>}
    real_annotations:
      - {id: 1, cx: 30, cy: 0, tip_x: 100, tip_y: 0}   # coords à ajuster
```

### Commandes & refs markdown

```bash
python scripts/generate-mockups.py --project "<client>/<projet>" --id "ch<NN>-<slug>"
# ou
python scripts/import-screenshot.py --project "<client>/<projet>" --input <capture.png> --id "ch<NN>-<slug>"
```

```markdown
<!-- TODO: générer via generate-mockups.py --id ch<NN>-<slug> -->
![<Titre>](screenshots/<guide>-ch<NN>-<slug>.png)
*Figure N.X : <légende avec annotations numérotées>*
```

## Connexions

- **Precedent:** [lien avec chapitre N-1]
- **Suivant:** [preparation chapitre N+1]

## Notes d'Ecriture

[Instructions specifiques, points d'attention]
[Rappel : insérer les refs images aux positions indiquées dans Ressources Visuelles]
```

### 5.5. Relecture Persona TOC

Après génération de la fiche, effectuer une relecture en adoptant le regard du persona déclaré dans overview.md (section "Relecture TOC") qui correspond au type du chapitre.

**5.5.1 — Identifier le relecteur**

1. Lire le champ `Relecteur TOC:` du chapitre cible dans INDEX.md
2. Si le champ est **absent ou vide** : SKIP ce step
3. Si **plusieurs personas** sont listés (séparés par des virgules) : exécuter chaque relecture séquentiellement

**5.5.2 — Charger le contexte de relecture**

1. Lire la section "Relecture TOC" d'overview.md pour trouver les focus points du persona identifié
2. Charger le fichier persona YAML référencé (profil expert, must-haves, deal-breakers)
3. Charger la documentation pertinente (terminologie.md, UNIVERS.md, document-rules.md selon les focus)

**5.5.3 — Relire la fiche en persona**

Adopter le regard du persona et vérifier chaque focus point déclaré dans overview.md. Les focus points varient selon le persona (univers, technique, etc.) — se fier à ceux listés dans overview.md, pas à une liste codée en dur.

Pour chaque problème détecté, formuler une **correction concrète** (pas un commentaire vague).

**5.5.4 — Intégrer les corrections**

Modifier directement la fiche toc-chapter avec les corrections. Ne pas créer de fichier de review séparé.

Afficher le résultat de la relecture à l'utilisateur :

```markdown
### Relecture Persona : [nom persona]

**Corrections intégrées :**
- [correction 1] → intégrée dans [section]
- [correction 2] → intégrée dans [section]

**Points validés :**
- [point OK 1]
- [point OK 2]

**Alertes (hors périmètre TOC) :**
- [alerte éventuelle pour l'étape d'écriture]
```

**5.5.5 — SKIP si** aucun champ `Relecteur TOC:` dans l'entrée du chapitre dans INDEX.md, ou si aucune section "Relecture TOC" dans overview.md.

### 6. Confirmer

Afficher:
- Chemin du fichier cree
- Resume des sections generees
- **Synthèse divergence/convergence :** éléments retenus de chaque variante (A, B, C) et pourquoi
- Alertes si informations manquantes
- **(--feedback)** Enrichissements intégrés depuis feedback personas
- **(relecture TOC)** Corrections intégrées depuis relecture [nom persona(s)]

## Output Format (mode --feedback)

```markdown
# TOC Chapter Regenerated: [Title]

**File:** `.toc/toc-chapter<XX>.md`
**Mode:** --feedback (régénération depuis INDEX.md + personas)

## Enrichissements Intégrés

- [x] [Spécification 1]: ajoutée dans [section]
- [x] [Spécification 2]: ajoutée dans [section]
- [x] [Note d'écriture 1]: intégrée dans Notes d'Écriture
- Conservé: [élément 1], [élément 2]

## Next Step

Réécrire le chapitre: `write-technical <N> --feedback` ou `write-user-guide <N> --feedback`
```

## Transition

Apres generation, proposer:
- Generer un autre chapitre: `write-toc-chapter <N+1>`
- Passer a l'ecriture: `write-technical <N>` ou `write-user-guide <N>`
- **(--feedback)** Réécrire le chapitre avec feedback: `write-technical <N> --feedback` ou `write-user-guide <N> --feedback`
- **(ressources visuelles)** Générer les mockups planifiés : `python scripts/generate-mockups.py --project "<client>/<projet>"`

## Templates de Structure par Type

### user-guide-chapter (sections spécifiques)

```markdown
## Objectif

[Ce que l'utilisateur va accomplir dans ce chapitre]

## Prérequis

- [ ] [Accès ou configuration nécessaire]

## Procédures

### Procédure : [Nom de la tâche]

1. [Étape 1 — 1 action]
2. [Étape 2 — 1 action]

**Vérification :** [Comment savoir que ça a réussi]

## Problèmes fréquents

### [Symptôme]
**Solution :** [étapes de résolution]
```

### technical-doc-chapter (sections spécifiques)

```markdown
## Vue d'ensemble

[Contexte et objectif de cette section]

## Prérequis

- [Dépendance 1]
- [Dépendance 2]

## Spécifications

### [Composant / Concept]

[Description technique précise]

**Paramètres :**
| Paramètre | Type | Valeur | Description |
|-----------|------|--------|-------------|
| [nom] | [type] | [valeur] | [description] |

**Exemple :**
```code
[exemple concret]
```

## Cas d'erreur

| Erreur | Cause | Solution |
|--------|-------|---------|
| [code/message] | [cause] | [action] |
```

## Quality Checklist

- [ ] Informations INDEX.md fidèlement reprises
- [ ] Template type correctement appliqué
- [ ] Output-style sélectionné et indiqué
- [ ] Personnages vérifiés dans documentation univers
- [ ] Lieux vérifiés dans documentation univers
- [ ] Terminologie conforme à terminologie.md
- [ ] Connexions chapitres adjacents cohérentes
- [ ] 3 variantes condensées générées avec axes de divergence distincts
- [ ] Tableau comparatif forces/risques présenté
- [ ] Synthèse explicite : éléments retenus de chaque variante avec justification
- [ ] Fiche finale = hybride cohérent (pas une variante choisie telle quelle)
- [ ] (--feedback) Enrichissements personas intégrés dans les sections appropriées
- [ ] (--feedback) Notes d'écriture explicites pour guider write-technical/write-user-guide
- [ ] (--feedback) Points forts conservés dans les spécifications
- [ ] (--feedback) Fiche régénérée depuis INDEX.md, pas patchée depuis l'existant
- [ ] (si relecture TOC) Persona(s) sélectionné(s) selon type du chapitre
- [ ] (si relecture TOC) Relecture persona effectuée, corrections intégrées
- [ ] (si relecture TOC) Focus points d'overview.md tous vérifiés
- [ ] (Step 2.7) Besoins visuels identifiés ou section Ressources Visuelles explicitement vide
- [ ] (Step 2.7) Chaque asset classifié : mockup / screenshot / diagram
- [ ] (Step 2.7) Bloc YAML mockups.yml généré pour chaque mockup
- [ ] (Step 2.7) Commandes CLI et refs markdown `![]()` produites pour le prompt write-*
- [ ] (Step 2.7) Aucune description textuelle de substitution pour une image planifiée
