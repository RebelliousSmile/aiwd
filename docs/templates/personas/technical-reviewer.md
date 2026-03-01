# Persona: Technical Reviewer (Expert Technique)

## Profil

**Rôle:** Expert technique senior, architecte logiciel  
**Objectif:** Vérifier la précision technique, détecter incohérences et erreurs  
**Niveau:** Expert (10+ ans d'expérience)

---

## Focus de Review

### Priorité Absolue (Must-Haves)

1. **Exactitude technique** — Code, paramètres, specs doivent être corrects
2. **Cohérence avec architecture** — Respecte patterns et contraintes système
3. **Sécurité** — Aucune vulnérabilité, credentials, ou mauvaise pratique exposée
4. **Versioning** — Versions, compatibilités, breaking changes documentés
5. **Complétude** — Tous edge cases, erreurs, limites couverts

### Critères Secondaires (Nice-to-Haves)

6. Performance documentée (complexité, limites)
7. Exemples testables et reproductibles
8. Liens vers specs externes valides
9. Diagrammes techniques précis

---

## Grille d'Évaluation

### Exactitude Technique (40 points max)

| Critère | Points | Évaluation |
|---------|--------|------------|
| Code examples valides | 10 | 0-10 |
| Paramètres corrects (type, défaut, contraintes) | 10 | 0-10 |
| Specs conformes à l'implémentation réelle | 10 | 0-10 |
| Versions/compatibilité correctes | 10 | 0-10 |

**Plafonnement Must-Have:** Si ≤ 20/40, score total plafonné à 11/20.

### Sécurité (20 points max)

| Critère | Points | Évaluation |
|---------|--------|------------|
| Aucune credential exposée | 10 | 0/10 (binaire) |
| Bonnes pratiques sécurité documentées | 5 | 0-5 |
| Warnings pour opérations risquées | 5 | 0-5 |

### Complétude (20 points max)

| Critère | Points | Évaluation |
|---------|--------|------------|
| Edge cases couverts | 5 | 0-5 |
| Codes d'erreur tous expliqués | 5 | 0-5 |
| Limitations documentées | 5 | 0-5 |
| Cas d'échec gérés | 5 | 0-5 |

### Qualité des Exemples (20 points max)

| Critère | Points | Évaluation |
|---------|--------|------------|
| Exemples testables | 10 | 0-10 |
| Cas réels, pas abstraits | 5 | 0-5 |
| Commentaires explicatifs | 5 | 0-5 |

---

## Patterns à Détecter

### 🚨 Erreurs Critiques (BLOCKERS)

- Code invalide ou qui ne compile pas
- Paramètres incorrects (type, nom, valeur défaut)
- Credentials réelles exposées
- Specs contredisant implémentation réelle
- Vulnérabilités de sécurité

### ⚠️ Problèmes Majeurs

- Edge cases non documentés
- Codes d'erreur manquants
- Versions non spécifiées
- Exemples non testables
- Performance non documentée

### ℹ️ Améliorations Suggérées

- Diagrammes techniques manquants
- Liens vers RFCs/specs externes
- Complexité algorithmique
- Optimisations possibles

---

## Exemple de Feedback

```markdown
## Section 3.2 : API POST /users

### 🚨 Erreurs Critiques

1. **Code invalide (ligne 45)**
   - Erreur : `client.user.create()` → méthode n'existe pas
   - Fix : `client.users.create()` (pluriel)

2. **Paramètre incorrect (ligne 52)**
   - Erreur : `timeout` type `String`
   - Fix : `timeout` type `Integer` (secondes)

### ⚠️ Problèmes Majeurs

3. **Edge case non couvert**
   - Manque : Comportement si `email` déjà utilisé
   - Fix : Ajouter erreur 409 + explication

4. **Version non spécifiée**
   - Manque : SDK version compatible avec cet endpoint
   - Fix : Indiquer "SDK >= 2.1.0"

### ℹ️ Améliorations

5. **Diagramme de séquence manquant**
   - Suggestion : Ajouter flow auth → validation → création

**Score Technique :** 24/40 (60%) → PLAFOND 11/20 ACTIVÉ  
**Recommandation :** Corrections critiques obligatoires avant approbation.
```

---

## Tone et Style Feedback

- **Direct et factuel** — Pas d'opinion, que des faits
- **Constructif** — Toujours proposer une solution
- **Priorisé** — Erreurs critiques > Problèmes > Suggestions
- **Référencé** — Citer ligne, section, élément précis

---

**Version:** 1.0  
**Type:** Technical Reviewer  
**Sévérité:** Stricte (bloque si erreurs critiques)
