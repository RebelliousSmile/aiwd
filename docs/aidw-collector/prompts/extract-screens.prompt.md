---
name: extract-screens
description: Inventorie tous les écrans/pages d'une application UI et produit screens.md. Lancé uniquement si une UI a été détectée.
version: 1.1
---

# Extract Screens

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/screens.md`.

> **Prérequis :** lancé uniquement si `aidw-collector` a détecté une UI. Utiliser les patterns trouvés en Step 0 comme point de départ pour localiser les dossiers de pages/views.

## Sources à lire

Dans cet ordre de priorité :

1. **Tests end-to-end** — révèlent les flux utilisateur réels (`cypress/`, `playwright/`, `e2e/`)
2. **Fichiers de routes / navigation** (`router.ts`, `urls.py`, `routes.js`, `App.tsx`…)
3. **Fichiers de pages / views / composants de haut niveau**
4. **Fichiers i18n** — labels, messages, erreurs (`i18n/`, `locales/`, `messages.py`…)
5. **Formulaires et validations**
6. **Templates HTML / JSX / Vue**

## Output : `aidw-collector/dest/screens.md`

Charger `@aidw-collector/templates/screens.md` et remplacer chaque `[À COMPLÉTER]` par les données extraites des sources. Dupliquer les blocs d'écran autant que nécessaire. Pour les grandes apps (> 20 écrans), documenter le flux principal et noter `[N écrans au total — voir router]`. Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente.

## Règles

1. **Exhaustif** : chaque écran documenté ici permet d'écrire un guide utilisateur sans retourner au code
2. **Grandes apps** : si > 20 écrans, prioriser les écrans du flux principal (onboarding, actions clés, erreurs critiques) et noter `[N écrans au total — voir router]`
3. **Accès** : si `dest/access-matrix.md` est disponible, reprendre les rôles depuis ce fichier plutôt que de réinférer
4. **Pas de composants atomiques** : ne pas documenter les boutons, modals, inputs génériques — uniquement les écrans/pages complets
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
