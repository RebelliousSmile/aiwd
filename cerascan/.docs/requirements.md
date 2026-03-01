# Spécifications: CERASCAN

## Vue d'Ensemble Fonctionnelle

CERASCAN est une solution SaaS multi-tenant permettant aux showrooms céramique d'offrir une expérience numérique à leurs clients. Le système repose sur des QR codes collés sur les échantillons physiques, scannables par smartphone pour accéder aux fiches produits détaillées. Architecture marque blanche permettant personnalisation complète par partenaire.

## Features Principales

### F-01: Multi-Tenant avec Marque Blanche

**Priorité:** Haute
**Statut:** Implémenté

**Description:**
Chaque partenaire distributeur dispose de son propre catalogue isolé (table MySQL séparée), domaine personnalisé, logo, et charte graphique. Architecture garantissant isolation stricte données entre partenaires.

**Use Cases:**
- UC-01: Création nouveau partenaire
- UC-02: Personnalisation marque blanche
- UC-03: Gestion multi-catalogues

**Règles de Gestion:**
- RG-01: Nom partenaire unique (slug)
- RG-02: Table catalogue nommée `{slug}_catalog`
- RG-03: Validation SQL whitelist stricte
- RG-04: Chaque partenaire = dossier QR codes isolé

---

### F-02: Import Catalogue CSV

**Priorité:** Haute
**Statut:** Implémenté

**Description:**
Import massif produits depuis fichier CSV fournisseur. Support INSERT/UPDATE basé sur référence produit. Validation format, logs détaillés, notifications email progression.

**Use Cases:**
- UC-04: Import initial catalogue (3000+ produits)
- UC-05: Mise à jour hebdomadaire (nouveaux produits)
- UC-06: Correction erreurs import

**Règles de Gestion:**
- RG-05: Colonnes obligatoires : reference, nom, longueur, largeur, finition
- RG-06: Génération compositeId automatique (INT|codeFournisseur|collection|teinte|L|l)
- RG-07: Si référence existe → UPDATE, sinon INSERT
- RG-08: Validation email super-utilisateur requise

---

### F-03: Génération QR Codes Batch

**Priorité:** Haute
**Statut:** Implémenté

