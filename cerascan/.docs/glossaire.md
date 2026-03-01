# Glossaire: CERASCAN

## Termes Métier Céramique

| Terme | Définition | Anglais |
|-------|------------|---------|
| **Carrelage** | Revêtement céramique pour sol ou mur | Tile |
| **Finition** | Aspect surface (Mat, Brillant, Structuré) | Finish |
| **Teinte** | Couleur principale du produit | Shade / Color |
| **Collection** | Gamme produits d'un fournisseur | Collection |
| **Référence** | Code unique produit (ex: INTFURN01COLL01GRAY60X60) | Reference / SKU |
| **Fournisseur** | Fabricant céramique (Imola, Refin, etc.) | Supplier |
| **Showroom** | Espace exposition physique échantillons | Showroom |
| **Échantillon** | Pièce céramique exposée avec QR code | Sample |

## Termes Techniques CERASCAN

| Terme | Définition | Contexte |
|-------|------------|----------|
| **Multi-tenant** | Architecture isolant données par partenaire | Chaque partenaire = base catalogue séparée |
| **ExecutionContext** | Objet encapsulant contexte partenaire + requête | Pattern SOLID, passé aux services |
| **Composite ID** | ID unique composite (ex: INT\|FURN01\|COLL01\|GRAY\|60\|60) | Clé primaire produits |
| **Partner** | Distributeur locataire de la solution | Showroom utilisant CERASCAN |
| **Marque blanche** | Personnalisation visuelle par partenaire | Logo, couleurs, domaine custom |
| **QR Code** | Code-barres 2D scannnable smartphone | Collé sur échantillons, ouvre fiche produit |
| **IndexedDB** | Base données navigateur (cache client) | Cache catalogue, 10-50ms |
| **PWA** | Progressive Web App (installable mobile) | Service worker, offline |
| **DatabaseService** | Service Node.js accès MySQL multi-tenant | Singleton, pool connexions |

## Acronymes

| Acronyme | Signification | Définition |
|----------|---------------|------------|
| **CSV** | Comma-Separated Values | Format import catalogue (Excel) |
| **TTL** | Time To Live | Durée validité cache (30 minutes) |
| **SSE** | Server-Sent Events | Push temps réel serveur → client |
| **EJS** | Embedded JavaScript | Moteur template HTML |
| **SaaS** | Software as a Service | Logiciel hébergé cloud |
| **B2B2C** | Business to Business to Consumer | EFFICERAM → Partenaires → Clients |
| **PWA** | Progressive Web App | App web installable |
| **QR** | Quick Response | Type code-barres 2D |
| **TTC** | Toutes Taxes Comprises | Prix avec TVA |
| **INT/EXT** | Intérieur/Extérieur | Destination produit |

## Composants Système

| Nom | Type | Description |
|-----|------|-------------|
| **EFFICERAM** | Entreprise | Développeur solution (Euro Ceramiche France) |
| **CERASCAN** | Plateforme | Solution SaaS multi-tenant |
| **DatabaseService** | Service Node.js | Couche abstraction MySQL |
| **ExecutionContext** | Classe | Contexte partenaire (pattern SOLID) |
| **catalogueCache** | Module JS | Cache IndexedDB client-side |
| **Alpine.js** | Framework JS | Réactivité UI (léger, 15KB) |
| **Tailwind CSS** | Framework CSS | Utility-first styling |
| **PDFKit** | Bibliothèque | Génération PDF fiches produits |
| **QRCode.js** | Bibliothèque | Génération QR codes |
| **mysql2** | Driver Node.js | Connexion MySQL avec pool |

## Termes Architecture

| Terme | Définition | Contexte |
|-------|------------|----------|
| **Service Layer** | Couche métier isolée des controllers | DatabaseService, QRService |
| **Repository Pattern** | Abstraction accès données | Pool MySQL, requêtes préparées |
| **Cache-First Pattern** | Stratégie cache prioritaire | IndexedDB → localStorage → serveur |
| **Isolation Données** | Séparation stricte données partenaires | Tables `{partner}_catalog` |
| **Factory Method** | Pattern création objets | ExecutionContext.fromRequest() |
| **Composite Key** | Clé primaire multi-champs | INT\|codeFournisseur\|collection\|teinte\|L\|l |
| **Middleware Express** | Fonction traitement requête | attachCurrentPartner, validateCSRF |
| **Prepared Statement** | Requête SQL paramétrée | Protection injection SQL |

## Termes Métier Spécifiques

| Terme | Définition | Exemple |
|-------|------------|---------|
| **codeFournisseur** | Code numérique fournisseur | FURN01, FURN02 |
| **intExt** | Destination produit | INT (intérieur), EXT (extérieur) |
| **compositeId** | ID composite unique produit | INTFURN01COLL01GRAY60X60 |
| **partnerDbName** | Nom base catalogue partenaire | cerascan_catalog, partner808_catalog |
| **qrCodePartnerId** | ID partenaire pour chemins QR | cerascan, partner808 |
| **prixPublicTTC** | Prix TTC affiché clients | 45.90 € |
| **userRole** | Rôle utilisateur | SELLER (vendeur), PARTNER (admin), VISITOR |

## Termes Frontend

| Terme | Définition | Contexte |
|-------|------------|----------|
| **Alpine.js directive** | Attribut HTML réactif | x-data, x-show, x-if, @click |
| **Cache strategy** | Stratégie gestion cache | Cache-first, Network-first |
| **Service Worker** | Script background PWA | Gestion cache, offline |
| **Manifest PWA** | Fichier config PWA | Icônes, couleurs, nom app |
| **Standalone mode** | Mode plein écran PWA | Sans barre navigation navigateur |
| **IndexedDB store** | Table IndexedDB | products (catalogue cached) |
| **TTL validation** | Vérification expiration cache | 30 minutes max |
| **Lazy loading** | Chargement différé images | Optimisation performance |

## Termes à Éviter

| Incorrect | Correct | Raison |
|-----------|---------|--------|
| "Base de données partenaire" | "Base catalogue partenaire" | Confusion avec table `partners` système |
| "ID produit" | "Composite ID" ou "référence" | Ambiguïté (pas d'auto-increment) |
| "Utilisateur admin" | "Super utilisateur" ou "Admin partenaire" | Deux niveaux distincts |
| "Tableau" | "Table MySQL" ou "catalogue" | Confusion (table HTML vs DB) |
| "Cache serveur" | "Cache client IndexedDB" | Cache côté navigateur |
| "Marque" | "Fournisseur" ou "marque produit" | Confusion avec marque blanche |
| "Partner ID" | "partnerId" (camelCase) | Convention code |
| "Database name" | "dbName" (camelCase) | Convention code |

---
**Lignes:** 115/250
**Màj:** 2026-02-28
**Sources:** architecture-core.md, multi-tenant-guide.md, product-structure.md, frontend-patterns.md
