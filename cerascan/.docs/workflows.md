# Workflows: CERASCAN

## Processus Métier

### Workflow 1: Onboarding Nouveau Partenaire

**Acteurs:** Super utilisateur (Laurent), nouveau partenaire
**Déclencheur:** Signature contrat partenaire
**Fréquence:** Occasionnel (1-2 nouveaux partenaires/mois)

**Étapes:**

```
[Laurent] → Création partenaire (admin74950)
                ↓
          [Système] Génère dbName, partnerId
                ↓
          [MySQL] CREATE TABLE {partner}_catalog
                ↓
          [Filesystem] Création /public/qrcodes/{partnerId}/
                ↓
[Laurent] → Configuration marque blanche (logo, couleurs, domaine)
                ↓
[Email] ← Identifiants admin partenaire (Sophie)
                ↓
[Sophie] → Première connexion /partner
                ↓
[Sophie] → Import catalogue initial CSV
                ↓
[Laurent] → Validation import (email notification)
                ↓
[Sophie] → Génération QR codes batch
                ↓
[Sophie] → Tests consultation produits
                ↓
[Laurent] → Configuration DNS domaine custom
                ↓
[Partenaire] → Mise en production showroom
```

**Décisions:**
- **Si partenaire test** → dbName='testing_catalog', pas de facturation
- **Si production** → dbName='{slug}_catalog', facturation mensuelle
- **Si domaine custom** → Configuration DNS CNAME

**Exceptions:**
- **Nom partenaire existant** : Modifier slug, régénérer dbName
- **Import initial échoue** : Assistance technique, correction CSV
- **Domaine custom indisponible** : Utiliser sous-domaine {partner}.cerascan.fr

---

### Workflow 2: Import Catalogue Hebdomadaire

**Acteurs:** Admin partenaire (Sophie), super utilisateur (Laurent)
**Déclencheur:** Réception CSV fournisseur (email/téléchargement)
**Fréquence:** Hebdomadaire

**Étapes:**

```
[Fournisseur] → Envoi CSV catalogue (email)
                ↓
[Sophie] → Téléchargement CSV, vérification format
                ↓
[Sophie] → Connexion /partner, upload CSV
                ↓
[Système] → Validation format (colonnes obligatoires)
                ↓
[Système] → Stockage temporaire fichier
                ↓
[Email] → Notification Laurent (validation requise)
                ↓
[Laurent] → Connexion /admin74950, validation import
                ↓
[Système] → Traitement CSV:
            - Parse lignes (skip header)
            - Pour chaque ligne:
              * Génération compositeId
              * INSERT ou UPDATE (si référence existe)
              * Log progression
                ↓
[DatabaseLogger] → Logs import (partner_logs table)
                ↓
[Email] → Rapport import (succès: N, erreurs: M)
                ↓
[Sophie] → Vérification catalogue /partner/products
                ↓
[Sophie] → Génération QR codes (nouveaux produits)
```

**Décisions:**
- **Si produit existe (référence)** → UPDATE champs (prix, finition, etc.)
- **Si produit nouveau** → INSERT avec compositeId généré
- **Si erreur ligne** → Log erreur, continue import autres lignes

**Exceptions:**
- **Format CSV invalide** : Email erreur à Sophie, correction requise
- **Encodage incorrect** : Email conseil "CSV UTF-8"
- **Produit dupliqué (compositeId)** : Log warning, skip ligne
- **Erreur connexion MySQL** : Rollback transaction, retry 3×

---

### Workflow 3: Consultation Produit Client Final

**Acteurs:** Visiteur showroom (Marc)
**Déclencheur:** Scan QR code échantillon
**Fréquence:** Continue (100-500 scans/jour/showroom)

**Étapes:**

```
[Marc] → Scan QR code (appareil photo smartphone)
                ↓
[QR URL] → https://partner808.cerascan.fr/produit/INTFURN01COLL01GRAY60X60
                ↓
[Express] → Middleware attachCurrentPartner (détecte partner808)
                ↓
[ExecutionContext] → fromRequest(req) [partnerId, dbName]
                ↓
[ProductController] → getProductByCompositeId(context, compositeId)
                ↓
[DatabaseService] → Query MySQL partner808_catalog
                ↓
[Cache Check] → IndexedDB lookup (10-50ms)
                ↓ HIT
[Alpine.js] → Rendu fiche produit (cached data)
                ↓ MISS
[EJS Template] → Rendu serveur (1500ms)
                ↓
[catalogueCache] → Sauvegarde IndexedDB (TTL 30min)
                ↓
[Marc] → Consultation fiche:
          - Dimensions, finition, prix
          - Photos, collection, fournisseur
          - Produits connexes
                ↓
[Marc] → Actions optionnelles:
          - Ajout liste d'envies
          - Partage email
          - Impression fiche
```

**Décisions:**
- **Si cache IndexedDB valide** → Affichage instantané (10-50ms)
- **Si cache expiré (> 30min)** → Rechargement serveur, mise à jour cache
- **Si visiteur authentifié** → Listes d'envies sauvegardées DB
- **Si visiteur anonyme** → Listes d'envies localStorage (temporaire)

**Exceptions:**
- **QR code endommagé** : Erreur scan, affichage message "QR illisible"
- **Produit supprimé** : Redirect catalogue, message "Produit indisponible"
- **Wi-Fi showroom indisponible** : Service Worker offline, cache IndexedDB
- **compositeId invalide** : Error 404, log erreur

---

### Workflow 4: Génération QR Codes Batch

