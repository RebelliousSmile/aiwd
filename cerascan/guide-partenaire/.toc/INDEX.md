# Table des Matières: Guide Partenaire CERASCAN

**Type:** user-guide  
**Client:** CERASCAN (EFFICERAM)  
**Version:** 1.0  
**Date:** 2026-02-28

---

## Vue d'ensemble

Guide technique pour propriétaires de showrooms partenaires CERASCAN. Accompagne la mise en place complète : préparation catalogue produits au format CSV (étape complexe nécessitant rigueur), import via interface partenaire, téléchargement QR codes, impression et pose en showroom. Ton technique patient, pas de promesses temporelles. Focus sur complexité correspondances nomenclature produits. Facturation débute uniquement après pose premières étiquettes QR.

**Important :** Prenez le temps nécessaire pour préparer correctement votre fichier CSV. La correspondance entre vos données produits et le format CERASCAN demande rigueur et patience.

---

## Concepts Principaux

- **Interface partenaire :** Espace web dédié pour uploader CSV catalogue, télécharger PDFs QR codes, gérer comptes commerciaux
- **Template CSV :** Fichier Excel structure prédéfinie (colonnes obligatoires : nom, référence, prix, catégorie)
- **Correspondances nomenclature :** Mapping entre catégories produits partenaire et catégories CERASCAN standardisées
- **QR codes batch :** Génération massive (100-2000 codes) en une opération, organisation grilles 10×10 (100 codes/page A4)
- **Validation import :** Vérification automatique colonnes obligatoires, doublons références, encodage UTF-8
- **Alertes email :** Notifications automatiques succès/erreurs import, QR codes prêts téléchargement

---

## Audience

**Vous = Propriétaire/Responsable showroom partenaire**
- Compétences : Excel/CSV, navigation web, impression documents
- Avez souscrit abonnement CERASCAN (pas encore facturé avant pose QR)
- Travail principal : préparer catalogue CSV (étape complexe), importer, récupérer QR codes, poser showroom

---

## Structure

### Chapitres Opérationnels (7)

1. **Comment Utiliser ce Guide** (2-3 pages)
2. **Préparer Votre Catalogue CSV** (5-7 pages)
3. **Importer Votre Catalogue** (3-4 pages)
4. **Récupérer Vos QR Codes** (2-3 pages)
5. **Imprimer et Poser en Showroom** (5-6 pages)
6. **Former Votre Équipe Commerciale** (2-3 pages)
7. **Mises à Jour et Support** (2-3 pages)

### Annexes (4)

- **Annexe A :** Template CSV Catalogue Vierge
- **Annexe B :** Checklist Validation Catalogue (15 points)
- **Annexe C :** Spécifications Techniques Impression QR
- **Annexe D :** Schémas Placement Showroom Types

---

## Chapitres

### Chapitre 01 : Comment Utiliser ce Guide

**Synopsis :**  
Mode d'emploi de ce guide technique. Objectif : accompagner mise en place CERASCAN showroom (pas convaincre, vous avez déjà souscrit). Comment lire : lecture linéaire (apprentissage complet) ou consultation par besoin (référence rapide). Workflow global 5 étapes : Préparer CSV (étape la plus longue) → Importer catalogue → Récupérer QR codes → Imprimer/poser showroom → Former commerciaux. Contacts support : administrateur CERASCAN uniquement (pas prestataire direct). Ressources complémentaires : Guide Commercial Showroom, templates, FAQ.

**Points clés :**
- Objectif guide : Accompagnement technique mise en place CERASCAN (ton patient, pas commercial)
- Comment lire : Lecture complète ou consultation rapide par besoin (sommaire interactif, index, recherche PDF)
- Conventions document : 💡 Astuce, ⚠️ Attention, ✅ Bonne pratique, captures écran annotées
- Workflow global 5 étapes : Préparer CSV (complexe, plusieurs heures à jours) → Importer (10 min) → Récupérer QR (notification email) → Imprimer/poser (variable) → Former équipe (30-45 min/commercial)
- Contact support : Administrateur CERASCAN (admin-cerascan@efficeram.fr, Lun-Ven 9h-18h) pour toute question technique, préparation CSV, erreurs import
- Ressources : Guide Commercial Showroom (formation commerciaux), template CSV (Annexe A), checklist validation (Annexe B), schémas placement (Annexe D)

