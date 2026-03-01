# Multi-Tenant Architecture: CERASCAN

## Principe d'Isolation

CERASCAN utilise une architecture multi-tenant basée sur l'isolation par table MySQL. Chaque partenaire dispose de sa propre table catalogue (`{partner}_catalog`), garantissant séparation stricte des données.

**Avantages:**
- ✅ Isolation données complète (pas de fuites inter-partenaires)
- ✅ Scalabilité horizontale (sharding futur par partenaire)
- ✅ Backup/restore granulaire (par partenaire)
- ✅ Sécurité renforcée (SQL injection limitée à 1 partenaire)

**Inconvénients:**
- ⚠️ Gestion schéma (migrations sur N tables)
- ⚠️ Requêtes cross-partenaires complexes (unions multiples)

## ExecutionContext (Pattern SOLID)

### Responsabilité Unique

**ExecutionContext** encapsule **contexte partenaire + requête HTTP**, propagé aux services métier. Remplace passage manuel parameterspartenaire multiple (partnerId, dbName, etc.).

**Avant (anti-pattern):**
```javascript
function getProducts(partnerId, dbName, partnerName) {
  // Trop de paramètres, erreurs faciles
}
```

**Après (SOLID):**
```javascript
const context = ExecutionContext.fromRequest(req);
function getProducts(context) {
  const { dbName } = context;
  // Contexte complet encapsulé
}
```

### Factory Methods

**3 méthodes création selon contexte:**

```javascript
// 1. Depuis requête HTTP (controllers Express)
const context = ExecutionContext.fromRequest(req);
// Extrait partner depuis req.currentPartner (middleware attachCurrentPartner)

// 2. Depuis objet partenaire (scripts, services)
const partner = await DatabaseService.getPartnerById(1);
const context = ExecutionContext.fromPartner(partner);

// 3. Contexte vide (opérations génériques, tests)
const context = ExecutionContext.empty();
// hasValidPartner() retourne false
```

### Propriétés Exposées

```javascript
context.partnerId        // ID partenaire (ex: '1')
context.dbName          // Nom base catalogue (ex: 'cerascan_catalog')
context.partnerName     // Nom commercial (ex: 'CERASCAN')
context.partnerSlug     // Slug URL-safe (ex: 'cerascan')
context.qrCodePartnerId // ID pour chemins fichiers (ex: 'cerascan')
context.domain          // Domaine custom (ex: 'www.mes-tarifs.com')

// Méthodes validation
context.hasValidPartner()  // true si contexte valide
context.getDebugInfo()     // Object complet pour logs
```

### Validation Contexte

```javascript
// Controllers : toujours valider avant utilisation
if (!context.hasValidPartner()) {
  return res.status(404).send('Partenaire invalide');
}

// Services : assume contexte valide (responsabilité controller)
async function getProducts(context) {
  // Pas de validation ici (principe SOLID)
  const products = await DatabaseService.getProducts(context.dbName);
  return products;
}
```

## DatabaseService - Couche Abstraction

### Architecture Service Layer

**DatabaseService** est un singleton fournissant API unifiée pour accès MySQL multi-tenant. Encapsule pool connexions, prepared statements, validation whitelist.

**Responsabilités:**
- Gestion pool connexions MySQL (réutilisation, timeout)
- Validation whitelist noms tables (protection injection SQL)
- Prepared statements (paramètres liés)
- Logging requêtes (DatabaseLogger par partenaire)
- Retry logique (erreurs temporaires MySQL)

### API CRUD Produits

**Récupération:**
```javascript
// Tous produits catalogue
const products = await DatabaseService.getProducts(context.dbName);
// SELECT * FROM {dbName} WHERE 1=1

// Produit par composite ID
const product = await DatabaseService.getProductByCompositeId(
  context.partnerName,
  'INTFURN01COLL01GRAY60X60'
);
// SELECT * FROM {dbName} WHERE compositeId = ? LIMIT 1

// Produits filtrés
const filtered = await DatabaseService.getProductsFiltered(context.dbName, {
  codeFournisseur: '100',
  categorie: 'CARRELAGE',
  finition: 'MAT'
});
// SELECT * FROM {dbName} WHERE codeFournisseur=? AND categorie=? AND finition=?

// Statistiques
const stats = await DatabaseService.getProductStats(context.dbName);
// { total: 3807, suppliers: 12, categories: 5, collections: 48 }
```

**Insertion:**
```javascript
const productData = {
  intExt: 'INT',
  codeFournisseur: '100',
  collection: 'COLL01',
  // ... autres champs
};

const insertedId = await DatabaseService.insertProduct(context.dbName, productData);
// INSERT INTO {dbName} (intExt, codeFournisseur, ...) VALUES (?, ?, ...)
// Note: pas d'auto-increment, compositeId généré avant insertion
```

**Mise à jour:**
```javascript
const updates = { prixPublicTTC: 45.99, finition: 'MAT' };
const conditions = { compositeId: 'INTFURN01COLL01GRAY60X60' };

const affectedRows = await DatabaseService.updateProduct(
  context.dbName,
  updates,
  conditions
);
// UPDATE {dbName} SET prixPublicTTC=?, finition=? WHERE compositeId=?
```

**Suppression:**
```javascript
// Suppression unitaire
const deleted = await DatabaseService.deleteProduct(
  context.dbName,
  { compositeId: 'INTFURN01...' }
);
// DELETE FROM {dbName} WHERE compositeId=? LIMIT 1

// Suppression masse
const deletedCount = await DatabaseService.deleteProducts(
  context.dbName,
  { codeFournisseur: '100' }
);
// DELETE FROM {dbName} WHERE codeFournisseur=?
```

