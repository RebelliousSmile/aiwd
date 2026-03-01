# Système QR Codes: CERASCAN

## Vue d'Ensemble

Le système QR codes est au cœur de CERASCAN : chaque échantillon physique showroom reçoit un QR code unique collé, scannnable smartphone pour accéder instantanément à la fiche produit complète.

**Workflow:**
```
[Échantillon carrelage] ← QR code collé
         ↓
[Scan smartphone] → https://partner808.cerascan.fr/produit/INTFURN01COLL01GRAY60X60
         ↓
[Fiche produit] Dimensions, finition, prix, photos, produits connexes
```

## Architecture Génération

### Composants

| Composant | Technologie | Rôle |
|-----------|-------------|------|
| **QRCode.js** | Bibliothèque Node.js | Génération images QR PNG |
| **PDFKit** | Bibliothèque Node.js | Génération PDFs fiches produits |
| **Filesystem** | Node.js fs/promises | Stockage QR codes + PDFs |
| **QRController** | Express controller | Endpoints génération batch |
| **DatabaseService** | Service layer | Marquage produits hasQRCode=1 |

### Structure Fichiers

```
public/qrcodes/
├── cerascan/                          # Partenaire root
│   ├── INTFURN01COLL01GRAY60X60.png  # QR code produit
│   ├── EXTFURN02COLL02BLUE40X40.png
│   └── pdf/
│       ├── INTFURN01COLL01GRAY60X60.pdf  # Fiche produit PDF
│       └── EXTFURN02COLL02BLUE40X40.pdf
│
├── partner808/                        # Partenaire 808
│   ├── INTFURN03...png
│   └── pdf/
│       └── INTFURN03...pdf
│
└── testing/                           # Partenaire test
    └── ...
```

**Isolation par partenaire:** Dossiers séparés garantissent pas de collisions noms fichiers.

## Génération QR Codes

### Paramètres QR

**Configuration recommandée:**
- **Taille:** 400×400 pixels (lisibilité optimale)
- **Correction erreur:** H (30%) - résistance dommages
- **Marge:** 4 modules (espace blanc autour)
- **Format:** PNG (compression lossless)

**Correction erreur:**
| Niveau | Capacité correction | Usage |
|--------|---------------------|-------|
| **L** | 7% | Documents haute qualité, impression HD |
| **M** | 15% | Usage standard |
| **Q** | 25% | Environnements contraints |
| **H** | 30% | Showroom (manipulations, usure) ✅ |

### API Génération

**Génération unitaire:**
```javascript
// src/services/QRService.js
const QRCode = require('qrcode');

async function generateQRCodeImage(url, size = 400, outputPath) {
  const options = {
    errorCorrectionLevel: 'H',
    type: 'image/png',
    quality: 1,
    margin: 4,
    width: size,
    color: {
      dark: '#000000',  // Noir
      light: '#FFFFFF'  // Blanc
    }
  };
  
  await QRCode.toFile(outputPath, url, options);
}

// Usage
const url = 'https://partner808.cerascan.fr/produit/INTFURN01COLL01GRAY60X60';
const path = 'public/qrcodes/partner808/INTFURN01COLL01GRAY60X60.png';
await generateQRCodeImage(url, 400, path);
```

