---
name: extract-client
description: Extrait le contexte client/produit d'un projet et produit CLIENT.md — nom, objet, audience, ton, contraintes.
version: 1.2
---

# Extract Client

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/CLIENT.md`.

## Sources à lire

Dans cet ordre de priorité (source plus haute = prioritaire, écrase les inférieures) :

1. **`collect.yml`** → champs `client.*` et `project.*` — volonté explicite de l'humain
2. **Contexte d'inventaire Step 0** — nom, objet, audience, stack détectés par l'orchestrateur
3. README (racine)
4. Fichiers de présentation (ABOUT.md, docs/intro*, wiki*, specs*)
5. `package.json` / `pyproject.toml` / `setup.py` → champ `description`
6. Commentaires d'en-tête des fichiers principaux

## Output : `aidw-collector/dest/CLIENT.md`

Charger `@aidw-collector/templates/CLIENT.md` et remplacer chaque `[À COMPLÉTER]` par les données extraites des sources. Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente des sources.

## Règles

1. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
2. **Priorité des sources** — collect.yml > Step 0 > fichiers projet ; si un champ est renseigné plus haut, l'utiliser tel quel sans chercher plus bas
3. **Inférence** — si un champ n'est pas explicite : inférer depuis le contexte disponible et noter `(inféré)` ; appliquer les critères du template pour le niveau technique et le ton
