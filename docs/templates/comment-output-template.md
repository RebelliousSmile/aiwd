# Template de Sortie comment.prompt.md

**Version:** 2.5
**Compatible:** review-pipeline v4.2+, comment.prompt.md v4.4+

## Vue d'Ensemble

Sortie **3 sections** pour routing automatique :

- **Section 1: TECHNICAL** → Corrections ponctuelles (typo, grammaire, termes)
- **Section 2: SYSTEMIC** → Patterns récurrents (≥3 chapitres)
- **Section 3: QUALITATIVE** → Scores personas + consensus

---

## Routing Automatique

| Section | Condition | Action |
|---------|-----------|--------|
| 1. TECHNICAL | ≥1 correction 🔴 ou 🟡 | `doctor` |
| 2. SYSTEMIC | ≥3 patterns OU 1 dealbreaker | `tone-finder` (PAUSE validation) |
| 3. QUALITATIVE | consensus < target ET it < 3 | CONTINUE LOOP |

**Flux :** `comment.prompt.md` génère `.wip/comments/<file>-personas.md` → `review-pipeline.md` parse → route vers outils

---

## Structure à Générer

```markdown
# Analyse Personas: <Titre>

**Date:** <date> | **Chapitre:** <NN> | **Personas:** X (Y projet + Z global)

---

## Section 1: TECHNICAL ISSUES → doctor.prompt.md

### Typographie
- [ ] 🔴 <Description problème>
  - Lignes: <liste>
  - Source: <Persona> (<contexte>)

### Grammaire
- [ ] 🟡 <Description problème>
  - Ligne X: "<erreur>" → "<correction>"
  - Source: <Persona>

### Terminologie
- [ ] 🔴 <Description problème>

### Formatting
- [ ] 🟡 <Description problème>

### Devil's Advocate (🟢 suggestions)
- [ ] 🟢 <Persona>: <Faiblesse 1 — citation ligne X>
- [ ] 🟢 <Persona>: <Faiblesse 2 — citation ligne Y>
- [ ] 🟢 <Persona>: <Faiblesse 3 — citation ligne Z>
[Répéter pour chaque persona]

**Priorités:** 🔴 Haute: X | 🟡 Moyenne: Y | 🟢 Basse: Z

**Routing:** <condition> → `doctor.prompt.md <args>`

---

## Section 2: SYSTEMIC PATTERNS → tone-finder.prompt.md

### Pattern 1: <Nom pattern>
- **Occurrences:** <n> instances (lignes: <liste>)
- **Impact:** <Critère> -X.Xpts
- **Source:** <Persona> (<contexte>)
- **Action output-style:**
  ```yaml
  section:
    - "Directive 1"
    - "Directive 2"
  ```

[Répéter pour chaque pattern]

**Recommandation:** <condition> → `tone-finder.prompt.md --improve-from-feedback`

---

## Section 3: QUALITATIVE SCORES

### Persona: <Nom> (<source>)

| Critère | Score | Poids | Pondéré | Evidence |
|---------|-------|-------|---------|----------|
| Engagement | X/20 | 0.X | X.X | [référence] |
| Clarté | X/20 | 0.X | X.X | [référence] |
| Immersion | X/20 | 0.X | X.X | [référence] |
| Satisfaction | X/20 | 0.X | X.X | [référence] |
| **TOTAL** | - | 1.0 | **X.X/20** | - |

**Must-haves:** ✅/❌ | **Deal-breakers:** ✅/❌

#### Craft Checklist
| # | Question | Réponse |
|---|----------|---------|
| N1/R1/S1 | <question> | <réponse avec citations> |
| ... | ... | ... |

#### Points Forts
- Fort 1 + exemple

#### Points d'Amélioration
- [ ] Amélioration 1

[Répéter pour chaque persona]

---

## CONSENSUS: X.X/20

| Persona | Score | Poids | Pondéré | Source |
|---------|-------|-------|---------|--------|
| <Nom> | X/20 | 1.0 | X.X | Projet |
| **CONSENSUS** | - | <sum> | **X.X/20** | - |

---

## ROUTING DECISIONS

**Section 1 (Technical):**
- IF ≥1 correction 🔴 ou 🟡: → `doctor`

**Section 2 (Systemic):**
- IF ≥3 patterns OU 1 dealbreaker: → `tone-finder --improve-from-feedback` (PAUSE validation)

**Section 3 (Iteration):**
- IF consensus < target ET it < 3 ET Δ ≥ 0.3: → CONTINUE LOOP
- ELSE: BREAK LOOP

**Action:**
- 🔴 < 14/20: Corrections prioritaires
- 🟡 14-17/20: Améliorations optionnelles
- ✅ ≥ 18/20: Validé
```

**Fichier de sortie:** `.wip/comments/<filename>-personas.md`

---

## Checklist Génération

- [ ] Cross-ref pj.md/document-rules.md effectué (Step 2.0) — incohérences = 🔴 CRITICAL
- [ ] **Craft Checklist complétée** (Step 2.0c) — toutes questions répondues par chaque persona
- [ ] Section 1 : Corrections avec lignes + source + priorités 🔴🟡🟢
- [ ] **Devil's Advocate** : 3 faiblesses par persona ajoutées en Section 1 comme 🟢
- [ ] **Scores cohérents avec Craft Checklist** — "tell" identifié ⇒ Immersion ≤ 16, etc.
- [ ] Section 2 : Patterns avec occurrences + impact + YAML action
- [ ] Section 3 : Poids critères = 1.0 par persona + consensus pondéré correct
- [ ] Vérification arithmétique : consensus recalculé = consensus affiché (±0.01)
- [ ] Divergence check : écart-type personas ≥ 1.0 OU divergence documentée
- [ ] ROUTING DECISIONS : 3 sections cohérentes
- [ ] Sauvegarde : `.wip/comments/<filename>-personas.md`

---

**Version:** 2.5
**Changelog:** v2.5: Ajout Craft Checklist dans Section 3 (par persona), Devil's Advocate dans Section 1 (🟢 suggestions), cohérence score/checklist. v2.4: Chemins .wip/, échelle /20, section vérification (cross-ref, arithmétique, divergence). v2.3: Fusion Quick Ref + Routing (tableau), suppression ASCII flux redondant
