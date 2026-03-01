# Rapport de Cohérence — workshop/

**Date :** 2026-03-01  
**Source de vérité :** CLAUDE.md v6.0 + README.md workshop v2.0  
**Scope :** `docs/prompts/workshop/*.md` (25 fichiers)

---

## Index de Référence (Source de Vérité)

| Convention | Valeur canonique |
|------------|-----------------|
| Paths clients | `<client>/<projet>/` |
| Argument principal | `$ARGUMENTS` (string unique) |
| Types de documents | `technical-doc | user-guide | api-doc | process-doc` |
| Output-style client | `<client>/.output-styles/` |
| Output-style projet | `.output-styles/<nom-projet>.md` |
| Personas client | `<client>/.templates/personas/` |
| Personas global | `docs/templates/personas/` |
| Personas projet | `.templates/personas/` |
| WIP comments | `.wip/comments/` |
| WIP changelog | `.wip/changelog/` |
| WIP reports | `.wip/reports/` |
| Docs client | `CLIENT.md`, `glossaire.md` |
| TOC | `.toc/INDEX.md`, `.toc/toc-chapter<NN>.md` |
| Scale scoring | `/20` (comment.prompt.md fait référence) |

---

## Section 1 — Issues par Prompt

### 🔴 BLOQUANT

---

#### `generate-toc.prompt.md` (v1.3) — Non upgradé

**Jamais migré vers doc-tech. Entièrement JdR.**

| # | Issue | Ligne(s) | Attendu |
|---|-------|----------|---------|
| 1 | Charge `docs.univers`, `docs.terminologie`, `docs.factions`, `docs.personnages`, `docs.histoire`, `docs.geographie`, `docs.magie` | Step 2.5 | Charger `docs.client`, `docs.glossaire`, `docs.projet` |
| 2 | Section `rules-files` dans bank.yml | Step 2.5 | N'existe pas en doc-tech |
| 3 | Output INDEX.md : `**Type:** [novel/scenario/roleplaying]` | Format sortie | `[technical-doc|user-guide|api-doc|process-doc]` |
| 4 | Output INDEX.md : `**Univers:** [univers]` | Format sortie | `**Client:** [client]` |
| 5 | Tags `[INTRO]` et `[REF ChXX]` référencent "PNJ" | Règles | Termes doc-tech : composants, concepts |
| 6 | Section `Personnages principaux` dans INDEX.md | Format sortie | N'existe pas en doc-tech |
| 7 | Champ `Relecteur TOC:` basé sur overview.md section "Relecture TOC" | Step 5.5 | Non documenté côté doc-tech |
| 8 | Mention "PNJ prétirés", "scenarios-details.md", "novels-details.md" | Step 2.5 | Supprimer |
| 9 | Routing : `write-roleplaying` en step 7 | Transition | `write-technical` ou `write-user-guide` |
| 10 | `argument-hint` : `"BRAINSTORM.md, extraction.txt, synopsis existant"` | Frontmatter | `"overview.md ou chemin source"` |

**Verdict : Upgrade complet requis — ce prompt n'est pas utilisable en doc-tech.**

---

#### `evaluate.prompt.md` — Obsolète JdR

| # | Issue | Attendu |
|---|-------|---------|
| 1 | Types : `novel`, `roleplaying`, `scenario`, `guide` | `technical-doc | user-guide | api-doc | process-doc` |
| 2 | Routing Step 6 → `write-novel.prompt.md` ou `write-roleplaying.prompt.md` | `write-technical.prompt.md` ou `write-user-guide.prompt.md` |
| 3 | Resource Scores : "Templates | LaTeX templates available" | Supprimer LaTeX |
| 4 | README workshop ne liste pas ce prompt | Soit upgrader, soit supprimer |

**Verdict : Prompt obsolète — non listé dans README. À supprimer ou remplacer par `init-project`.**

---

#### `write-toc.prompt.md` — Doublon obsolète

| # | Issue | Attendu |
|---|-------|---------|
| 1 | Enregistre `toc.md` (racine projet) | Générer `.toc/INDEX.md` |
| 2 | Types : `novel`, `roleplaying`, `scenario` | Types doc-tech |
| 3 | Routing → `write-novel.prompt.md` ou `write-roleplaying.prompt.md` | Types doc-tech |
| 4 | Charge `<univers>/.docs/UNIVERS.md` | Charger `<client>/.docs/CLIENT.md` |
| 5 | Non listé dans README workshop | — |

