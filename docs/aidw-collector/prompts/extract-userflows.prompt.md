---
name: extract-userflows
description: Extrait les flux utilisateurs (parcours, tâches, user stories) d'une application UI et produit userflows.md. Lancé uniquement si une UI a été détectée. Se concentre sur ce que l'utilisateur FAIT, pas sur comment le code est structuré.
version: 1.0
---

# Extract Userflows

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/userflows.md`.

> **Prérequis :** lancé uniquement si `aidw-collector` a détecté une UI. L'objectif est de documenter des **tâches utilisateur** traversant potentiellement plusieurs écrans — pas d'inventorier les écrans (c'est le rôle de `extract-screens`).

## Sources à lire

Dans cet ordre de priorité :

1. **Tests end-to-end** — scénarios comportementaux les plus précis (`cypress/`, `playwright/`, `e2e/`, `tests/e2e/`)
   - Chaque `describe` / `it` / `test` avec un nom métier → un flux potentiel
   - Les `cy.visit()`, `page.goto()` révèlent les enchaînements d'écrans
2. **Fichiers Gherkin / BDD** — user stories formalisées (`*.feature`, `features/`, `specs/`)
   - Format `Given / When / Then` → traduit directement en étapes utilisateur
3. **Tests d'intégration nommés métier** (si e2e absents) — `*.spec.ts`, `*_test.py` avec des noms de scénario ("should allow user to...", "when user does...")
4. **Fichiers de user stories** (`user-stories/`, `stories/`, `docs/stories/`, issues exportées)
5. **README** — sections "Getting Started", "How to use", "Usage", "Tutoriel"
6. **`dest/screens-*.md`** si déjà produit — pour connaître les noms d'écrans et routes, et reconstituer les enchaînements
7. **Fallback (si aucune des sources précédentes n'est disponible)** — lire dans cet ordre :
   - `dest/requirements.md` si disponible — les use cases révèlent des flux implicites
   - Fichiers de processus métier (`docs/functional/`, `memory-bank/functional/`, `business-processes.md`)
   - Fichiers de personas / utilisateurs (`users.md`, `personas.md`) — parcours utilisateurs décrits
   - Routes + vues UI — reconstituer les flux depuis l'enchaînement routes + controllers (dernier recours)

## Ce qu'il faut extraire

**Un flux = une tâche utilisateur avec un objectif clair**, qui peut traverser plusieurs écrans.

Exemples de flux à rechercher :
- Inscription / connexion / récupération de mot de passe
- Création / modification / suppression d'un objet métier principal
- Processus multi-étapes (formulaire en plusieurs pages, wizard)
- Export / import de données
- Gestion de profil / paramètres
- Flux de validation / approbation
- Recherche et filtrage
- Consultation d'un rapport / tableau de bord

**Ne pas inclure :**
- Détails d'implémentation (appels API, state management, queries)
- Flux purement techniques (migrations, scripts admin)
- Comportements de composants isolés (clic sur un bouton sans contexte)

## Output : `aidw-collector/dest/userflows-[groupe].md`

Charger `@aidw-collector/templates/userflows.md` et :
- Remplacer le tableau des rôles avec les rôles identifiés (depuis `dest/access-matrix.md` si disponible, sinon depuis les sources)
- Dupliquer le bloc délimité par `<!-- BLOC FLUX -->` et `<!-- FIN BLOC FLUX -->` — **un bloc par tâche utilisateur**
- Ajouter des lignes dans le tableau des étapes selon la complexité réelle du flux (pas de limite)
- Ordonner les flux du plus fréquent / fondamental au plus rare / avancé
- Remplacer chaque `[À COMPLÉTER]` par les données extraites
- Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente des sources

**Règle de split :** si l'application a plus de 8 flux utilisateurs OU si un fichier dépasserait 250 lignes, produire plusieurs fichiers thématiques. Exemples de découpe naturelle :
- `userflows-client.md` (flux des utilisateurs finaux : consultation, sélection, partage)
- `userflows-admin.md` (flux d'administration : gestion, import, configuration)
- `userflows-[rôle].md` (nommé par rôle si les flux sont clairement segmentés par rôle)

Chaque fichier inclut le **tableau des rôles** en en-tête + les blocs FLUX correspondants.

## Règles

1. **Tâche, pas écran** — chaque flux décrit ce que l'utilisateur VEUT ACCOMPLIR, pas comment l'écran est organisé
2. **Nommer depuis l'usage** — "Envoyer une facture", pas "Cliquer sur le bouton Envoyer de la page Invoice"
3. **Étapes = verbes utilisateur** — "Saisir", "Sélectionner", "Cliquer sur", "Valider", jamais "Le système appelle l'API"
4. **Erreurs depuis les sources** — uniquement les messages trouvés dans i18n, tests ou templates — sinon `[À COMPLÉTER]`
5. **Flux minimum** — si aucune source de flux n'est trouvée (pas de tests e2e, pas de feature files, pas de README usage), produire un flux squelette par module détecté en Step 0 et marquer tous les champs `[À COMPLÉTER]`
6. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
