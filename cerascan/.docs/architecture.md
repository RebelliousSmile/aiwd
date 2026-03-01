# Architecture: CERASCAN (EFFICERAM)

## Vue d'Ensemble

```
[Client Mobile/Desktop]
        ↓ HTTPS
[Express.js App] ←→ [ExecutionContext] (contexte partenaire)
        ↓
[DatabaseService] (couche abstraction)
        ↓
[MySQL Pool] → [partners] (table système)
            → [cerascan_catalog] (partenaire root)
            → [partner808_catalog] (partenaire N)
            → [testing_catalog] (partenaire test)

[Frontend]
  - Alpine.js (réactivité UI)
  - IndexedDB (cache catalogue)
  - Service Worker (PWA offline)
```

**Stack Technique:**
- **Backend:** Node.js (LTS) + Express.js ^4.x
- **Database:** MySQL ^8.x + mysql2 driver ^3.x (pool connexions)
- **Frontend:** Alpine.js ^3.x + EJS templates + Tailwind CSS ^3.x
- **Cache:** IndexedDB (client-side, 50+ MB)
- **PDF/QR:** PDFKit ^0.13.x + QRCode.js ^1.5.x
- **PWA:** Service Worker + Manifest dynamique
- **Infrastructure:** OVH France (souveraineté données)

## Composants Principaux

### 1. ExecutionContext (Pattern SOLID)

**Type:** Classe / Factory
**Responsabilités:** Encapsuler contexte partenaire + requête HTTP
**Technologies:** JavaScript ES6

**Factory methods:**
```javascript
ExecutionContext.fromRequest(req)       // Controllers Express
ExecutionContext.fromPartner(partner)   // Services métier
ExecutionContext.empty()                // Tests/opérations génériques
```

**Propriétés:**
- `partnerId` : ID partenaire (ex: '1')
- `dbName` : Nom base catalogue (ex: 'cerascan_catalog')
- `partnerName` : Nom commercial (ex: 'CERASCAN')
- `qrCodePartnerId` : ID pour chemins fichiers (ex: 'cerascan')
- `hasValidPartner()` : Validation contexte

**Dépendances:** Table `partners` (validation partenaire)

### 2. DatabaseService (Service Layer Pattern)

**Type:** Service singleton
**Responsabilités:** Couche abstraction MySQL, isolation multi-tenant
**Technologies:** mysql2, pool connexions

**API Produits:**
```javascript
// CRUD
getProducts(dbName)                          // Tous produits
getProductByCompositeId(partner, compositeId) // Produit unique
insertProduct(dbName, productData)          // Insertion
updateProduct(dbName, updates, conditions)  // Mise à jour
deleteProduct(dbName, conditions)           // Suppression

// Requêtes spécialisées
getProductsFiltered(dbName, filters)        // Filtrage
getProductStats(dbName)                     // Statistiques
```

**API Partenaires:**
```javascript
getAllPartners()                    // Liste partenaires
getPartnerById(partnerId)           // Partenaire par ID
getPartnerByDomain(domain)          // Partenaire par domaine
createPartner(partnerData)          // Création
updatePartner(partnerId, updates)   // Mise à jour
```

**Sécurité:**
- Prepared statements (injection SQL)
- Validation whitelist noms tables
- Isolation stricte par partenaire

**Dépendances:** Pool MySQL, logger custom

### 3. Frontend Cache (IndexedDB)

**Type:** Module JavaScript client-side
**Responsabilités:** Cache catalogue produits, optimisation performance
**Technologies:** IndexedDB API, Alpine.js

**Stratégie Cache-First:**
```
1. IndexedDB lookup (10-50ms) → HIT ✅
2. localStorage fallback (150-300ms) → HIT ⚠️
3. Serveur API (1500ms) → MISS ❌
```

**Structure IndexedDB:**
- **Database:** euroceramic_catalogue (v1)
- **Store:** products
- **Key:** 'catalogue'
- **Indexes:** timestamp, userRole

**API Module:**
```javascript
catalogueCache.save(products, userRole)   // Sauvegarde
catalogueCache.load(currentRole)          // Chargement (TTL 30min)
catalogueCache.clear()                    // Nettoyage
catalogueCache.isAvailable()              // Test support
```

**Performances:**
- Première visite: ~1500ms (serveur)
- Visites suivantes: ~10-50ms (IndexedDB) → 30× plus rapide