**Verdict : Doublon de `generate-toc.prompt.md` (avant upgrade). À supprimer.**

---

#### `add-text.prompt.md` (v1.0) — Ecryme hardcodé

| # | Issue | Ligne(s) | Attendu |
|---|-------|----------|---------|
| 1 | Charge `@ecryme/.docs/terminologie.md` | Context Resources | `@<client>/.docs/glossaire.md` |
| 2 | Charge `@ecryme/.output-styles/solo-ecryme.md` (exemple hardcodé) | Context Resources | `@<client>/.output-styles/<output-style>.md` |
| 3 | Style obligatoire : "Prose XIXe soutenue" | Rules | Supprimer — Ecryme-specific |
| 4 | Style obligatoire : "Sensorialité écrymienne" | Rules | Supprimer — Ecryme-specific |
| 5 | Style obligatoire : "Métaphores mécaniques présentes naturellement" | Rules | Supprimer — Ecryme-specific |
| 6 | Usage CLI : `chapitres/chapitre02.md "Soupape face Vertige"` | Arguments | Exemple Ecryme hardcodé |

**Verdict : Outil Ecryme-specific. À généraliser (supprimer références Ecryme) ou déplacer vers `ecryme/.claude/`.**

---

### 🟡 IMPORTANT

---

#### `comment.prompt.md` (v4.5) — Résidus JdR/Univers

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Persona Discovery : `<univers>/<projet>/.templates/personas/` | Section "Persona Discovery" | `<client>/<projet>/.templates/personas/` |
| 2 | Persona Discovery : `<univers>/.templates/personas/` | Section "Persona Discovery" | `<client>/.templates/personas/` |
| 3 | Auto-selection table : types `novel`, `roleplaying`, `scenario` encore présents | Section auto-selection | Garder uniquement `user-guide`, `technical-doc`, `api-doc`, `process-doc` |
| 4 | Redondancy Check 2.0b : "PNJ re-décrits, vocabulaire re-traduit, disclaimers répétés, conseils MJ génériques" | Step 2.0b | Adapter au contexte doc-tech |
| 5 | `.docs/comments/` dans l'ancien format (section dépréciée) | Ancien format `<details>` | `.wip/comments/` (déjà correct dans Step 4) |
| 6 | `@docs/templates/comment-output-template.md` — ce fichier existe-t-il ? | Context | Vérifier existence ou créer |
| 7 | Hiérarchie personas : "Universe personas: weight 0.8" | Step 3 Consensus | Adapter : "Client personas: weight 0.8" |
| 8 | Craft Checklist 2.0c : seulement USER-GUIDE et TECHNICAL-DOC | Step 2.0c | Ajouter `api-doc` et `process-doc` |

---

#### `tone-finder.prompt.md` — Non upgradé (partiel)

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | `argument-hint` : `<univers> [--only novel|rules|scenario]` | Frontmatter | `<client> [--only technical-formal|user-friendly|...]` |
| 2 | Step 4 template : `# Output Style: [Univers] — [Type]` | Step 4 | `# Output Style: [Client] — [Type]` |
| 3 | Step 6 report : `<univers>/.output-styles/<univers>-[type].md` | Step 6 | `<client>/.output-styles/<type-style>.md` |
| 4 | Mode Questionnaire : questions centrées prose fictive, dialogues, POV | Step 3 | Adapter aux styles doc-tech (technique/guide/API) |
| 5 | Mode Amélioration : charge `.docs/comments/chapitre*-personas.md` | Step 7A | `.wip/comments/` |
| 6 | Changelog path : `.docs/output-style-changelog.md` | Step 7D | `<client>/.docs/output-style-changelog.md` ou `.wip/reports/` |
| 7 | `bank.yml update` section : clés `novel`, `rules`, `scenario` | Step 6 | Clés doc-tech |

---

#### `generate-persona.prompt.md` (v2.1) — Paths JdR

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Scope : `<univers>/.templates/personas/` | Section Scope | `<client>/.templates/personas/` |
| 2 | Scope : `<univers>/<projet>/.templates/personas/` | Section Scope | `<client>/<projet>/.templates/personas/` ou `.templates/personas/` |
| 3 | Template YAML : `<univers>_knowledge`, `wot_knowledge` | Template | Adapter en `<client>_knowledge` ou supprimer |
| 4 | `criteria_rulebook` / `criteria_scenario` dans template | Template | Pour doc-tech : `criteria_technical` / `criteria_guide` si nécessaire |
| 5 | Exemples : `casual-reader`, `fan-wot`, `beta-lecteur-cible` — tous JdR | Examples | Ajouter exemples doc-tech (`technical-reviewer`, `clarity-expert`) |
| 6 | Save path affiché : `<univers>/.templates/personas/` | Output section | `<client>/.templates/personas/` |