**Longueur estimée :** 2-3 pages  
**Output-style :** `user-friendly.md` (ton technique accessible)  
**Persona :** `clarity-expert.md` (navigation guide claire, contacts explicites)

---

### Chapitre 02 : Préparer Votre Catalogue CSV

**Synopsis :**  
Étape la plus complexe et chronophage du workflow. Comprendre correspondances entre votre nomenclature produits actuelle et structure CERASCAN. Télécharger template CSV vierge depuis interface partenaire. Comprendre colonnes obligatoires (nom, référence, prix, catégorie) vs optionnelles (dimensions, finitions, stock, fournisseur). Mapper vos catégories vers catégories CERASCAN standardisées (Int/Ext, finitions). Remplir fichier Excel : bonnes pratiques encodage UTF-8, format colonnes, gestion caractères spéciaux. Collecter images produits (JPG/PNG 1200×1200px min, nomenclature = références produits). Checklist validation 15 points avant import. Temps nécessaire : plusieurs heures à plusieurs jours selon taille catalogue et qualité données existantes.

**Points clés :**
- Complexité étape CSV : Correspondances nomenclature entre vos données et structure CERASCAN (catégories, finitions, références), rigueur nécessaire pour éviter erreurs import
- Template CSV structure : Télécharger depuis interface partenaire section "Import", fichier Excel avec colonnes prédéfinies, exemples valides fournis
- Colonnes obligatoires (4) : nom (texte libre 100 char max), référence (identifiant unique produit, alphanumériques acceptés), prix (format numérique, point décimal), catégorie (liste fermée CERASCAN : Intérieur/Extérieur, finitions standardisées)
- Colonnes optionnelles (6) : dimensions (format LxlxH en cm), finitions (texte libre), stock (booléen disponible/rupture), fournisseur (nom fournisseur céramique), couleur (texte libre), poids (kg format numérique)
- Correspondances catégories : Mapper vos catégories produits (ex: "Carrelage mural cuisine") vers catégories CERASCAN (ex: "Intérieur > Mural > Céramique"), tableau correspondances fourni dans guide
- Bonnes pratiques Excel : Encodage UTF-8 obligatoire (Fichier > Enregistrer sous > CSV UTF-8), format texte pour références (éviter conversion automatique nombres), supprimer caractères spéciaux (œ, æ, accents acceptés), pas de virgules dans cellules (remplacer par points-virgules)
- Images produits : Formats JPG ou PNG uniquement, résolution minimum 1200×1200 pixels (qualité affichage mobile), nomenclature fichier = référence produit exacte (ex: INTFURN01COLL01GRAY60X60.jpg), stocker dans dossier structuré avant upload
- Checklist validation 15 points (Annexe B) : Colonnes obligatoires présentes, aucun doublon références, encodage UTF-8 vérifié, catégories valides CERASCAN, prix format numérique, images nomenclature correcte, pas de cellules vides colonnes obligatoires
- Temps réaliste : Variable selon catalogue (50 produits = quelques heures, 500 produits = 1-3 jours, 2000 produits = plusieurs jours), qualité données existantes impacte durée (nomenclature déjà structurée vs à créer)

**Longueur estimée :** 5-7 pages  
**Output-style :** `user-friendly.md` (procédures détaillées, exemples concrets)  
**Persona :** `clarity-expert.md` (correspondances explicitées, instructions Excel claires)

---

### Chapitre 03 : Importer Votre Catalogue

