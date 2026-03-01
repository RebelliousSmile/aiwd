# Deployment & PWA: CERASCAN

## Infrastructure

**Hébergement:** OVH Cloud France (souveraineté données)
**Architecture:** Serveur unique Node.js + MySQL (scalabilité horizontale future)
**Domaine principal:** cerascan.fr
**Sous-domaines partenaires:** {partner}.cerascan.fr (wildcard DNS)

## Stack Déployé

| Composant | Technologie | Configuration |
|-----------|-------------|---------------|
| **Serveur HTTP** | Node.js LTS | PM2 (process manager, auto-restart) |
| **Reverse Proxy** | Nginx | HTTPS termination, gzip, cache statique |
| **Base données** | MySQL 8.x | InnoDB, charset utf8mb4 |
| **Stockage fichiers** | Filesystem | /public/qrcodes/{partnerId}/ |
| **Certificats SSL** | Let's Encrypt | Auto-renewal via certbot |

## Configuration PWA

### Manifest Dynamique

**Route:** `app.get('/manifest.json', attachCurrentPartner, ...)`

Le manifest est généré dynamiquement pour s'adapter à chaque partenaire :

```javascript
// Exemple manifest partner808
{
  "name": "PARTNER808 - Catalogue Carrelages",
  "short_name": "PARTNER808",
  "description": "Catalogue produits céramiques",
  "start_url": "/?source=pwa",
  "display": "standalone",
  "theme_color": "#1565C0",
  "background_color": "#ffffff",
  "orientation": "portrait",
  "lang": "fr-FR",
  "icons": [
    { "src": "/favicon-16x16.png", "sizes": "16x16", "type": "image/png" },
    { "src": "/favicon-32x32.png", "sizes": "32x32", "type": "image/png" },
    { "src": "/apple-touch-icon.png", "sizes": "180x180", "type": "image/png" },
    { "src": "/icon-192x192.png", "sizes": "192x192", "type": "image/png", "purpose": "any maskable" },
    { "src": "/icon-512x512.png", "sizes": "512x512", "type": "image/png", "purpose": "any maskable" }
  ],
  "shortcuts": [
    { "name": "Catalogue", "url": "/catalogue", "description": "Parcourir produits" },
    { "name": "Listes", "url": "/wishlists", "description": "Mes listes d'envies" }
  ]
}
```