---

#### `persona-trainer.prompt.md` — Paths JdR + mauvais chemins WIP

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Context load : `@<univers>/.templates/personas/$ARGUMENTS[0].yml` | Context | `@<client>/.templates/personas/$ARGUMENTS[0].yml` |
| 2 | Feedback sources : `@.docs/comments/*.md` | Context | `@.wip/comments/*.md` |
| 3 | Step 2.1 : `@.docs/comments/*.md` | Step 2.1 | `@.wip/comments/*.md` |
| 4 | Output : `<univers>/.templates/personas/` | Output section | `<client>/.templates/personas/` |
| 5 | Génère `.docs/personas-changelog.md` | Step 4.2 | `.wip/reports/personas-changelog.md` |
| 6 | Step 5.1 : `--personas "<persona-id>-v2"` — syntaxe non documentée dans comment | Step 5.1 | Vérifier si `comment.prompt.md` supporte `--personas` |
| 7 | Exemples : `olivier-larue`, `mj-medfan` — JdR | Examples | Ajouter exemples doc-tech |

---

#### `research.prompt.md` — Context JdR

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Charge `@<univers>/.docs/UNIVERS.md` | Context | Supprimer ou remplacer par `@<client>/.docs/CLIENT.md` |
| 2 | Charge `@<univers>/.docs/terminologie.md` | Context | `@<client>/.docs/glossaire.md` |
| 3 | Charge `@<univers>/.docs/lieux.md` | Context | N/A en doc-tech |
| 4 | Charge `@<univers>/.docs/personnages.md` | Context | N/A en doc-tech |
| 5 | Transition → `workshop/evaluate.prompt.md` | Transition | → `brainstorm` ou `write-technical` selon cas |

---

#### `write-technical.prompt.md` (v1.1) — Chemin override incorrect

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | `@.claude/output-style.md` comme project override | Context Resources | `@.output-styles/<nom-projet>.md` (convention init-project v1.2) |
| 2 | Step 5 nommé "Brainstorm & Refine" | Step 5 | Renommer "Affinage & Vérification" (brainstorm = prompt distinct) |
| 3 | Transition : `workshop/review-technical.prompt.md` (chemin court) | Transition | `@docs/prompts/workshop/review-technical.prompt.md` ou style cohérent |

---

#### `write-user-guide.prompt.md` (v1.0) — Incohérences vs write-technical

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | `@.claude/output-style.md` comme project override | Context Resources | `@.output-styles/<nom-projet>.md` |
| 2 | Ne charge PAS le toc-chapter en PREMIER | Step 1 | Charger `.toc/toc-chapter<XX>.md` en premier pour extraire output-style (comme write-technical) |
| 3 | Pas de Step 1 confirmation explicite des ressources chargées | Step 1 | Ajouter confirmation (comme write-technical) |
| 4 | Transition : `workshop/comment.prompt.md` (chemin court) | Transition | Cohérent avec style des autres prompts |

---

#### `review-technical.prompt.md` (v1.0) — Système de score incohérent

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | `@.claude/output-style.md` comme project override | Context | `@.output-styles/<nom-projet>.md` |
| 2 | Scoring A/B/C/D/F (lettres) | Step 8 | `/20` ou rester en % — mais définir la passerelle avec comment.prompt.md |
| 3 | Critère "Immersion" dans Clarity & Accessibility Review | Step 4 | Renommer en "Fluidité de lecture" ou supprimer |
| 4 | Rapport non sauvegardé explicitement dans `.wip/reports/` | Step 10 + Output | Spécifier : `Save to .wip/reports/chapitre<XX>-review.md` |
| 5 | Routing on Approval : "Route to: workshop/doctor.prompt.md" | Transition | Ajouter étape intermédiaire `comment.prompt.md` (pipeline README) |

---

