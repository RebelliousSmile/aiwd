---
name: extract-architecture
description: Extrait la stack technique et l'architecture d'un projet et produit architecture.md.
version: 1.0
---

# Extract Architecture

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/architecture.md`.

## Sources à lire

1. `package.json`, `requirements.txt`, `pyproject.toml`, `Gemfile`, `go.mod`, `Cargo.toml`
2. Structure des dossiers (inférer le pattern architectural)
3. Fichiers de configuration principaux (`app.py`, `main.ts`, `index.js`, `server.*`…)
4. Fichiers de modèles/entités (`models/`, `schemas/`, `types/`, `entities/`)
5. Fichiers de routes / controllers
6. Fichiers de services / logique métier
7. Docs d'architecture existantes

## Ce qu'il faut extraire

- **Stack** : langages + versions, framework principal, BDD, frontend, outils de build
- **Pattern** : MVC, Clean Arch, microservices, monolithe… justifié depuis la structure
- **Modules** : liste des modules/domaines avec leur responsabilité et leur dossier
- **Flux de données** : comment les données circulent (entrée → traitement → sortie)
- **Modèles de données clés** : entités principales, champs importants, relations
- **Endpoints API** : méthode + route + description (si API exposée)
- **Dépendances clés** : 10 bibliothèques les plus importantes avec leur rôle

## Output : `aidw-collector/dest/architecture.md`

```markdown
# Architecture — [Nom Projet]

## Stack Technique

| Couche | Technologie | Version |
|--------|-------------|---------|
| [couche] | [techno] | [version ou "latest"] |

## Pattern Architectural

[description du pattern — justifié depuis la structure du code]

## Modules Principaux

| Module | Responsabilité | Dossier |
|--------|---------------|---------|
| [module] | [rôle] | [chemin] |

## Flux de Données

[description du flux principal : requête/entrée → traitement → sortie/réponse]

## Modèles de Données Clés

| Entité | Champs importants | Relations | Notes |
|--------|-------------------|-----------|-------|
| [entité] | [champs] | [relations] | [comportement notable] |

## Endpoints API Principaux

| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
| [GET/POST…] | [/route] | [usage] | [oui/non] |

## Dépendances Clés

| Bibliothèque | Rôle | Obligatoire |
|--------------|------|-------------|
| [lib] | [usage] | oui/non |
```

## Règles

- Versions exactes si trouvées dans les fichiers de config, sinon `[À COMPLÉTER]`
- Si pas d'API exposée → omettre la section Endpoints
- Pattern architectural : jamais d'hypothèse — si non déterminable, écrire `[À COMPLÉTER]`