**Dépendances:** Alpine.js (store global), API catalogue

### 4. QR System

**Type:** Module backend + frontend
**Responsabilités:** Génération QR codes, PDFs, gestion fichiers
**Technologies:** QRCode.js, PDFKit, filesystem

**Workflow génération:**
```
1. Admin sélectionne produits
2. Génération batch QR codes (PNG)
3. Stockage: public/qrcodes/{partnerId}/{compositeId}.png
4. Génération PDFs fiches (optionnel)
5. Stockage: public/qrcodes/{partnerId}/pdf/{compositeId}.pdf
```

**Paramètres QR:**
- Taille: 400×400px (recommandé)
- Correction erreur: H (30%)
- Format: PNG avec marge
- URL: https://{domain}/produit/{compositeId}

**API:**
```javascript
generateQRCodesForProducts(context, products)  // Génération batch
generateQRCodeImage(url, size)                 // Génération unitaire
generateProductPDF(context, product)           // PDF fiche
```

**Dépendances:** DatabaseService, ExecutionContext, filesystem

## Flux de Données

### Flux 1: Consultation Produit (Scan QR)

```
[Client] → Scan QR code échantillon
        ↓
[QR URL] → https://partner808.cerascan.fr/produit/INTFURN01COLL01GRAY60X60
        ↓
[Express] → attachCurrentPartner middleware (détecte partner808)
        ↓
[ExecutionContext] → fromRequest(req) [partnerId=808, dbName=partner808_catalog]
        ↓
[ProductController] → getProductByCompositeId(context, compositeId)
        ↓
[DatabaseService] → SELECT * FROM partner808_catalog WHERE compositeId=?
        ↓
[EJS Template] → Rendu fiche produit + Alpine.js reactive
        ↓
[Client] → Affichage fiche + cache IndexedDB
```

### Flux 2: Import Catalogue CSV

```
[Admin] → Upload CSV via UI
        ↓
[Multer] → Stockage temporaire fichier
        ↓
[Express] → /admin/import-csv [POST]
        ↓
[ImportController] → Validation format (colonnes obligatoires)
        ↓
[CSV Parser] → Extraction lignes (skip header)
        ↓
[Loop] → Pour chaque ligne:
          - Génération compositeId
          - INSERT/UPDATE DatabaseService
          - Log progression
        ↓
[DatabaseLogger] → Logs import (table partner_logs)
        ↓
[Response] → {success: true, imported: 3500, errors: 12}
        ↓
[Client] → Notification + refresh catalogue
```

### Flux 3: Génération QR Batch

```
[Admin] → Sélection "Tous produits sans QR"
        ↓
[Express] → /admin/generate-qr-codes [POST]
        ↓
[QRController] → getProducts(dbName) → filtrer sans QR
        ↓
[Loop] → Pour chaque produit:
          - Génération URL unique
          - QRCode.toFile(path, url, options)
          - UPDATE hasQRCode=1
        ↓
[Filesystem] → Écriture public/qrcodes/{partnerId}/
        ↓
[Response] → {success: true, generated: 245}
        ↓
[SSE] → Push progression temps réel (optionnel)
```

## Intégrations Externes

| Système | Type | Usage | Criticité |
|---------|------|-------|-----------|
| **MySQL Server** | Base données | Stockage multi-tenant | Haute |
| **OVH Cloud** | Infrastructure | Hébergement app + DB | Haute |
| **Email SMTP** | Service | Notifications admin imports | Moyenne |
| **IndexedDB** | API Navigateur | Cache client-side | Moyenne |
| **Service Worker** | API Navigateur | PWA offline | Basse |

## Contraintes Techniques

- **Multi-tenant strict:** Tables `{partner}_catalog` isolées, validation whitelist SQL
- **Composite IDs:** Pas d'auto-increment, clé primaire composite (6 champs)
- **Performance mobile:** Catalogue 3000+ produits, cache IndexedDB obligatoire
- **QR codes massifs:** Génération batch jusqu'à 5000 QR, gestion mémoire Node.js
- **Marque blanche:** Manifest PWA dynamique, CSS variables par partenaire
- **Offline-first:** Service Worker cache assets + pages, fallback offline.html
- **Sécurité:** CSRF tokens, validation inputs, prepared statements, whitelist tables

---
**Lignes:** 219/250
**Sources:** architecture-core.md, multi-tenant-guide.md, frontend-patterns.md, product-structure.md
