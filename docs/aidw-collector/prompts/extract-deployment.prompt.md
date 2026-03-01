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

Les annotations `{ }` indiquent comment remplir chaque section — ne pas les inclure dans la sortie.

```markdown
# Déploiement — [Nom Projet]

## Prérequis Système

| Composant | Version minimale | Obligatoire | Notes |
|-----------|-----------------|-------------|-------|
| [composant] | [version — sinon À COMPLÉTER] | oui/non | [notes] |

## Environnements

{n'inclure que les environnements détectés — pas de lignes vides}

| Env | Description | URL / Cible | Particularités |
|-----|-------------|-------------|---------------|
| dev | [description] | [URL — sinon À COMPLÉTER] | [particularités] |
| staging | [description] | [URL — sinon À COMPLÉTER] | [particularités] |
| prod | [description] | [URL — sinon À COMPLÉTER] | [particularités] |

## Variables d'Environnement

{jamais les valeurs — uniquement clés, rôle, format attendu}
{grouper par domaine fonctionnel si > 10 variables}

| Variable | Rôle | Obligatoire | Format attendu |
|----------|------|-------------|---------------|
| [VAR_NAME] | [usage — sinon À COMPLÉTER] | oui/non | [ex: URL, UUID, booléen] |

## Pipeline CI/CD

{steps depuis les fichiers CI — dans l'ordre d'exécution}

| Étape | Déclencheur | Actions | Environnement cible |
|-------|-------------|---------|---------------------|
| Build | [push/PR/tag] | [actions] | [env] |
| Test | [déclencheur] | [actions] | [env] |
| Deploy | [déclencheur] | [actions] | [env — sinon À COMPLÉTER] |

Si pas de CI/CD → noter "Déploiement manuel" et documenter la procédure depuis les scripts.

## Procédure d'Installation

{étapes depuis le CI/CD, README ou scripts — dans l'ordre}

```bash
# [étapes extraites des scripts ou du README]
```

## Infrastructure

{depuis Dockerfile, CI/CD, terraform — tableaux, pas de prose libre}

| Composant | Type | Provider | Notes |
|-----------|------|----------|-------|
| [app] | [Container / VM / Serverless / PaaS] | [AWS/GCP/Azure/Fly/Vercel…] | [config clé] |
| [BDD] | [Managed / Self-hosted] | [provider] | [version, région] |

## Dépendances & Services Externes

| Service | Rôle | Obligatoire | Variable d'env associée |
|---------|------|-------------|------------------------|
| [service] | [usage — sinon À COMPLÉTER] | oui/non | `[VAR_NAME]` |

---
**Sources :** [liste des fichiers lus]
**Màj :** YYYY-MM-DD
```

## Règles

1. **Jamais les valeurs** des variables d'environnement — uniquement clés, rôle, format
2. **Variables non documentées** : si `.env.example` absent, chercher dans le code (`process.env.*`) et noter `(inféré depuis code)`
3. **CI/CD absent** → noter "Déploiement manuel" et documenter depuis les scripts trouvés
4. **Infrastructure = tableaux** — ne jamais décrire l'infra en prose libre
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