#### `write-toc-chapter.prompt.md` (v1.4) — Chemin feedback incorrect

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Context : `@.docs/comments/<chapitre>-personas.md` | Context (--feedback) | `@.wip/comments/<chapitre>-personas.md` |
| 2 | Quality Checklist : "Personnages vérifiés dans documentation univers" | Checklist | "Termes vérifiés dans glossaire.md" |
| 3 | Quality Checklist : "Lieux vérifiés dans documentation univers" | Checklist | Supprimer (N/A en doc-tech) |
| 4 | Quality Checklist : "Terminologie conforme à terminologie.md" | Checklist | "Terminologie conforme à glossaire.md" |
| 5 | Template toc-chapter : section "Personnages" avec tableau `[perso]/[action]` | Template Step 5 | Remplacer par "Composants / Acteurs" selon type |
| 6 | Template toc-chapter : section "Lieux" | Template Step 5 | Supprimer ou remplacer par "Systèmes / Interfaces" pour user-guide |

---

#### `doctor.prompt.md` (v1.5) — Résidus JdR mineurs

| # | Issue | Localisation | Attendu |
|---|-------|-------------|---------|
| 1 | Rules : "NEVER invent content not supported by TOC, terminologie, or UNIVERS.md" | Rules | `glossaire.md` au lieu de `terminologie` + `CLIENT.md` au lieu de `UNIVERS.md` |
| 2 | Apply step : "Terminologie per terminologie.md (strict)" | Step 2 Apply | `glossaire.md` |
| 3 | Exemple correction : `*saidin* écrit *saidar*` | Output Format | Exemple WoT — remplacer par exemple doc-tech |
| 4 | Step 1.5 : "conseils MJ génériques" dans cross-chapter scan | Step 1.5 | Supprimer |

---

### 🟢 OPTIONNEL

---

#### `univers-extract.prompt.md` — Legacy JdR ($1/$2)

| # | Issue | Attendu |
|---|-------|---------|
| 1 | Utilise `$1` et `$2` (non `$ARGUMENTS`) | `$ARGUMENTS` |
| 2 | Entièrement JdR — intentionnel ? | Peut rester si usage legacy actif |

**Note :** Ce prompt est absent du README workshop v2.0 mais toujours présent sur disque. S'il est encore utilisé pour les projets JdR actifs (`archipels`, `wot`, `ecryme`, `demonslayer`), il peut rester tel quel.

---

#### `extract.prompt.md` / `extract-chunk.prompt.md` / `extract-distribute.prompt.md` / `extract-debug.prompt.md`

| # | Issue | Attendu |
|---|-------|---------|
| 1 | Distribution vers `<univers>/.docs/UNIVERS.md`, `terminologie.md`, `rules-files/` | Distribution doc-tech : `<client>/.docs/CLIENT.md`, `glossaire.md` |
| 2 | Catégorie "Lore" dans classification | Remplacer par "Context" |
| 3 | Catégorie "Rules" dans classification | Remplacer par "Requirements" ou "Specifications" |
| 4 | `extract-distribute` : références `<univers>` dans chemins git | `<client>` |

**Note :** Ces prompts ont une architecture générique (split PDF, chunks) mais les destinations de distribution sont JdR-only. Si utilisés en doc-tech, la distribution Phase C est à reconfigurer.

---

#### `tabula-rasa.prompt.md`

| # | Issue | Attendu |
|---|-------|---------|
| 1 | Scanne `$ARGUMENTS/chapitres/*.tex` | Supprimer `.tex` (LaTeX obsolète) |

---

## Section 2 — Patterns Systémiques

### Pattern A : `<univers>` au lieu de `<client>` (8 fichiers)

Présent dans : `comment.prompt.md`, `tone-finder.prompt.md`, `generate-persona.prompt.md`, `persona-trainer.prompt.md`, `research.prompt.md`, `extract*.prompt.md`, `evaluate.prompt.md`, `write-toc.prompt.md`

**Cause racine :** Migration doc-tech incomplète. Les prompts créés/modifiés après le v6.0 (init-project, brainstorm, write-technical, write-user-guide) utilisent `<client>`. Les anciens prompts non upgradés gardent `<univers>`.

### Pattern B : `.docs/comments/` au lieu de `.wip/comments/` (4 fichiers)

Présent dans : `comment.prompt.md` (section dépréciée), `persona-trainer.prompt.md`, `tone-finder.prompt.md` (Mode Amélioration), `write-toc-chapter.prompt.md` (frontmatter feedback)

**Cause racine :** Chemin migré dans comment v4.2 (2026-02-14) mais non propagé vers les prompts qui lisent ces fichiers.

### Pattern C : `.claude/output-style.md` (3 fichiers)

Présent dans : `write-technical.prompt.md`, `write-user-guide.prompt.md`, `review-technical.prompt.md`

