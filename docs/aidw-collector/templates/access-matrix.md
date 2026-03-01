# Matrice Rôles & Accès — [Nom Projet]

## Mécanisme d'Authentification

- **Type :** [À COMPLÉTER — JWT | Session | OAuth2 | SSO | Basic]
- **Provider :** [À COMPLÉTER — Keycloak | Auth0 | Passport | custom]
- **Token / Claims :** [À COMPLÉTER — champs utilisés pour identifier le rôle]

## Rôles Identifiés

<!-- Ajouter une ligne par rôle — noms exacts depuis le code, casse respectée -->

| Rôle | Description | Identifiant code | Source |
|------|-------------|-----------------|--------|
| [À COMPLÉTER] | [À COMPLÉTER] | `[À COMPLÉTER]` | [À COMPLÉTER] |
| [À COMPLÉTER] | [À COMPLÉTER] | `[À COMPLÉTER]` | [À COMPLÉTER] |

## Matrice d'Accès

<!-- Ajouter une colonne par rôle identifié ci-dessus — ✓ = accès, ✗ = refusé, ? = règle non trouvée -->
<!-- Si > 5 rôles : créer un tableau par domaine fonctionnel plutôt qu'un seul tableau large -->

| Fonctionnalité / Écran | Public | [Rôle 1] | [Rôle 2] | [Rôle 3] |
|------------------------|--------|----------|----------|----------|
| [À COMPLÉTER] | ✓/✗ | ✓/✗/? | ✓/✗/? | ✓/✗/? |
| [À COMPLÉTER] | ✓/✗ | ✓/✗/? | ✓/✗/? | ✓/✗/? |

## Comportements Différenciés par Rôle

<!-- Même route, contenu conditionnel selon le rôle — dupliquer le bloc ### pour chaque fonctionnalité concernée -->

### [Fonctionnalité / Écran]

| Rôle | Comportement | Source |
|------|-------------|--------|
| [À COMPLÉTER] | [À COMPLÉTER — ce qu'il voit / peut faire] | [À COMPLÉTER] |
| [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |

## Gestion des Accès Non Autorisés

- **Redirection :** [À COMPLÉTER]
- **Message affiché :** [À COMPLÉTER]
- **Comportement API :** [À COMPLÉTER — ex: 401, 403]

## Gaps Identifiés

<!-- Une ligne par cellule "?" dans la matrice ou règle introuvable dans les sources -->

| Fonctionnalité | Rôle(s) concerné(s) | Règle non trouvée |
|---------------|----------------------|-------------------|
| [À COMPLÉTER] | [À COMPLÉTER] | [À COMPLÉTER] |

---
**Sources :** [À COMPLÉTER]
**Màj :** YYYY-MM-DD