**Synopsis :**  
Connexion interface partenaire (URL et identifiants fournis par administrateur CERASCAN). Navigation section "Import Catalogue". Upload fichier CSV préparé (drag & drop ou sélection fichier). Validation automatique temps réel : vérification colonnes obligatoires présentes, détection doublons références produits, contrôle encodage UTF-8. Lecture et compréhension messages erreur validation (ligne problématique, colonne concernée, correction suggérée). Suivi progression import (barre progression, nombre produits traités/total). Alertes email automatiques (succès avec récapitulatif produits importés, ou erreurs avec diagnostics détaillés). Correction erreurs : modifier fichier CSV original, sauvegarder, relancer import. Procédure itérative jusqu'à import sans erreur.

**Points clés :**
- Connexion interface partenaire : URL fournie par email administrateur (ex: https://partner808.cerascan.fr/login), identifiants login (email) + mot de passe initial (changement recommandé première connexion)
- Section Import Catalogue : Menu latéral gauche, icône "📥 Import", bouton "Importer Nouveau Catalogue" visible page principale
- Upload fichier CSV : Zone drag & drop (glisser-déposer fichier) ou bouton "Sélectionner fichier" (navigateur fichiers), poids maximum 50 Mo, formats acceptés .csv .txt uniquement
- Validation automatique temps réel : Lecture fichier après upload, vérifications (colonnes obligatoires : nom, référence, prix, catégorie présentes, doublons références détectés, encodage UTF-8 contrôlé, formats colonnes validés numériques/texte)
- Messages erreur compréhensibles : Ligne problématique affichée (ex: "Ligne 42"), colonne concernée identifiée (ex: "Colonne 'prix' invalide"), correction suggérée (ex: "Format attendu : nombre décimal, point comme séparateur")
- Progression import : Barre progression visuelle (0-100%), nombre produits traités affichés (ex: "250/500 produits importés"), estimation temps restant (variable selon taille catalogue)
- Alertes email automatiques : Email succès (sujet "Import réussi - 500 produits", récapitulatif catégories, confirmation QR codes en génération), email erreur (sujet "Import échoué - erreurs détectées", liste erreurs ligne par ligne, fichier joint avec lignes problématiques surlignées)
- Correction et réimport : Télécharger fichier erreurs (depuis email ou interface), corriger lignes problématiques Excel, sauvegarder CSV UTF-8, relancer import (bouton "Réimporter"), répéter jusqu'à succès
- Délai traitement serveur : Import validé immédiatement (quelques secondes validation), traitement complet variable (50 produits = 1-2 min, 500 produits = 5-10 min, 2000 produits = 30-60 min), notification email fin traitement

**Longueur estimée :** 3-4 pages  
**Output-style :** `user-friendly.md` (captures interface annotées, exemples erreurs)  
**Persona :** `clarity-expert.md` (messages erreur explicites, procédure correction claire)

---

### Chapitre 04 : Récupérer Vos QR Codes

**Synopsis :**  
Attente notification email "QR codes prêts téléchargement" (délai variable selon taille catalogue : 50 produits = quelques minutes, 500 produits = 10-30 min, 2000 produits = 1-2h). Connexion interface partenaire, accès section "Télécharger QR Codes". Téléchargement fichier ZIP contenant PDFs QR codes organisés grilles 10×10 (100 codes par page A4). Décompression fichier ZIP localement. Vérification organisation fichiers : nomenclature PDFs (ex: qr-batch-001.pdf, qr-batch-002.pdf), correspondance produits ↔ QR codes (ordre références produits CSV). Archivage PDFs localement (pour réimpression future si QR codes showroom abîmés, perdus).

**Points clés :**
- Notification email génération : Sujet "QR codes prêts - [Nom Partenaire]", contenu (nombre QR codes générés, lien direct téléchargement, instructions impression), délai réception variable (fonction taille catalogue, serveur peut mettre 5 min à 2h)
- Section Télécharger QR Codes interface : Menu latéral "📥 Télécharger QR", page affiche historique imports (date import, nombre produits, statut QR codes : En cours/Prêt/Téléchargé), bouton "Télécharger ZIP" visible ligne import concerné
- Organisation fichier ZIP : Contient N fichiers PDF (N = nombre produits ÷ 100 arrondi supérieur, ex: 500 produits = 5 PDFs), nomenclature séquentielle (qr-batch-001.pdf contient codes 1-100, qr-batch-002.pdf contient codes 101-200, etc.), chaque PDF = grille 10×10 codes (100 QR codes par page A4)
- Correspondance produits ↔ QR codes : Ordre QR codes = ordre lignes CSV import, vérifier correspondance visuelle (scanner 1er QR code batch-001.pdf, vérifier correspond 1er produit CSV), fichier mapping inclus ZIP (mapping.csv : colonnes référence_produit, url_qr, position_pdf)
- Vérification intégrité : Décompresser ZIP, compter PDFs attendus (nombre produits ÷ 100), ouvrir 1er PDF vérifier lisibilité QR codes (pas de corruption fichier), scanner 3-5 QR codes aléatoires vérifier URLs correctes (domaine partenaire, références produits valides)
- Archivage local recommandé : Créer dossier "CERASCAN_QR_[Date]" sur ordinateur, copier tous PDFs dedans, sauvegarder fichier mapping.csv, créer backup dossier (clé USB, cloud), permet réimpression future sans redemander administrateur

**Longueur estimée :** 2-3 pages  
**Output-style :** `user-friendly.md` (procédure téléchargement simple, vérification organisation)  
**Persona :** `clarity-expert.md` (correspondances produits explicites)

---

### Chapitre 05 : Imprimer et Poser en Showroom

**Synopsis :**  
Spécifications techniques impression QR codes : imprimante A4 couleur (noir et blanc acceptable mais couleur recommandé), résolution 300 DPI minimum (qualité scan smartphone), papier adhésif recommandé (alternative : papier standard + plastification). Procédure impression : configurer imprimante (qualité haute, échelle 100%), imprimer 1 page test, scanner QR code test vérifier lisibilité. Découpe QR codes (ciseaux ou massicot, marge 5mm autour code, coins arrondis optionnels durabilité). Plastification optionnelle mais fortement recommandée (protection usure, durabilité +2 ans minimum). Placement stratégique showroom : hauteur accessible visiteurs (1,20-1,50m), proximité échantillons physiques (QR collé directement sur échantillon ou support adjacent), visibilité lumière ambiante (éviter reflets plastification). Fixation : adhésif double-face qualité (3M VHB recommandé résistance, patafix déconseillée chute), colle néoprène surfaces rugueuses (céramique brute, béton). Maintenance physique régulière : inspection mensuelle QR codes (détection abîmés, décollés), remplacement QR usés (réimpression depuis PDFs archivés), nettoyage surface QR (chiffon sec, pas produits chimiques).

**Points clés :**
- Spécifications impression QR codes : Imprimante A4 couleur (noir blanc acceptable, couleur améliore visibilité), résolution 300 DPI minimum obligatoire (scan smartphone nécessite qualité haute, 150 DPI insuffisant), papier adhésif autocollant recommandé (marque Avery, Herma qualité bureau), alternative papier standard 80-100g + plastification après découpe
- Configuration imprimante : Mode qualité haute (settings imprimante : "High Quality" ou "Best"), échelle impression 100% (désactiver "Ajuster à la page", sinon QR codes réduits illisibles), orientation portrait (PDFs déjà formatés portrait), marges minimales (paramètre imprimante : marges 5mm)
- Test impression qualité : Imprimer 1 page test (qr-batch-001.pdf page 1), scanner 3-5 QR codes aléatoires page test avec smartphone, vérifier ouverture URLs correctes instantanément (délai <2 secondes), si échec scan : augmenter résolution imprimante ou nettoyer tête impression
- Découpe QR codes : Ciseaux bureautique standards (découpe manuelle 100-200 codes), massicot A4 recommandé (découpe précise 200-2000 codes, investissement ~50€), marge 5mm autour QR code (espace blanc nécessaire scan), coins arrondis optionnels (durabilité légèrement améliorée, perforatrice coins ronds)
- Plastification protection : Plastifieuse A4 (investissement ~30-80€, ex: Fellowes, Leitz), pochettes plastification 80-125 microns (125 microns recommandé showroom forte fréquentation), procédure (découper QR → insérer pochette → passer plastifieuse → laisser refroidir 2 min), durabilité QR plastifié +2 ans minimum vs 6 mois papier seul
- Placement showroom stratégique : Hauteur 1,20-1,50m accessible adultes et enfants, proximité immédiate échantillon (QR collé directement sur échantillon céramique OU support adjacent carton/plexi <10cm distance), éviter zones reflets (lumière directe fenêtres, néons plafond sur plastification), zones passage fréquent visiteurs (entrée showroom, allées principales)
- Fixation durable : Adhésif double-face 3M VHB (résistance extrême, tenue 5+ ans, coût ~1€/QR code), colle néoprène surfaces rugueuses (céramique brute, béton showroom industriel), alternatives déconseillées (patafix chute fréquente, scotch standard décolle 1-3 mois)
- Maintenance physique : Inspection mensuelle tour showroom (vérifier QR codes présents, lisibles, pas décollés), remplacement QR abîmés (réimpression depuis PDFs archivés Chapitre 4, coût impression seul ~0,10€/QR), nettoyage surface QR plastifié (chiffon microfibre sec, pas alcool/produits chimiques dégradent plastification)
- Schémas placement types (Annexe D) : Configuration petit showroom (50-200 produits, 1 allée centrale, QR sur présentoirs muraux), moyen showroom (200-500 produits, 3-4 allées, QR échantillons sol + muraux), grand showroom (500-2000 produits, zones thématiques, QR + bornes tactiles complémentaires)

**Longueur estimée :** 5-6 pages  
**Output-style :** `user-friendly.md` (schémas visuels placement, procédures techniques détaillées)  
**Persona :** `clarity-expert.md` (instructions impression accessibles non-technique)

---

### Chapitre 06 : Former Votre Équipe Commerciale

**Synopsis :**  
Objectif formation : rendre vos commerciaux autonomes utilisation application CERASCAN quotidiennement. Prérequis : créer comptes commerciaux via interface partenaire (section "Utilisateurs", rôle "Commercial showroom"). Programme formation suggéré 30-45 minutes par commercial : 1) Présentation CERASCAN fonctionnement général (5-10 min), 2) Connexion application mobile/tablette (5 min), 3) Consulter catalogue produits (10 min navigation, recherche), 4) Informations privilégiées commerciaux (prix professionnels, stocks, marges visibles uniquement compte commercial, 10 min), 5) Cas d'usage pratiques showroom (accompagner visiteur, proposer alternatives produit rupture, consulter listes d'envie clients, 10 min). Support formation : renvoi Guide Commercial Showroom (document séparé 5-10 pages fourni administrateur CERASCAN). Suivi post-formation : questions commerciaux quotidiennes → vous contactez administrateur CERASCAN si nécessaire assistance technique.

