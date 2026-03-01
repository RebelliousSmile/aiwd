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

Produire le fichier suivant. Les annotations entre `{` `}` indiquent comment trouver ou inférer chaque champ — ne pas les inclure dans la sortie.

```markdown
# Client : [nom-kebab] — [Nom Officiel]
{nom-kebab = identifiant court dérivé du nom officiel, ex: cerascan, generation-pdf}

## Contexte

[Secteur / domaine métier — sinon À COMPLÉTER]

[Description de l'organisation et objet du produit — orienté utilisateur : ce que le produit
fait pour eux, 2-3 phrases max]

## Audience Cible

- **Primaire :** [profil, niveau technique, contexte d'usage — sinon À COMPLÉTER]
- **Secondaire :** [profil — si identifiable, sinon À COMPLÉTER]

**Niveau technique :** [débutant | intermédiaire | expert | À COMPLÉTER]
{inférer depuis : types génériques, patterns avancés (DI, CQRS, monades...) → expert ;
config complexe, multi-services → intermédiaire ; scripts simples, CLI basique → débutant}

## Ton & Style

- **Ton :** [ton observé dans les docs existants — sinon inférer depuis README — sinon À COMPLÉTER]
- **Conventions :** [tutoiement/vouvoiement, langue(s) — sinon À COMPLÉTER]
- **Formats préférés :** [code, tableaux, listes, prose — sinon À COMPLÉTER]

## Contraintes

### Techniques
- [contrainte technique identifiée — sinon À COMPLÉTER]

### Métier
- [contrainte métier identifiée — sinon À COMPLÉTER]

### Légales / Réglementaires
- [RGPD, licences, certifications — sinon À COMPLÉTER]

---
**Sources :** [liste des fichiers lus]
**Màj :** YYYY-MM-DD
```

## Règles

1. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
2. **Priorité des sources** — collect.yml > Step 0 > fichiers projet ; si un champ est renseigné plus haut, l'utiliser tel quel sans chercher plus bas
3. **Inférence** — si un champ n'est pas explicite : inférer depuis le contexte disponible et noter `(inféré)` ; appliquer les critères du template pour le niveau technique et le ton
