---
name: comment
description: Persona-based reader analysis of chapter quality and engagement
argument-hint: <file> [persona1 persona2...] [--all]
version: 4.5
changelog:
  - version: 4.5
    date: 2026-02-20
    changes:
      - "Ajout champ patience dans Persona Schema Reference (défini par generate-persona, lu par comment)"
      - "Si persona définit patience.scoring_impact, l'appliquer lors du scoring (Step 2)"
  - version: 4.4
    date: 2026-02-15
    changes:
      - "Nouveau Step 2.0c : Craft Checklist obligatoire par type de chapitre (novel/rules/scenario) — questions spécifiques AVANT scoring"
      - "Nouveau Step 2.5 : Avocat du Diable — chaque persona DOIT lister 3 faiblesses après scoring, ajoutées en Section 1 comme 🟢 suggestions"
      - "Objectif : forcer la recherche active de problèmes, contrer le biais de confirmation quand le même modèle écrit et relit"
  - version: 4.3
    date: 2026-02-14
    changes:
      - "Nouveau Step 2.0b : Redundancy Check inter-chapitres (PNJ re-décrits, vocabulaire re-traduit, disclaimers répétés, conseils MJ génériques)"
      - "Impact scoring : ≥3 redondances = Clarté -1pt"
  - version: 4.2
    date: 2026-02-14
    changes:
      - "Cross-référencement obligatoire pj.md/document-rules.md dès it.1 (Step 2.0)"
      - "Vérification arithmétique du consensus obligatoire (Step 3.1)"
      - "Divergence check: écart-type personas < 1.0 → réévaluation (Step 2.3)"
      - "Chemins .wip/comments/ au lieu de .docs/comments/"
  - version: 4.1
    date: 2026-02-14
    changes:
      - "Ajout smart loading: personas peuvent charger références depuis bank.yml (loading_strategy: from_bank_yml)"
      - "Nouvelle étape 1.1 Load Smart References: détecte rules-files.systeme, docs.univers, univers/.docs/sources/"
      - "Exemple mj-medfan: charge adrenaline-d100.md + Engrenages-regles.txt pour vérifier statblocks vs vanilla"
      - "Fallback legacy: si loading_strategy absent, utilise liste statique reference_documents"
  - version: 4.0
    date: 2026-02-13
    changes:
      - "Passage échelle /20 (18+ production-ready, 14+ seuil, 11 plafond must-have, 8 plafond deal-breaker)"
      - "Ajout must-haves check (Step 2.1-2.3) avec plafonds score"
      - "Output format migration vers 3-section template (routing doctor/tone-finder/iteration)"
---

# Comment: Persona Reader Analysis

## Goal

Evaluate text through reader personas, scoring engagement/clarity/immersion/satisfaction. **Complements doctor.prompt.md (technical) with qualitative feedback.**

**Output:** Structured 3-section format for routing to doctor/tone-finder/iteration (see template below).

## Context

**Chapter to analyze:**
```markdown
@$ARGUMENTS[0]
```

**Bank configuration:**
```yaml
@bank.yml
```

**Output template:**
```markdown
@docs/templates/comment-output-template.md
```

## Persona Discovery **[1]**

Scan in priority order (project > universe > global):

```bash
# 1. Project personas (most specific)
<univers>/<projet>/.templates/personas/*.yml

# 2. Universe personas
<univers>/.templates/personas/*.yml

# 3. Global personas (fallback)
docs/templates/personas/*.yml
```

**Auto-selection by document type:**

| Document Type | Recommended Personas |
|---------------|---------------------|
| `novel` | player-immersive, casual-reader *(legacy)* |
| `roleplaying` | gm-practitioner, player-immersive *(legacy)* |
| `scenario` | gm-practitioner + project personas *(legacy)* |
| `user-guide` | clarity-expert + project personas (end-user, domain-expert) |
| `technical-doc` | technical-reviewer, clarity-expert + project personas |
| `api-doc` | technical-reviewer + project personas (developer) |
| `process-doc` | compliance-checker, clarity-expert + project personas |

