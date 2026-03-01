---
name: Review Pipeline
description: Orchestrates personas feedback, technical corrections, and output-style improvements
argument-hint: <chapter-range> [--personas list] [--threshold N] [--target N]
---

# Review Pipeline Agent

Agent de relecture industriel garantissant qualité ≥ seuil (défaut 14/20) avant publication.

**Pipeline :** Feedback personas → Triage (technique vs structurel) → Correction ou Réécriture → Amélioration continue output-styles

| Action | Condition | Prompt | Output |
|--------|-----------|--------|--------|
| Feedback | Toujours | comment.prompt.md | .wip/comments/*-personas.md |
| Correction technique | Issues patchables (typos, stats, termes) | doctor.prompt.md | .wip/reports/doctor-report-*.md |
| Réécriture informée | Must-haves structurels manquants | write-roleplaying/novel --feedback | chapitres/chapitre<NN>.md (v2) |
| Amélioration style | Patterns systémiques | tone-finder | output-style v+1 |
| Calibration personas | Personas trop indulgents après 3 it. | persona-trainer | personas/*.yml v+1 |

## Workflow

```
chapitres/chapitre<NN>.md
         │
         ▼
    comment.prompt.md (personas) → .wip/comments/chapitre<NN>-personas.md
         │                          (sections: technical / systemic / qualitative)
         ├────────────────┬────────────────┐
         ▼                ▼                ▼
    Technical        Systemic         Qualitative
    issues           patterns         scores
         │                │                │
         ▼                │                ▼
    TRIAGE               │            Check target
    ┌─────┴──────┐       │                │
    │            │       │                │
    ▼            ▼       │                │
 Patchable   Structurel  │                │
    │            │       │                │
    ▼            ▼       ▼                │
 doctor    write-*    tone-finder         │
 (patch)   --feedback (output-style)      │
    │      (rewrite)     │                │
    └────────┴───────────┴────────────────┘
                          │
         ┌────────────────┴─────────────────┐
         │ Score < target ?                 │
         └────────────────┬─────────────────┘
                    ┌─────┴──────┐
                    ▼            ▼
              CONTINUE       BREAK
              LOOP           (target atteint
              (it < 5)        ou plateau)
```

## Input

**Format :** `<chapter-range> [options]`

```bash
# Exemples
2              # Chapitre 02
2-5            # Chapitres 02 à 05
all            # Tous les chapitres
2,4,6          # Chapitres spécifiques
```

**Options :**
- `--personas <list>` : Personas séparés par virgule (défaut: auto-détection)
- `--threshold <N>` : Seuil minimum (défaut: 14/20)
- `--target <N>` : Score cible itératif (défaut: 18/20)
- `--dry-run` : Afficher plan sans exécuter

**Note :** Échelle /20 (18-20 = production-ready, 15-17 = corrections mineures, 12-14 = réserves, 9-11 = non utilisable)

## Context

**Charger AU DÉBUT de Step 1 :**
```
@bank.yml                                   # Config + type document
@docs/typographie.md                        # Règles français

# Personas scannés dans Step 1.1.5:
@<projet>/.templates/personas/*.yml         # Priorité 1 (poids 1.0)
@<univers>/.templates/personas/*.yml        # Priorité 2 (poids 0.8)
@docs/templates/personas/*.yml              # Priorité 3 (poids 0.5)

# Chaînage: Exécuter @docs/agents/writing-pipeline.md avant si chapitres manquants
```

**⚠️ SPEC PAR VALEUR, PAS PAR RÉFÉRENCE**

Chaque agent ou step qui invoque un prompt (comment, doctor, write-roleplaying...) doit :
1. **Lire intégralement** le fichier prompt AVANT d'exécuter (via Read tool, pas @reference passée en paramètre)
2. **Embarquer le contenu** dans le prompt de l'agent — ne pas supposer que l'agent connaît le fichier
3. Si le prompt est trop long pour un seul agent : lancer **un agent par persona** en parallèle, puis un **agent de consolidation** qui agrège les scores et produit le rapport 3-sections

Un agent qui reçoit `"exécute comment.prompt.md"` sans le contenu du fichier improvise. C'est la source principale de sauts d'étapes et de scores mal calculés.

## Rules

1. **AUTO-DETECT PERSONAS** : TOUS personas compatibles (projet + univers + global), rotation dynamique
2. **ROTATION DYNAMIQUE** : Retirer personas ≥ target après chaque itération (économie tokens)
3. **SUIVRE LE TEMPLATE 3-SECTIONS** : Section 1 → triage (doctor vs rewrite), Section 2 → tone-finder, Section 3 → iteration
4. **TRIAGE OBLIGATOIRE** : Classifier CHAQUE issue Section 1 en « patchable » ou « structurel » AVANT de choisir doctor vs rewrite. Si ≥2 personas plafonnées à 11/20 par must-haves structurels → rewrite --feedback, pas doctor
5. **TONE-FINDER AUTOMATIQUE** : Si Section 2 contient patterns, lancer tone-finder sans validation humaine
6. **CHANGELOG AVANT MODIFICATION** : Les corrections sont tracées dans `.wip/changelog/chapitre<NN>-changelog.md` (pas de fichier .bak)
7. **RE-VALIDATION POST-CORRECTION** : Re-run comment après doctor OU rewrite OU tone-finder (nouvelle itération)
8. **ITÉRATION LIMITÉE** : Max 3 itérations par chapitre, STOP si plateau (Δ < 1.0 entre deux itérations consécutives)
9. **BREAK SI POOL VIDE** : TOUS personas ≥ target (pool actif vide) → STOP
10. **TARGET PAR DÉFAUT** : 18/20 (qualité premium), changeable via --target
11. **REWRITE > PATCH** : En cas de doute entre doctor et rewrite, préférer rewrite. Un texte organique est toujours supérieur à un patchwork
12. **SPEC PAR VALEUR** : Lire intégralement chaque fichier prompt avant exécution. Ne jamais passer une référence (`@file`) sans avoir lu le contenu — un agent qui improvise une spec saute des étapes

## Process

### Step 1: Planning

**1.1 Parse & Validate**
- Chapitres : Convertir range → liste numéros
- Seuil : `--threshold` OU 14/20
- Target : `--target` OU 18/20 (mode itératif, max 3 cycles)
- Valider existence `chapitres/chapitre<NN>.md` → **STOP si absents** avec liste manquants

**1.1.5 Auto-Detect Personas** (si `--personas` non spécifié)

Scanner dans l'ordre de priorité :
```bash
# Priorité 1: Projet (poids 1.0)
<projet>/.templates/personas/*.yml

# Priorité 2: Univers (poids 0.8)
<univers>/.templates/personas/*.yml

# Priorité 3: Global (poids 0.5)
docs/templates/personas/*.yml
```

Filtrer selon type document (bank.yml) :
- `type: roleplaying` → **Inclure :** gm-*, player-*, fan-*, mj-*
- `type: novel` → **Inclure :** reader-*, fan-*, literary-*
- `type: technical` → **Inclure :** specialist-*, reviewer-*

**Sélection dynamique :**
1. **Itération 1 :** TOUS personas compatibles trouvés (pas de limite)
2. **Itérations suivantes :** Retirer personas dont le delta < 0.3 entre 2 itérations (persona en plateau individuel)
3. **Économie tokens :** Personas en plateau retirés du pool actif
4. **BREAK :** Quand pool actif vide (tous en plateau ou ≥ target)

**1.2 Confirm Plan**
```
Chapitre: [02]
Personas pool initial (7 total):
  - fan-wot (univers 0.8)
  - professeure-francais (univers 0.8)
  - mj-medfan (univers 0.8)
  - mj-zombiology (univers 0.8)
  - gm-practitioner (global 0.5)
  - player-immersive (global 0.5)
  - casual-reader (global 0.5)

Seuil: 14/20
Target: 18/20 (rotation dynamique, max 3 itérations)

[Continuer] [Modifier] [Annuler]
```

### Step 2: Review Execution

**Pour chaque chapitre :**

**LOOP** (si `--target` spécifié, max 5 itérations) :

**2.1 Feedback Personas**

**Avant d'exécuter :** Lire intégralement `docs/prompts/workshop/comment.prompt.md` (Read tool). Embarquer le contenu dans le prompt agent — ne pas passer une référence.

**Stratégie selon nombre de personas :**
- **≤3 personas :** Un agent unique exécute comment.prompt.md pour tous les personas, produit le rapport 3-sections complet
- **≥4 personas :** Lancer un agent par persona en parallèle (chacun reçoit comment.prompt.md + le chapitre + son fichier persona), puis un agent de consolidation qui agrège les scores et produit le rapport 3-sections final

```bash
@docs/prompts/workshop/comment.prompt.md chapitres/chapitre<NN>.md \
  --personas "<liste-détectée-ou-spécifiée>"
```
→ `.wip/comments/chapitre<NN>-personas.md` (3 sections: technical / systemic / qualitative)

**Checklist avant de retourner le résultat :**
- [ ] Section 1 : chaque issue classifiée patchable/structurel avec justification
- [ ] Section 2 : patterns systémiques listés (ou « aucun »)
- [ ] Section 3 : score individuel par persona + consensus pondéré calculé
- [ ] Aucune étape de comment.prompt.md sautée (vérifier Steps 1→N du fichier lu)

**2.2 Analyze Results** (rotation dynamique)

Parser `.wip/comments/chapitre<NN>-personas.md` (template 3 sections) :

**Section 1: TECHNICAL ISSUES** → TRIAGE doctor vs rewrite
- Compter corrections par priorité : 🔴 (critical), 🟡 (important), 🟢 (optional)
- Extraire description pour --remarks
- **CLASSIFIER chaque issue :**
  - **Patchable** = insertion isolée sans impact sur le tissu narratif (typo, stat incorrecte, terme manquant, kanji, disclaimer ponctuel, exemple de résolution ajouté en encadré)
  - **Structurel** = must-have absent qui nécessite une refonte du texte narratif (descriptions sensorielles absentes de toute la prose, ratio narratif/mécanique cassé, codes culturels absents des descriptions de PNJ, structure de chapitre inadaptée)

**Section 2: SYSTEMIC PATTERNS** → tone-finder.prompt.md
- Détecter patterns répétés (≥3 chapitres) ou deal-breakers (1 persona)
- Extraire YAML fix proposés

**Section 3: QUALITATIVE SCORES** → iteration control + rotation
- Extraire : persona_scores[] (individuel), consensus_score (moyenne pondérée)
- **Mettre à jour pool actif :** Retirer personas ≥ target (18/20) OU en plateau individuel (Δ < 0.3)
- Logger : `Chapitre <NN> [it.X]: consensus <score>/20, pool actif: [<personas-restants>]`

**Exemple rotation :**
```
It.1: pool [A, B, C, D, E] → scores [17.5, 14.0, 16.0, 18.0, 15.0]
      → retirer D (≥ 18/20 target)
      → pool actif it.2: [A, B, C, E]

It.2: pool [A, B, C, E] → scores [17.8, 15.5, 16.8, 16.2]
      → Δ persona: A +0.3 (plateau), B +1.5, C +0.8, E +1.2
      → retirer A (Δ < 0.3, plateau individuel)
      → pool actif it.3: [B, C, E]

It.3: pool [B, C, E] → scores [16.0, 17.2, 16.8]
      → Δ persona: B +0.5, C +0.4, E +0.6
      → max 3 itérations → BREAK
```

**2.3 Check Target** (rotation dynamique)
- **IF pool actif vide (tous personas ≥ 18/20 ou en plateau) :** `✅ BREAK LOOP (objectif atteint pour tous personas)`
- **IF itération > 1 ET Δ_consensus < 1.0 :** `⚠️ BREAK LOOP (plateau, personas bloqués: [liste])`
- **IF itération = 3 :** `⚠️ BREAK LOOP (limite atteinte, personas restants: [scores])`
- **ELSE :** CONTINUE LOOP (Step 2.4 puis Step 2.1 avec pool actif réduit)

**2.4 Conditional Actions** (selon template 3-sections + triage)

**A.1 Corrections techniques patchables** (SI Section 1 contient corrections patchables 🔴 OU 🟡)

**Avant d'exécuter :** Lire intégralement `docs/prompts/workshop/doctor.prompt.md` (Read tool). Embarquer le contenu dans le prompt agent.

```bash
@docs/prompts/workshop/doctor.prompt.md chapitres/chapitre<NN>.md \
  --remarks "<Section 1: Technical Issues>"
```
→ `.wip/reports/doctor-report-chapitre<NN>-it<X>.md` + changelog mis à jour

**Checklist avant de retourner le résultat :**
- [ ] Toutes les corrections 🔴 traitées
- [ ] Corrections 🟡 traitées ou justifiées (skip avec raison)
- [ ] Changelog mis à jour avec liste des modifications
- [ ] Aucune étape de doctor.prompt.md sautée

**A.2 Réécriture informée** (SI Section 1 contient issues structurelles OU ≥2 personas plafonnées à 11/20 par must-haves)

Le doctor ne peut pas résoudre les must-haves structurels par insertion — le texte doit être réécrit organiquement depuis la TOC.

**Avant d'exécuter :** Lire intégralement `docs/prompts/workshop/write-roleplaying.prompt.md` (ou `write-novel.prompt.md`) via Read tool. Embarquer le contenu dans le prompt agent. Ne pas lire le chapitre existant — la réécriture repart de la TOC.

```bash
# Déterminer le type de document depuis bank.yml
# type: roleplaying/scenario → write-roleplaying
# type: novel → write-novel

# Réécriture depuis TOC + feedback personas comme contraintes
@docs/prompts/workshop/write-roleplaying.prompt.md <NN> \
  --feedback .wip/comments/chapitre<NN>-personas.md

# OU pour un roman:
@docs/prompts/workshop/write-novel.prompt.md <NN> \
  --feedback .wip/comments/chapitre<NN>-personas.md
```

→ `chapitres/chapitre<NN>.md` (réécrit) + changelog mis à jour

**Checklist avant de retourner le résultat :**
- [ ] TOC lue (pas le chapitre existant)
- [ ] Feedback personas intégré comme contraintes (issues structurelles adressées)
- [ ] Cross-Chapter Awareness compilé (Step 1.5 du write prompt)
- [ ] Fichier sauvegardé, taille vérifiée (wc -l)

**Critères de choix A.1 vs A.2 :**

| Critère | → A.1 Doctor | → A.2 Rewrite |
|---------|-------------|---------------|
| Personas plafonnées ≤11/20 | 0-1 | ≥2 |
| Issues patchables par insertion | Majorité | Minorité |
| % du texte narratif à modifier | <20% | >20% |
| Must-haves absents du tissu narratif | 0 | ≥1 |
| Δ estimé post-doctor | ≥ +2.0 pts | < +2.0 pts (patchwork) |

**Note :** Si doute entre A.1 et A.2, préférer A.2. Un texte réécrit organiquement sera toujours plus cohérent qu'un texte patché. Le coût est plus élevé (réécriture complète) mais le résultat est meilleur.

**B. Amélioration output-style** (SI Section 2 contient patterns ≥1)
```bash
# Lancer automatiquement tone-finder
@docs/prompts/workshop/tone-finder.prompt.md <univers> \
  --improve-from-feedback .wip/comments/chapitre<NN>-personas-it<X>.md

# Logger patterns détectés
→ `.wip/reports/tone-finder-report-chapitre<NN>-it<X>.md`

# Si output-style modifié (v+1):
  → Logger: "Output-style mis à jour: <univers>/.output-styles/<style>.md v+1"
  → CONTINUE LOOP (retour Step 2.1 pour re-validation)

# Si aucune modification nécessaire:
  → Logger: "Patterns mineurs, aucune modification output-style"
  → CONTINUE LOOP (si itérations restantes)
```

**C. Amélioration personas** (SI consensus < threshold après 3 itérations)
```bash
# Workflow post-mortem (manuel ou automatique)
# Détecte personas trop indulgents ou ratant issues critiques

# PAUSE: Demander validation humaine
⚠️ Chapitre bloqué < 14/20 après 3 itérations
Issues structurelles détectées par doctor mais pas par personas

[Améliorer personas] [Accepter score] [Annuler]

IF [Améliorer personas]:
  1. Identifier personas problématiques (scores > consensus + 3pts)
  2. @docs/prompts/workshop/persona-trainer.prompt.md <persona-id> \
       --feedback-files .wip/comments/chapitre<NN>-*.md \
       --issues-missed "<description issues ratées>"
  3. Valider persona v2.0 sur chapitre test
  4. IF amélioré: Remplacer persona, re-run review-pipeline
```

**D. Comparaison scores** (si doctor OU rewrite OU tone-finder exécuté)
- Logger : `Δ Consensus: +X.X pts (persona1: +Y, persona2: +Z)`
- **IF toujours < threshold ET aucune itération restante :**
  - `⚠️ STOP — Décision humaine requise :`
  - `[Réécrire --feedback]` → Action A.2 (write-roleplaying/novel --feedback)
  - `[Modifier TOC]` → write-toc-chapter puis write-roleplaying/novel
  - `[Accepter score]` → Passer au chapitre suivant
  - `[Améliorer personas]` → voir Action C ci-dessus
- **ELSE IF itérations restantes :** CONTINUE LOOP (retour Step 2.1)

**END LOOP**

**2.5 Summary Report** (fin de batch)
- Créer `.wip/review-summary-<date>.md` avec :
  - Tableau scores (chapitre, consensus avant/après, actions, itérations)
  - Patterns détectés + recommandations
  - Prochaines étapes (checklist)

**Note Performance :** ~3min/chapitre séquentiel, ~7min/5 chapitres parallèle (batch 3), +2min/itération si `--target`

## Output

**Fichiers générés :**
```
.wip/comments/chapitre<NN>-personas.md           # Feedback personas
.wip/reports/doctor-report-chapitre<NN>-it<X>.md # Corrections (si doctor, par itération)
.wip/reports/tone-finder-report-*.md             # Rapports tone-finder
.wip/review-summary-<date>.md                    # Rapport global
.wip/changelog/chapitre<NN>-changelog.md         # Historique corrections cumulatif
```

**Console :**
```
Chapitre 02 [it.1]: 13.0/20 → doctor... ✓
Chapitre 02 [it.2]: 15.6/20 → doctor... ✓
Chapitre 02 [it.3]: 16.2/20 → plateau détecté (Δ +0.6 < 1.0)
✅ Chapitre 02: score final 16.2/20 (3 itérations)

Note: Étapes varient selon options (--skip-doctor, --improve-style, --target)
```

## Examples

```bash
# Review complet avec auto-détection personas
review-pipeline.md all

# Chapitres ciblés, personas spécifiques, seuil strict
review-pipeline.md 2-5 --personas fan-wot,professeure-francais --threshold 17

# Review rapide sans corrections, juste feedback
review-pipeline.md all --skip-doctor

# Mode itératif: améliorer jusqu'à 18/20 minimum
review-pipeline.md all --target 18 --improve-style

# Qualité premium avec itérations parallèles
review-pipeline.md 2-7 --target 19 --parallel
```

**Chaînage avec writing-pipeline :**
```bash
writing-pipeline.md 2-5        # Génère chapitres
review-pipeline.md 2-5         # Relecture complète
```

---

**Version:** 4.3
**Changelog:**
- v4.3: Règle 12 SPEC PAR VALEUR. Bloc ⚠️ dans Context. Steps 2.1/A.1/A.2 : instruction "lire intégralement avant exécuter" + checklist par step. Stratégie multi-agents pour ≥4 personas (parallèle + consolidation)
- v4.2: Seuil plateau harmonisé (Δ < 1.0). Rotation par delta persona (Δ < 0.3 = plateau individuel). Chemins .wip/. Nettoyage références /10 → /20
- v4.1: Remplacement fichiers .bak.itN par changelog cumulatif (.wip/changelog/chapitre<NN>-changelog.md)
- v4.0: Triage doctor vs rewrite --feedback. Issues structurelles (must-haves absents du tissu narratif) routées vers write-roleplaying/novel --feedback au lieu de doctor. Critères de choix explicites. Règle 4 TRIAGE OBLIGATOIRE + règle 11 REWRITE > PATCH
- v3.1: Intégration tone-finder dans LOOP avec validation humaine, template structuré comment (3 sections)
- v3.0: Ajout `--target` avec boucle itérative (max 3 it., détection plateau Δ<0.3)
- v2.0: Suppression commentaires HTML, ajout Goal, fusion steps, condensation
- v1.0: Version initiale