**Points clés :**
- Création comptes commerciaux : Interface partenaire section "Utilisateurs", bouton "Ajouter Commercial", formulaire (nom, prénom, email commercial, mot de passe temporaire généré automatiquement), rôle "Commercial showroom" (permissions limitées : consultation catalogue, infos privilégiées, pas modification produits)
- Présentation CERASCAN commerciaux (5-10 min) : Qu'est-ce que CERASCAN (showroom digital, QR codes produits), bénéfices visiteurs (autonomie infos, listes d'envie sauvegardées), bénéfices commerciaux (infos privilégiées prix pro/stocks, accompagnement client facilité, outil vente moderne)
- Connexion application (5 min) : URL application mobile (ex: https://partner808.cerascan.fr), identifiants fournis (email + mot de passe temporaire), premier login changement mot de passe obligatoire, tester connexion smartphone/tablette showroom (réseau Wi-Fi)
- Consulter catalogue produits (10 min) : Navigation par catégories (Intérieur/Extérieur, finitions), recherche textuelle (nom produit, référence), scanner QR code physique (appareil photo tablette/smartphone), fiche produit complète (photos HD, dimensions, prix, disponibilité, produits connexes suggérés)
- Informations privilégiées commerciaux (10 min) : Prix professionnels affichés (prix public masqués visiteurs standard), stocks temps réel (disponible immédiat, rupture temporaire, délai livraison), marges commerciales calculées automatiquement (prix achat fournisseur → prix vente public), avantage vente (proposer alternatives si rupture, négociation marge)
- Cas d'usage pratiques (10 min) : Scénario 1 (visiteur cherche carrelage cuisine 60×60 cm mat → recherche catalogue → proposer 3 options disponibles), Scénario 2 (produit rupture stock → consulter alternatives similaires → proposer produit connexe disponible), Scénario 3 (visiteur sauvegarde 5 produits liste envie → commercial accède liste historique → relance client par email produits consultés)
- Guide Commercial Showroom (document séparé) : PDF 5-10 pages fourni administrateur CERASCAN, contenu (connexion rapide, consultation catalogue, infos privilégiées, 5 scénarios showroom détaillés, FAQ 10 questions commerciaux), format imprimable plastifié comptoir showroom (consultation rapide pendant service clients)
- Suivi post-formation : Premier jour commercial accompagné (vous supervisez utilisation, répondez questions temps réel), questions récurrentes commerciaux → noter et contacter administrateur CERASCAN si assistance technique nécessaire (ex: bug application, infos produit manquantes), formation complémentaire possible (sur demande administrateur)

**Longueur estimée :** 2-3 pages  
**Output-style :** `user-friendly.md` (programme formation structuré, scénarios concrets)  
**Persona :** `clarity-expert.md` (étapes formation claires, renvois Guide Commercial explicites)

---

### Chapitre 07 : Mises à Jour et Support

**Synopsis :**  
Mettre à jour catalogue produits à votre convenance (aucune contrainte fréquence, facturation CERASCAN pas impactée par nombre mises à jour). Procédure mise à jour : préparer fichier CSV complet (remplace intégralement version précédente, pas de delta/ajouts partiels), importer via interface partenaire (procédure identique Chapitre 3), administrateur CERASCAN alerté automatiquement par email notification import, regénération QR codes uniquement produits modifiés/ajoutés (produits inchangés conservent QR codes existants), téléchargement nouveaux PDFs QR codes (procédure identique Chapitre 4), impression et pose nouveaux QR showroom (remplacement QR codes produits modifiés, ajout QR nouveaux produits). Contacter administrateur CERASCAN pour toute question technique : email admin-cerascan@efficeram.fr, téléphone +33 X XX XX XX (horaires Lun-Ven 9h-18h). Types questions fréquentes : aide préparation CSV (correspondances nomenclature complexes), erreurs validation import (diagnostics), QR codes manquants après import, problème connexion interface partenaire, demande compte commercial supplémentaire. FAQ 10 questions courantes détaillées avec réponses procédurales.

**Points clés :**
- Mises à jour catalogue à convenance : Vous décidez quand mettre à jour (ajout nouveaux produits fournisseur, changements prix saisonniers, retrait produits obsolètes discontinués), aucune fréquence imposée (typique showrooms : 1x/mois nouveaux produits, 1x/trimestre changements prix, 1x/an rotation catalogue complet), facturation CERASCAN inchangée (abonnement mensuel fixe indépendant nombre mises à jour)
- Procédure mise à jour technique : Préparer CSV complet (reprendre CSV import initial, ajouter/modifier/supprimer lignes produits, sauvegarder CSV UTF-8), importer via interface partenaire (section "Import Catalogue", même procédure Chapitre 3), validation automatique identique (colonnes obligatoires, doublons, encodage), administrateur CERASCAN reçoit email notification automatique (sujet "Mise à jour catalogue [Partenaire]", pas d'intervention manuelle administrateur nécessaire sauf erreurs)
- Regénération QR codes intelligente : Système compare CSV nouveau vs ancien, détecte produits modifiés (changement prix, catégorie, nom), détecte produits ajoutés (nouvelles lignes), détecte produits supprimés (lignes retirées CSV), regénère QR uniquement produits modifiés/ajoutés (produits inchangés conservent QR codes existants validité permanente), optimise coût impression (pas réimpression totale showroom)
- Téléchargement nouveaux PDFs : Notification email "Mise à jour QR codes prête", interface partenaire section "Télécharger QR", bouton "Télécharger Mise à jour [Date]" (fichier ZIP contient uniquement nouveaux/modifiés QR codes, pas l'intégralité), fichier mapping inclus (mapping-update.csv : colonnes référence_produit, action: nouveau/modifié/supprimé, position_pdf)
- Impression et pose partiels : Imprimer uniquement nouveaux PDFs mise à jour (économie papier, plastification, temps), identifier QR codes à remplacer showroom (produits modifiés prix/catégorie, retirer ancien QR coller nouveau), ajouter QR codes nouveaux produits (échantillons nouveaux arrivés showroom), supprimer QR codes produits retirés catalogue (décoller showroom, archiver au cas où)
- Contact support administrateur CERASCAN : Email admin-cerascan@efficeram.fr (usage standard, non-urgent, réponse <24h), téléphone +33 X XX XX XX (urgent uniquement, horaires Lun-Ven 9h-18h), types questions (aide préparation CSV correspondances nomenclature complexes, erreurs validation import incompréhensibles, QR codes manquants après import validé, problème technique connexion interface partenaire, demande création comptes commerciaux supplémentaires, questions facturation abonnement)
- FAQ 10 questions courantes : Q1 (Erreur "Colonne obligatoire manquante" → vérifier présence colonnes nom/référence/prix/catégorie CSV), Q2 (QR code ne scanne pas après impression → résolution imprimante <300 DPI ou QR abîmé plastification), Q3 (Modifier prix produit existant → modifier ligne CSV produit concerné, réimporter complet, nouveau QR généré automatiquement), Q4 (Supprimer produit obsolète → retirer ligne CSV produit, réimporter, produit désactivé application automatiquement), Q5 (Ajouter 50 nouveaux produits → ajouter 50 lignes CSV existant, réimporter complet, 50 nouveaux QR générés), Q6 (Délai génération QR codes → variable 5 min à 2h selon taille catalogue, notification email réception), Q7 (Plusieurs comptes admin partenaire possibles → oui demander administrateur CERASCAN section "Utilisateurs"), Q8 (Commercial oublié mot de passe → réinitialiser interface partenaire section "Utilisateurs" ligne commercial concerné), Q9 (Statistiques consultation produits disponibles → oui interface partenaire section "Analytics" graphiques consultation/période), Q10 (Coût réimpression QR codes → aucun coût logiciel CERASCAN, PDFs archivés réutilisables gratuits, coût impression papier/plastification à charge partenaire ~0,15€/QR code)

**Longueur estimée :** 2-3 pages  
**Output-style :** `user-friendly.md` (procédures mises à jour claires, FAQ accessible)  
**Persona :** `clarity-expert.md` (support explicite, réponses FAQ procédurales détaillées)

---

## Estimation Globale

**Longueur totale :** 22-30 pages (7 chapitres + 4 annexes)  
**Ton :** Technique, patient, concret (pas commercial)  
**Focus :** Complexité préparation CSV (correspondances nomenclature)  
**Audience :** Propriétaires showrooms (compétences Excel/web)

---

**Date génération :** 2026-02-28  
**Générée depuis :** overview.md v3.0 (ton technique, suppression promesses commerciales)