**Usage:**
```bash
# Auto-detect personas
comment.prompt.md chapitres/chapitre01.md

# Specific personas
comment.prompt.md chapitres/chapitre01.md clarity-expert technical-reviewer

# All discovered personas
comment.prompt.md chapitres/chapitre01.md --all
```

## Process **[2-3-4]**

### 1. Load Personas

For each persona (auto or specified):
1. Load .yml definition
2. Parse expectations, criteria, weights
3. Note source level (project/universe/global)
4. **Check if `reference_documents.loading_strategy: "from_bank_yml"`** :
   - Si OUI : Charger références depuis bank.yml (voir 1.1 Load Smart References)
   - Si NON : Charger liste statique `reference_documents` (legacy)

### 1.1 Load Smart References (depuis bank.yml)

Si persona définit `loading_strategy: "from_bank_yml"` :

**1. Charger bank.yml du projet** (déjà chargé en contexte)

**2. Détecter références selon clés persona :**
- `client_docs` → `bank.yml > docs.client`, `docs.glossaire`, `docs.architecture`
- `project_docs` → `bank.yml > docs.projet`, sources du projet

**3. Appliquer exclusions persona** (si définies) :
- Exclure fichiers `persona.background.reference_documents.exclusions`

**4. Charger fichiers détectés :**
```markdown
@<client>/.docs/CLIENT.md     # Documentation client (contexte)
@<client>/.docs/glossaire.md  # Termes métier (référence terminologique)
```

**5. Conserver en mémoire pour cross-check étape 2**