**Génération batch:**
```javascript
// src/controllers/QRController.js
async function generateQRCodesForProducts(context, products) {
  const qrDir = getPartnerQRCodeDir(context.qrCodePartnerId);
  const results = { success: 0, errors: [] };
  
  for (const product of products) {
    try {
      const url = `https://${context.domain}/produit/${product.compositeId}`;
      const path = `${qrDir}/${product.compositeId}.png`;
      
      await generateQRCodeImage(url, 400, path);
      await DatabaseService.updateProduct(
        context.dbName,
        { hasQRCode: 1 },
        { compositeId: product.compositeId }
      );
      
      results.success++;
    } catch (error) {
      results.errors.push({ product: product.compositeId, error: error.message });
    }
  }
  
  return results;
}
```

### Endpoints API

**POST /admin/generate-qr-codes**

**Body:**
```json
{
  "option": "new_products",  // "all" | "new_products" | "selection"
  "productIds": [],          // Si option="selection"
  "size": 400,
  "errorCorrection": "H"
}
```

**Response:**
```json
{
  "success": true,
  "generated": 245,
  "errors": [
    { "product": "INTFURN01...", "error": "Filesystem write error" }
  ],
  "downloadUrl": "/downloads/qr-batch-1709132400.zip"
}
```

## Génération PDFs Fiches Produits

### Template PDF

**Structure fiche produit:**
```
┌────────────────────────────────────┐
│  [Logo Partenaire]                 │
│                                    │
│  [Photo Produit]                   │
│                                    │
│  Nom Produit                       │
│  Collection - Fournisseur          │
│                                    │
│  Dimensions: 60×60 cm              │
│  Finition: Mat                     │
│  Teinte: Gris                      │
│  Prix: 45.90 € TTC                 │
│                                    │
│  [QR Code]                         │
│  Scannez pour plus d'infos         │
└────────────────────────────────────┘
```

### API Génération PDF

```javascript
// src/services/PDFService.js
const PDFDocument = require('pdfkit');
const fs = require('fs');

async function generateProductPDF(context, product) {
  const pdfDir = `${getPartnerQRCodeDir(context.qrCodePartnerId)}/pdf`;
  const path = `${pdfDir}/${product.compositeId}.pdf`;
  
  // Créer dossier si inexistant
  await fs.promises.mkdir(pdfDir, { recursive: true });
  
  const doc = new PDFDocument({
    size: 'A6',           // Format carte postale (105×148 mm)
    margin: 10
  });
  
  const stream = fs.createWriteStream(path);
  doc.pipe(stream);
  
  // Logo partenaire (si disponible)
  if (context.logo) {
    doc.image(context.logo, 10, 10, { width: 50 });
  }
  
  // Photo produit (si disponible)
  if (product.imageUrl) {
    doc.image(product.imageUrl, 10, 70, { width: 85, height: 85 });
  }
  
  // Informations produit
  doc.fontSize(14).font('Helvetica-Bold')
     .text(product.nom, 10, 165, { width: 85, align: 'left' });
  
  doc.fontSize(10).font('Helvetica')
     .text(`${product.collection} - ${product.marque}`, 10, 185);
  
  doc.fontSize(9)
     .text(`Dimensions: ${product.longueur}×${product.largeur} cm`, 10, 200)
     .text(`Finition: ${product.finition}`, 10, 212)
     .text(`Teinte: ${product.teinte}`, 10, 224);
  
  if (product.prixPublicTTC) {
    doc.fontSize(12).font('Helvetica-Bold')
       .text(`${product.prixPublicTTC} € TTC`, 10, 240);
  }
  
  // QR code intégré
  const qrPath = `${getPartnerQRCodeDir(context.qrCodePartnerId)}/${product.compositeId}.png`;
  if (fs.existsSync(qrPath)) {
    doc.image(qrPath, 10, 260, { width: 60, height: 60 });
    doc.fontSize(7).text('Scannez pour plus d\'infos', 10, 325);
  }
  
  doc.end();
  
  return new Promise((resolve, reject) => {
    stream.on('finish', resolve);
    stream.on('error', reject);
  });
}
```

## Workflows Génération

### Workflow 1: Post-Import (Nouveaux Produits)

**Déclencheur:** Import CSV terminé, produits sans QR détectés
**Fréquence:** Hebdomadaire

```
[Admin] → Connexion /admin
         ↓
[Système] → Affiche: "245 produits sans QR code"
         ↓
[Admin] → Clic "Générer QR nouveaux produits"
         ↓
[QRController] → Query: SELECT * FROM {dbName} WHERE hasQRCode=0
         ↓
[Loop 245×] → Pour chaque produit:
                - Génération QR PNG
                - UPDATE hasQRCode=1
                - Émission SSE progression (optionnel)
         ↓
[ZIP Archive] → Compression QR codes + PDFs
         ↓
[Download] → Téléchargement ZIP (~50 MB)
         ↓
