# Workshop: Writing Prompts System

Système de prompts interconnectés pour l'écriture de documents narratifs et JdR.

## Vue d'Ensemble

```
                    ┌─────────────┐
                    │  research   │
                    └──────┬──────┘
                           │
                           ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  write-toc  │────▶│  evaluate   │────▶│ write-novel │
└─────────────┘     └─────────────┘     └──────┬──────┘
                           │                    │
                           │                    ▼
                           │            ┌──────────────────┐
                           │            │  review-chapter  │
                           │            └────────┬─────────┘
                           │                     │
                           ▼                     ▼
                    ┌─────────────────┐   ┌─────────────┐
                    │write-roleplaying│   │   doctor    │
                    └────────┬────────┘   └─────────────┘
                             │                   ▲
                             ▼                   │
                    ┌─────────────────┐          │
                    │ review-roleplay │──────────┘
                    └─────────────────┘
```

## Flux de Travail

### Flux Principal (Sérialisé)

1. **research** → Recherche et documentation
2. **evaluate** → Évaluation du projet
3. **write-toc** → Création du sommaire
4. **write-novel** OU **write-roleplaying** → Rédaction
5. **review-chapter** OU **review-roleplay** → Revue
6. **doctor** → Analyse persona

### Flux Extraction (Multi-Session)

Pour importer du contenu depuis un PDF source :

```
extract (Session 1)     → Setup + Split PDF
    ↓
extract-chunk (×N)      → 1 chunk par session
    ↓
extract-distribute      → Fusion + Distribution + Cleanup
```

Stockage intermédiaire : `docs/extraction/<source-name>/`

### Flux Alternatifs

| Point d'Entrée | Cas d'Usage |
|----------------|-------------|
| `research` | Nouveau sujet à documenter |
| `evaluate` | Projet existant à analyser |
| `write-toc` | Structure à créer |
| `write-novel` | Chapitre spécifique à rédiger |
| `review-*` | Texte existant à réviser |
| `doctor` | Analyse qualitative seule |

