# Rapport de Cohérence — Workshop Prompts
**Agent :** coherence-checker v2.0
**Scope :** `docs/prompts/workshop/` (~25 fichiers)
**Date :** 2026-02-28

---

## 1. Index Normalisé (Conventions de Référence)

| Convention | Valeur correcte | Valeur incorrecte fréquente |
|------------|----------------|-----------------------------|
| Variable argument | `$ARGUMENTS` | `$1`, `$2` |
| Chemin client | `<client>/<projet>/` | `<univers>/<projet>/` |
| Types de documents | `technical-doc \| user-guide \| api-doc \| process-doc` | `novel \| scenario \| roleplaying \| guide` |
| WIP artefacts | `.wip/comments/`, `.wip/reports/`, `.wip/changelog/` | `.docs/comments/`, `.claude/comments/` |
| Lexique métier | `glossaire.md` | `terminologie.md` |
| Output-style path | `<client>/.output-styles/<fichier>.md` | `.claude/output-style.md` |
| Transition paths | `@docs/prompts/workshop/<prompt>.md` | `workshop/<prompt>.md` |
| Scoring | `/20` | A-F (letter grades) |

---

## 2. Inventaire des Incohérences

### 🔴 BLOQUANT — Prompts à réécrire ou supprimer

#### `generate-toc.prompt.md`
**Statut :** Jamais upgradé — entièrement JdR
- Types `novel/scenario/roleplaying` partout
- Sources : `terminologie.md`, `UNIVERS.md`, `factions.md`, `personnages.md`, `geographie.md`, `magie.md`
- Step 2.5 : "PNJ prétirés", "PNJ clés", `novels-details.md`, `scenarios-details.md`
- Template INDEX.md : `Type: [novel/scenario/roleplaying]`, `Univers: [univers]`, section `Personnages principaux`
- Transition : `write-roleplaying.prompt.md`
- **→ Action : Upgrade complet requis (priorité #1)**

#### `evaluate.prompt.md`
**Statut :** Obsolète — routing JdR + LaTeX
- Types : `novel/roleplaying/scenario/guide`
- Routing vers : `write-novel.prompt.md`, `write-roleplaying.prompt.md`
- Score : "LaTeX templates available"
- Fonctionnalité remplacée par `init-project.prompt.md` + `brainstorm.prompt.md`
- **→ Action : Supprimer**

#### `write-toc.prompt.md`
**Statut :** Doublon obsolète de `generate-toc`
- Types JdR, routing JdR
- Output : `toc.md` (au lieu de `.toc/INDEX.md`)
- **→ Action : Supprimer (superseded by generate-toc)**

#### `add-text.prompt.md`
**Statut :** Hardcodé pour Écryme — non générique
- Paths hardcodés : `@ecryme/.docs/terminologie.md`, `@ecryme/.output-styles/solo-ecryme.md`
- Style "Prose XIXe soutenue", "sensorialité écrymienne"
- **→ Action : Déplacer vers `ecryme/` ou supprimer du workshop générique**

---

### 🟠 IMPORTANT — Prompts partiellement contaminés

#### `comment.prompt.md` (v4.5)
- `<univers>/<projet>/` → doit être `<client>/<projet>/`
- "Universe personas" → "Client personas"
- Table auto-sélection : types `novel`, `roleplaying`, `scenario` encore présents
- Step 2.0b : "PNJ re-décrits", "conseils MJ génériques"
- Step 2.0c Craft Checklist : manque `process-doc` et `api-doc`
- `.docs/comments/` dans ancienne section → `.wip/comments/`
- **→ Action : Fix paths + types (v4.5 → v4.6)**

#### `tone-finder.prompt.md`
- `<univers>` partout → `<client>`
- Types JdR dans argument-hint : `--only novel|rules|scenario`
- Steps 3A-3D : questions entièrement narratives (POV fiction, dialogues, Fantasy/Horror)
- Step 4 output : `# Output Style: [Univers] — [Type]`
- Step 6 : `<univers>/.output-styles/<univers>-[type].md`
- Mode Amélioration : `.docs/comments/` → `.wip/comments/`
- **→ Action : Upgrade `<univers>` → `<client>` + questions techniques**

#### `generate-persona.prompt.md` (v2.1)
- Paths : `<univers>/.templates/personas/` et `<univers>/<projet>/.templates/personas/`
- Exemples : `casual-reader`, `fan-wot` (JdR), criteria `rulebook`/`scenario`
- Scope output : `<univers>/<projet>/`
- **→ Action : Fix paths + exemples**

#### `persona-trainer.prompt.md`
- Path : `@<univers>/.templates/personas/`
- `.docs/comments/` → `.wip/comments/`
- Exemples : "olivier-larue", "mj-medfan"
- Crée `.docs/personas-changelog.md` → devrait être `.wip/`
- **→ Action : Fix paths + exemples**

#### `research.prompt.md`
- Charge : `<univers>/.docs/UNIVERS.md`, `terminologie.md`, `lieux.md`, `personnages.md`
- Routing vers `evaluate.prompt.md` (obsolète)
- **→ Action : Fix paths + routing**

#### `doctor.prompt.md` (v1.5)
- Règle NEVER : "terminologie.md" et "UNIVERS.md" → `glossaire.md`
- Step : "Terminologie per terminologie.md (strict)" → `glossaire.md`
- Exemple correction : `*saidin* écrit *saidar*` (résidu WoT)
- **→ Action : Fix terminologie + exemple**

#### `write-toc-chapter.prompt.md` (v1.4)
- Frontmatter : `@.docs/comments/` → `.wip/comments/`
- Template toc-chapter : sections `Personnages` et `Lieux` — JdR
- Changelog : types `novel | rules | scenario`
- **→ Action : Fix paths + types**

#### `write-technical.prompt.md` (v1.1)
- `.claude/output-style.md` → `<client>/.output-styles/<fichier>.md`
- Step 5 nommé "brainstorm" (confusion avec le prompt brainstorm)
- Transition : `workshop/review-technical.prompt.md` → `@docs/prompts/workshop/`
- **→ Action : Fix path + nommage step + transition**

#### `write-user-guide.prompt.md` (v1.0)
- `.claude/output-style.md` → `<client>/.output-styles/<fichier>.md`
- Load order différent de `write-technical` (incohérence structurelle)
- Transition : `workshop/comment.prompt.md` → `@docs/prompts/workshop/`
- **→ Action : Fix path + load order + transition**

#### `review-technical.prompt.md` (v1.0)
- `.claude/output-style.md` → `<client>/.output-styles/<fichier>.md`
- Scoring A-F ≠ `/20` (utilisé par `comment.prompt.md`)
- Critère "Immersion" — résidu JdR
- Pas d'output vers `.wip/reports/`
- **→ Action : Fix path + scoring + critères + output**

---

### 🟡 OPTIONNEL — Legacy intentionnel (pas de blocage)

#### `univers-extract.prompt.md`
- `$1`/`$2` au lieu de `$ARGUMENTS`
- Entièrement JdR (terminologie, histoire, factions)
- **→ Question ouverte : legacy intentionnel pour JdR existant ou à purger ?**

#### `extract.prompt.md`, `extract-chunk.prompt.md`, `extract-distribute.prompt.md`, `extract-debug.prompt.md`
- Distribution vers `UNIVERS.md`/`terminologie.md`/`rules-files/` — JdR
- `<univers>` dans paths
- **→ Question ouverte : legacy ou adapter pour `<client>` ?**

#### `tabula-rasa.prompt.md`
- `.tex` files dans le reset pattern — résidu LaTeX mineur
- **→ Action : Nettoyage mineur (pas urgent)**

---

## 3. Incohérences Systémiques Transversales

| # | Incohérence | Prompts affectés | Sévérité |
|---|-------------|-----------------|----------|
| 1 | `.docs/comments/` vs `.wip/comments/` | comment, tone-finder, persona-trainer, write-toc-chapter | 🟠 |
| 2 | `<univers>/` vs `<client>/` | comment, generate-persona, persona-trainer, research, tone-finder, extract* | 🟠 |
| 3 | Types JdR (novel/roleplaying/scenario) | generate-toc, comment, evaluate, write-toc, write-toc-chapter | 🔴 |
| 4 | `.claude/output-style.md` wrong path | write-technical, write-user-guide, review-technical | 🟠 |
| 5 | `terminologie.md` vs `glossaire.md` | generate-toc, doctor, write-toc-chapter | 🟠 |
| 6 | Scoring A-F vs /20 | review-technical vs comment | 🟠 |
| 7 | Transition paths partiels (`workshop/` vs `@docs/prompts/workshop/`) | write-technical, write-user-guide | 🟡 |
| 8 | Prompts obsolètes toujours présents | evaluate, write-toc, add-text | 🔴 |

---

## 4. Prompts Propres (aucune action requise) ✅

- `brainstorm.prompt.md` (v2.1)
- `init-project.prompt.md` (v1.5)
- `client-extract.prompt.md` (v1.1)
- `upgrade.prompt.md`
- `coherence-checker.md` (v2.0)

---

## 5. Plan d'Action Prioritisé

### Phase 1 — Suppressions (immédiat, pas de risque)
1. Supprimer `evaluate.prompt.md`
2. Supprimer `write-toc.prompt.md`
3. Déplacer/supprimer `add-text.prompt.md`
4. Mettre à jour `README.md` (retirer les entrées supprimées)

### Phase 2 — Upgrades critiques (bloquants pipeline)
5. **`generate-toc.prompt.md`** — upgrade complet (types, sources, template INDEX.md, transition)
6. **`write-technical.prompt.md`** — fix output-style path + step nommage + transition
7. **`write-user-guide.prompt.md`** — fix output-style path + load order + transition
8. **`review-technical.prompt.md`** — fix output-style path + scoring /20 + output .wip/reports/

### Phase 3 — Fix paths systémiques
9. **`comment.prompt.md`** — `<univers>` → `<client>`, `.docs/` → `.wip/`, types doc
10. **`tone-finder.prompt.md`** — `<univers>` → `<client>`, questions techniques
11. **`write-toc-chapter.prompt.md`** — paths .wip + types

### Phase 4 — Fix prompts secondaires
12. **`doctor.prompt.md`** — terminologie + exemple
13. **`research.prompt.md`** — paths + routing
14. **`generate-persona.prompt.md`** — paths + exemples
15. **`persona-trainer.prompt.md`** — paths + .wip

### Phase 5 — Décisions legacy (requiert input)
16. Décider du sort de `univers-extract.prompt.md` et `extract*.prompt.md`

---

## 6. Questions Ouvertes

1. **`univers-extract.prompt.md`** : Garder tel quel pour legacy JdR ou upgrader vers `<client>` ?
2. **`extract*.prompt.md`** : Pipeline extraction PDF — adapter pour clients ou laisser legacy ?
3. **`add-text.prompt.md`** : Déplacer dans `ecryme/` ou simplement supprimer ?
