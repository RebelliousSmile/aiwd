---
name: extract-access
description: Extrait la matrice des rôles et permissions d'un projet et produit access-matrix.md. Lancé uniquement si une gestion d'auth/rôles a été détectée.
version: 1.0
---

# Extract Access

À partir de l'inventaire partagé par `aidw-collector` et des fichiers du projet, produis `aidw-collector/dest/access-matrix.md`.

> **Prérequis :** ce prompt est lancé uniquement si `aidw-collector` a détecté une gestion d'authentification ou de rôles.

## Sources à lire

1. Fichiers de guards / middleware / decorators (`auth.guard.ts`, `middleware/auth.py`, `@roles()`…)
2. Définitions des rôles (`roles.ts`, `constants/roles.py`, enums…)
3. Politiques d'accès (`policies/`, `permissions/`, `acl/`)
4. Conditions d'affichage dans les templates (`v-if="isAdmin"`, `{hasRole && ...}`)
5. Tests d'accès (révèlent les règles implicites)
6. Configuration OAuth / JWT (scopes, claims)

## Ce qu'il faut extraire

- **Rôles** : noms exacts depuis le code (casse respectée : `ADMIN`, `Operator`, `client`)
- **Règles d'accès** : qui peut accéder à quoi (par fonctionnalité, écran ou endpoint)
- **Comportements différenciés** : même écran, contenu différent selon le rôle
- **Gestion des refus** : redirection, message d'erreur, comportement

## Output : `aidw-collector/dest/access-matrix.md`

```markdown
# Matrice Rôles & Accès — [Nom Projet]

## Rôles Identifiés

| Rôle | Description | Identifiant code | Source |
|------|-------------|-----------------|--------|
| [Rôle] | [description] | `[ROLE_CONSTANT]` | [fichier] |

## Matrice d'Accès

| Fonctionnalité / Écran | Public | [Rôle 1] | [Rôle 2] | [Rôle 3] |
|------------------------|--------|----------|----------|----------|
| [fonctionnalité] | ✓/✗ | ✓/✗ | ✓/✗ | ✓/✗ |

## Comportements Différenciés par Rôle

[Description des cas où l'UI varie selon le rôle — même route, contenu conditionnel]

## Gestion des Accès Non Autorisés

- **Redirection :** [vers quelle page]
- **Message :** [libellé affiché]
- **Comportement API :** [code HTTP retourné]
```

## Règles

- Noms de rôles **exacts depuis le code** — ne pas renommer pour clarté
- Si les règles d'accès sont implicites (vérifiées uniquement dans les templates) → noter explicitement
- Signaler dans INSTALL.md les fonctionnalités sans règle d'accès documentée
