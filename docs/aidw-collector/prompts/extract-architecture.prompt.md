---
name: extract-architecture
description: Extrait la stack technique et l'architecture d'un projet et produit architecture.md.
version: 1.1
---

# Extract Architecture

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/architecture.md`.

Utiliser la stack détectée en Step 0 comme point de départ — approfondir depuis les fichiers de configuration et le code source.

## Sources à lire

Dans cet ordre, en notant versions exactes si disponibles :

1. **Manifestes de dépendances** : `package.json`, `requirements.txt`, `pyproject.toml`, `Gemfile`, `go.mod`, `Cargo.toml`
2. **Structure des dossiers** — inférer le pattern architectural (MVC, Clean Arch, microservices…)
3. **Fichiers d'entrée** : `app.py`, `main.ts`, `index.js`, `server.*`
4. **Modèles / entités** : `models/`, `schemas/`, `types/`, `entities/`
5. **Routes / controllers**
6. **Services / logique métier**
7. **Configurations d'intégration** : `.env.example`, fichiers clients API, adapters
8. **Docs d'architecture existantes**

## Output : `aidw-collector/dest/architecture.md`

Les annotations `{ }` indiquent comment remplir chaque section — ne pas les inclure dans la sortie.

```markdown
# Architecture — [Nom Projet]

## Stack Technique

{versions exactes depuis les manifestes — sinon À COMPLÉTER}

| Couche | Technologie | Version |
|--------|-------------|---------|
| Frontend | [techno — sinon À COMPLÉTER] | [version] |
| Backend | [techno — sinon À COMPLÉTER] | [version] |
| Base de données | [techno — sinon À COMPLÉTER] | [version] |
| Build / Tooling | [techno — sinon À COMPLÉTER] | [version] |

## Pattern Architectural

{justifier depuis la structure des dossiers — jamais d'hypothèse ; si non déterminable → À COMPLÉTER}

**Pattern :** [MVC | Clean Architecture | Microservices | Monolithe | JAMstack | À COMPLÉTER]

[2-3 phrases justifiant ce choix depuis la structure observée]

## Modules Principaux

| Module | Responsabilité | Dossier |
|--------|---------------|---------|
| [module] | [rôle — sinon À COMPLÉTER] | [chemin] |

## Flux de Données Principal

{schéma ASCII obligatoire + étapes numérotées — représenter le flux le plus fréquent}

```
[Entrée] → [Couche 1] → [Couche 2] → [Sortie]
                            ↓
                     [Service externe]
```

**Étapes :**
1. [étape 1]
2. [étape 2]

## Modèles de Données Clés

{champs importants = identifiant, champs métier clés, clés étrangères — pas tous les champs}

| Entité | Champs clés | Relations | Notes |
|--------|-------------|-----------|-------|
| [entité] | [id, champs métier, FK] | [→ autre entité] | [comportement notable — sinon À COMPLÉTER] |

## Endpoints API Principaux

{si pas d'API exposée → omettre cette section}
{prioriser : top 10 endpoints par domaine fonctionnel — pas toutes les routes}

| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
| [GET/POST…] | [/route] | [usage — sinon À COMPLÉTER] | oui/non |

## Intégrations Externes

{APIs tierces, queues, stockages, services cloud — depuis .env.example, adapters, clients}

| Service | Type | Usage | Obligatoire |
|---------|------|-------|-------------|
| [service] | [REST API / Queue / Storage / OAuth…] | [rôle — sinon À COMPLÉTER] | oui/non |

Si aucune intégration externe détectée → écrire `Aucune intégration externe identifiée`.

## Dépendances Clés

{grouper par couche, pas de limite fixe — inclure toute dépendance ayant un rôle architectural}

### Frontend
| Bibliothèque | Rôle |
|--------------|------|
| [lib] | [usage] |

### Backend
| Bibliothèque | Rôle |
|--------------|------|
| [lib] | [usage] |

### Infrastructure / Outils
| Bibliothèque | Rôle |
|--------------|------|
| [lib] | [usage] |

---
**Sources :** [liste des fichiers lus]
**Màj :** YYYY-MM-DD
```

## Règles

1. **Versions exactes** depuis les manifestes — sinon `[À COMPLÉTER]`
2. **Pattern = observation, pas hypothèse** — si non déterminable depuis la structure → `[À COMPLÉTER]`
3. **Flux de données = schéma ASCII obligatoire** — si impossible à représenter → décrire en étapes numérotées
4. **Endpoints = top 10 par domaine** — si le projet a plus de 10 routes, sélectionner les plus représentatives et noter `[N routes au total — voir code source]`
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
