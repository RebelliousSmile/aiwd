---
name: extract-deployment
description: Extrait la configuration de déploiement d'un projet et produit deployment.md — environnements, variables, procédure, CI/CD, infrastructure.
version: 1.1
---

# Extract Deployment

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/deployment.md`.

> **Prérequis :** lancé uniquement si `aidw-collector` a détecté une configuration de déploiement. Utiliser les patterns trouvés en Step 0 comme point de départ.

## Sources à lire

Dans cet ordre de priorité :

1. **Variables d'environnement** : `.env.example`, `.env.sample`, `.env.template`
2. **Conteneurs** : `Dockerfile`, `docker-compose.yml`, `docker-compose.*.yml`
3. **CI/CD** : `.github/workflows/*.yml`, `.gitlab-ci.yml`, `Jenkinsfile`, `.circleci/`
4. **Scripts de déploiement** : `deploy.sh`, `Makefile`, `scripts/deploy*`
5. **Infrastructure as code** : `terraform/`, `kubernetes/`, `helm/`, `serverless.yml`
6. **README** : sections "Installation", "Déploiement", "Getting Started"
7. **Code source** : `process.env.*`, `os.environ.*`, `config.get(`, `Settings()` (si .env.example absent)

## Output : `aidw-collector/dest/deployment.md`

Charger `@aidw-collector/templates/deployment.md` et remplacer chaque `[À COMPLÉTER]` par les données extraites des sources. N'inclure que les environnements détectés. Ne jamais inclure les valeurs des variables d'environnement. Si pas de CI/CD, noter "Déploiement manuel" dans la section Pipeline. Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente.

## Règles

1. **Jamais les valeurs** des variables d'environnement — uniquement clés, rôle, format
2. **Variables non documentées** : si `.env.example` absent, chercher dans le code (`process.env.*`) et noter `(inféré depuis code)`
3. **CI/CD absent** → noter "Déploiement manuel" et documenter depuis les scripts trouvés
4. **Infrastructure = tableaux** — ne jamais décrire l'infra en prose libre
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
