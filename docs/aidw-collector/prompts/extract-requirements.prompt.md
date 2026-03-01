---
name: extract-requirements
description: Extrait les spécifications fonctionnelles (features, use cases, règles métier) d'un projet et produit requirements.md. Lancé uniquement si des fichiers de user stories, BDD features, ou spécifications fonctionnelles sont détectés.
version: 1.0
---

# Extract Requirements

À partir du contexte d'inventaire partagé par `aidw-collector` (Step 0) et des fichiers du projet, produis `aidw-collector/dest/requirements.md`.

> **Prérequis :** lancé uniquement si `aidw-collector` a détecté des specs (`user-stories/`, `*.feature`, `specs/`, `requirements*`, `SPECS*`, `docs/` avec sections "Features" ou "Use Cases"). L'objectif est de documenter **ce que le système fait** du point de vue métier — pas comment il est codé.

## Sources à lire

Dans cet ordre de priorité :

1. **Fichiers Gherkin / BDD** — user stories formalisées (`*.feature`, `features/`, `specs/`) → chaque `Feature:` = une feature, chaque `Scenario:` = un use case
2. **Fichiers de user stories** (`user-stories*.md`, `stories/`, `docs/stories/`) → format épique/story/critère d'acceptation
3. **Issues exportées** (`issues.csv`, `backlog.md`, `BACKLOG.md`) → tickets avec labels "feature", "enhancement", "user story"
4. **README — section fonctionnelle** — sections "Features", "Fonctionnalités", "Ce que fait le projet", "Cas d'usage"
5. **Documentation métier** (`docs/`, `documentation/`, `memory-bank/functional/`) — specs, processus, règles métier
6. **Tests d'intégration / unitaires nommés métier** — `describe("User can...")`, `it("should allow...")` → révèlent des use cases implicites

## Ce qu'il faut extraire

**Une feature = une capacité métier distincte** pouvant servir à plusieurs types d'utilisateurs.

Exemples de features à rechercher :
- Authentification / gestion des comptes
- Import / export de données
- Recherche et filtrage
- Notifications / alertes
- Génération de rapports / exports
- Gestion multi-tenant / multi-organisation
- Intégration externe (API, webhook, SSO)

**Ne pas inclure :**
- Détails d'implémentation (endpoints, schémas de base de données, algorithmes)
- Configuration technique (variables d'environnement, dépendances)
- Features purement internes sans impact utilisateur visible

## Output : `aidw-collector/dest/requirements.md`

Charger `@aidw-collector/templates/requirements.md` et :
- Rédiger la **Vue d'Ensemble Fonctionnelle** (3-5 lignes) depuis le README ou les docs
- Dupliquer le bloc délimité par `<!-- BLOC FEATURE -->` et `<!-- FIN BLOC FEATURE -->` — **un bloc par feature principale**
- Ordonner par priorité décroissante (Haute → Moyenne → Basse)
- Numéroter séquentiellement : F-01, F-02, F-03…
- Compléter la section **Contraintes Non-Fonctionnelles** si documentées (performance, sécurité, compatibilité)
- Remplacer chaque `[À COMPLÉTER]` par les données extraites
- Ne laisser `[À COMPLÉTER]` que si l'information est réellement absente des sources

## Règles

1. **Fonctionnel, pas technique** — "Importer un catalogue CSV" (feature), pas "Parser CSV avec multer et insérer en MySQL" (implémentation)
2. **Use cases depuis l'usage réel** — formuler en `Qui fait quoi pour atteindre quel objectif`
3. **Règles de gestion depuis le code ou les docs** — uniquement les règles trouvées explicitement dans les sources, pas d'inférence
4. **Priorité depuis les sources** — si non documentée, inférer depuis la fréquence d'utilisation ou l'importance dans le README
5. **Statut réaliste** — "Implémenté" si le code existe et fonctionne, "Partiel" si incomplet, "Planifié" si doc seulement
6. **Volume maîtrisé** — si > 15 features détectées, regrouper les features similaires ou documenter uniquement les Haute priorité, les autres en tableau synthétique
7. **Ne jamais inventer** — information absente → `[À COMPLÉTER]`