## Prompts Disponibles

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `evaluate` | Évalue un projet, détermine le type | bank.yml | Route vers write-* |
| `write-toc` | Crée le sommaire détaillé | Brief projet | toc.md |
| `write-novel` | Écrit du contenu narratif | Numéro chapitre | chapitre.tex |
| `write-roleplaying` | Écrit du contenu JdR | Numéro chapitre | chapitre.tex |
| `research` | Recherche croisée web | Sujet | Rapport recherche |
| `review-chapter` | Vérifie conformité narrative | chapitre.tex | Rapport review |
| `review-roleplay` | Vérifie conformité JdR + règles | chapitre.tex | Rapport review |
| `doctor` | Analyse par personas | chapitre.tex | Rapport persona |
| `generate-persona` | Crée définition persona | Description lecteur | persona.yml |
| `extract` | Setup extraction PDF (Session 1) | projet + PDF | progress.md + chunks |
| `extract-chunk` | Extrait 1 chunk (Sessions 2-N) | progress.md | classified/*.md |
| `extract-distribute` | Fusionne et distribue (Final) | progress.md | Fichiers univers |
| `extract-debug` | Diagnostique les problèmes | progress.md | Rapport debug |

## Ressources Requises

Chaque prompt charge ses ressources depuis `bank.yml` :

| Ressource | Localisation | Utilisée Par |
|-----------|--------------|--------------|
| output-style | `<univers>/.output-styles/` | Tous |
| docs | `<univers>/.docs/` | Tous |
| toc | `toc.md` (projet) | write-*, review-* |
| templates | `<univers>/.templates/` | write-* |
| rules-files | `docs/rules-files/` | write-roleplaying, review-roleplay |
| personas | `docs/templates/personas/` ou `<univers>/.templates/personas/` | doctor, comment |

## Configuration: bank.yml

Chaque projet doit avoir un `bank.yml` à sa racine :

```yaml
document:
  name: "Mon Projet"
  univers: "archipels"
  type: "novel"  # ou "roleplaying"

output-style:
  global: "archipels/.output-styles/latex-archipels.md"

docs:
  univers: "archipels/.docs/UNIVERS.md"
  terminologie: "archipels/.docs/terminologie.md"

toc:
  fichier: "toc.md"

templates:
  package: "archipels/.templates/archipels.sty"
```

Voir `docs/templates/bank.yml` pour le template complet.

## Transitions Entre Prompts

### evaluate → write-*

```
Type détecté = novel → write-novel
Type détecté = roleplaying/scenario → write-roleplaying
```

### research → evaluate

```
Rapport recherche généré → Re-évaluation avec nouvelles infos
```

### write-* → review-*

```
write-novel → review-chapter
write-roleplaying → review-roleplay
```

### review-* → doctor

```
Rating A ou B → doctor
Rating C → Fixes puis re-review
Rating D/F → Re-write
```

### write-toc → write-*

```
TOC approuvé + type novel → write-novel
TOC approuvé + type roleplaying → write-roleplaying
```

## Utilisation

### Démarrer un Nouveau Projet

```bash
# 1. Créer bank.yml
cp docs/templates/bank.yml <projet>/bank.yml
# Éditer avec les valeurs du projet

# 2. Évaluer le projet
@docs/prompts/workshop/evaluate.prompt.md <projet>

# 3. Créer le sommaire
@docs/prompts/workshop/write-toc.prompt.md <projet>

# 4. Rédiger les chapitres
@docs/prompts/workshop/write-novel.prompt.md 1
@docs/prompts/workshop/write-novel.prompt.md 2
# etc.
```

### Réviser un Chapitre Existant

```bash
# Review technique
@docs/prompts/workshop/review-chapter.prompt.md chapitre03.tex

# Analyse persona
@docs/prompts/workshop/doctor.prompt.md chapitre03.tex
```

### Recherche Documentaire

```bash
# Rechercher un sujet
@docs/prompts/workshop/research.prompt.md "histoire des navigateurs archipels"

# Intégrer les résultats
@docs/prompts/workshop/evaluate.prompt.md <projet>
```

## Personas (pour doctor.prompt.md)

Hiérarchie de chargement :
1. `<projet>/.templates/personas/` (surcharge projet)
2. `<univers>/.templates/personas/` (défaut univers)
3. `docs/templates/personas/` (défaut global)

| Persona | Pour | Focus |
|---------|------|-------|
| `casual-reader` | Roman | Divertissement, fluidité |
| `lore-enthusiast` | Roman/Guide | Worldbuilding, cohérence |
| `gm-practitioner` | JdR | Utilisabilité, instructions |
| `player-immersive` | JdR | Atmosphère, immersion |
| `editor-critical` | Tous | Qualité, style |
| `speedreader` | Tous | Rythme, efficacité |

Personas spécifiques par univers (exemples) :
- Archipels : `marin-veteran`, `explorateur-curieux`
- WoT : `fan-canonique`, `novice-univers`

## Rapports Générés

| Prompt | Rapport | Localisation |
|--------|---------|--------------|
| research | Rapport recherche | `docs/research/<date>-<sujet>.md` |
| evaluate | Rapport évaluation | Console (non sauvegardé) |
| write-toc | Table des matières | `toc.md` |
| review-* | Rapport review | Console (non sauvegardé) |
| doctor | Rapport persona | Console (non sauvegardé) |

## Comparaison avec AIDD

| AIDD (Code) | Workshop (Écriture) |
|-------------|---------------------|
| elaborate | evaluate |
| plan | write-toc |
| implement | write-novel / write-roleplaying |
| review_functional | review-chapter / review-roleplay |
| - | doctor (nouveau) |
| research | research (adapté) |

## Version

**Version:** 1.0
**Date:** 2025-01-30
**Auteur:** Généré par Claude Code
