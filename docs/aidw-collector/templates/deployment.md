# Déploiement — [Nom Projet]

## Prérequis Système

| Composant | Version minimale | Obligatoire | Notes |
|-----------|-----------------|-------------|-------|
| [À COMPLÉTER] | [À COMPLÉTER] | oui/non | [À COMPLÉTER] |
| [À COMPLÉTER] | [À COMPLÉTER] | oui/non | [À COMPLÉTER] |

## Environnements

<!-- Supprimer les lignes des environnements non détectés dans le projet -->

| Env | Description | URL / Cible | Particularités |
|-----|-------------|-------------|---------------|
| dev | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |
| staging | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER — ou supprimer cette ligne] |
| prod | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |

## Variables d'Environnement

<!-- Ajouter une ligne par variable — jamais les valeurs, uniquement clé + rôle + format -->

| Variable | Rôle | Obligatoire | Format attendu |
|----------|------|-------------|---------------|
| [À COMPLÉTER] | [À COMPLÉTER] | oui/non | [ex: URL, UUID, booléen, chemin] |
| [À COMPLÉTER] | [À COMPLÉTER] | oui/non | [À COMPLÉTER] |

## Pipeline CI/CD

<!-- Supprimer ou adapter les lignes selon le pipeline réel — ajouter des lignes si nécessaire -->
<!-- Si pas de CI/CD : remplacer ce tableau par "Déploiement manuel — voir Procédure ci-dessous" -->

| Étape | Déclencheur | Actions | Environnement cible |
|-------|-------------|---------|---------------------|
| Build | [push sur main / PR / tag] | [À COMPLÉTER] | [À COMPLÉTER] |
| Test | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |
| Deploy | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |

## Procédure d'Installation

```bash
# Extraire les étapes depuis le README, les scripts ou le pipeline CI/CD
# Exemple attendu :
# git clone <repo>
# cp .env.example .env
# npm install
# npm run build
# npm start
[À COMPLÉTER]
```

## Infrastructure

<!-- Ajouter une ligne par composant (app, base de données, cache, CDN, etc.) -->

| Composant | Type | Provider | Notes |
|-----------|------|----------|-------|
| [À COMPLÉTER] | [Container / VM / Serverless / PaaS / Managed] | [AWS / GCP / Azure / Fly / Vercel / autre] | [À COMPLÉTER] |
| [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |

## Dépendances & Services Externes

<!-- Ajouter une ligne par service tiers utilisé (APIs externes, queues, stockages) -->

| Service | Rôle | Obligatoire | Variable d'env associée |
|---------|------|-------------|------------------------|
| [À COMPLÉTER] | [À COMPLÉTER] | oui/non | `[À COMPLÉTER]` |

---
**Sources :** [À COMPLÉTER]
**Màj :** YYYY-MM-DD
