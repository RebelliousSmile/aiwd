# Overview: Guide Administrateur CERASCAN

> **📋 Ce guide en 1 minute :** Documentation opérationnelle pour administrer CERASCAN (création partenaires, import catalogues, génération QR codes). Destiné à Laurent (administrateur métier non-technique). Lecture complète : 3h. Formation : 4-5h sur 3-4 jours. Autonomie 90% opérations quotidiennes garantie.

---

## Objectif du Document

Documentation opérationnelle pour administrer la plateforme CERASCAN multi-tenant : créer des partenaires, importer leurs catalogues, générer les QR codes et résoudre les problèmes courants.

**Workflow typique :** Lancer nouveau partenaire en 20 minutes (création → import CSV 500 produits → génération QR codes → livraison PDFs). Showroom opérationnel en 48h.

## Audience Cible

**Vous = Administrateur CERASCAN**
- Gérez tous les partenaires (création, configuration, support)
- Importez catalogues produits, générez QR codes
- Résolvez problèmes opérationnels quotidiens
- **Compétences :** Usage avancé web, Excel/CSV
- **Délégué au prestataire technique :** Infrastructure serveur, base de données, code source

## Prérequis

**Avant de commencer, Laurent doit avoir :**
- ✅ Accès interface administration CERASCAN (login/password)
- ✅ Excel ou équivalent installé (préparation fichiers CSV)
- ✅ Navigateur web moderne (Chrome, Firefox, Edge récent)
- ✅ Coordonnées support technique (email, téléphone prestataire)

**Connaissances préalables :**
- Notions base de données (comprendre "table", "enregistrement")
- Usage Excel/CSV (ouvrir, modifier, enregistrer fichiers)
- Navigation web avancée (formulaires, upload fichiers)

## Périmètre

### Couvert

**Structure linéaire en 7 chapitres :**