### API Partenaires

**Récupération:**
```javascript
// Tous partenaires (table système)
const partners = await DatabaseService.getAllPartners();
// SELECT * FROM partners ORDER BY name

// Partenaire par ID
const partner = await DatabaseService.getPartnerById(1);
// SELECT * FROM partners WHERE id=? LIMIT 1

// Partenaire par domaine (résolution multi-tenant)
const partner = await DatabaseService.getPartnerByDomain('partner808.cerascan.fr');
// SELECT * FROM partners WHERE domain=? OR slug=? LIMIT 1
```

**Création:**
```javascript
const partnerData = {
  name: 'Nouveau Partenaire',
  slug: 'nouveau-partenaire',
  dbName: 'nouveau_catalog',
  domain: 'nouveau.cerascan.fr',
  email: 'admin@nouveau.com'
};

const partnerId = await DatabaseService.createPartner(partnerData);
// INSERT INTO partners (...) VALUES (...)
// Puis CREATE TABLE {dbName} LIKE cerascan_catalog
```

**Mise à jour:**
```javascript
const updates = { email: 'newemail@exemple.com', logo: '/uploads/logo.png' };
const affectedRows = await DatabaseService.updatePartner(1, updates);
// UPDATE partners SET email=?, logo=? WHERE id=?
```

### Sécurité Multi-Tenant

**Validation Whitelist:**
```javascript
// DatabaseService vérifie dbName contre table partners
const validTables = await connection.query('SELECT dbName FROM partners');
if (!validTables.includes(dbName)) {
  throw new Error(`Table non autorisée: ${dbName}`);
}
```

**Prepared Statements:**
```javascript
// TOUJOURS utiliser paramètres liés (jamais concaténation)
const query = 'SELECT * FROM ?? WHERE compositeId = ?';
const params = [dbName, compositeId];
const results = await connection.query(query, params);

// mysql2 échappe automatiquement ?? (identifiants) et ? (valeurs)
```

**Isolation Logs:**
```javascript
// Logs stockés dans table partenaire (pas de logs centralisés)
const logTable = `${dbName.replace('_catalog', '_logs')}`;
await connection.query(
  `INSERT INTO ?? (level, message, timestamp) VALUES (?, ?, ?)`,
  [logTable, 'ERROR', message, Date.now()]
);
```

## Patterns Multi-Tenant

### Pattern 1: Middleware attachCurrentPartner

**Rôle:** Résoudre partenaire depuis domaine requête, attacher à `req.currentPartner`

```javascript
// src/middleware/attachCurrentPartner.js
async function attachCurrentPartner(req, res, next) {
  const hostname = req.hostname; // partner808.cerascan.fr
  
  try {
    // Résolution partenaire par domaine
    const partner = await DatabaseService.getPartnerByDomain(hostname);
    
    if (!partner) {
      // Fallback partenaire root (cerascan.fr)
      req.currentPartner = await DatabaseService.getPartnerById(1);
    } else {
      req.currentPartner = partner;
    }
    
    next();
  } catch (error) {
    next(error);
  }
}

// app.js
app.use('*', attachCurrentPartner); // Appliqué toutes routes
```

### Pattern 2: Service Layer Injection

**Rôle:** Services métier acceptent ExecutionContext, pas req (découplage HTTP)

```javascript
// ❌ Couplage HTTP (anti-pattern)
class ProductService {
  async getProducts(req) {
    const dbName = req.currentPartner.dbName; // Dépendance req
  }
}

// ✅ Découplage (SOLID)
class ProductService {
  async getProducts(context) {
    const dbName = context.dbName; // Contexte abstrait
    return DatabaseService.getProducts(dbName);
  }
}

// Controller
router.get('/products', async (req, res) => {
  const context = ExecutionContext.fromRequest(req);
  const products = await ProductService.getProducts(context);
  res.json(products);
});
```

### Pattern 3: Repository Pattern MySQL

**Rôle:** Pool connexions MySQL réutilisé, timeout gestion, retry automatique

```javascript
// DatabaseService utilise pool mysql2
const pool = mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  connectionLimit: 10,
  queueLimit: 0,
  waitForConnections: true,
  connectTimeout: 10000
});

// Méthode executeQuery (retry logique)
async function executeQuery(query, params, retries = 3) {
  try {
    const [results] = await pool.query(query, params);
    return results;
  } catch (error) {
    if (retries > 0 && error.code === 'PROTOCOL_CONNECTION_LOST') {
      console.warn(`Connexion MySQL perdue, retry (${retries} restants)`);
      return executeQuery(query, params, retries - 1);
    }
    throw error;
  }
}
```

## Migrations Multi-Tenant

**Problématique:** Appliquer modification schéma sur N tables partenaires

**Solution:** Script migration itère sur tous partenaires

```javascript
// migrations/add-column-promo.js
const partners = await DatabaseService.getAllPartners();

for (const partner of partners) {
  const { dbName } = partner;
  
  try {
    await pool.query(`ALTER TABLE ?? ADD COLUMN promo BOOLEAN DEFAULT FALSE`, [dbName]);
    console.log(`✓ Migration appliquée: ${dbName}`);
  } catch (error) {
    console.error(`✗ Erreur migration ${dbName}:`, error.message);
  }
}
```

---
**Lignes:** 244/250
**Sources:** multi-tenant-guide.md, architecture-core.md, database-operations.md