**Cause racine :** Convention obsolète — remplacée par `.output-styles/<nom-projet>.md` dans init-project v1.2 (2026-03-01).

### Pattern D : Types JdR résiduels dans auto-selection (3 fichiers)

Présent dans : `generate-toc.prompt.md`, `comment.prompt.md`, `evaluate.prompt.md`

**Cause racine :** Table auto-selection jamais mise à jour.

### Pattern E : Scoring incohérent `/20` vs `A-F`

Présent dans : `review-technical.prompt.md` (A-F) vs `comment.prompt.md` (/20)

**Impact :** Ambigu pour le pipeline `review-technical → comment`. Quel seuil de passage ?

---

## Section 3 — Priorités de Correction

### Sprint 1 — Bloquant (correction obligatoire avant usage doc-tech)

| Priorité | Fichier | Action |
|----------|---------|--------|
| P1 | `generate-toc.prompt.md` | Upgrade complet : types doc-tech, sources CLIENT.md/glossaire.md, output INDEX.md sans JdR |
| P2 | `add-text.prompt.md` | Supprimer code Ecryme hardcodé ou déplacer vers `ecryme/.claude/` |
| P3 | `evaluate.prompt.md` | Supprimer (remplacé par `init-project` + `brainstorm`) |
| P4 | `write-toc.prompt.md` | Supprimer (doublon obsolète de `generate-toc`) |

### Sprint 2 — Important (correction pour cohérence pipeline)

| Priorité | Fichier | Action |
|----------|---------|--------|
| P5 | `comment.prompt.md` | Remplacer `<univers>` par `<client>`, mettre à jour auto-selection, `.wip/comments/` dans section dépréciée |
| P6 | `tone-finder.prompt.md` | Mettre à jour `<univers>` → `<client>`, `.docs/comments/` → `.wip/comments/`, types |
| P7 | `write-toc-chapter.prompt.md` | `.docs/comments/` → `.wip/comments/`, terminologie.md → glossaire.md, nettoyage template |
| P8 | `write-technical.prompt.md` | `.claude/output-style.md` → `.output-styles/<nom-projet>.md` |
| P9 | `write-user-guide.prompt.md` | `.claude/output-style.md` → `.output-styles/<nom-projet>.md` + load order |
| P10 | `review-technical.prompt.md` | `.claude/output-style.md`, scoring A-F → /20 ou définir passerelle, `.wip/reports/` |
| P11 | `doctor.prompt.md` | `terminologie.md` → `glossaire.md`, `UNIVERS.md` → `CLIENT.md`, exemple WoT |
| P12 | `generate-persona.prompt.md` | `<univers>` → `<client>` dans paths et examples |
| P13 | `persona-trainer.prompt.md` | `<univers>` → `<client>`, `.docs/comments/` → `.wip/comments/` |
| P14 | `research.prompt.md` | Context : `CLIENT.md/glossaire.md` + transition vers pipeline doc-tech |

### Sprint 3 — Optionnel

| Priorité | Fichier | Action |
|----------|---------|--------|
| P15 | `univers-extract.prompt.md` | `$1/$2` → `$ARGUMENTS` (si encore actif) |
| P16 | `extract*.prompt.md` | Distribution Phase C : ajouter variante doc-tech |
| P17 | `tabula-rasa.prompt.md` | Supprimer `*.tex` du scan |

---

## Vérification : Fichier `docs/templates/comment-output-template.md`

Le prompt `comment.prompt.md` charge `@docs/templates/comment-output-template.md`.  
**À vérifier** : ce fichier existe-t-il sur disque ?

```bash
ls docs/templates/comment-output-template.md
```

S'il est absent → le prompt `comment` ne peut pas fonctionner correctement.

---

## Résumé

| Catégorie | Nombre |
|-----------|--------|
| Prompts BLOQUANTS (non utilisables en doc-tech) | 4 |
| Prompts IMPORTANTS (pipeline affecté) | 10 |
| Prompts OPTIONNELS (legacy acceptable) | 3 |
| Prompts CLEAN | 8 |

**Prompts CLEAN** : `brainstorm`, `init-project`, `write-toc-chapter` (partiellement), `write-technical` (partiellement), `comment` (partiellement), `doctor` (partiellement), `client-extract`, `upgrade`

---

**Généré par :** `@docs/prompts/workshop/coherence-checker` (adapté)  
**Prochain sprint :** `@docs/prompts/workshop/upgrade.prompt.md` sur `generate-toc.prompt.md` en priorité
