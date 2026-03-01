---
name: extract-glossaire
description: Extrait le vocabulaire technique et métier d'un projet et produit glossaire.md — termes bruts depuis le code et les docs.
version: 1.0
---

# Extract Glossaire

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/glossaire.md`.

## Sources à lire

1. Noms de fichiers, dossiers, modules (vocabulaire structurel)
2. Noms de classes, fonctions, variables métier (pas les utilitaires génériques)
3. Traductions / fichiers i18n (termes affichés à l'utilisateur)
4. Commentaires et docstrings
5. Docs existantes (README, wiki, specs)
6. Routes et noms d'endpoints API
7. Noms de tables/collections de base de données

## Ce qu'il faut extraire

Pour chaque terme :
- **Forme exacte** telle qu'elle apparaît dans le code (casse respectée)
- **Contexte d'usage** : dans quel module, pour quoi
- **Définition** : inférée depuis le code et les docs, ou `[À COMPLÉTER]`

Priorité aux termes **métier** (domaine de l'application) plutôt que techniques génériques (`handleClick`, `useState`…).

## Output : `aidw-collector/dest/glossaire.md`

```markdown
# Glossaire — [Nom Projet]

| Terme | Définition | Source |
|-------|-----------|--------|
| [terme exact] | [définition inférée] | [fichier ou module] |

## Termes à valider avec les stakeholders

- **[terme]** : [pourquoi la définition est incertaine]
```

## Règles

- Termes bruts : respecter la casse du code (`userId`, pas `User ID`)
- Minimum 15 termes, maximum non limité
- Si un terme apparaît partout sans définition claire → section "À valider"
- Ne pas inclure les termes purement techniques génériques (HTTP, JSON, null…) sauf s'ils ont un sens spécifique dans ce projet
