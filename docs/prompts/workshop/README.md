# Workshop: Système de Prompts Documentation

Système de prompts interconnectés pour la création de documentation technique multi-clients.

## Pipeline Principal

```
init-project
     │
     ▼
brainstorm ──→ generate-toc
                    │
                    ▼
            write-toc-chapter ──→ comment (relecture TOC)
                    │
                    ▼
         write-technical / write-user-guide
                    │
                    ▼
            review-technical
                    │
                    ▼
                comment (personas)
                    │
                    ├── Patchable ──→ doctor ──→ build-icml.py
                    └── Structurel ──→ write-technical --feedback
```

## Prompts Disponibles

### Initialisation & Configuration

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `init-project` | Audite et initialise l'arborescence d'un projet | `<client>/<projet>` | Structure complète + bank.yml |
| `client-extract` | Extrait sources brutes → fichiers thématiques client | `<client> [options]` | CLIENT.md, glossaire.md, … |
| `tone-finder` | Génère un output-style depuis des sources | `<client> [sources]` | `<client>-<type>.md` |
| `generate-persona` | Crée une définition de persona | Description lecteur | `<id>.yml` |
| `persona-trainer` | Affine une persona depuis des feedbacks | `<id> --feedback-files` | `<id>.yml` mis à jour |

### Conception

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `brainstorm` | Itère sur l'overview jusqu'à generate-toc | `<client>/<projet>` | overview.md enrichi |
| `generate-toc` | Génère l'INDEX.md et la structure chapitres | overview.md | `.toc/INDEX.md` |
| `write-toc-chapter` | Écrit le plan détaillé d'un chapitre | Numéro chapitre | `.toc/toc-chapterNN.md` |

### Rédaction

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `write-technical` | Rédige un chapitre documentation technique | N [--feedback] | `chapitres/chapitreNN.md` |
| `write-user-guide` | Rédige un chapitre guide utilisateur | N [--feedback] | `chapitres/chapitreNN.md` |
| `add-text` | Ajoute du contenu à un chapitre existant | Chapitre + contenu | Chapitre enrichi |

### Review & Qualité

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `review-technical` | Revue technique d'un chapitre (précision, cohérence) | `chapitreNN.md` | Rapport review |
| `comment` | Évaluation par personas (3 lecteurs) | `chapitreNN.md` | `.wip/comments/` |
| `doctor` | Corrections chirurgicales ciblées | `chapitreNN.md` | Chapitre corrigé + changelog |

### Utilitaires

| Prompt | Rôle | Entrée | Sortie |
|--------|------|--------|--------|
| `research` | Recherche documentaire sur un sujet | Sujet entre guillemets | Rapport recherche |
| `upgrade` | Auto-évaluation itérative d'un prompt | Résultat à améliorer | Version améliorée |
| `tabula-rasa` | Repart de zéro sur un projet | `<client>/<projet>` | Projet réinitialisé |

### Extraction PDF (Multi-Session)

Pour importer du contenu depuis un PDF source :

```
extract (Session 1)     → Setup + Split PDF
    │
extract-chunk (× N)     → 1 chunk par session parallèle
    │
extract-distribute      → Fusion + Distribution + Cleanup
    │
extract-debug           → Diagnostique si problème
```

Stockage intermédiaire : `docs/extraction/<source-name>/`

## Ressources Chargées depuis bank.yml

| Ressource | Localisation | Utilisée par |
|-----------|--------------|--------------|
| `output-style` | `<client>/.output-styles/` | Tous les write-* |
| `docs.client` | `<client>/.docs/CLIENT.md` | write-*, review-*, comment |
| `docs.glossaire` | `<client>/.docs/glossaire.md` | write-*, review-*, comment |
| `toc` | `.toc/INDEX.md` | write-*, review-technical |
| `personas` | `<client>/.templates/personas/` ou `docs/templates/personas/` | comment, doctor |

## Configuration : bank.yml

```yaml
document:
  name: "Guide Utilisateur API"
  client: "acme-corp"
  type: "user-guide"  # technical-doc | user-guide | api-doc | process-doc

output-style:
  global: "acme-corp/.output-styles/acme-corp-user-guide.md"

docs:
  client: "acme-corp/.docs/CLIENT.md"
  glossaire: "acme-corp/.docs/glossaire.md"

toc:
  fichier: ".toc/INDEX.md"

personas:
  global:
    - "docs/templates/personas/technical-reviewer.yml"
  client:
    - "acme-corp/.templates/personas/clarity-expert.yml"
  projet: []

overview: "overview.md"

icml:
  chapitres-source: "chapitres/"
  chapitres-order: []
  output: "output/guide-utilisateur-api.icml"
```

Voir `docs/templates/bank.yml` pour le template complet.

## Utilisation

### Nouveau projet

```bash
# Initialiser
@docs/prompts/workshop/init-project.prompt.md acme-corp/mon-projet

# Brainstormer l'overview
@docs/prompts/workshop/brainstorm.prompt.md acme-corp/mon-projet

# Générer la table des matières
@docs/prompts/workshop/generate-toc.prompt.md acme-corp/mon-projet
```

### Pipeline chapitre (industrialisé)

```bash
# 1. Plan détaillé du chapitre
@docs/prompts/workshop/write-toc-chapter.prompt.md 3

# 2. Écriture
@docs/prompts/workshop/write-technical.prompt.md 3

# 3. Revue technique
@docs/prompts/workshop/review-technical.prompt.md chapitre03.md

# 4. Commentaires personas
@docs/prompts/workshop/comment.prompt.md chapitres/chapitre03.md

# 5a. Corrections patchables
@docs/prompts/workshop/doctor.prompt.md chapitre03.md

# 5b. Réécriture si feedback structurel
@docs/prompts/workshop/write-technical.prompt.md 3 --feedback .wip/comments/chapitre03-comment.md
```

### Extraction depuis PDF

```bash
# Extraire des sources client
@docs/prompts/workshop/client-extract.prompt.md acme-corp

# Ou extraction manuelle depuis PDF (multi-session)
@docs/prompts/workshop/extract.prompt.md acme-corp sources/document.pdf
```

## Personas Disponibles

Hiérarchie de chargement :
1. `.templates/personas/` (projet — surcharge)
2. `<client>/.templates/personas/` (client)
3. `docs/templates/personas/` (global)

### Personas Documentation Technique (défaut)

| Persona | Focus | Sévérité |
|---------|-------|----------|
| `technical-reviewer` | Exactitude technique, sécurité, cohérence architecture | Stricte |
| `clarity-expert` | Compréhensibilité, accessibilité, jargon | Modérée |
| `compliance-checker` | Exhaustivité, conformité style, traçabilité TOC | Stricte |

## Rapports Générés

| Prompt | Rapport | Localisation |
|--------|---------|--------------|
| `review-technical` | Rapport review | `.wip/reports/` |
| `comment` | Évaluations personas | `.wip/comments/` |
| `doctor` | Changelog corrections | `.wip/changelog/` |
| `tone-finder` | Rapport style | `.wip/reports/` |
| `research` | Rapport recherche | `docs/research/` |

---

**Version:** 2.0
**Date:** 2026-03-01