**Adaptation par partenaire:**
- Nom dynamique : `${PARTNER_NAME} - Catalogue Carrelages`
- Nom court : Slug partenaire en majuscules
- Couleur thème : Personnalisable (défaut #1565C0)

### Service Worker

**Fichier:** `public/sw.js`
**Version:** cerascan-v1 (incrémenter pour forcer mise à jour)

**Stratégies de cache:**

```javascript
// Cache-First: Assets statiques (longue durée)
/^.*\.(css|js|png|jpg|svg|woff2)$/
  → cache cerascan-assets-v1
  → fallback réseau si absent cache

// Network-First: Pages dynamiques (catalogue, produits)
/^\/(catalogue|produit|partner)/
  → réseau en priorité
  → cache cerascan-pages-v1 si offline
  → fallback offline.html si tout échoue
```

**Caches utilisés:**
- `cerascan-assets-v1` : Assets statiques (CSS, JS, images, polices)
- `cerascan-pages-v1` : Pages HTML dynamiques (catalogue, fiches)

**Gestion mise à jour:**
```javascript
// Détection nouveau Service Worker
self.addEventListener('install', (event) => {
  self.skipWaiting(); // Force activation immédiate
});

// Nettoyage anciens caches
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames
          .filter(name => name.startsWith('cerascan-') && name !== 'cerascan-assets-v1' && name !== 'cerascan-pages-v1')
          .map(name => caches.delete(name))
      );
    })
  );
});
```

### Enregistrement Service Worker

**Fichier:** `public/js/sw-register.js`

**Fonctionnalités:**
- Enregistrement automatique au chargement page
- Détection mises à jour avec notification utilisateur
- Gestion bouton "Installer l'app" (beforeinstallprompt)
- Détection mode standalone (app installée)

**Notifications mise à jour:**
```javascript
navigator.serviceWorker.addEventListener('controllerchange', () => {
  // Nouveau SW activé
  showUpdateNotification("Nouvelle version disponible. Rafraîchir ?");
});
```

### Icônes PWA

**Générées:** Lettres "CS" (CERASCAN) en bleu sur fond blanc

| Fichier | Taille | Usage |
|---------|--------|-------|
| `favicon-16x16.png` | 16×16 | Favicon navigateur (tab) |
| `favicon-32x32.png` | 32×32 | Favicon navigateur (barre adresse) |
| `favicon.png` | 32×32 | Fallback favicon |
| `apple-touch-icon.png` | 180×180 | iOS home screen |
| `icon-192x192.png` | 192×192 | Android home screen, splash |
| `icon-512x512.png` | 512×512 | Android splash, store listing |

**Format:** PNG avec transparence, optimisées TinyPNG

## Déploiement Application

### Environnements

| Environnement | URL | Usage |
|---------------|-----|-------|
| **Production** | https://cerascan.fr | Partenaires réels |
| **Testing** | https://testing.cerascan.fr | Tests intégration |
| **Local** | http://localhost:3000 | Développement |

### Variables d'Environnement

**Fichier:** `.env` (gitignored)

```bash
# Base données
DB_HOST=localhost
DB_USER=cerascan_user
DB_PASSWORD=***
DB_DATABASE=euroceramic_mysql

# Serveur
PORT=3000
NODE_ENV=production

# Email notifications
SMTP_HOST=smtp.exemple.com
SMTP_PORT=587
SMTP_USER=noreply@cerascan.fr
SMTP_PASSWORD=***
ADMIN_EMAIL=admin@efficeram.fr

# Sécurité
SESSION_SECRET=***
CSRF_SECRET=***

# PWA
BASE_URL=https://cerascan.fr
```

### Procédure Déploiement

**Déploiement manuel (SSH):**

```bash
# 1. Connexion serveur
ssh user@cerascan.fr

# 2. Pull dernières modifications
cd /var/www/cerascan
git pull origin main

# 3. Installation dépendances
npm install --production

# 4. Migrations DB (si nécessaire)
npm run migrate

# 5. Rebuild assets (si modifs CSS/JS)
npm run build

# 6. Restart PM2
pm2 restart cerascan

# 7. Vérification logs
pm2 logs cerascan --lines 50

# 8. Test smoke (curl endpoints critiques)
curl https://cerascan.fr/healthcheck
```

**Déploiement automatisé (future CI/CD):**
```yaml
# .github/workflows/deploy.yml
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm test
      - run: ssh deploy@cerascan.fr 'cd /var/www/cerascan && git pull && pm2 restart cerascan'
```

## Configuration Nginx

**Fichier:** `/etc/nginx/sites-available/cerascan`

```nginx
# Redirect HTTP → HTTPS
server {
  listen 80;
  server_name cerascan.fr *.cerascan.fr;
  return 301 https://$host$request_uri;
}

# HTTPS
server {
  listen 443 ssl http2;
  server_name cerascan.fr *.cerascan.fr;

  # SSL Certificates (Let's Encrypt)
  ssl_certificate /etc/letsencrypt/live/cerascan.fr/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/cerascan.fr/privkey.pem;
  ssl_protocols TLSv1.2 TLSv1.3;

  # Gzip compression
  gzip on;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml;

  # Cache statique (assets)
  location ~* \.(css|js|png|jpg|jpeg|gif|svg|woff2|ttf)$ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }

  # Proxy vers Node.js
  location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_cache_bypass $http_upgrade;
  }

  # Rate limiting (protection DDoS)
  limit_req_zone $binary_remote_addr zone=api:10m rate=100r/m;
  location /api/ {
    limit_req zone=api burst=20;
    proxy_pass http://localhost:3000;
  }
}
```

## Monitoring & Logs

**PM2 Monitoring:**
```bash
pm2 monit             # Monitoring temps réel (CPU, RAM)
pm2 logs cerascan     # Logs application
pm2 logs cerascan --err  # Logs erreurs uniquement
```

**Logs MySQL:**
```bash
tail -f /var/log/mysql/error.log      # Erreurs MySQL
tail -f /var/log/mysql/slow-query.log # Requêtes lentes
```

**Logs Nginx:**
```bash
tail -f /var/log/nginx/access.log   # Requêtes HTTP
tail -f /var/log/nginx/error.log    # Erreurs Nginx
```

**DatabaseLogger (logs applicatifs):**
- Table : `{partner}_logs` (logs isolés par partenaire)
- Rétention : 90 jours
- Niveaux : ERROR, WARN, INFO, DEBUG

## Troubleshooting Déploiement

### PWA non installable

**Symptômes:** Bouton "Installer l'app" n'apparaît pas

**Diagnostic:**
```bash
# Vérifier HTTPS
curl -I https://cerascan.fr

# Vérifier manifest
curl https://cerascan.fr/manifest.json

# Vérifier Service Worker
curl https://cerascan.fr/sw.js
```

**Solutions:**
- Vérifier certificat SSL valide (HTTPS requis)
- Vérifier manifest.json accessible (pas d'erreur 404)
- Vérifier Service Worker enregistré (DevTools → Application → Service Workers)

### Service Worker cache obsolète

**Symptômes:** Modifications CSS/JS non visibles après déploiement

**Solution:**
```javascript
// Incrémenter version cache dans sw.js
const CACHE_VERSION = 'cerascan-assets-v2'; // v1 → v2

// Ou forcer refresh
navigator.serviceWorker.getRegistrations().then(registrations => {
  registrations.forEach(reg => reg.unregister());
});
location.reload(true); // Hard refresh
```

### Erreur "Partner not found" après déploiement

**Symptômes:** 404 sur tous sous-domaines partenaires

**Diagnostic:**
```bash
# Vérifier DNS wildcard
dig partner808.cerascan.fr

# Vérifier table partners
mysql> SELECT id, name, slug, dbName FROM partners;
```

**Solutions:**
- Configurer DNS wildcard `*.cerascan.fr` → IP serveur
- Vérifier Nginx `server_name *.cerascan.fr`

---
**Lignes:** 248/250
**Sources:** PWA-Configuration.md, deployment-guides/