**Chapitre 1 : Comment Utiliser ce Guide** (2-3 pages)
- À propos de ce guide (objectif, périmètre, ce qui est/n'est pas couvert)
- Comment lire (lecture linéaire vs consultation, conventions utilisées)
- Workflow global 4 étapes (schéma : Créer partenaire → Importer CSV → Générer QR → Livrer)
- Contacts et support (prestataire technique, questions fonctionnelles, feedback guide)
- Ressources complémentaires (autres guides, templates, formations)
- **Output-style :** `user-friendly.md` (ton direct, opérationnel)
- **Persona :** `clarity-expert.md` (instructions claires navigation guide)

**Chapitre 2 : Catalogue et Pages du Front** (6-7 pages)
- Gestion catalogue produits (visualisation, recherche, modification)
- Configuration pages front (accueil, catégories, templates)
- Activer/désactiver produits
- Prévisualisation interface visiteur
- **Output-style :** `user-friendly.md` (procédures pas-à-pas)
- **Persona :** `clarity-expert.md` (instructions claires pour opérations)

**Chapitre 3 : Voir ce que vos clients ont aimé** (4-5 pages)
- Consulter les produits sauvegardés par les visiteurs
- Statistiques : quels produits sont les plus populaires
- Exporter les listes (CSV, rapports)
- Supprimer anciennes listes (conformité RGPD)
- **Output-style :** `user-friendly.md` (langage métier, statistiques accessibles)
- **Persona :** `compliance-checker.md` (vérifier conformité RGPD)

**Chapitre 4 : Gestion des Partenaires** (8-10 pages)
- Créer/modifier/supprimer partenaires
- Configuration marque blanche (logo, couleurs, domaine)
- Gestion utilisateurs par partenaire (Sophie, Julie, etc.)
- Rôles et permissions
- Statistiques par partenaire
- **Output-style :** `user-friendly.md` (procédures création détaillées)
- **Persona :** `compliance-checker.md` (exhaustivité étapes, sécurité permissions)

**Chapitre 5 : Page d'Import (CSV / QR Code / PDF)** (8-10 pages)
- Import catalogue CSV (template, validation, procédure)
- Génération QR codes batch (sélection, paramètres)
- Création PDFs impression (formats, organisation)
- Diagnostics erreurs import
- Historique imports
- **Output-style :** `process-documentation.md` (workflows structurés, SOP)
- **Persona :** `compliance-checker.md` (exhaustivité diagnostics erreurs)

**Chapitre 6 : Opérations de Maintenance** (4-5 pages)
- Monitoring santé application (logs, indicateurs)
- Statistiques globales (usage, performances)
- Procédures maintenance courante
- Troubleshooting opérationnel
- Contact support technique
- **Output-style :** `technical-formal.md` (précision technique, indicateurs)
- **Persona :** `technical-reviewer.md` (validité procédures maintenance)

**Chapitre 7 : Guide de Référence Rapide** (10-15 pages)
**A. Tableaux Synthétiques** (7 tableaux essentiels)
1. Permissions par rôle (Admin / Vendeuse / Visiteur : qui peut faire quoi)
2. Format fichier CSV catalogue (colonnes obligatoires/optionnelles + exemples)
3. Codes erreurs import (20 codes fréquents : code → signification → solution)
4. Indicateurs monitoring (10 métriques : nom → valeur normale → seuil alerte)
5. Formats fichiers acceptés (images, CSV, PDFs : extensions, tailles max)
6. Délais opérations (import 500 produits : 2min, génération 1000 QR : 3min, etc.)
7. Contacts support (problème → Laurent résout / Appeler prestataire)

**B. Checklists Opérationnelles** (5 checklists)
- ✅ Créer nouveau partenaire (12 étapes numérotées)
- ✅ Importer catalogue CSV (10 étapes + validations)
- ✅ Générer et imprimer QR codes (8 étapes)
- ✅ Diagnostiquer import échoué (arbre décisionnel 15 branches)
- ✅ Que faire en cas de problème ? (flowchart : symptôme → action)

**C. Glossaire** (30 termes)
- Termes techniques : ExecutionContext, multi-tenant, PWA, IndexedDB, marque blanche
- Vocabulaire métier céramique : Int/Ext, finitions, dimensions, QR code batch
- Acronymes : CSV, PDF, RGPD, SaaS, etc.

**D. Contacts et Support**
- Tableau escalade (15 problèmes types : Laurent / Prestataire)
- Coordonnées support technique (email, téléphone, heures ouverture)
- SLA support (réponse <4h urgent, <24h standard)

**Output-style :** `technical-formal.md` (tableaux structurés, référence précise)
**Persona :** `compliance-checker.md` (exhaustivité référence, aucune info manquante)

**Annexes** (5 pages)
- **Annexe A :** Template CSV catalogue complet (fichier Excel exemple avec 50 produits réels)
- **Annexe B :** Schémas fonctionnels (4 diagrammes annotés)
- **Annexe C :** FAQ administration (20 questions/réponses concrètes)
  1. Le QR code ne scanne pas, que faire ?
  2. L'import CSV échoue avec "Format invalide", pourquoi ?
  3. Comment supprimer un partenaire ?
  4. Peut-on modifier un produit après génération QR codes ?
  5. Combien de temps pour importer 1000 produits ?
  6. Les images produits ne s'affichent pas, pourquoi ?
  7. Comment réinitialiser mot de passe utilisateur ?
  8. Peut-on regénérer QR codes sans réimporter catalogue ?
  9. Quelle taille maximale fichier CSV acceptée ?
  10. Comment exporter statistiques sur 6 mois ?
  (+ 10 autres questions courantes)
- **Output-style :** `user-friendly.md` (questions/réponses accessibles)
- **Persona :** `clarity-expert.md` (réponses claires et actionnables)

### Non couvert
- **Infrastructure technique** (déploiement serveur, configuration Nginx, PM2) — Délégué au prestataire technique
- **Développement** (code source, API interne, modifications techniques) — Hors périmètre administration
- **Migrations base de données** (schéma, indexes, requêtes SQL) — Prestataire uniquement
- **Guide utilisateur final** (visiteurs showroom) — Document séparé si nécessaire
- **Documentation commerciale/marketing** — Hors périmètre technique

## Tone & Style

**Style:** User-friendly avec éléments techniques (hybride)
- Ton accessible et pédagogique (pas de jargon développeur)
- Schémas fonctionnels simplifiés (pas de diagrammes UML)
- Procédures opérationnelles pas-à-pas
- Captures d'écran interface administration
- Exemples concrets (import catalogue réel, génération QR)
- Encadrés "⚠️ Attention" pour opérations sensibles
- Glossaire intégré pour termes techniques inévitables (ExecutionContext, multi-tenant)

## Livrables Attendus

- **Format:** PDF haute-définition (via ICML → InDesign)
- **Longueur estimée:** 53-65 pages (7 chapitres + annexes)
- **Temps de lecture estimé :**
  - **Lecture complète :** 3 heures (tout comprendre)
  - **Chapitres essentiels :** 1h30 (Ch. 1, 4, 5 → créer partenaire + import)
  - **Référence rapide :** 10 minutes (Ch. 7 → tableaux synthétiques)
- **Structure:** 7 chapitres linéaires (vue d'ensemble → sections interface → référence rapide)
- **Compléments:** 
  - **Schémas fonctionnels simplifiés :**
    1. Architecture CERASCAN (vue métier : visiteur → QR → app → catalogue)
    2. Flux multi-tenant (partenaires → bases séparées, schéma conceptuel)
    3. Workflow import catalogue (CSV → validation → import → QR codes)
    4. Processus génération QR codes (sélection produits → génération → PDFs)
  - **Captures d'écran interface admin** (tableau de bord, import CSV, génération QR)
  - **Templates Excel/CSV** (catalogue produits avec exemples)
  - **Checklists opérationnelles** (création partenaire, import catalogue, troubleshooting)

## Contraintes

- **Captures d'écran obligatoires** — Interface administration doit être illustrée (contrairement aux docs purement techniques)
- **Pas de code source** — Aucun snippet JavaScript/SQL (hors périmètre Laurent)
- **Vulgarisation technique** — Expliquer concepts (multi-tenant, ExecutionContext) sans jargon développeur
- **Procédures visuelles** — Screenshots + numérotation étapes (1. Cliquer sur..., 2. Remplir champ...)
- **Glossaire intégré** — Termes techniques inévitables expliqués en français accessible
- **Escalade vers support** — Identifier clairement quand contacter prestataire technique (opérations hors périmètre)

## Métriques de Succès

**Objectifs pédagogiques :**
- ✅ **Après 1h de lecture :** Laurent comprend l'architecture CERASCAN et sait naviguer dans l'interface
- ✅ **Après 2h :** Laurent peut créer un partenaire et importer un catalogue seul
- ✅ **Après 3h :** Laurent est autonome pour 90% des opérations quotidiennes
- ✅ **Référence quotidienne :** Laurent résout un problème en <5 min avec le Chapitre 7

**Indicateurs d'efficacité :**
- Temps création partenaire : **<10 minutes** (avec guide sous les yeux)
- Import catalogue 500 produits : **<15 minutes** (préparation CSV incluse)
- Résolution problème import échoué : **<20 minutes** (diagnostics + correction)
- Appels support technique : **<1 par semaine** (Laurent autonome)

## Parcours de Formation

**Timeline d'apprentissage recommandée :**

**Jour 1 — Découverte (2h)**
1. Lire Chapitre 1 : Vue d'ensemble (30 min)
2. Lire Chapitre 4 : Gestion des partenaires (45 min)
3. Créer partenaire test "Demo Showroom" (20 min pratique)
4. Lire Chapitre 7 : Référence rapide (25 min, survol)

**Jour 2 — Premier Import (1h30)**
1. Lire Chapitre 5 : Page d'Import (45 min)
2. Préparer fichier CSV test (20 produits) (15 min)
3. Importer catalogue test + générer 20 QR codes (30 min pratique)

**Jour 3 — Autonomie (1h)**
1. Lire Chapitres 2, 3, 6 (sections avancées) (45 min)
2. Tester toutes les sections interface (15 min exploration)

**Jour 4+ — Usage Quotidien**
- Consulter Chapitre 7 (référence rapide) selon besoins
- Créer vrais partenaires et importer catalogues réels
- Utiliser FAQ (Annexe C) pour problèmes courants

**Total formation : 4-5 heures réparties sur 3-4 jours**

---

## Maintenance du Guide

**Responsabilité :** Laurent (EFFICERAM) avec support prestataire technique

**Fréquence révision :**
- **Mise à jour mineure :** Tous les 3 mois (corrections, ajouts FAQ)
- **Mise à jour majeure :** Tous les 6-12 mois (nouvelles fonctionnalités, refonte UI)
- **Révision urgente :** Si changement critique interface (breaking change)

**Changelog :**
Version du guide trackée dans fichier séparé `CHANGELOG-guide-admin.md`

**Contributeurs :**
- **Auteur principal :** Laurent (EFFICERAM)
- **Support technique :** Prestataire externe (review sections techniques)
- **Contact révisions :** laurent@efficeram.fr

---

## Métadonnées

| Propriété | Valeur |
|-----------|--------|
| **Version** | 1.0 |
| **Date création** | 2026-02-28 |
| **Auteur** | Laurent (EFFICERAM) |
| **Statut** | Draft |
| **Dernière révision** | 2026-02-28 |
| **Prochaine révision** | 2026-05-28 (3 mois) |
| **Public cible** | Administrateur CERASCAN (profil métier) |
| **Langue** | Français |
| **Format source** | Markdown → ICML → InDesign → PDF |
