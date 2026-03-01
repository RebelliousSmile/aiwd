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

```markdown
# Glossaire — [Nom Projet]

## Termes Métier

| Terme | Contexte | Définition | Source |
|-------|---------|-----------|--------|
| [terme exact] | [module ou dossier] | [définition inférée — sinon À COMPLÉTER] | [fichier] |

## Termes Techniques Spécifiques

{termes techniques qui ont un sens particulier dans CE projet — pas les généralités}

| Terme | Contexte | Définition | Source |
|-------|---------|-----------|--------|
| [terme exact] | [module ou dossier] | [définition inférée — sinon À COMPLÉTER] | [fichier] |

## Termes UI

{labels, libellés, noms d'écrans affichés à l'utilisateur — depuis i18n ou templates}

| Terme affiché | Correspondance code | Définition | Source |
|---------------|---------------------|-----------|--------|
| [label UI] | `[clé i18n ou variable]` | [ce que ça désigne] | [fichier] |

## Acronymes

| Acronyme | Signification | Définition | Source |
|----------|---------------|-----------|--------|
| [ACRONYME] | [forme longue] | [définition — sinon À COMPLÉTER] | [fichier] |

## Termes à Valider

{termes dont la définition est incertaine — à confirmer avec les stakeholders}

- **[terme]** : [pourquoi la définition est incertaine]

---
**Sources :** [liste des fichiers lus]
**Màj :** YYYY-MM-DD
```

## Règles

1. **Casse exacte du code** — `userId` pas `User ID`, `OrderStatus` pas `Order Status`
2. **Métier avant technique** — en cas de doute sur l'inclusion, privilégier les termes du domaine
3. **Définition incertaine → "À valider"** — ne jamais inventer une définition ; si le sens n'est pas clair depuis les sources → section "Termes à Valider"
4. **Toutes les catégories obligatoires** — si une catégorie n'a aucun terme, écrire `Aucun terme identifié`