**Description:**
Génération massive QR codes PNG (jusqu'à 5000) avec URLs uniques par produit. Support génération sélective (nouveaux produits, tous, sélection manuelle). Export ZIP QR codes + PDFs fiches.

**Use Cases:**
- UC-07: Génération QR nouveaux produits (post-import)
- UC-08: Régénération complète (changement domaine)
- UC-09: Génération sélective (produits spécifiques)

**Règles de Gestion:**
- RG-09: Taille QR : 400×400px (recommandé)
- RG-10: Correction erreur : H (30%)
- RG-11: URL format : https://{domain}/produit/{compositeId}
- RG-12: Stockage : public/qrcodes/{partnerId}/{compositeId}.png

---

### F-04: Consultation Produit Mobile-First

**Priorité:** Haute
**Statut:** Implémenté

**Description:**
Fiche produit optimisée mobile, chargement ultra-rapide via cache IndexedDB (10-50ms). Support scan QR → affichage fiche instantané. Informations complètes : dimensions, finition, prix, photos, produits connexes.

**Use Cases:**
- UC-10: Scan QR code showroom
- UC-11: Navigation catalogue mobile
- UC-12: Recherche produit (dimensions, finition, fournisseur)

**Règles de Gestion:**
- RG-13: Cache IndexedDB TTL 30 minutes
- RG-14: Fallback localStorage si IndexedDB indisponible
- RG-15: Service Worker offline pour assets critiques
- RG-16: Lazy loading images (optimisation performance)

---

### F-05: PWA Offline-First

**Priorité:** Moyenne
**Statut:** Implémenté

**Description:**
Progressive Web App installable, fonctionnement offline via Service Worker. Cache assets statiques (CSS, JS, images), pages catalogue, fallback offline.html. Manifest dynamique par partenaire.

**Use Cases:**
- UC-13: Installation app smartphone/desktop
- UC-14: Consultation catalogue hors ligne (Wi-Fi showroom faible)
- UC-15: Notifications mise à jour app

**Règles de Gestion:**
- RG-17: Manifest dynamique adapté au partenaire (nom, icônes, couleurs)
- RG-18: Service Worker cache stratégies : Cache-First (assets), Network-First (pages)
- RG-19: Icônes PWA : 16×16, 32×32, 180×180, 192×192, 512×512

---

### F-06: Listes d'Envies

**Priorité:** Moyenne
**Statut:** Implémenté

**Description:**
Sauvegarde sélection produits visiteur. Stockage localStorage (anonyme) ou base données (authentifié). Partage par email, export PDF.

**Use Cases:**
- UC-16: Ajout produit liste d'envies
- UC-17: Partage liste par email
- UC-18: Impression liste (PDF)

**Règles de Gestion:**
- RG-20: Visiteur anonyme → localStorage (temporaire)
- RG-21: Utilisateur authentifié → base données (persistant)
- RG-22: Limite 50 produits par liste

---

### F-07: Statistiques Consultation

**Priorité:** Basse
**Statut:** Partiellement implémenté

**Description:**
Analytics consultation produits : produits populaires, fournisseurs consultés, statistiques temporelles. Dashboard admin partenaire.

**Use Cases:**
- UC-19: Consultation dashboard statistiques
- UC-20: Export rapport consultation (CSV)
- UC-21: Comparaison fournisseurs

**Règles de Gestion:**
- RG-23: Log consultation produit (anonymisé)
- RG-24: Agrégation hebdomadaire/mensuelle
- RG-25: Rétention données 12 mois

---

## Use Cases Détaillés

### UC-01: Création Nouveau Partenaire

**Acteur:** Super utilisateur (Laurent)
**Pré-conditions:** Contrat partenaire signé, informations complètes (nom, logo, domaine)
**Post-conditions:** Partenaire créé, table catalogue créée, identifiants envoyés

**Scénario nominal:**
1. Connexion /admin74950
2. Clic "Nouveau Partenaire"
3. Saisie formulaire (nom, slug, email admin, logo)
4. Validation → Système génère partnerId, dbName, crée table MySQL
5. Email envoyé admin partenaire (identifiants, lien première connexion)
6. Configuration DNS domaine custom (optionnel)

**Scénarios alternatifs:**
- **Alt 1 (slug existant):** Message erreur, suggestion slug alternatif
- **Alt 2 (erreur création table):** Rollback, log erreur MySQL, notification admin

---

### UC-04: Import Initial Catalogue

**Acteur:** Admin partenaire (Sophie)
**Pré-conditions:** Fichier CSV fournisseur, compte partenaire actif
**Post-conditions:** Catalogue importé, QR codes générés, email confirmation

**Scénario nominal:**
1. Téléchargement CSV fournisseur (email ou site)
2. Connexion /partner
3. Upload CSV (glisser-déposer ou parcourir)
4. Validation format → Système affiche aperçu (nombre produits, colonnes détectées)
5. Email notification super-utilisateur (validation requise)
6. Validation super-utilisateur → Import lancé (processing CSV)
7. Email rapport import (succès: 3500, erreurs: 12)
8. Vérification catalogue /partner/products
9. Génération QR codes (voir UC-07)

**Scénarios alternatifs:**
- **Alt 1 (format CSV invalide):** Email erreur, correction requise, lien documentation
- **Alt 2 (colonnes manquantes):** Liste colonnes manquantes, blocage import
- **Alt 3 (erreur ligne):** Import partiel, log erreurs détaillé, email rapport

---

### UC-10: Scan QR Code Showroom

**Acteur:** Visiteur showroom (Marc)
**Pré-conditions:** QR code collé échantillon, smartphone avec appareil photo
**Post-conditions:** Fiche produit affichée, consultation loggée

**Scénario nominal:**
1. Repérage échantillon carrelage showroom
2. Ouverture appareil photo smartphone
3. Scan QR code → URL détectée
4. Clic notification URL → Ouverture navigateur
5. Chargement fiche produit (cache IndexedDB 10-50ms)
6. Consultation informations (dimensions, finition, prix, photos)
7. Actions optionnelles (liste d'envies, partage, produits connexes)

**Scénarios alternatifs:**
- **Alt 1 (QR endommagé):** Erreur scan, message "QR illisible", redirection catalogue général
- **Alt 2 (produit supprimé):** Redirect catalogue, message "Produit indisponible"
- **Alt 3 (Wi-Fi indisponible):** Service Worker offline, affichage cache, message "Mode hors ligne"

---

## Règles Métier

| ID | Règle | Composants Concernés |
|----|-------|---------------------|
| RG-01 | Nom partenaire unique (slug validé) | CreatePartner, validatePartnerName |
| RG-02 | Table catalogue nommée `{slug}_catalog` | DatabaseService, ExecutionContext |
| RG-03 | Validation SQL whitelist stricte | DatabaseService.executeQuery |
| RG-06 | CompositeId = INT\|codeFournisseur\|collection\|teinte\|L\|l | ProductService.generateCompositeId |
| RG-13 | Cache IndexedDB TTL 30 minutes | catalogueCache.load |
| RG-20 | Visiteur anonyme → localStorage | WishlistController |
| RG-23 | Log consultation produit (anonymisé RGPD) | ProductController.logView |

## Contraintes Non-Fonctionnelles

- **Performance:** 
  - Temps réponse fiche produit < 100ms (cache hit)
  - Génération 1000 QR codes < 5 minutes
  - Chargement catalogue 3000+ produits < 2 secondes (première visite)

- **Scalabilité:** 
  - Support 100+ partenaires simultanés
  - 10,000 utilisateurs concurrents
  - 500 scans QR/minute/partenaire

- **Disponibilité:** 
  - Uptime 99.5% (hors maintenance programmée)
  - Dégradation gracieuse (mode offline)

- **Sécurité:** 
  - Chiffrement TLS 1.3
  - Prepared statements SQL (injection)
  - CSRF tokens (formulaires)
  - Rate limiting API (100 req/min/IP)

---
**Lignes:** 236/250
**Sources:** user-stories-essential.md, business-processes.md, architecture-core.md
