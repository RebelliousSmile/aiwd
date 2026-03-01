# Overview: Guide Partenaire CERASCAN

## Objectif du Document

Guide technique pour mettre en place CERASCAN dans votre showroom. Ce guide vous accompagne dans la préparation de votre catalogue produits au format CSV (étape la plus importante et complexe), l'import via votre interface partenaire, le téléchargement des QR codes, leur impression et pose en showroom.

**Important :** La facturation CERASCAN ne débute qu'à la pose des premières étiquettes QR en showroom. Vous pouvez donc prendre tout le temps nécessaire pour préparer correctement votre fichier CSV. La correspondance entre vos données produits et le format CERASCAN demande de la rigueur et de la patience.

## Audience Cible

**Vous = Propriétaire/Responsable showroom partenaire**
- Avez souscrit abonnement CERASCAN après présentation par l'administrateur
- Compétences : Excel/CSV, navigation web, impression documents
- **Votre travail principal :**
  - Préparer catalogue produits au format CSV (étape complexe nécessitant rigueur)
  - Comprendre correspondances entre vos données et structure CERASCAN
  - Importer catalogue via interface partenaire
  - Télécharger, imprimer et poser QR codes en showroom
  - Former équipe commerciale à l'utilisation application

**Autres rôles mentionnés (hors périmètre ce guide) :**
- **Vos commerciaux :** Utilisent l'application quotidiennement (voir Guide Commercial Showroom)
- **Visiteurs showroom :** Scannent QR codes avec smartphone (aucune documentation nécessaire)
- **Administrateur CERASCAN :** Votre contact support (voir Guide Administrateur)

## Prérequis

**Avant de commencer, vous devez avoir :** [6]
- ✅ Abonnement CERASCAN activé (souscrit auprès administrateur)
- ✅ Accès interface partenaire (URL + login/mot de passe fournis)
- ✅ Excel ou équivalent installé (préparation fichiers CSV)
- ✅ Imprimante A4 couleur disponible (300 DPI minimum)
- ✅ Catalogue produits céramique complet (liste produits, prix, dimensions, catégories, images)

**Connaissances préalables :**
- Usage Excel/CSV (ouvrir, modifier, enregistrer fichiers, comprendre structure colonnes)
- Navigation web (formulaires, upload fichiers)
- Impression documents PDF

**Note importante :** La préparation du fichier CSV est l'étape la plus complexe et chronophage. Elle nécessite de bien comprendre la correspondance entre votre nomenclature produits actuelle et la structure attendue par CERASCAN. Prenez le temps nécessaire pour cette étape.

## Périmètre

### Couvert

**Structure linéaire en 7 chapitres :**

**Chapitre 1 : Comment Utiliser ce Guide** (2-3 pages)
- À propos de ce guide (objectif, périmètre)
- Comment lire (lecture linéaire vs consultation par besoin)
- Workflow global 5 étapes (Préparer CSV → Importer → Récupérer QR → Imprimer → Former équipe)
- Contacts et support (administrateur CERASCAN)
- Ressources complémentaires (Guide Commercial, templates)
- **Output-style :** `user-friendly.md`
- **Persona :** `clarity-expert.md`

**Chapitre 2 : Préparer Votre Catalogue CSV** (5-7 pages) [3]
- **Complexité de cette étape :** Comprendre correspondances entre votre nomenclature et structure CERASCAN
- Télécharger template CSV vierge depuis interface partenaire
- Colonnes obligatoires (nom, référence, prix, catégorie) : définitions précises, exemples valides/invalides
- Colonnes optionnelles (dimensions, finitions, stock, fournisseur) : quand les utiliser
- **Correspondances nomenclature :** Mapper vos catégories produits vers catégories CERASCAN, gérer variantes (couleurs, finitions), standardiser références produits
- Remplir fichier Excel : bonnes pratiques (encodage UTF-8, format texte vs numérique, gestion caractères spéciaux), pièges courants (doublons références, catégories invalides, prix mal formatés)
- Collecter images produits : formats JPG/PNG, résolution minimum 1200x1200px, nomenclature images = références produits
- Checklist validation avant import (15 points de contrôle, voir Annexe B)
- **Temps nécessaire :** Variable selon taille catalogue et qualité données existantes. Comptez plusieurs heures à plusieurs jours pour première préparation.
- **Output-style :** `user-friendly.md` (procédures pas-à-pas, exemples concrets)
- **Persona :** `clarity-expert.md` (instructions Excel claires, correspondances explicitées)

**Chapitre 3 : Importer Votre Catalogue** (3-4 pages)
- Se connecter à votre interface partenaire (URL, login)
- Naviguer vers section "Import Catalogue"
- Upload fichier CSV (drag & drop ou sélection fichier)
- Validation temps réel : vérification colonnes obligatoires, détection doublons, contrôle encodage
- Lecture des erreurs : comprendre messages validation, identifier lignes problématiques
- Suivi progression import (barre progression, nombre produits traités)
- Alertes email automatiques (succès avec récapitulatif, erreurs avec diagnostics détaillés)
- Corriger erreurs et réimporter : modifier CSV, sauvegarder, relancer import
- **Output-style :** `user-friendly.md` (captures interface annotées)
- **Persona :** `clarity-expert.md` (messages erreur compréhensibles)

