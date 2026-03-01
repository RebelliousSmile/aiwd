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

Charger `@aidw-collector/templates/architecture.md` et remplacer chaque `[À COMPLÉTER]` par les données extraites des sources. Dupliquer les lignes de tableau autant que nécessaire. Omettre les sections sans données (ex: "Endpoints API" si pas d'API exposée). Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente.

## Règles

1. **Versions exactes** depuis les manifestes — sinon `[À COMPLÉTER]`
2. **Pattern = observation, pas hypothèse** — si non déterminable depuis la structure → `[À COMPLÉTER]`
3. **Flux de données = schéma ASCII obligatoire** — si impossible à représenter → décrire en étapes numérotées
4. **Endpoints = top 10 par domaine** — si le projet a plus de 10 routes, sélectionner les plus représentatives et noter `[N routes au total — voir code source]`
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
