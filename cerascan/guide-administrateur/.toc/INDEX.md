# Table des Matières: Guide Administrateur CERASCAN

**Type:** technical-doc  
**Client:** CERASCAN (EFFICERAM)  
**Version:** 1.0  
**Date:** 2026-02-28

---

## Vue d'ensemble

Documentation opérationnelle pour administrer la plateforme CERASCAN multi-tenant. Destiné à Laurent (patron EFFICERAM), administrateur métier non-technique. Le guide couvre la création de partenaires, l'import de catalogues produits, la génération de QR codes et le troubleshooting opérationnel. Lecture complète : 3h. Formation : 4-5h sur 3-4 jours. Autonomie 90% des opérations quotidiennes garantie après formation.

**Exemple concret :** Laurent lance le partenaire "Carrelage Dupont" en 20 minutes : création partenaire (10 min) → import catalogue CSV 500 produits (5 min) → génération 500 QR codes (2 min) → téléchargement PDFs impression. Showroom opérationnel en 48h.

---

## Concepts Principaux

- **Multi-tenant :** Architecture isolant les données par partenaire (tables MySQL séparées : `cerascan_catalog`, `partner808_catalog`, etc.). Chaque partenaire = base catalogue dédiée.
- **ExecutionContext :** Pattern encapsulant le contexte partenaire (partnerId, dbName, partnerName). Propagé à tous les services pour garantir isolation données.
- **Marque blanche :** Personnalisation complète par partenaire (logo, couleurs, domaine personnalisé).
- **QR codes batch :** Génération massive (100-5000 codes) pour lier échantillons showroom aux fiches produits digitales.
- **Rôles :** Admin (Laurent), Admin partenaire (Sophie), Vendeuse (Julie), Visiteur (Marc).

---

## Personas Administrateur

**Laurent - Patron EFFICERAM / Admin CERASCAN**
- Profil NON technique (pas développeur)
- Gère tous les partenaires (création, import, QR codes)
- Résout problèmes opérationnels (diagnostics erreurs import, troubleshooting)
- Délègue infrastructure technique au prestataire externe

