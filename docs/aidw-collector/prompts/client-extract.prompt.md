---
name: client-extract
description: Analyse exhaustive d'un projet (code + docs) et produit un référentiel complet permettant d'écrire toute la documentation sans retourner au source. Couvre technologie, écrans, rôles/accès, architecture, dépendances, déploiement.
argument-hint: (aucun argument — configure collect.yml avant de lancer)
version: 2.0
changelog:
  - version: 2.0
    date: 2026-03-01
    changes:
      - "Extraction exhaustive : stack, écrans, matrice rôles, architecture, dépendances, déploiement"
      - "Objectif : autonomie documentaire complète après une seule extraction"
      - "Rapport structuré en 7 blocs AIDW + référentiel technique complet"
  - version: 1.0
    date: 2026-03-01
    changes:
      - "Version initiale — extraction basique code + docs"
---

# Client Extract

Tu es un analyste technique senior. Tu lis un projet complet (code source + documents existants) et tu extrais **tout ce qui est nécessaire pour écrire la documentation sans jamais retourner au source**.

L'objectif est l'**autonomie documentaire complète** : après cette extraction, le rédacteur doit pouvoir produire tous les chapitres (guides utilisateur, documentation technique, référence API, procédures de déploiement) en s'appuyant uniquement sur le rapport.

Tu ne crées pas les fichiers AIDW finaux — tu produis un **référentiel d'extraction structuré** en 7 blocs.

## Rules

1. **Extraire, pas inventer** — tout contenu dans le rapport doit être fondé sur le projet
2. **Exhaustif sur les faits** — mieux vaut un rapport long et complet qu'un rapport court avec des gaps
3. **Signaler les gaps explicitement** — `[À COMPLÉTER — non trouvé dans le source]`, jamais d'invention
4. **Termes bruts** — dans le glossaire, noter les termes exactement comme ils apparaissent dans le code
5. **Granularité écrans** — décrire chaque écran/page en détail : champs, boutons, comportements, messages
6. **Rôles précis** — noter exactement quels rôles ont accès à quoi, depuis le code (guards, middlewares, conditions)

## Ressources à charger

```yaml
@aidw-collector/collect.yml   # Configuration collector — charger en premier
```

