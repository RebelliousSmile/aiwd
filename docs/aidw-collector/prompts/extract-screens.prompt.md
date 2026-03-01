---
name: extract-screens
description: Inventorie tous les écrans/pages d'une application UI et produit screens.md. Lancé uniquement si une UI a été détectée.
version: 1.0
---

# Extract Screens

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/screens.md`.

> **Prérequis :** ce prompt est lancé uniquement si `aidw-collector` a détecté une UI (composants, pages, views, templates).

## Sources à lire

1. Fichiers de routes / navigation (`router.ts`, `urls.py`, `routes.js`, `App.tsx`…)
2. Fichiers de pages / views / composants de haut niveau
3. Templates HTML / JSX / Vue
4. Fichiers de traductions (labels, messages, erreurs — `i18n/`, `locales/`, `messages.py`…)
5. Formulaires et validations
6. Tests end-to-end (révèlent les flux utilisateur)

## Ce qu'il faut extraire — par écran

```
Écran : [Nom lisible]
Route : [URL / chemin]
Accès : [public | authentifié | rôle(s) requis]
Description : [à quoi sert cet écran — 2 lignes max]

Éléments visibles :
  - [champ / bouton / tableau / formulaire]
  - [comportement associé si connu]

Actions disponibles :
  - [action] → [résultat / redirection / message]

Messages système :
  - Succès : [message depuis i18n ou code]
  - Erreur : [message depuis i18n ou code]

Notes :
  - [comportement conditionnel, validation, cas limite]
```

Grouper les écrans par **module fonctionnel**.

## Output : `aidw-collector/dest/screens.md`

```markdown
# Inventaire des Écrans — [Nom Projet]

## Module : [Nom]

### [Nom de l'écran]
- **Route :** [URL]
- **Accès :** [public | rôle(s)]
- **Description :** [2 lignes]
- **Éléments :** [liste]
- **Actions :** [action → résultat]
- **Messages :** [succès / erreur]
- **Notes :** [comportements conditionnels]
```

## Règles

- **Exhaustif** : chaque écran documenté ici permet d'écrire un guide utilisateur sans retourner au code
- Si les messages i18n ne sont pas trouvés → `[À COMPLÉTER — libellé non trouvé]`
- Si les rôles d'accès ne sont pas explicites → noter `[À COMPLÉTER]` et signaler dans INSTALL.md
- Ne pas documenter les composants réutilisables (boutons, modals génériques) — uniquement les écrans/pages complets
