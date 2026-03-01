# AIDW Collector

Kit à copier dans un projet cible pour extraire toute l'information nécessaire à la documentation AIDW.

**Point d'entrée unique :** `aidw-collector.md`

## Ce que ça produit

`aidw-collector` détecte automatiquement le projet, sélectionne les extracteurs pertinents et les lance en parallèle :

| Fichier produit | Contenu | Condition |
|----------------|---------|-----------|
| `CLIENT.md` | Contexte, audience, ton, contraintes | Toujours |
| `glossaire.md` | Vocabulaire technique extrait du code | Toujours |
| `architecture.md` | Stack, modules, flux de données | Toujours |
| `screens.md` | Inventaire des écrans (champs, actions, messages) | Si UI détectée |
| `access-matrix.md` | Matrice rôles × fonctionnalités | Si auth/rôles détectés |
| `deployment.md` | Environnements, variables, procédure | Si config déploiement détectée |
| `INSTALL.md` | Commandes de déploiement + gaps | Toujours |

## Utilisation

### 1. Copier ce dossier dans le projet cible

```bash
cp -r docs/aidw-collector/ /chemin/vers/mon-projet/
```

### 2. (Optionnel) Renseigner `collect.yml`

Uniquement pour forcer ou corriger l'auto-détection. Laisser vide pour une détection entièrement automatique.

### 3. Lancer l'agent

Avec n'importe quel LLM (Claude, GPT, Gemini…) ayant accès aux fichiers du projet :

```
@aidw-collector/aidw-collector.md
```

L'agent analyse le projet, sélectionne les extracteurs, les lance en parallèle, puis produit les fichiers dans `aidw-collector/dest/`.

### 4. Déployer dans generationPDF

Suivre les instructions dans `aidw-collector/dest/INSTALL.md` (généré automatiquement).

```bash
# Exemple pour cerascan :
cp aidw-collector/dest/CLIENT.md        generationPDF/cerascan/.docs/
cp aidw-collector/dest/glossaire.md     generationPDF/cerascan/.docs/
cp aidw-collector/dest/architecture.md  generationPDF/cerascan/.docs/
cp aidw-collector/dest/screens.md       generationPDF/cerascan/.docs/
cp aidw-collector/dest/access-matrix.md generationPDF/cerascan/.docs/
cp aidw-collector/dest/deployment.md    generationPDF/cerascan/.docs/
```

Puis décommenter les entrées correspondantes dans `bank.yml → docs:`.

### 5. Prochaine étape AIDW

Rédiger `overview.md` (brief éditorial du document à produire),  
puis lancer `@docs/prompts/workshop/generate-toc.prompt.md`.

## Prérequis

- Un LLM avec accès aux fichiers du projet (Claude, GPT-4, Gemini…)
- Aucun Python, aucun Pandoc requis à cette étape

