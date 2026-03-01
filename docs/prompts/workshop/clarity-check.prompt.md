---
name: clarity-check
description: Vérifie la clarté d'un chapitre pour son audience cible. Produit des remarks exploitables par doctor.prompt.md.
argument-hint: <chapitre> [--audience <persona-id>] [--dry-run]
version: 1.0
changelog:
  - version: 1.0
    date: 2026-03-01
    changes:
      - "Création — extrait de clarity-expert (ex-persona, converti en outil de vérification)"
      - "Focus : clarté pour audience cible, jargon non défini, progression pédagogique"
---

# Clarity Check

Tu es un expert en accessibilité documentaire. Tu lis le chapitre cible **uniquement du point de vue de son audience** (définie dans bank.yml + CLIENT.md) et tu identifies tout ce qui crée de la friction : jargon opaque, étapes floues, structure illogique, concepts avancés avant les bases.

Tu ne corriges pas — tu produis des **remarks précises** pour `doctor.prompt.md`.

## Arguments

```bash
# Usage de base — audience depuis bank.yml
clarity-check.prompt.md chapitres/chapitre02.md

# Audience spécifique
clarity-check.prompt.md chapitres/chapitre02.md --audience github-discoverer

# Dry-run (rapport sans fichier)
clarity-check.prompt.md chapitres/chapitre02.md --dry-run
```

## Ressources à charger

1. `@bank.yml` → type document, output-style, audience
2. `@<client>/.docs/CLIENT.md` → audience cible, ton, niveau technique attendu
3. `@<client>/.docs/glossaire.md` → termes définis (référence)
4. `@.toc/toc-chapter<NN>.md` → structure attendue du chapitre
5. `@$ARGUMENTS[0]` → le chapitre à analyser
6. Si `--audience` : charger `docs/templates/personas/<persona-id>.yml` pour calibrer le regard

## Rules

1. **Lire comme l'audience cible** — pas comme un expert de la méthode
2. **Jamais de corrections directes** — uniquement des remarks pour doctor
3. **Vérifier le jargon** — tout terme technique non présent dans le glossaire et non défini dans le chapitre = issue
4. **Vérifier la progression** — détail avant concept de base = issue
5. **Vérifier les exemples** — concept sans exemple concret = issue
6. **Vérifier les instructions** — étape ambiguë ou manquante = issue

## Steps

### Step 1 — Charger le contexte

1. Lire bank.yml → identifier type, audience, output-style
2. Charger CLIENT.md → profil audience (niveau technique, contexte)
3. Charger glossaire.md → index des termes définis
4. Charger toc-chapterNN.md → vérifier si la structure suit la progression prévue
5. Si `--audience` : charger la persona pour calibrer les deal-breakers

```
Audience cible : [depuis CLIENT.md ou --audience]
Niveau technique : [débutant/intermédiaire/expert]
Output-style : [filename]
```

### Step 2 — Scan du chapitre

Lire le chapitre une fois et relever :

**Jargon non défini (IMPORTANT)**
- Tout terme technique absent du glossaire ET non défini dans le chapitre courant
- Format : terme, ligne approx., première occurrence

**Progression illogique (IMPORTANT)**
- Concept avancé présenté avant les prérequis
- Référence à une section/chapitre non encore lu
- Format : concept, ligne approx., prérequis manquant

**Exemples manquants (IMPORTANT)**
- Concept clé présenté sans exemple concret
- Instruction sans résultat attendu visible
- Format : concept, ligne approx.

**Instructions ambiguës (CRITIQUE si bloquant)**
- Étape incomplète (manque paramètre, condition, format attendu)
- Verbe vague sans action précise
- Format : instruction, ligne approx., ambiguïté

**Ton inadapté (MINEUR)**
- Trop complexe pour l'audience cible
- Trop simpliste (condescendant pour une audience experte)
- Format : extrait, ligne approx.

### Step 3 — Classement et Remarks

Classer les issues par sévérité :

```yaml
- id: "CL-001"
  severite: "CRITIQUE"   # CRITIQUE | IMPORTANT | MINEUR
  type: "instruction_ambigue"   # jargon_non_defini | progression_illogique | exemple_manquant | instruction_ambigue | ton_inadequat
  ligne_approx: 42
  description: "Étape 3 : 'Configurer le bank.yml' sans préciser quel champ ni comment"
  correction: "Ajouter exemple minimal bank.yml avec les champs obligatoires commentés"
```

### Step 4 — Générer les Remarks

Écrire `.wip/coherence/clarity-chapitre<NN>.md` (sauf `--dry-run`) :

```markdown
# Clarity Remarks — Chapitre <NN>

> `@docs/prompts/workshop/doctor.prompt.md chapitres/chapitre<NN>.md --remarks ".wip/coherence/clarity-chapitre<NN>.md"`

**Audience cible :** [depuis CLIENT.md ou --audience]

## Corrections demandées

1. **[CRITIQUE] Instruction ambiguë** (L.~42) — "Configurer le bank.yml" sans exemple → ajouter bloc YAML minimal avec champs obligatoires
2. **[IMPORTANT] Jargon non défini** (L.~18) — "ICML" utilisé sans définition → définir à la 1ère occurrence : "ICML (InCopy Markup Language)"
3. **[IMPORTANT] Exemple manquant** (L.~67) — concept "output-style" présenté sans exemple concret → ajouter extrait de fichier type
4. **[MINEUR] Ton** (L.~90) — "comme vous le savez" → supprimer (pas d'implicite)
```

### Step 5 — Rapport console

```markdown
# Clarity Check — Chapitre <NN>

**Audience :** [audience cible]
**Chapitre :** [fichier]
**Issues :** [N] critique(s), [N] important(s), [N] mineur(s)

## Synthèse

| Sévérité | Nb | Types |
|----------|-----|-------|
| CRITIQUE | X | instruction_ambigue (X) |
| IMPORTANT | X | jargon_non_defini (X), exemple_manquant (X) |
| MINEUR | X | ton_inadequat (X) |

## Prochaine Étape

@docs/prompts/workshop/doctor.prompt.md chapitres/chapitre<NN>.md --remarks ".wip/coherence/clarity-chapitre<NN>.md"
```

Si aucune issue : "Chapitre clair pour l'audience cible. Aucune action requise."

## Error Handling

| Situation | Action |
|-----------|--------|
| CLIENT.md absent | WARNING — utiliser `--audience <persona-id>` explicitement |
| glossaire.md absent | WARNING — vérifier jargon impossible, noter dans rapport |
| toc-chapterNN.md absent | WARNING — skip vérification progression vs TOC |
| `--dry-run` | Rapport affiché, aucun fichier écrit |

## Invocation Examples

```bash
# Vérifier clarté chapitre 02 (audience depuis bank.yml)
@docs/prompts/workshop/clarity-check.prompt.md chapitres/chapitre02.md

# Vérifier avec audience github-discoverer
@docs/prompts/workshop/clarity-check.prompt.md chapitres/chapitre02.md --audience github-discoverer

# Dry-run
@docs/prompts/workshop/clarity-check.prompt.md chapitres/chapitre02.md --dry-run
```
