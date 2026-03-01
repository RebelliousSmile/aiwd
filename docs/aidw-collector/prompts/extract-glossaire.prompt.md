---
name: extract-glossaire
description: Extrait le vocabulaire technique et métier d'un projet et produit glossaire.md — termes bruts depuis le code et les docs, organisés par catégorie.
version: 1.1
---

# Extract Glossaire

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/glossaire.md`.

Utiliser le domaine et la stack détectés en Step 0 pour cibler en priorité les dossiers et modules métier du projet.

## Sources à lire

Dans cet ordre, en filtrant sur les termes **métier et techniques spécifiques** (pas les utilitaires génériques) :

1. Noms de classes, fonctions, variables métier (ex: `InvoiceProcessor`, `OrderStatus`)
2. Noms de fichiers, dossiers, modules (vocabulaire structurel)
3. Routes et noms d'endpoints API
4. Noms de tables/collections de base de données
5. Fichiers i18n / traductions (termes affichés à l'utilisateur)
6. Commentaires et docstrings
7. Docs existantes (README, wiki, specs)

**Exclure** : termes purement génériques sans sens spécifique au projet (`handleClick`, `useState`, `HTTP`, `JSON`, `null`…) — sauf s'ils ont un sens particulier ici.

## Output : `aidw-collector/dest/glossaire.md`

Charger `@aidw-collector/templates/glossaire.md` et remplacer chaque `[À COMPLÉTER]` par les données extraites des sources. Dupliquer les lignes de tableau autant que nécessaire. Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente.

## Règles

1. **Casse exacte du code** — `userId` pas `User ID`, `OrderStatus` pas `Order Status`
2. **Métier avant technique** — en cas de doute sur l'inclusion, privilégier les termes du domaine
3. **Définition incertaine → "À valider"** — ne jamais inventer une définition ; si le sens n'est pas clair depuis les sources → section "Termes à Valider"
4. **Toutes les catégories obligatoires** — si une catégorie n'a aucun terme, écrire `Aucun terme identifié`
