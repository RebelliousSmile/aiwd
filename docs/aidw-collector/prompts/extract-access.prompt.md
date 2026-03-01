---
name: extract-access
description: Extrait la matrice des rôles et permissions d'un projet et produit access-matrix.md. Lancé uniquement si une gestion d'auth/rôles a été détectée.
version: 1.1
---

# Extract Access

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/access-matrix.md`.

> **Prérequis :** lancé uniquement si `aidw-collector` a détecté une gestion d'authentification ou de rôles. Utiliser la liste de patterns détectés (Step 0) comme point de départ de la recherche.

## Sources à lire

Dans cet ordre de priorité :

1. **Tests d'accès** — révèlent les règles réelles, y compris implicites (`*.spec.ts`, `*_test.py`, `*.test.js`)
2. **Guards / middleware / decorators** (`auth.guard.ts`, `middleware/auth.py`, `@roles()`…)
3. **Définitions des rôles** (`roles.ts`, `constants/roles.py`, enums, constantes)
4. **Politiques d'accès** (`policies/`, `permissions/`, `acl/`)
5. **Configuration OAuth / JWT** (scopes, claims, providers)
6. **Conditions d'affichage dans les templates** (`v-if="isAdmin"`, `{hasRole && ...}`)

## Output : `aidw-collector/dest/access-matrix.md`

Les annotations `{ }` indiquent comment trouver chaque champ — ne pas les inclure dans la sortie.

```markdown
# Matrice Rôles & Accès — [Nom Projet]

## Mécanisme d'Authentification

{type de mécanisme depuis config OAuth/JWT/session — sinon À COMPLÉTER}

- **Type :** [JWT | Session | OAuth2 | SSO | Basic | À COMPLÉTER]
- **Provider :** [Keycloak | Auth0 | Passport | custom | À COMPLÉTER]
- **Token / Claims :** [champs utilisés pour identifier le rôle — sinon À COMPLÉTER]

## Rôles Identifiés

{noms exacts depuis le code, casse respectée : ADMIN, Operator, client}

| Rôle | Description | Identifiant code | Source |
|------|-------------|-----------------|--------|
| [Rôle] | [description — sinon À COMPLÉTER] | `[ROLE_CONSTANT]` | [fichier] |

## Matrice d'Accès

{si > 5 rôles : créer un tableau par domaine fonctionnel plutôt qu'un seul tableau large}

| Fonctionnalité / Écran | Public | [Rôle 1] | [Rôle 2] | [Rôle 3] |
|------------------------|--------|----------|----------|----------|
| [fonctionnalité] | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ |

Cellule `?` = règle d'accès non trouvée dans les sources → signaler comme gap.

## Comportements Différenciés par Rôle

{même route, contenu conditionnel selon le rôle}

### [Fonctionnalité / Écran]

| Rôle | Comportement | Source |
|------|-------------|--------|
| [Rôle 1] | [ce qu'il voit / peut faire] | [fichier] |
| [Rôle 2] | [ce qu'il voit / peut faire] | [fichier] |

## Gestion des Accès Non Autorisés

- **Redirection :** [vers quelle page — sinon À COMPLÉTER]
- **Message affiché :** [libellé — sinon À COMPLÉTER]
- **Comportement API :** [code HTTP retourné — ex: 401, 403 — sinon À COMPLÉTER]

## Gaps Identifiés

| Fonctionnalité | Rôle(s) concerné(s) | Règle non trouvée |
|---------------|----------------------|-------------------|
| [fonctionnalité] | [rôle] | [description du gap] |

---
**Sources :** [liste des fichiers lus]
**Màj :** YYYY-MM-DD
```

## Règles

1. **Noms de rôles exacts depuis le code** — ne pas renommer pour clarté, casse respectée
2. **Tests > code** — si une règle d'accès est contredite entre un test et le code source, signaler la contradiction et noter les deux versions
3. **Règles implicites** — si la restriction est vérifiée uniquement dans les templates UI → noter explicitement `(implicite, template uniquement)`
4. **Gaps → section dédiée** — toute cellule `?` dans la matrice + toute règle introuvable → ligne dans "Gaps Identifiés" (repris par l'orchestrateur dans INSTALL.md)
5. **Ne jamais inventer** — information absente → `[À COMPLÉTER]` ou `?` selon le contexte
