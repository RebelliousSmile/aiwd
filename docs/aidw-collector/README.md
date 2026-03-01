# AIDW Collector

Kit à copier dans un projet cible pour extraire toute l'information nécessaire à la documentation AIDW.

## Ce que ça produit

`client-extract` analyse le projet (code + docs existants) et produit des fichiers prêts à placer dans `<client>/.docs/` :

| Fichier | Contenu | Obligatoire |
|---------|---------|-------------|
| `CLIENT.md` | Contexte client, audience, ton, contraintes | Oui |
| `glossaire.md` | Vocabulaire technique extrait du code | Oui |
| `architecture.md` | Stack, modules, flux de données | Si projet tech |
| `screens.md` | Inventaire des écrans (champs, actions, messages) | Si app UI |
| `access-matrix.md` | Matrice rôles × fonctionnalités | Si auth/rôles |
| `deployment.md` | Environnements, prérequis, procédure | Si déployable |
| `INSTALL.md` | Commandes de déploiement + gaps identifiés | Toujours |

## Utilisation

### 1. Copier ce dossier dans le projet cible

```bash
cp -r docs/aidw-collector/ /chemin/vers/cerascan/
```

### 2. Remplir `collect.yml`

```bash
# Ouvrir et renseigner les champs :
# project.name, project.type, client.name, client.audience
# Désactiver les outputs non pertinents (ex: screens.md: false si pas d'UI)
```

### 3. Lancer le prompt d'extraction

Avec n'importe quel LLM (Claude, GPT, Gemini…) ayant accès aux fichiers du projet :

```
@aidw-collector/prompts/client-extract.prompt.md
```

Le prompt analyse le projet et produit les fichiers dans `aidw-collector/dest/`.

### 4. Déployer dans generationPDF

```bash
# Copier dans <client>/.docs/ (depuis generationPDF/)
cp cerascan/aidw-collector/dest/CLIENT.md        cerascan/.docs/
cp cerascan/aidw-collector/dest/glossaire.md     cerascan/.docs/
cp cerascan/aidw-collector/dest/architecture.md  cerascan/.docs/
cp cerascan/aidw-collector/dest/screens.md       cerascan/.docs/
cp cerascan/aidw-collector/dest/access-matrix.md cerascan/.docs/
cp cerascan/aidw-collector/dest/deployment.md    cerascan/.docs/
```

Puis décommenter les entrées correspondantes dans `bank.yml → docs:`.

### 5. Prochaine étape AIDW

Rédiger `overview.md` (brief éditorial — ce que le document va couvrir),  
puis lancer `@docs/prompts/workshop/generate-toc.prompt.md`.

## Prérequis

- Un LLM avec accès aux fichiers du projet (Claude, GPT-4, Gemini…)
- Aucun Python, aucun Pandoc requis à cette étape