**Acteurs:** Admin partenaire (Sophie)
**Déclencheur:** Import catalogue terminé, nouveaux produits sans QR
**Fréquence:** Hebdomadaire (après import)

**Étapes:**

```
[Sophie] → Connexion /admin, page "Génération QR"
                ↓
[Système] → Affiche stats: 245 produits sans QR
                ↓
[Sophie] → Sélection option "Nouveaux produits"
                ↓
[Sophie] → Configuration:
            - Taille: Moyenne (400×400px)
            - Correction: H (30%)
                ↓
[Sophie] → Clic "Générer"
                ↓
[QRController] → getProducts(dbName) WHERE hasQRCode=0
                ↓
[Loop] → Pour chaque produit (245 itérations):
          1. Génération URL unique
          2. QRCode.toFile(path, url, options)
          3. UPDATE hasQRCode=1
          4. Émission SSE progression (optionnel)
                ↓
[Filesystem] → Écriture /public/qrcodes/{partnerId}/
                ↓
[Système] → Génération ZIP archive (QR PNG + PDFs)
                ↓
[Response] → {success: true, generated: 245, downloadUrl: '/downloads/qr-batch-{timestamp}.zip'}
                ↓
[Sophie] → Téléchargement ZIP (~50 MB)
                ↓
[Sophie] → Décompression, impression PDF
                ↓
[Personnel] → Collage QR codes sur échantillons showroom
```

**Décisions:**
- **Si < 100 produits** → Génération synchrone, réponse immédiate
- **Si 100-1000 produits** → Génération asynchrone, SSE progression
- **Si > 1000 produits** → Génération background job, email notification fin

**Exceptions:**
- **Erreur écriture filesystem** : Log erreur, skip produit, continue
- **QR existant (hasQRCode=1)** : Skip produit (sauf régénération forcée)
- **Mémoire Node.js insuffisante** : Génération par lots de 500

---

## Standard Operating Procedures (SOP)

### SOP-01: Résolution Erreur Import CSV

**Objectif:** Diagnostiquer et corriger erreurs import catalogue
**Responsable:** Admin partenaire (Sophie) + Support technique (Laurent)

**Instructions:**
1. **Consulter email rapport import** → Identifier lignes en erreur
2. **Vérifier format CSV:**
   - [ ] Encodage UTF-8 (accents corrects)
   - [ ] Colonnes obligatoires présentes (reference, nom, longueur, largeur, finition)
   - [ ] Séparateur virgule (pas point-virgule)
3. **Corriger erreurs courantes:**
   - Colonnes manquantes → Ajouter avec valeurs vides
   - Prix mal formaté (45,90) → Remplacer par point (45.90)
   - Références dupliquées → Supprimer doublons Excel
4. **Tester import sur fichier réduit** → 10 premières lignes
5. **Si succès** → Import fichier complet
6. **Si échec persistant** → Contact support Laurent (capture d'écran erreur + CSV anonymisé)

**Vérifications:**
- [ ] Email rapport import reçu
- [ ] Erreurs identifiées et corrigées
- [ ] Test import 10 lignes OK
- [ ] Import complet OK
- [ ] Catalogue vérifié /partner/products

---

### SOP-02: Vérification Qualité QR Codes

**Objectif:** S'assurer lisibilité et fonctionnement QR codes avant impression
**Responsable:** Admin partenaire (Sophie)

**Instructions:**
1. **Télécharger ZIP QR codes** → Décompresser archive
2. **Vérifier aléatoirement 10 QR codes:**
   - [ ] Afficher QR à l'écran (PNG)
   - [ ] Scanner avec smartphone (appareil photo ou app)
   - [ ] Vérifier URL correcte (https://{domain}/produit/{compositeId})
   - [ ] Vérifier fiche produit s'ouvre correctement
3. **Tests qualité impression:**
   - [ ] Imprimer 1 feuille test (6 QR codes)
   - [ ] Scanner QR codes imprimés
   - [ ] Vérifier lisibilité (pas de bavures)
4. **Si tous tests OK** → Impression batch complète
5. **Si problème** → Régénérer QR codes concernés (taille supérieure, correction H)

**Vérifications:**
- [ ] 10 QR codes testés à l'écran
- [ ] 10 QR codes scannés OK
- [ ] Feuille test imprimée et scannée OK
- [ ] Impression batch lancée

---

### SOP-03: Activation/Désactivation Produits Saisonniers

**Objectif:** Gérer visibilité produits selon disponibilité stocks/saisons
**Responsable:** Admin partenaire (Sophie)

**Instructions:**
1. **Connexion /admin** → Page "Gestion Catalogue"
2. **Filtrer produits:**
   - Par fournisseur (ex: FURN01)
   - Par collection (ex: COLL_WINTER)
3. **Sélection produits à désactiver** → Checkbox multiples
4. **Action "Désactiver"** → Confirmation modal
5. **Vérification:**
   - [ ] Produits n'apparaissent plus catalogue public
   - [ ] QR codes redirigent vers message "Produit indisponible"
   - [ ] Logs modification enregistrés
6. **Pour réactivation:**
   - Filtrer "Produits désactivés"
   - Sélection → Action "Activer"

**Vérifications:**
- [ ] Produits désactivés (statut updated)
- [ ] Catalogue public vérifié (produits absents)
- [ ] QR codes testés (redirection correcte)
- [ ] Logs audit consultés

---
**Lignes:** 248/250
**Sources:** business-processes.md, import-workflow.md, user-stories-essential.md