[Impression] → Collage échantillons showroom
```

### Workflow 2: Régénération Complète

**Déclencheur:** Changement domaine partenaire, migration serveur
**Fréquence:** Rare (1-2×/an)

```
[Admin] → Sélection "Tous les produits" + option "Forcer régénération"
         ↓
[Système] → Confirmation modal ("3807 QR seront régénérés, continuer ?")
         ↓
[Background Job] → Génération asynchrone (10-15 minutes)
         ↓
[Email Notification] → "Génération terminée: 3807 QR codes"
         ↓
[Admin] → Téléchargement ZIP (~200 MB)
```

## Optimisations Performance

### Génération Batch

**Problématique:** Génération 1000+ QR codes séquentielle = 5-10 minutes

**Solution:** Parallélisation avec limite concurrence

```javascript
const pLimit = require('p-limit');
const limit = pLimit(10); // Max 10 QR simultanés

async function generateQRCodesBatch(context, products) {
  const promises = products.map(product => 
    limit(() => generateQRCodeForProduct(context, product))
  );
  
  const results = await Promise.allSettled(promises);
  
  return {
    success: results.filter(r => r.status === 'fulfilled').length,
    errors: results.filter(r => r.status === 'rejected').map(r => r.reason)
  };
}
```

**Gain:** 1000 QR codes en ~2 minutes (au lieu de 8-10 minutes)

### Cache Images Produits

**Problématique:** Génération PDF charge image distante (fournisseur) = lent + dépendance réseau

**Solution:** Cache local images

```javascript
// Cache images dans public/cache/images/{codeFournisseur}/
const cachedImage = await cacheProductImage(product.imageUrl, product.codeFournisseur);
doc.image(cachedImage, ...); // Utilise cache local
```

### Compression ZIP

**Problématique:** ZIP 1000 PNG (400×400) = 150+ MB, téléchargement lent

**Solution:** PNG optimisés (pngquant) + compression ZIP max

```javascript
const archiver = require('archiver');

const archive = archiver('zip', {
  zlib: { level: 9 } // Compression maximale
});

// Optimiser PNG avant ajout ZIP
const optimizedPath = await optimizePNG(qrPath); // pngquant
archive.file(optimizedPath, { name: `${product.compositeId}.png` });
```

**Gain:** ZIP 1000 QR codes ~80 MB (au lieu de 150 MB)

## Troubleshooting

### QR code illisible après impression

**Symptômes:** Scan échoue, erreur "QR code invalide"

**Causes possibles:**
- Résolution impression insuffisante (< 300 DPI)
- Papier brillant (reflets)
- QR code trop petit (< 2×2 cm)

**Solutions:**
- Augmenter taille QR (600×600 px au lieu de 400×400)
- Utiliser papier mat (pas brillant)
- Imprimer test 1 feuille avant batch

### Erreur "Filesystem write error"

**Symptômes:** Génération QR échoue, log "EACCES: permission denied"

**Causes:**
- Permissions dossier `public/qrcodes/` insuffisantes
- Dossier partenaire inexistant

**Solutions:**
```bash
# Vérifier permissions
ls -la public/qrcodes/

# Corriger permissions
chmod -R 755 public/qrcodes/
chown -R www-data:www-data public/qrcodes/

# Créer dossier partenaire si manquant
mkdir -p public/qrcodes/{partnerId}
mkdir -p public/qrcodes/{partnerId}/pdf
```

### Génération batch timeout (> 5 min)

**Symptômes:** Requête HTTP timeout, génération incomplète

**Solution:** Passer en mode asynchrone + notification email

```javascript
// QRController.js
router.post('/admin/generate-qr-codes', async (req, res) => {
  const { products } = req.body;
  
  if (products.length > 500) {
    // Mode asynchrone pour batch volumineux
    generateQRCodesAsync(context, products, req.user.email);
    return res.json({ 
      success: true, 
      message: 'Génération lancée en arrière-plan. Email envoyé à la fin.' 
    });
  }
  
  // Mode synchrone pour batch < 500
  const results = await generateQRCodesBatch(context, products);
  res.json(results);
});
```

---
**Lignes:** 246/250
**Sources:** memory-bank/guides/, PWA-Configuration.md, product-structure.md