**Sophie - Admin Partenaire (utilisatrice finale)**
- Gère son showroom (pas d'administration multi-tenant)
- Utilise interface limitée préparée par Laurent
- Non couverte par ce guide (utilise l'app directement)

---

## Structure

### Partie I : Comprendre (Chapitre 01)
- **Chapitre 01 :** Vue d'ensemble CERASCAN

### Partie II : Administrer (Chapitres 02-06)
- **Chapitre 02 :** Catalogue et Pages du Front
- **Chapitre 03 :** Voir ce que vos clients ont aimé
- **Chapitre 04 :** Gestion des Partenaires
- **Chapitre 05 :** Page d'Import (CSV / QR Code / PDF)
- **Chapitre 06 :** Opérations de Maintenance

### Partie III : Référencer (Chapitre 07)
- **Chapitre 07 :** Guide de Référence Rapide

### Annexes
- **Annexe A :** Template CSV Catalogue
- **Annexe B :** Schémas Fonctionnels
- **Annexe C :** FAQ Administration

---

## Chapitres

### Chapitre 01 : Comment Utiliser ce Guide

**Synopsis :**  
Mode d'emploi de ce guide : objectif (manuel opérationnel administration), comment lire (lecture linéaire vs consultation rapide), conventions utilisées. Workflow global en 4 étapes (créer partenaire → importer CSV → générer QR → livrer PDFs). Contacts support technique (prestataire externe) et fonctionnel. Procédure feedback sur le guide. Ressources complémentaires (autres guides CERASCAN, templates, formations).

**Points clés :**
- Objectif guide : Manuel opérationnel administration multi-tenant (pas présentation CERASCAN, vous connaissez déjà)
- Comment lire : Lecture complète (3h formation) ou consultation rapide par besoin (index, recherche)
- Conventions document : 💡 Astuce, ⚠️ Attention, ✅ Bonne pratique, 📋 Exemple concret
- Workflow global 4 étapes : Créer partenaire (10 min, Ch4) → Importer catalogue (5 min, Ch5) → Générer QR (2 min, Ch5) → Livrer PDFs (3 min)
- Contacts support technique : Prestataire externe (serveur, DB, bugs code), email/téléphone, délai <4h urgent
- Feedback guide : Signaler erreurs, procédures floues, suggestions amélioration (doc-cerascan@efficeram.fr)
- Ressources : Guide Partenaire (showrooms), Guide Commercial (vendeuses), templates CSV (Annexe A), formations disponibles

**Longueur estimée :** 2-3 pages  
**Output-style :** `user-friendly.md` (ton direct, opérationnel, pas pédagogique)  
**Persona :** `clarity-expert.md` (navigation claire, contacts accessibles)

---

### Chapitre 02 : Catalogue et Pages du Front

**Synopsis :**  
Gestion du catalogue produits via l'interface administration : visualiser, rechercher, modifier et activer/désactiver produits. Configuration des pages front (accueil, catégories, templates d'affichage) pour personnaliser l'expérience visiteur. Prévisualisation de l'interface visiteur avant mise en ligne pour vérifier cohérence visuelle et navigation.

**Points clés :**
- Visualiser catalogue partenaire : liste produits avec filtres (catégorie, prix, disponibilité), recherche textuelle, tri (alphabétique, prix, date ajout)
- Modifier produits individuels : éditer fiche produit (nom, description, prix, dimensions, images), corriger erreurs, ajouter infos manquantes
- Activer/désactiver produits en masse : sélection multiple, activation/désactivation temporaire (ex: rupture stock), suppression définitive (confirmation requise)
- Configuration pages front : paramétrer page accueil (bannière, produits mis en avant), organiser hiérarchie catégories (Int/Ext, finitions, dimensions)
- Personnaliser templates affichage : choisir champs affichés sur fiches produits (prix visible/masqué selon rôle utilisateur), layout grilles/liste
- Prévisualiser interface visiteur : mode aperçu temps réel, tester parcours utilisateur (scan QR fictif → fiche produit), valider avant publication

**Longueur estimée :** 6-7 pages  
**Output-style :** `user-friendly.md` (procédures pas-à-pas avec screenshots)  
**Persona :** `clarity-expert.md` (instructions claires, opérations intuitives)

---

### Chapitre 03 : Voir ce que vos clients ont aimé

**Synopsis :**  
Consultation des listes d'envie sauvegardées par les visiteurs showroom. Analyse statistique des produits les plus populaires pour identifier tendances et optimiser catalogue. Export des données pour rapports commerciaux. Gestion conformité RGPD : suppression anciennes listes, anonymisation données visiteurs.

**Points clés :**
- Consulter listes d'envie visiteurs : accès listes anonymisées (pas de données personnelles), voir produits sauvegardés par session visiteur, timestamps consultation
- Statistiques produits populaires : top 20 produits les plus ajoutés aux listes, répartition par catégorie (Int/Ext, finitions), tendances temporelles (semaine, mois)
- Taux conversion liste → achat : si intégration CRM, suivi produits liste transformés en ventes réelles, calcul ROI showroom digital
- Export rapports : télécharger CSV listes d'envie (produits, dates, partenaire), générer rapports PDF pour direction, intégration Excel pour analyses
- Conformité RGPD : suppression automatique listes >6 mois (paramétrable), anonymisation données visiteurs (pas de tracking nominatif), consentement cookies obligatoire
- Gestion données personnelles : droit oubli (suppression manuelle si demande visiteur), export données utilisateur (si compte créé), registre traitements RGPD

**Longueur estimée :** 4-5 pages  
**Output-style :** `user-friendly.md` (langage métier, statistiques accessibles)  
**Persona :** `compliance-checker.md` (vérifier exhaustivité procédures RGPD, conformité légale)

---

### Chapitre 04 : Gestion des Partenaires

**Synopsis :**  
Création, modification et suppression de partenaires (franchises, distributeurs). Configuration marque blanche complète : upload logo, personnalisation couleurs, domaine personnalisé. Gestion des utilisateurs par partenaire : création comptes (Sophie admin, Julie vendeuse), attribution rôles et permissions. Consultation statistiques d'usage par partenaire pour monitoring activité.

**Points clés :**
- Créer nouveau partenaire : procédure complète 12 étapes (nom commercial, identifiant unique, coordonnées contact, création base MySQL dédiée), génération ExecutionContext partenaire, validation configuration
- Configuration marque blanche : upload logo partenaire (formats acceptés: PNG, SVG, dimensions recommandées 500x200px), personnaliser palette couleurs (primaire, secondaire, accents), configurer domaine personnalisé (ex: carrelage-dupont.cerascan.app)
- Créer utilisateurs partenaire : ajouter Sophie (rôle Admin partenaire, accès gestion catalogue/QR codes), ajouter Julie (rôle Vendeuse, accès infos privilégiées showroom), définir mots de passe temporaires
- Rôles et permissions : tableau permissions détaillé (Admin multi-tenant: tout, Admin partenaire: catalogue+stats+users, Vendeuse: consultation infos privilégiées, Visiteur: consultation publique)
- Statistiques par partenaire : nombre scans QR codes (jour/semaine/mois), produits consultés (top 20), utilisateurs actifs (Sophie/Julie), taux conversion listes d'envie
- Modifier/supprimer partenaire : éditer infos partenaire (coordonnées, logo, couleurs), suspendre temporairement (désactivation sans suppression données), supprimer définitivement (confirmation + backup obligatoire)

**Longueur estimée :** 8-10 pages  
**Output-style :** `user-friendly.md` (procédures création détaillées, screenshots chaque étape)  
**Persona :** `compliance-checker.md` (exhaustivité étapes création, sécurité permissions, aucun oubli)

---

### Chapitre 05 : Page d'Import (CSV / QR Code / PDF)

**Synopsis :**  
Import de catalogues produits via fichiers CSV : template Excel fourni, validation temps réel, procédure complète. Génération batch de QR codes : sélection produits, paramétrage, suivi progression. Création PDFs impression : formats disponibles, organisation fichiers, téléchargement. Diagnostics erreurs import : 20 codes erreurs fréquents avec solutions. Historique imports pour traçabilité.

**Points clés :**
- Template CSV catalogue : fichier Excel exemple 50 produits réels, colonnes obligatoires (nom, référence, prix, catégorie) vs optionnelles (description, dimensions, finitions), formats acceptés (texte UTF-8, nombres décimaux, images URLs)
- Procédure import CSV : upload fichier (taille max 10 MB, 5000 produits max), validation automatique temps réel (colonnes manquantes, formats incorrects, doublons références), preview 20 premières lignes avant confirmation
- Traitement import : barre progression (estimation temps: 500 produits = 2 min, 1000 produits = 4 min), logs détaillés (lignes insérées, warnings, erreurs), validation finale (vérifier compteurs produits catalogue)
- Génération QR codes batch : sélection produits (tous, par catégorie, par filtre prix), paramètres génération (taille QR: small/medium/large, format image: PNG/SVG, numérotation: auto/manuelle), lancement batch (suivi progression temps réel)
- Création PDFs impression : téléchargement automatique après génération (organisation: 1 PDF par lot 100 codes), formats disponibles (A4 grille 10x10, étiquettes adhésives 50x30mm), aperçu avant impression
- Diagnostics erreurs import : 20 codes fréquents (E001: colonne manquante, E002: format prix invalide, E003: référence doublon, E010: encodage incorrect UTF-8, etc.), tableau code → message → cause → solution pas-à-pas
- Historique imports : liste tous imports (date, partenaire, nb produits, statut succès/échec), télécharger logs détaillés (CSV erreurs pour correction), réimporter fichier corrigé

**Longueur estimée :** 8-10 pages  
**Output-style :** `process-documentation.md` (workflows structurés SOP, étapes numérotées claires)  
**Persona :** `compliance-checker.md` (exhaustivité diagnostics erreurs, tous cas couverts)

---

### Chapitre 06 : Opérations de Maintenance

**Synopsis :**  
Monitoring santé application : consultation logs système, indicateurs performances temps réel. Statistiques globales multi-tenant : usage tous partenaires, produits les plus consultés, pics d'utilisation. Procédures maintenance courante : vider caches, régénérer indexes, nettoyer logs. Troubleshooting opérationnel : problèmes récurrents avec solutions. Identification quand contacter support technique prestataire.

**Points clés :**
- Tableau de bord monitoring : indicateurs santé temps réel (serveur: vert/orange/rouge, base données: temps réponse <100ms, cache IndexedDB: taux hit >80%), alertes automatiques (email si seuil dépassé)
- Consulter logs système : interface web logs (pas SSH), filtres par niveau (info, warning, error, critical), recherche textuelle, export CSV logs période définie
- Métriques performances : temps chargement catalogue (objectif <2s pour 3000 produits), temps génération QR batch (objectif 1000 codes en 3 min), taux disponibilité PWA offline (objectif >95%)
- Statistiques globales multi-tenant : nombre total partenaires actifs, total produits catalogues tous partenaires, total scans QR codes (tous showrooms), top 50 produits consultés (agrégation multi-tenant)
- Maintenance courante Laurent : vider cache application (si problème affichage), régénérer indexes recherche (si résultats incohérents), nettoyer logs >3 mois (libérer espace disque)
- Troubleshooting problèmes récurrents : QR codes ne scannent pas (causes: impression floue, liens cassés, cache mobile), import CSV échoue (causes: encodage, colonnes, format prix), application lente (causes: cache plein, trop produits actifs)
- Contacter support technique : arbre décisionnel (Laurent résout: cache, logs, imports simples | Prestataire résout: serveur, base données, migrations, PWA), informations à fournir (logs, captures écran, étapes reproduction), SLA support (réponse <4h urgent, <24h standard)

**Longueur estimée :** 4-5 pages  
**Output-style :** `technical-formal.md` (indicateurs techniques précis, métriques chiffrées)  
**Persona :** `technical-reviewer.md` (validité procédures maintenance, pertinence indicateurs)

---

### Chapitre 07 : Guide de Référence Rapide

**Synopsis :**  
Référence rapide pour consultation quotidienne sans relire guide complet. Sept tableaux synthétiques : permissions rôles, format CSV, codes erreurs, indicateurs monitoring, formats fichiers, délais opérations, contacts support. Cinq checklists opérationnelles : création partenaire, import CSV, génération QR, diagnostics erreurs, escalade problèmes. Glossaire 30 termes techniques. Contacts support avec SLA.

**Points clés :**
- Tableau 1 - Permissions par rôle : Admin multi-tenant (Laurent: tout), Admin partenaire (Sophie: catalogue+stats+users), Vendeuse (Julie: infos privilégiées), Visiteur (Marc: consultation publique), matrice détaillée qui peut faire quoi
- Tableau 2 - Format CSV catalogue : 15 colonnes (5 obligatoires: nom, référence, prix, catégorie, image | 10 optionnelles: description, dimensions, finitions, Int/Ext, stock, etc.), types données (texte, nombre, URL), exemples valides
- Tableau 3 - Codes erreurs import : 20 codes fréquents (E001-E020), structure code → message français → cause technique → solution actionnable pas-à-pas, exemples fichiers incorrects vs corrigés
- Tableau 4 - Indicateurs monitoring : 10 métriques (temps réponse serveur, CPU, mémoire, DB connexions, cache hit rate, etc.), valeur normale, seuil warning, seuil critique, action corrective
- Tableau 5 - Formats fichiers acceptés : images (JPEG, PNG, WebP, max 2MB), CSV (UTF-8, max 10MB), PDFs (A4, max 50MB), extensions autorisées, rejets automatiques
- Tableau 6 - Délais opérations : import 500 produits (2 min), import 1000 produits (4 min), génération 1000 QR codes (3 min), création partenaire (10 min avec guide), estimations temps Laurent
- Tableau 7 - Contacts support : 15 problèmes types (problème → Laurent résout seul / Appeler prestataire), coordonnées support (email, téléphone, heures ouverture 9h-18h), SLA (<4h urgent, <24h standard)
- Checklist 1 - Créer partenaire : 12 étapes numérotées (infos obligatoires, configuration, validation), temps estimé 10 min, pièges à éviter
- Checklist 2 - Importer catalogue : 10 étapes (préparer CSV, valider template, upload, traiter, vérifier), temps estimé 15 min pour 500 produits
- Checklist 3 - Générer QR codes : 8 étapes (sélectionner produits, paramétrer, générer batch, télécharger PDFs, imprimer), temps estimé 5 min pour 500 codes
- Checklist 4 - Diagnostiquer import échoué : arbre décisionnel 15 branches (symptôme → vérifications → solution), flowchart visuel
- Checklist 5 - Que faire problème : flowchart symptôme → action Laurent / escalade prestataire, procédure collecte infos (logs, screenshots, étapes reproduction)
- Glossaire 30 termes : ExecutionContext, multi-tenant, PWA, IndexedDB, marque blanche, SaaS, RGPD, QR code batch, CSV, pool connexions, cache, service worker, Alpine.js, composite ID, etc. (définitions français accessible)
- Contacts et SLA : email support (support@efficeram.fr), téléphone (01 XX XX XX XX), heures ouverture (9h-18h lundi-vendredi), SLA urgent <4h (blocage total), standard <24h (problème non-bloquant)

**Longueur estimée :** 10-15 pages  
**Output-style :** `technical-formal.md` (tableaux structurés précis, référence exhaustive)  
**Persona :** `compliance-checker.md` (exhaustivité référence, aucune info manquante, tous cas couverts)

---

## Annexes

### Annexe A : Template CSV Catalogue Complet

**Synopsis :**  
Fichier Excel exemple avec 50 produits céramique réels (anonymisés). Explication détaillée colonne par colonne : nom, type, format, exemples valides/invalides. Guide préparation fichier : collecter données fournisseur, uniformiser formats, vérifier cohérence. Cas d'usage : imports initiaux (catalogue complet), imports incrémentaux (nouveaux produits), mises à jour (corrections prix/stock).

**Points clés :**
- Fichier Excel téléchargeable : 50 produits exemple (carrelage Int/Ext, dimensions variées, prix réalistes), structure complète 15 colonnes, données cohérentes prêtes à importer
- Colonnes obligatoires détaillées : nom (texte 100 car max), référence (alphanumérique unique 20 car), prix (décimal format XX.XX euros), catégorie (liste fixe: Int/Ext/Mural/Sol), image (URL https:// valide)
- Colonnes optionnelles détaillées : description (texte 500 car), dimensions (format LxlxH en cm), finitions (mat/brillant/satiné), stock (nombre entier), poids (kg décimal), origine (pays ISO), certification (CE/NF)
- Formats acceptés : texte UTF-8 (pas ISO-8859-1), nombres décimaux avec point (pas virgule), dates ISO (YYYY-MM-DD), URLs complètes (https://), booléens (0/1 ou true/false)
- Exemples valides vs invalides : prix valide "49.90" vs invalide "49,90€", référence valide "CAR-INT-001" vs invalide "Carrelage 1" (espaces), image valide "https://cdn.example.com/product.jpg" vs invalide "product.jpg" (chemin relatif)

**Longueur estimée :** 2 pages  
**Output-style :** `user-friendly.md` (guide pratique Excel, exemples concrets)  
**Persona :** `clarity-expert.md` (instructions claires préparation fichier)

---

### Annexe B : Schémas Fonctionnels

**Synopsis :**  
Quatre diagrammes annotés illustrant architecture et workflows CERASCAN. Schéma 1 : Architecture globale (visiteur → QR → app → catalogue → base partenaire). Schéma 2 : Flux multi-tenant (isolation bases MySQL par partenaire). Schéma 3 : Workflow import catalogue (CSV → validation → parsing → insertion → QR codes). Schéma 4 : Processus génération QR codes batch (sélection → génération → PDFs → impression).

**Points clés :**
- Schéma 1 - Architecture CERASCAN : visiteur showroom → scan QR code physique → app mobile PWA → requête API (ExecutionContext) → catalogue partenaire (MySQL) → fiche produit affichée, annotations acteurs (Marc, Julie, Sophie, Laurent)
- Schéma 2 - Multi-tenant : table système `partners` → bases catalogues séparées (cerascan_catalog, partner808_catalog, testing_catalog) → isolation stricte (aucune requête cross-partenaire), ExecutionContext garantit routing correct
- Schéma 3 - Import catalogue : Laurent upload CSV → validation colonnes/formats → parsing lignes → insertion produits base partenaire → indexation recherche → notification succès/erreurs, chemins erreurs annotés
- Schéma 4 - Génération QR codes : Laurent sélectionne produits (filtres) → paramétrage (taille, format) → batch génération (boucle produits) → création PDFs (grilles 10x10) → téléchargement → impression showroom, estimations temps annotées

**Longueur estimée :** 2 pages (1 page = 2 schémas)  
**Output-style :** `user-friendly.md` (diagrammes simplifiés, annotations accessibles)  
**Persona :** `clarity-expert.md` (schémas compréhensibles non-technique)

---

### Annexe C : FAQ Administration

**Synopsis :**  
Vingt questions/réponses fréquentes Laurent. Problèmes techniques (QR ne scannent pas, import échoue, images manquantes), questions opérationnelles (modifier produit après QR, délais imports, taille fichiers), questions sécurité (réinitialiser mots de passe, supprimer partenaire, exporter statistiques). Réponses actionnables avec étapes concrètes et références chapitres guide.

**Points clés :**
- Q1 : Le QR code ne scanne pas, que faire ? → Réponse : vérifier qualité impression (résolution min 300 DPI), tester scan avec plusieurs smartphones, régénérer QR code si corrompu (Chapitre 5, section "Regénérer QR codes")
- Q2 : Import CSV échoue avec "Format invalide", pourquoi ? → Réponse : vérifier encodage UTF-8 (pas ISO-8859-1), vérifier colonnes obligatoires présentes, vérifier format prix (point pas virgule), consulter code erreur (Annexe A, tableau codes erreurs)
- Q3 : Comment supprimer un partenaire ? → Réponse : Chapitre 4 section "Supprimer partenaire", procédure : backup obligatoire → confirmation double → suppression base catalogue → archivage logs, irréversible après 30 jours
- Q4 : Peut-on modifier un produit après génération QR codes ? → Réponse : Oui, modifications produit (prix, description, images) propagées automatiquement, QR codes restent valides (liens stables), pas besoin régénérer sauf changement référence produit
- Q5 : Combien de temps pour importer 1000 produits ? → Réponse : ~4 minutes (estimation), dépend connexion internet (upload CSV), charge serveur, validation temps réel peut ralentir si nombreuses erreurs (Chapitre 5, tableau délais opérations)
- Q6 : Les images produits ne s'affichent pas, pourquoi ? → Réponse : vérifier URLs images accessibles (https:// publiques), vérifier formats acceptés (JPEG, PNG, WebP max 2MB), vérifier cache navigateur (Ctrl+F5), consulter logs erreurs (Chapitre 6)
- Q7 : Comment réinitialiser mot de passe utilisateur ? → Réponse : Chapitre 4 section "Gestion utilisateurs", procédure : accéder liste utilisateurs partenaire → clic utilisateur → "Réinitialiser mot de passe" → mot de passe temporaire généré → envoyer par email utilisateur
- Q8 : Peut-on regénérer QR codes sans réimporter catalogue ? → Réponse : Oui, Chapitre 5 section "Regénérer QR codes", sélectionner produits existants → générer nouveaux codes (si QR abîmés/perdus), anciens codes restent valides sauf suppression manuelle
- Q9 : Quelle taille maximale fichier CSV acceptée ? → Réponse : 10 MB, ~5000 produits max (dépend colonnes remplies), si catalogue >5000 produits, faire imports multiples (découper par catégories), Annexe A "Template CSV"
- Q10 : Comment exporter statistiques sur 6 mois ? → Réponse : Chapitre 3 section "Export rapports", sélectionner période (dates début/fin), choisir format (CSV, PDF), télécharger rapport (listes d'envie, produits consultés, scans QR codes)
- Q11-Q20 : (questions opérationnelles supplémentaires) Créer plusieurs partenaires simultanément ? Limiter accès vendeuse à certains produits ? Changer logo partenaire après création ? Consulter logs imports anciens ? Désactiver temporairement partenaire ? Modifier permissions utilisateur ? Générer QR codes formats personnalisés ? Importer produits avec variantes ? Exporter catalogue partenaire ? Contacter support en urgence ?

**Longueur estimée :** 1 page (20 Q/R compactes, références chapitres)  
**Output-style :** `user-friendly.md` (questions naturelles, réponses actionnables)  
**Persona :** `clarity-expert.md` (réponses claires, liens utiles vers chapitres)

---

## Métadonnées

**Document généré par :** generate-toc v1.3  
**Source :** overview.md + documentation CERASCAN (.docs/)  
**Total pages estimé :** 53-65 pages (7 chapitres + 3 annexes)  
**Temps lecture complet :** 3 heures  
**Temps formation :** 4-5 heures sur 3-4 jours  
**Prochaine étape :** Génération toc-chapter<XX>.md détaillés (optionnel)

---

## Notes Production

- **Output-styles utilisés :** user-friendly (dominance), process-documentation (Ch. 5), technical-formal (Ch. 6-7)
- **Personas review :** clarity-expert (compréhensibilité), compliance-checker (exhaustivité RGPD/erreurs), technical-reviewer (validité technique)
- **Schémas à produire :** 4 diagrammes fonctionnels (Annexe B), captures écran interface (tous chapitres sauf 7)
- **Templates à fournir :** Fichier Excel catalogue 50 produits (Annexe A)
- **Métriques succès :** Laurent autonome 90% après 3h lecture, <1 appel support/semaine, création partenaire <10 min, import 500 produits <15 min
