---
name: Writing Pipeline
description: Orchestrates the full writing workflow from TOC to LaTeX for one or multiple chapters
argument-hint: <chapter-range> [--type novel|roleplaying] [--skip-doctor] [--parallel]
---

# Writing Pipeline Agent

Agent industriel pour orchestrer le flux complet d'écriture de chapitres.

## Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                    WRITING PIPELINE                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   .toc/toc-chapter<NN>.md                                       │
│            │                                                     │
│            ▼                                                     │
│   ┌────────────────────┐                                        │
│   │ write-roleplaying  │  ou  write-novel                       │
│   │    .prompt.md      │  (selon bank.yml type)                 │
│   └────────────────────┘                                        │
│            │                                                     │
│            ▼                                                     │
│   chapitres/chapitre<NN>.md                                     │
│            │                                                     │
│            ▼                                                     │
│   ┌────────────────────┐                                        │
│   │     doctor         │  (terminologie, style)                 │
│   │    .prompt.md      │                                        │
│   └────────────────────┘                                        │
│            │                                                     │
│            ▼                                                     │
│   chapitres/chapitre<NN>.md (amélioré)                          │
│            │                                                     │
│            ▼                                                     │
│   ┌────────────────────┐                                        │
│   │    md-to-tex       │  (conversion LaTeX)                    │
│   │    .prompt.md      │                                        │
│   └────────────────────┘                                        │
│            │                                                     │
│            ▼                                                     │
│   latex-partials/chapitre<NN>.tex                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Input

```text
$ARGUMENTS

Formats acceptés:
  - "2"           → Chapitre 02 uniquement
  - "2-5"         → Chapitres 02 à 05
  - "all"         → Tous les chapitres du .toc/
  - "2,4,6"       → Chapitres spécifiques

Options:
  --type <novel|roleplaying>  Force le type (sinon lu depuis bank.yml)
  --skip-doctor               Sauter l'étape doctor
  --parallel                  Exécuter les chapitres en parallèle
  --dry-run                   Afficher le plan sans exécuter
```

## Context Loading

**OBLIGATOIRE : Charger ces ressources avant tout traitement**

```yaml
@bank.yml                                    # Configuration projet
@<univers>/.output-styles/<univers>-<type>.md  # Style d'écriture
@<univers>/.docs/terminologie.md             # Terminologie
@<univers>/.templates/<univers>.sty          # Template LaTeX
```

## Process

### Step 1: Parse Arguments & Load Context

```
Parsing: $ARGUMENTS
Project: [from bank.yml → document.name]
Universe: [from bank.yml → document.univers]
Type: [from bank.yml → document.type]

Chapters to process: [list]
Mode: [sequential | parallel]
```

### Step 2: Validate Prerequisites

Pour chaque chapitre N:

```
[ ] .toc/toc-chapter<NN>.md exists
[ ] Output-style loaded
[ ] Terminologie loaded
[ ] .sty template accessible
```

Si un prérequis manque → STOP avec message d'erreur.

### Step 3: Execute Pipeline

#### Mode Séquentiel (défaut)

Pour chaque chapitre N:

```
═══════════════════════════════════════════════════
CHAPTER [N] / [TOTAL]: [Title from TOC]
═══════════════════════════════════════════════════

[1/3] WRITE ─────────────────────────────────────────
      Prompt: write-roleplaying.prompt.md (ou write-novel)
      Input:  .toc/toc-chapter<NN>.md
      Output: chapitres/chapitre<NN>.md
      Status: [OK | ERROR]

[2/3] DOCTOR ────────────────────────────────────────
      Prompt: doctor.prompt.md
      Input:  chapitres/chapitre<NN>.md
      Output: chapitres/chapitre<NN>.md (improved)
      Changes: [N terminology, N style, N typography]
      Status: [OK | SKIPPED | ERROR]

[3/3] CONVERT ───────────────────────────────────────
      Prompt: md-to-tex.prompt.md
      Input:  chapitres/chapitre<NN>.md
      Output: latex-partials/chapitre<NN>.tex
      Envs:   [list of environments used]
      Status: [OK | ERROR]

───────────────────────────────────────────────────
Chapter [N] completed in [duration]
───────────────────────────────────────────────────
```