**Chapitre 4 : Récupérer Vos QR Codes** (2-3 pages)
- Notification email "QR codes prêts" (délai variable selon taille catalogue)
- Accéder section "Télécharger QR Codes" interface partenaire
- Télécharger PDFs générés (fichier ZIP contenant grilles 10x10, 100 codes par page A4)
- Décompresser et vérifier fichiers : nomenclature PDFs, correspondance produits ↔ QR codes
- Archiver PDFs localement (pour réimpression future si QR abîmés)
- **Output-style :** `user-friendly.md`
- **Persona :** `clarity-expert.md`

**Chapitre 5 : Imprimer et Poser en Showroom** (5-6 pages)
- Spécifications impression : imprimante A4 couleur, 300 DPI minimum, papier adhésif ou standard + plastification
- Procédure impression (configuration imprimante, réglages qualité, impression test 1 page)
- Découpe QR codes (ciseaux ou massicot, marge 5mm autour code)
- Plastification (optionnelle mais recommandée pour durabilité)
- Placement showroom : hauteur accessible (1,20-1,50m), proximité échantillons, visibilité lumière
- Fixation : adhésif double-face qualité (3M VHB recommandé), colle néoprène pour surfaces rugueuses
- Maintenance physique : inspection régulière, remplacement QR abîmés (réimprimer depuis PDFs archivés)
- Schémas placement types (petit/moyen/grand showroom, voir Annexe D)
- **Output-style :** `user-friendly.md` (schémas visuels)
- **Persona :** `clarity-expert.md`

**Chapitre 6 : Former Votre Équipe Commerciale** (2-3 pages) [3]
- **Objectif :** Rendre vos commerciaux autonomes sur application CERASCAN
- **Prérequis :** Créer comptes commerciaux via interface partenaire (section "Utilisateurs")
- **Programme formation suggéré :**
  1. Présentation CERASCAN : Fonctionnement général, bénéfices pour visiteurs et commerciaux
  2. Connexion application : URL, identifiants, premier login
  3. Consulter catalogue produits : Navigation, recherche, scanner QR code
  4. Informations privilégiées commerciaux : Prix professionnels, stocks, marges (visibles uniquement compte commercial)
  5. Cas d'usage pratiques : Accompagner visiteur, proposer alternatives, consulter listes d'envie
- **Temps formation suggéré :** 30-45 minutes par commercial
- **Support formation :** Renvoi vers **Guide Commercial Showroom** (document séparé, 5-10 pages)
- **Suivi post-formation :** Questions commerciaux → vous contactez administrateur CERASCAN si nécessaire
- **Output-style :** `user-friendly.md`
- **Persona :** `clarity-expert.md`

**Chapitre 7 : Mises à Jour et Support** (2-3 pages) [7][8]
- **Mettre à jour catalogue (à votre convenance) :** [7] Vous décidez quand mettre à jour votre catalogue (ajout nouveaux produits, changements prix, retrait produits obsolètes). Aucune contrainte de fréquence. Procédure : préparer CSV complet (remplace intégralement version précédente), importer via interface partenaire, administrateur CERASCAN alerté automatiquement par email, regénération QR codes pour produits modifiés/ajoutés uniquement, téléchargement nouveaux PDFs, impression et pose nouveaux QR en showroom
- **Contacter l'administrateur CERASCAN :** [8] Pour toute question ou problème technique, contactez **uniquement l'administrateur CERASCAN** (pas de contact direct prestataire technique). Contact : email admin-cerascan@efficeram.fr, téléphone +33 X XX XX XX (horaires Lun-Ven 9h-18h). Types questions fréquentes : aide préparation CSV (correspondances nomenclature), erreurs validation import, QR codes manquants après import, problème connexion interface partenaire, demande compte commercial supplémentaire
- **FAQ courantes (10 questions) :**
  1. Erreur "Colonne obligatoire manquante" → Vérifier présence colonnes nom, référence, prix, catégorie
  2. QR code ne scanne pas après impression → Vérifier résolution impression (300 DPI min), qualité QR code
  3. Comment modifier prix produit existant → Modifier CSV (ligne produit), réimporter, nouveaux QR générés
  4. Comment supprimer produit obsolète → Retirer ligne du CSV, réimporter (produit désactivé automatiquement)
  5. Comment ajouter 50 nouveaux produits → Ajouter 50 lignes au CSV existant, réimporter complet
  6. Délai génération QR codes → Variable (quelques minutes à quelques heures selon taille catalogue)
  7. Puis-je avoir plusieurs comptes admin partenaire → Oui, demander à administrateur CERASCAN
  8. Commercial a oublié mot de passe → Réinitialiser depuis interface partenaire, section "Utilisateurs"
  9. Statistiques consultation produits disponibles → Oui, voir interface partenaire section "Analytics"
  10. Coût réimpression QR codes → Aucun coût logiciel (PDFs archivés réutilisables), coût impression/papier à votre charge
