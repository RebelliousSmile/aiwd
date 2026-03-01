---
name: extract-client
description: Extrait le contexte client/produit d'un projet et produit CLIENT.md — nom, objet, audience, ton, contraintes.
version: 1.0
---

# Extract Client

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/CLIENT.md`.

## Sources à lire

Dans cet ordre de priorité :
1. README (racine)
2. Fichiers de présentation existants (ABOUT.md, docs/intro*, wiki*, specs*)
3. `package.json` → champ `description`
4. `pyproject.toml` / `setup.py` → champ `description`
5. Commentaires d'en-tête des fichiers principaux
6. Hints `collect.yml` → `client.*` et `project.*`

## Ce qu'il faut extraire

- **Nom** : nom officiel du projet (tel qu'il apparaît dans les sources)
- **Objet** : à quoi sert le projet, quel problème il résout (2-4 phrases)
- **Secteur** : domaine métier si identifiable
- **Audience** : qui utilise ce projet — profil, niveau technique, contexte d'usage
- **Ton observé** : style des docs existants (formel, direct, corporate, communauté…)
- **Contraintes** : techniques, légales, organisationnelles identifiables dans les sources

## Output : `aidw-collector/dest/CLIENT.md`

```markdown
# Client : [nom-kebab] — [Nom Projet]

## Contexte
[secteur, description de l'organisation, objet du produit]

## Audience Cible
- **Primaire :** [profil, niveau technique, contexte d'usage]
- **Secondaire :** [profil — si identifiable]

**Niveau technique :** [débutant | intermédiaire | expert]

## Ton & Style
- [ton observé dans les docs existants]
- [conventions : tutoiement/vouvoiement, langue(s)]
- [formats préférés : code, tableaux, listes…]

## Contraintes
- [contrainte identifiée dans les sources]
- [À COMPLÉTER — si non trouvé dans les sources]
```

## Règles

- Si l'audience n'est pas explicite, l'inférer depuis les fonctionnalités et le ton des docs
- Ne jamais inventer un secteur ou une audience — écrire `[À COMPLÉTER]` si non trouvé
- Le ton doit refléter les docs existants, pas les conventions AIDW
