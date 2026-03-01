---
name: extract-deployment
description: Extrait la configuration de déploiement d'un projet et produit deployment.md — environnements, variables, procédure, dépendances externes.
version: 1.0
---

# Extract Deployment

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/deployment.md`.

> **Prérequis :** ce prompt est lancé uniquement si `aidw-collector` a détecté une configuration de déploiement.

## Sources à lire

1. `.env.example`, `.env.sample`, `.env.template`
2. `Dockerfile`, `docker-compose.yml`, `docker-compose.*.yml`
3. CI/CD : `.github/workflows/*.yml`, `.gitlab-ci.yml`, `Jenkinsfile`, `.circleci/`
4. Scripts de déploiement (`deploy.sh`, `Makefile`, `scripts/deploy*`)
5. Configuration cloud (`terraform/`, `kubernetes/`, `helm/`, `serverless.yml`)
6. `README` sections "Installation", "Déploiement", "Getting Started"
7. Chercher dans le code : `process.env`, `os.environ`, `config.get`, `Settings()`

## Ce qu'il faut extraire

- **Prérequis système** : OS, mémoire, CPU, ports, logiciels requis
- **Environnements** : dev, staging, prod — différences notables
- **Variables d'environnement** : clés + rôle (jamais les valeurs)
- **Procédure d'installation** : étapes depuis les scripts et CI
- **Dépendances externes** : services tiers obligatoires (BDD, cache, email, stockage…)
- **Infrastructure** : hébergement, conteneurs, services managés

## Output : `aidw-collector/dest/deployment.md`

```markdown
# Déploiement — [Nom Projet]

## Prérequis Système

| Composant | Version minimale | Obligatoire | Notes |
|-----------|-----------------|-------------|-------|
| [composant] | [version] | oui/non | [notes] |

## Environnements

| Env | Description | Particularités |
|-----|-------------|---------------|
| dev | [description] | [particularités] |
| prod | [description] | [particularités] |

## Installation

```bash
# [étapes depuis le CI/CD ou les scripts]
```

## Variables d'Environnement

| Variable | Rôle | Obligatoire | Format attendu |
|----------|------|-------------|---------------|
| [VAR_NAME] | [usage] | oui/non | [format, jamais la valeur] |

## Dépendances & Intégrations Externes

| Service | Rôle | Obligatoire | Notes |
|---------|------|-------------|-------|
| [service] | [usage] | oui/non | [config clé] |

## Infrastructure

[hébergement, conteneurs, services managés — depuis Dockerfile/CI/terraform]
```

## Règles

- Variables d'environnement : **jamais les valeurs**, uniquement clés + rôle + format
- Si `.env.example` absent → chercher `process.env.*` / `os.environ.*` dans le code
- Si pas de CI/CD → noter "déploiement manuel" et documenter depuis les scripts trouvés
- Signaler dans INSTALL.md les variables sans description ni exemple de format