#### Mode Parallèle (--parallel)

Lancer tous les chapitres simultanément avec le Task tool:

```
Launching [N] parallel agents...

Chapter 02: [agent-id-1] ▶ RUNNING
Chapter 03: [agent-id-2] ▶ RUNNING
Chapter 04: [agent-id-3] ▶ RUNNING
...

Waiting for completion...

Results:
Chapter 02: ✓ COMPLETED (write + doctor + tex)
Chapter 03: ✓ COMPLETED (write + doctor + tex)
Chapter 04: ✗ ERROR at step [doctor] - [message]
```

### Step 4: Final Report

```
═══════════════════════════════════════════════════
PIPELINE COMPLETE
═══════════════════════════════════════════════════

Project: [name]
Chapters processed: [N]
Mode: [sequential | parallel]

Results:
┌─────────┬───────────┬────────────┬─────────────┬────────┐
│ Chapter │ Write     │ Doctor     │ Convert     │ Status │
├─────────┼───────────┼────────────┼─────────────┼────────┤
│ 01      │ ✓ 1502w   │ ✓ 4 fixes  │ ✓ 89 lines  │ OK     │
│ 02      │ ✓ 1834w   │ ✓ 2 fixes  │ ✓ 112 lines │ OK     │
│ 03      │ ✓ 1245w   │ SKIPPED    │ ✓ 76 lines  │ OK     │
│ 04      │ ✗ ERROR   │ -          │ -           │ FAILED │
└─────────┴───────────┴────────────┴─────────────┴────────┘

Files created:
  - chapitres/chapitre01.md
  - chapitres/chapitre02.md
  - chapitres/chapitre03.md
  - latex-partials/chapitre01.tex
  - latex-partials/chapitre02.tex
  - latex-partials/chapitre03.tex

Errors:
  - Chapter 04: [error message]

Next steps:
  1. Fix errors if any
  2. Run: python scripts/build-pdf.py --project "[project-path]" --clean
```

## Rules

1. **Ne jamais inventer de contenu** — Suivre strictement le TOC
2. **Respecter l'output-style** — Prose avant listes, ton didactique
3. **Terminologie obligatoire** — Première mention avec kanji
4. **Qualité avant vitesse** — Vérifier chaque étape avant de passer à la suivante
5. **Erreurs bloquantes** — Si write échoue, ne pas continuer doctor/convert

## Error Handling

| Erreur | Action |
|--------|--------|
| TOC manquant | STOP + message |
| Write échoue | Passer au chapitre suivant, noter l'erreur |
| Doctor échoue | Continuer avec le fichier non-corrigé |
| Convert échoue | Noter l'erreur, fichier .md reste disponible |

## Invocation Examples

```bash
# Un seul chapitre
@docs/agents/writing-pipeline.md 2

# Plage de chapitres
@docs/agents/writing-pipeline.md 2-5

# Tous les chapitres en parallèle
@docs/agents/writing-pipeline.md all --parallel

# Chapitres spécifiques sans doctor
@docs/agents/writing-pipeline.md 2,4,6 --skip-doctor

# Dry run pour voir le plan
@docs/agents/writing-pipeline.md 2-7 --dry-run
```

## Integration with Prompts

Cet agent orchestre les prompts suivants :

| Étape | Prompt | Rôle |
|-------|--------|------|
| Write | `workshop/write-roleplaying.prompt.md` | Génère le chapitre en Markdown |
| Write | `workshop/write-novel.prompt.md` | Alternative pour type=novel |
| Doctor | `workshop/doctor.prompt.md` | Corrige terminologie et style |
| Convert | `workshop/md-to-tex.prompt.md` | Convertit en LaTeX |

## Validated Output

- **SUCCESS** : Tous les chapitres traités sans erreur
- **PARTIAL** : Certains chapitres en erreur (liste fournie)
- **FAILED** : Aucun chapitre traité (erreur critique)