**Exemple clarity-expert (guide utilisateur CERASCAN) :**
- ✅ Charge `cerascan/.docs/glossaire.md` → termes métier (compte commercial, liste d'envie, etc.)
- ✅ Charge `cerascan/.docs/CLIENT.md` → audience cible, ton attendu
- Peut vérifier : terminologie cohérente, audience niveau basique respecté

### 2.0 Mandatory Cross-Reference Check (AVANT scoring)

**Pour tout document technique (`technical-doc`, `api-doc`) :**

1. **Charger le glossaire client** (si existe dans bank.yml > docs.glossaire)
2. **Charger les spécifications de référence** (si existe dans bank.yml > docs.architecture)
3. **Scanner chaque valeur technique, paramètre et référence croisée :**
   - Vérifier : terme utilisé = terme exact du glossaire
   - Vérifier : valeur chiffrée cohérente avec spécifications
   - Vérifier : renvois vers d'autres sections pointent vers du contenu existant
4. **Toute incohérence = issue 🔴 CRITICAL en Section 1**
   - Flag : `[CROSS-REF FAIL]` + fichier source + ligne + valeur attendue vs trouvée

**Cette étape est NON-NÉGOCIABLE.** Une valeur technique incorrecte invalide tout score de Clarté > 12/20.

### 2.0b Redundancy Check — Inter-Chapter (AVANT scoring, chapitres 02+)

**Pour les chapitres 02+**, scanner les chapitres précédents dans `chapitres/` et relever les redondances :

1. **Termes re-définis** : un terme technique ou métier reçoit-il sa définition complète alors qu'il a déjà été introduit dans un chapitre antérieur ? → Flag 🟡 Section 1 `[REDUNDANCY]` avec suggestion de renvoi
2. **Procédures répétées** : une procédure déjà documentée est-elle re-décrite intégralement (au lieu d'un renvoi) ? → Flag 🟡 Section 1 `[REDUNDANCY]`
3. **Avertissements répétés** : un encadré ⚠️ Attention apparaît-il pour un risque déjà signalé dans un chapitre antérieur ? → Flag 🟡 Section 1 `[REDUNDANCY]`
4. **Introductions redondantes** : des formulations d'introduction génériques identiques apparaissent-elles dans plusieurs chapitres ? → Flag 🟡 Section 2 comme pattern systémique

**Impact scoring :** ≥3 redondances dans un même chapitre = Clarté -1pt (information noyée dans les répétitions).

### 2.0c Craft Checklist (AVANT scoring, obligatoire)

**Chaque persona DOIT répondre explicitement aux questions ci-dessous avant de scorer.** Les réponses alimentent les Sections 1 et 3. Une question sans réponse = review invalide.

**Déterminer le type de chapitre** depuis la TOC ou bank.yml, puis appliquer la checklist correspondante :

#### Chapitres USER-GUIDE (guides utilisateur, tutoriels, manuels)

| # | Question | Réponse attendue |
|---|----------|-----------------|
| U1 | **Complétude procédures** : Chaque tâche documentée a-t-elle une procédure pas-à-pas complète (début → résultat visible) ? Lister les tâches incomplètes. | Tâches incomplètes ou "toutes complètes" |
| U2 | **Un verbe par étape** : Chaque étape de procédure contient-elle une seule action ? Lister les étapes avec actions multiples (ex: "Cliquer X et remplir Y"). | Étapes multi-actions ou "chaque étape = 1 action" |
| U3 | **Captures d'écran** : Les étapes visuellement complexes ont-elles une capture ou un placeholder documenté ? Lister les étapes sans support visuel où il en faudrait un. | Étapes sans visuel ou "toutes couvertes" |
| U4 | **Vérification résultat** : L'utilisateur peut-il vérifier que chaque procédure a réussi ? (message de confirmation, état visible, critère mesurable). | Procédures sans critère de succès ou "toutes vérifiables" |
| U5 | **Troubleshooting** : Les erreurs prévisibles (connexion, permissions, état inattendu) sont-elles documentées avec une solution ? | Erreurs manquantes ou "toutes couvertes" |
| U6 | **Langage audience** : Le texte utilise-t-il le niveau de langage déclaré (novice/intermédiaire/expert) de façon cohérente ? Citer les passages hors-niveau. | Citations ou "niveau cohérent" |

#### Chapitres TECHNICAL-DOC (spécifications, architecture, API)

| # | Question | Réponse attendue |
|---|----------|-----------------|
| T1 | **Exhaustivité technique** : Toutes les mécaniques documentées ont-elles leurs paramètres/valeurs précis ? Lister ce qui est flou ou manquant. | Manques ou "tout documenté" |
| T2 | **Exemples concrets** : Chaque concept technique a-t-il au moins un exemple (code, commande, valeur réelle) ? Lister les concepts sans exemple. | Concepts sans exemple ou "tous illustrés" |
| T3 | **Vérifiabilité** : Chaque assertion technique est-elle testable/vérifiable ? Identifier les affirmations non vérifiables ("environ", "généralement"). | Assertions floues ou "tout vérifiable" |
| T4 | **Cohérence terminologique** : Les termes techniques sont-ils utilisés de façon cohérente avec le glossaire et les chapitres précédents ? | Incohérences ou "terminologie cohérente" |
| T5 | **Prérequis explicites** : Les dépendances et prérequis sont-ils clairement listés en début de section ? | Prérequis manquants ou "tous explicites" |

**Impact sur le scoring :** Les réponses négatives à la checklist DOIVENT se refléter dans les scores correspondants. Un chapitre avec des passages "tell" identifiés ne peut pas avoir Immersion > 16. Un chapitre sans exemples chiffrés ne peut pas avoir Clarté > 14. Un guide utilisateur avec étapes multi-actions ne peut pas avoir Clarté > 14. Un doc technique avec assertions non vérifiables ne peut pas avoir Clarté > 12.

---

### 2. Read as Persona **[3]**

**Embody each persona and evaluate:**

| Criterion | Score (1-20) | Evidence | Weighted |
|-----------|--------------|----------|----------|
| Engagement | X/20 | [examples] | X * weight |
| Clarity | X/20 | [examples] | X * weight |
| Immersion | X/20 | [examples] | X * weight |
| Satisfaction | X/20 | [examples] | X * weight |
| **SUBTOTAL** | - | - | **X.X/20** |

**2.1 Check Must-Haves** (avant score final)

Pour chaque persona, vérifier `expectations.must_have`:
- ✅ TOUS présents → score final = subtotal
- ❌ 1+ manquant → **score MAX 11/20** + flag 🔴 CRITICAL Section 1

**2.2 Check Deal-Breakers**

Pour chaque persona, vérifier `expectations.deal_breakers`:
- ✅ TOUS absents → score final = subtotal (ou plafond must-have)
- ❌ 1+ présent → **score MAX 8/20** + flag 🔴 DEALBREAKER Section 2

**Score final = MIN(subtotal, plafond_must_haves, plafond_deal_breakers)**

### 2.3 Divergence Check

**Après avoir scoré TOUS les personas :**

1. Calculer l'écart-type des scores persona
2. **Si écart-type < 1.0 point** : les personas sont trop homogènes
   - Identifier le persona dont le profil est le plus éloigné du consensus
   - Re-évaluer ce persona en se demandant : "Qu'est-ce qui manque SPÉCIFIQUEMENT à ce lecteur ?"
   - Documenter au moins 1 divergence explicite en Section 3

**Objectif :** Chaque itération doit contenir au moins 1 divergence documentée (aspect où 2+ personas sont en désaccord de ≥2 points).

### 2.5 Devil's Advocate (APRÈS scoring, obligatoire)

**Chaque persona DOIT répondre à la question suivante APRÈS avoir scoré :**

> « Qu'est-ce qu'un critique sévère reprocherait à ce chapitre ? Lister 3 faiblesses concrètes, même mineures, avec citations du texte. »

**Règles :**
- Les 3 faiblesses doivent être **distinctes** des issues déjà remontées en Section 1
- Elles doivent citer un **passage précis** du texte (numéro de ligne ou citation)
- Elles peuvent être des suggestions d'enrichissement, pas seulement des erreurs
- Si un persona ne trouve vraiment que 2 faiblesses, il doit justifier pourquoi la troisième n'existe pas

**Routing :** Les faiblesses sont ajoutées en **Section 1 comme 🟢 suggestions** (pas de correction automatique, mais visibles pour le doctor ou le relecteur humain).

**Objectif :** Contrer le biais de confirmation. Forcer la recherche active de problèmes au lieu de la validation passive. Un texte sans aucune faiblesse n'existe pas.

---

### 3. Consensus Score **[8]**

**Weighted by persona relevance:**
- Project personas: weight 1.0 (most relevant)
- Universe personas: weight 0.8
- Global personas: weight 0.5

```python
consensus = sum(score * relevance_weight) / sum(relevance_weight)
```

**Échelle /20:**
- **18-20/20** : Production-ready (publiable immédiatement)
- **15-17/20** : Corrections mineures (1-2h, typos/formatage)
- **12-14/20** : Utilisable avec réserves (must-haves OK, nice-to-have manquants)
- **9-11/20** : Non utilisable (must-haves manquants, corrections majeures)
- **6-8/20** : Inadéquat (deal-breakers présents, réécriture partielle)
- **1-5/20** : Inutilisable (hors sujet, réécriture complète)

### 3.1 Arithmetic Verification

**OBLIGATOIRE avant de publier le consensus :**

1. Recalculer : `sum(score_i × weight_i)` pour chaque persona
2. Recalculer : `consensus = total_pondéré / sum(weights)`
3. Vérifier : le score affiché en titre = le score calculé (tolérance ±0.01)
4. Si écart > 0.01 : corriger AVANT publication

**Le fichier ne doit contenir qu'UN SEUL score consensus, pas deux valeurs différentes.**

### 4. Save to `.wip/comments/` **[4]**

**Output file:** `.wip/comments/<chapitre>-personas.md`

Example: `chapitre01-personas.md`, `chapitre02-personas.md`

## Output Format **[4]**

**IMPORTANT:** Use ONLY the 3-section template structure:

```markdown
@docs/templates/comment-output-template.md
```

**Do NOT use the old format below** (kept for reference only):

<details>
<summary>OLD FORMAT (deprecated, do not use)</summary>

```markdown
# Retour Personas : <Chapitre Title>

**Date d'analyse :** <date>
**Fichier analysé :** `<chemin>`
**Personas utilisées :** X personas (Y globales + Z projet)

---

## 📊 Scores Globaux

| Persona | Niveau | Score | Verdict |
|---------|--------|-------|---------|
| **<Nom>** | Global/Projet | X.X/20 | ✅/⚠️/❌ Commentaire |
| **CONSENSUS** | - | **X.X/20** | ✅ **Verdict global** |

---

## 👤 <Persona 1>

**Profil :** <reading_style summary> — Priorité <top criterion>

### Scores Détaillés
- **Engagement** : X/20 (weight 0.X) = X.X
- **Clarté** : X/20 (weight 0.X) = X.X ⭐ (si prioritaire)
- **Immersion** : X/20 (weight 0.X) = X.X
- **Satisfaction** : X/20 (weight 0.X) = X.X
- **TOTAL** : **X.X/20**

### ✅ Must-Haves Validés
- [x] Requis 1
- [ ] Requis 2 — ⚠️ Remarque

### 🔴 Deal-Breakers
- Aucun / [Description] à [location]

### 🎯 Points Forts
- Point fort 1 avec exemple
- Point fort 2

### ⚠️ Points d'Amélioration
- [ ] Amélioration 1
- [ ] Amélioration 2

### 💬 Citation
> "Feedback du persona (voix authentique)"

---

[Répéter pour chaque persona]

---

## 🎯 Consensus

### Unanimes Positifs
- Élément apprécié par tous

### Unanimes Négatifs  
- Élément critiqué par tous

### Divergences
| Aspect | Persona A | Persona B |
|--------|-----------|-----------|
| ... | Opinion | Opinion différente |

---

## 📋 Action Immédiate

**IF consensus < 14/20:**
- 🔴 Corrections prioritaires
- Retour à doctor.prompt.md ou réécriture

**IF consensus >= 14/20:**
- ✅ Chapitre validé
- Améliorations optionnelles
```

**Save to:** `.wip/comments/<filename>-personas.md`

Example: `chapitre01-personas.md`

## Persona Schema Reference

```yaml
persona:
  id: "<slug-id>"
  name: "<Display Name>"
  description: "<1-2 sentence description>"

  reading_style:
    pace: "fast|medium|slow"
    attention: "focused|scanning|deep"
    tolerance_for_complexity: "low|medium|high"

  patience:  # Optional (v4.5) — profil de tolérance au contenu non-exploitable
    profile: "<slug>"
    baseline: "low|medium|high"
    chapter_decay: "none|low|moderate|steep"
    triggers_impatience: [list]
    tolerates: [list]
    scoring_impact: "<pénalités concrètes>"

  expectations:
    must_have: [list]
    nice_to_have: [list]
    deal_breakers: [list]

  criteria:
    engagement: { weight: 0.X, measures: [...] }
    clarity: { weight: 0.X, measures: [...] }
    immersion: { weight: 0.X, measures: [...] }
    satisfaction: { weight: 0.X, measures: [...] }
    # Weights MUST sum to 1.0
```

## Create Missing Personas

Use `generate-persona.prompt.md` to create project-specific personas:

```bash
# Example
@docs/prompts/workshop/generate-persona.prompt.md "Commercial showroom CERASCAN"
```

## Quality Checklist

- [ ] Personas discovered from appropriate directories
- [ ] **Craft Checklist complétée** (Step 2.0c) — toutes les questions ont une réponse
- [ ] Each persona fully embodied during reading
- [ ] Scores justified with text evidence
- [ ] **Scores cohérents avec Craft Checklist** — une étape multi-action identifiée ⇒ Clarté ≤ 14, etc.
- [ ] Must-haves and deal-breakers checked
- [ ] Redundancy check inter-chapitres effectué (chapitres 02+)
- [ ] **Devil's Advocate complété** (Step 2.5) — 3 faiblesses par persona, ajoutées en Section 1
- [ ] Consensus weighted by relevance
- [ ] Output saved to `.wip/comments/`
- [ ] Action plan if score < 14/20