- **Output-style :** `user-friendly.md`
- **Persona :** `clarity-expert.md`

### Non couvert
- Utilisation quotidienne par commerciaux (voir **Guide Commercial Showroom**)
- Administration multi-tenant (voir **Guide Administrateur**)
- Architecture technique, configuration serveur
- Gestion base de données, migrations

### Non couvert
- Utilisation quotidienne par commerciaux (voir **Guide Commercial Showroom**)
- Administration multi-tenant, création partenaires (voir **Guide Administrateur**)
- Configuration serveur, architecture technique (délégué prestataire)

---

## Annexes

- **Annexe A :** Template CSV Catalogue Vierge (fichier Excel uniquement, instructions détaillées dans Chapitre 2) [3]
- **Annexe B :** Checklist Validation Catalogue (15 points de contrôle avant import)
- **Annexe C :** Spécifications Techniques Impression QR Codes (résolution, formats, matériaux)
- **Annexe D :** Schémas Placement Showroom Types (3 configurations : petit/moyen/grand)

## Tone & Style

**Style :** User-friendly (user-friendly.md)
- Adresser directement le lecteur : "vous" (propriétaire showroom)
- Terminologie par rôles (pas de prénoms) : "l'administrateur CERASCAN", "vos commerciaux", "les visiteurs"
- Ton accessible, pédagogique, encourageant
- Captures d'écran annotées
- Procédures étape-par-étape numérotées
- Encadrés "💡 Astuce", "⚠️ Attention", "✅ Bonne pratique"
- Éviter jargon technique non expliqué

---

## Annexes

- **Annexe A :** Template CSV Catalogue Vierge (fichier Excel, instructions détaillées dans Chapitre 2)
- **Annexe B :** Checklist Validation Catalogue (15 points de contrôle avant import)
- **Annexe C :** Spécifications Techniques Impression QR Codes (résolution, formats, matériaux)
- **Annexe D :** Schémas Placement Showroom Types (3 configurations suggérées)

## Tone & Style

**Style :** User-friendly (user-friendly.md)
- Ton technique, patient, concret
- Adresser directement : "vous" (propriétaire showroom)
- Terminologie par rôles : "l'administrateur CERASCAN", "vos commerciaux", "les visiteurs"
- Captures d'écran annotées (interface partenaire, Excel)
- Procédures étape-par-étape détaillées
- Encadrés "💡 Astuce", "⚠️ Attention", "✅ Bonne pratique"
- **Insister sur complexité préparation CSV** (correspondances nomenclature, temps nécessaire)
- **Pas de promesses temporelles** (pas de "72h opérationnel", "autonomie garantie")

## Livrables Attendus

- **Format :** PDF haute-définition (via ICML → InDesign)
- **Longueur estimée :** 22-30 pages
- **Audience :** Propriétaires showrooms (compétences Excel/web)
- **Contexte :** Lecteur a vu présentation CERASCAN, abonnement souscrit, pas encore facturé

## Contraintes

- Ton technique patient (pas commercial)
- Focus sur difficulté préparation CSV (correspondances complexes)
- Captures d'écran interface partenaire (chaque étape critique)
- Schémas placement QR showroom (3-4 configurations)
- Terminologie cohérente avec Guide Administrateur
- PDF interactif (liens, sommaire)

## Prochaines Étapes

1. ✅ Brainstorm structure (validé)
2. ✅ Upgrade overview (ton technique, suppression promesses commerciales)
3. Génération TOC détaillée (INDEX.md)
4. Écriture chapitres (style user-friendly technique)
5. Review par personas (clarity-expert)
6. Export ICML et layout InDesign

---

## Métadonnées

| Attribut | Valeur |
|----------|--------|
| **Document** | Guide Partenaire CERASCAN |
| **Version** | 3.0 |
| **Date création** | 2026-02-28 |
| **Auteur** | fxgui |
| **Statut** | Draft (pré-TOC) |
| **Audience** | Propriétaires showrooms partenaires |
| **Longueur** | 22-30 pages |
| **Ton** | Technique, patient, concret (pas commercial) |
| **Focus** | Préparation CSV (étape complexe) |

---

**Changelog :**
- **v3.0 (2026-02-28) :** Réécriture complète — suppression promesses commerciales ("72h opérationnel", "autonomie 95%"), suppression timings irréalistes, suppression exemple Carrelage Dupont, suppression parcours formation, ton technique patient, focus complexité préparation CSV (correspondances nomenclature), insistance "prendre le temps nécessaire", facturation après pose QR (pas de pression temporelle)
- **v2.0 (2026-02-28) :** Upgrade marketing (obsolète, approche commerciale inadaptée)
- **v1.1 (2026-02-28) :** Structure 7 chapitres, terminologie par rôles
- **v1.0 (2026-02-28) :** Version initiale