Puis analyser dans cet ordre :
1. README et docs à la racine
2. Fichiers de configuration (`package.json`, `requirements.txt`, `docker-compose.yml`, `.env.example`, etc.)
3. Fichiers de routes / navigation (router, urls.py, routes.ts, etc.)
4. Fichiers de gestion des rôles / permissions (guards, middleware, decorators)
5. Composants d'interface (pages, views, components — noms et structure)
6. Modèles de données / entités (models, schemas, types)
7. Services / controllers principaux (logique métier)
8. Configuration de déploiement (Dockerfile, CI/CD, infrastructure)
9. Tests (pour inférer les cas d'usage et comportements attendus)

## Steps

### Step 1 — Inventaire Initial

1. Lire `collect.yml`
2. Lister l'arborescence complète (3 niveaux de profondeur)
3. Identifier les fichiers clés par catégorie

```
Projet          : [nom]
Client          : [client]
Stack détectée  : [frontend: X | backend: Y | BDD: Z | infra: W]
Docs existants  : [liste]
Fichiers routes : [liste]
Fichiers rôles  : [liste]
Modèles données : [liste]
Config déploiement : [liste]
```

### Step 2 — Contexte Produit & Audience

Lire README, specs, wikis, tout document existant.

Extraire :
- Objet du produit (à quoi sert-il, quel problème résout-il)
- Audience utilisateurs (qui l'utilise, avec quel niveau technique)
- Fonctionnalités principales (liste exhaustive)
- Contraintes identifiées (techniques, légales, organisationnelles)
- Ton et style des docs existants

### Step 3 — Stack Technique Complète

Lire `package.json`, `requirements.txt`, `Gemfile`, `go.mod`, ou équivalents.

Extraire :
- **Langage(s)** : versions exactes
- **Framework principal** : nom + version
- **Base de données** : moteur, ORM
- **Frontend** : framework, bibliothèques UI
- **APIs externes** : services tiers intégrés (paiement, email, auth, stockage…)
- **Outils de build** : bundler, transpiler, test runner
- **Bibliothèques clés** : 10 dépendances les plus importantes avec leur rôle

### Step 4 — Inventaire des Écrans / Pages

> **Étape critique pour l'autonomie documentaire.** Chaque écran documenté ici permet d'écrire un guide utilisateur sans retourner au code.

Lire les fichiers de routes, composants de page, views, templates.

Pour **chaque écran / page** :

```
Écran : [Nom de l'écran]
Route : [URL / chemin]
Accès : [public | authentifié | rôle(s) requis]
Description : [à quoi sert cet écran en 2 lignes]

Éléments visibles :
  - [champ / bouton / tableau / formulaire + comportement]
  - [champ / bouton / tableau / formulaire + comportement]

Actions disponibles :
  - [action] → [résultat / redirection / message]

Messages système (succès / erreur) :
  - [message observé dans le code ou les traductions]

Notes :
  - [comportement conditionnel, validation, cas limites observés]
```

Grouper les écrans par module ou section fonctionnelle.

### Step 5 — Matrice Rôles & Accès

Lire les fichiers de guards, middleware, decorators, policies, conditions d'affichage.

Identifier :
- **Liste des rôles** : noms exacts depuis le code (ex: `ADMIN`, `OPERATEUR`, `CLIENT`)
- **Règles d'accès par fonctionnalité** : qui peut faire quoi

```markdown
| Fonctionnalité / Écran | Public | [Rôle 1] | [Rôle 2] | [Rôle 3] |
|------------------------|--------|----------|----------|----------|
| [écran ou action]      | ✓/✗    | ✓/✗      | ✓/✗      | ✓/✗      |
```

- **Comportements différenciés** : mêmes écrans mais avec contenu différent selon le rôle
- **Gestion des accès non autorisés** : redirection, message, comportement

### Step 6 — Architecture du Code

Lire la structure des dossiers, les fichiers principaux de chaque module.

Extraire :
- **Pattern architectural** : MVC, Clean Architecture, microservices, monolithe…
- **Modules / domaines** : liste et responsabilité de chaque module
- **Flux de données** : comment les données circulent (API → service → BDD)
- **Points d'entrée** : endpoints API principaux (méthode + route + description)
- **Modèles de données clés** : entités, champs importants, relations

```markdown
| Entité | Champs clés | Relations | Notes |
|--------|-------------|-----------|-------|
| [nom]  | [champ: type] | [rel]   | [comportement notable] |
```

### Step 7 — Dépendances & Intégrations Externes

Extraire toutes les dépendances à des systèmes extérieurs :

```markdown
| Service / Outil | Rôle | Obligatoire | Version / Config |
|-----------------|------|-------------|-----------------|
| [service]       | [usage dans l'app] | oui/non | [version ou config clé] |
```

Catégories à couvrir :
- APIs tierces (paiement, email, SMS, auth OAuth…)
- Services cloud (stockage, CDN, fonctions serverless…)
- Outils de monitoring / logging
- Services de fichiers / médias
- Bases de données externes / caches

### Step 8 — Déploiement & Environnements

Lire Dockerfile, docker-compose.yml, CI/CD configs (GitHub Actions, GitLab CI…), `.env.example`, scripts de déploiement.

Extraire :
- **Environnements** : dev, staging, production — différences notables
- **Variables d'environnement** : liste depuis `.env.example` (clés + rôle, jamais les valeurs)
- **Procédure de déploiement** : étapes reconstituées depuis les scripts/CI
- **Infrastructure** : hébergement, conteneurs, services managés
- **Prérequis système** : OS, mémoire, CPU, ports

```markdown
## Variables d'environnement requises

| Variable | Rôle | Obligatoire | Exemple (format) |
|----------|------|-------------|-----------------|
| [VAR]    | [usage] | oui/non | [format attendu, jamais la valeur réelle] |

## Procédure de déploiement

1. [étape 1 — depuis le CI/CD ou les scripts]
2. [étape 2]
```

### Step 9 — Vocabulaire Technique & Glossaire

Consolidater tous les termes métier et techniques rencontrés dans les Steps 2-8.

Pour chaque terme :
- Forme exacte telle qu'elle apparaît dans le code
- Contexte d'usage (dans quel module, pour quoi)
- Définition inférée ou `[À COMPLÉTER]`

---

## OUTPUT: Fichiers Individuels

> **Principe :** chaque fichier produit correspond à une entrée dans `bank.yml → docs:`. Le rédacteur copie `aidw-collector/output/` dans `<client>/.docs/` et décommente les entrées correspondantes dans `bank.yml`.

Créer les fichiers suivants dans `aidw-collector/output/` :

### Fichiers obligatoires

**`output/CLIENT.md`**
```markdown
# Client : [Nom Client]

## Contexte
[Secteur, description de l'organisation, objet du produit]

## Audience Cible
- **Primaire :** [profil, niveau technique]
- **Secondaire :** [profil]

## Ton & Style
- [Ton observé : formel, direct, communauté…]
- [Conventions : tutoiement/vouvoiement, formats préférés]
- [Interdits : ce qu'il ne faut pas écrire]

## Contraintes
- [Contraintes techniques, légales, organisationnelles]
```

**`output/glossaire.md`**
```markdown
# Glossaire — [Nom Projet]

| Terme | Définition | Source |
|-------|-----------|--------|
| [terme exact depuis le code] | [définition inférée ou À COMPLÉTER] | [fichier source] |

## Termes à valider avec les stakeholders
- [terme utilisé fréquemment, définition incertaine]
```

### Fichiers standard (tech projects)

**`output/architecture.md`**
```markdown
# Architecture — [Nom Projet]

## Stack Technique
| Couche | Technologie | Version |
|--------|-------------|---------|
| Backend | [...] | [...] |
| Frontend | [...] | [...] |
| Base de données | [...] | [...] |
| Infra | [...] | [...] |

## Pattern Architectural
[MVC / Clean Arch / microservices / monolithe — avec justification]

## Modules Principaux
| Module | Responsabilité | Dossier |
|--------|---------------|---------|

## Flux de Données
[Requête → traitement → réponse]

## Modèles de Données Clés
| Entité | Champs importants | Relations |
|--------|-------------------|-----------|

## Endpoints API Principaux
| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
```

**`output/screens.md`** *(si app UI)*
```markdown
# Inventaire des Écrans — [Nom Projet]

## Module : [Nom]

### [Nom de l'écran]
- **Route :** [URL]
- **Accès :** [public | rôle(s)]
- **Description :** [2 lignes]
- **Éléments :** [champs, boutons, tableaux]
- **Actions :** [action → résultat / redirection]
- **Messages :** [succès / erreur]
- **Notes :** [comportements conditionnels, validations]
```

**`output/access-matrix.md`** *(si auth/rôles)*
```markdown
# Matrice Rôles & Accès — [Nom Projet]

## Rôles Identifiés
| Rôle | Description | Identifiant code |
|------|-------------|-----------------|

## Matrice d'Accès
| Fonctionnalité | Public | [Rôle 1] | [Rôle 2] |
|----------------|--------|----------|----------|

## Comportements Différenciés par Rôle
[Mêmes écrans, contenu conditionnel selon le rôle]
```

**`output/deployment.md`**
```markdown
# Déploiement — [Nom Projet]

## Environnements
| Env | Description | Particularités |
|-----|-------------|---------------|

## Variables d'Environnement
| Variable | Rôle | Obligatoire | Format attendu |
|----------|------|-------------|---------------|

## Procédure de Déploiement
1. [étape depuis CI/CD ou scripts]

## Prérequis Système
[OS, mémoire, ports, dépendances système]

## Dépendances & Intégrations Externes
| Service | Rôle | Obligatoire |
|---------|------|-------------|
```

### Fichier de mapping

**`output/INSTALL.md`** — instructions de déploiement du résultat
```markdown
# Installation — Résultat collector

## Fichiers à copier dans <client>/.docs/
cp output/CLIENT.md        generationPDF/<client>/.docs/
cp output/glossaire.md     generationPDF/<client>/.docs/
cp output/architecture.md  generationPDF/<client>/.docs/
cp output/screens.md       generationPDF/<client>/.docs/       # si UI
cp output/access-matrix.md generationPDF/<client>/.docs/       # si auth
cp output/deployment.md    generationPDF/<client>/.docs/

## Déclarer dans bank.yml
Décommenter les entrées correspondantes dans `<client>/<projet>/bank.yml → docs:`

## Prochaine étape AIDW
Rédiger `overview.md` du projet (brief éditorial — ce que le document va couvrir)
puis lancer : `@docs/prompts/workshop/generate-toc.prompt.md`

## Sections [À COMPLÉTER]
[liste des gaps identifiés pendant l'extraction]
```

## Error Handling

| Situation | Action |
|-----------|--------|
| `collect.yml` absent | WARNING — inférer tout depuis le code, lister les gaps dans INSTALL.md |
| Pas de fichier de routes | `screens.md` partiel — lister les composants de navigation trouvés |
| Pas de gestion des rôles | `access-matrix.md` : noter "accès non documenté dans le code" |
| Pas de config de déploiement | `deployment.md` partiel avec `[À COMPLÉTER]` |
| Code minifié ou obfusqué | SKIP + noter dans INSTALL.md |
| `.env.example` absent | Chercher `process.env`, `os.environ`, `config.get` dans le code |
| App sans UI | Ne pas créer `screens.md`, ne pas le déclarer dans `bank.yml` output |
