# Utilisateurs: CERASCAN

## Personas

### Persona 1: Marc - Visiteur Showroom

**Profil:**
- Âge: 35-50 ans
- Fonction: Client final (particulier/professionnel BTP)
- Compétences techniques: Basiques (usage smartphone)

**Contexte d'usage:**
Visite showroom céramique, souhaite consulter infos produits de manière autonome sans solliciter vendeur. Utilise smartphone pour scanner QR codes collés sur échantillons. Recherche dimensions, prix, disponibilité.

**Objectifs:**
1. Accéder rapidement aux infos produits (scan → fiche)
2. Comparer plusieurs produits (caractéristiques, prix)
3. Sauvegarder sélection (listes d'envies)
4. Partager sélection (email, impression)

**Frustrations:**
- Attendre disponibilité vendeur
- Informations incomplètes sur étiquettes physiques
- Oublier références produits sélectionnés

**Attentes documentation:**
- Pas de documentation (UX intuitive)
- Aide contextuelle dans l'app si besoin
- FAQ courtes (Comment scanner ? Comment sauvegarder ?)

---

### Persona 2: Julie - Vendeuse Showroom

**Profil:**
- Âge: 25-40 ans
- Fonction: Conseillère vente showroom
- Compétences techniques: Intermédiaires (usage quotidien web)

**Contexte d'usage:**
Accompagne clients en showroom, utilise CERASCAN sur tablette/mobile pour accéder infos privilégiées (stocks, prix pro, produits connexes). Consultation rapide pendant échange client.

**Objectifs:**
1. Accéder infos privilégiées (prix, stocks)
2. Proposer alternatives/produits connexes
3. Consulter historique listes clients
4. Générer devis rapidement

**Frustrations:**
- Informations erronées/obsolètes
- Lenteur chargement catalogue (3000+ produits)
- Perte accès en zone Wi-Fi faible

**Attentes documentation:**
- Guide rapide (1-2 pages PDF)
- Tutoriel vidéo court (3-5 min)
- Procédures pas-à-pas (accès rôle SELLER)

---

### Persona 3: Sophie - Admin Partenaire

**Profil:**
- Âge: 35-50 ans
- Fonction: Propriétaire/Responsable showroom
- Compétences techniques: Moyennes (usage bureautique, CSV)

**Contexte d'usage:**
Gère catalogue produits (import CSV, activation/désactivation), personnalise marque blanche (logo, couleurs), consulte statistiques consultation. Utilisation hebdomadaire depuis bureau.

**Objectifs:**
1. Maintenir catalogue à jour (imports CSV fréquents)
2. Personnaliser apparence (logo, couleurs, infos contact)
3. Suivre performances (produits consultés, fournisseurs populaires)
4. Générer QR codes pour nouveaux produits

**Frustrations:**
- Erreurs import CSV (colonnes manquantes, encodage)
- Temps génération QR codes (lenteur batch 1000+)
- Statistiques peu détaillées

**Attentes documentation:**
- Guide utilisateur complet (PDF 20-30 pages)
- Tutoriels par tâche (import, QR, stats)
- FAQ troubleshooting (problèmes courants)
- Captures d'écran annotées

---

### Persona 4: Laurent - Super Utilisateur

**Profil:**
- Âge: 40-55 ans
- Fonction: Développeur/Patron EFFICERAM
- Compétences techniques: Élevées (dev Node.js/MySQL)

**Contexte d'usage:**
Supervise réseau partenaires, configure application, automatise tâches, monitore performances. Accès interface admin super-utilisateur (`/admin74950`). Utilisation quotidienne.

**Objectifs:**
1. Superviser réseau partenaires (dashboard multi-tenant)
2. Automatiser imports/QR (scripts, batch)
3. Garantir sécurité (logs audit, contrôle accès)
4. Optimiser performances (monitoring, caching)

**Frustrations:**
- Tâches manuelles chronophages (génération QR un par un)
- Manque visibilité activité partenaires
- Erreurs import CSV (messages peu clairs)

**Attentes documentation:**
- Documentation technique (architecture, API)
- Guides développeur (patterns, services)
- Procédures admin (multi-tenant, sécurité)
- Diagrammes architecture (PlantUML)

## Parcours Utilisateurs

### Parcours 1: Consultation Produit (Marc - Visiteur)

**Persona concerné:** Marc (Visiteur Showroom)
**Fréquence:** Ponctuel (1 fois par visite showroom)

**Étapes:**
1. **Arrivée showroom** → Repérage échantillons carrelage
2. **Scan QR code** → Application appareil photo ou app dédiée
3. **Chargement fiche** → URL ouvre fiche produit (10-50ms cache)
4. **Consultation infos** → Dimensions, finition, prix, collection
5. **Actions:**
   - Ajout liste d'envies (optionnel)
   - Consultation produits connexes (optionnel)
   - Partage par email (optionnel)
6. **Retour showroom** → Continue navigation physique

**Points de friction:**
- QR code endommagé/illisible
- Wi-Fi showroom instable
- Fiche produit trop longue (scroll mobile)

---

### Parcours 2: Import Catalogue (Sophie - Admin)

**Persona concerné:** Sophie (Admin Partenaire)
**Fréquence:** Hebdomadaire (nouveaux produits fournisseur)

**Étapes:**
1. **Réception CSV fournisseur** → Email/téléchargement
2. **Vérification format** → Colonnes obligatoires (reference, nom, dimensions)
3. **Connexion CERASCAN** → Page partenaire (/partner)
4. **Upload CSV** → Glisser-déposer ou parcourir
5. **Validation fichier** → Système vérifie format, affiche aperçu
6. **Attente email confirmation** → Email envoyé super-utilisateur
7. **Import effectif** → Super-utilisateur valide, import lancé
8. **Vérification catalogue** → Consultation nouveaux produits
9. **Génération QR codes** → Pour produits sans QR (voir parcours 3)

**Points de friction:**
- Format CSV incorrect (encodage, colonnes)
- Produits dupliqués (gestion références existantes)
- Attente validation super-utilisateur (délai)

---

### Parcours 3: Génération QR Batch (Sophie - Admin)

**Persona concerné:** Sophie (Admin Partenaire)
**Fréquence:** Après chaque import (hebdomadaire)

**Étapes:**
1. **Connexion admin** → Page administration (/admin)
2. **Sélection option QR** → "Générer QR codes"
3. **Choix produits:**
   - Tous produits sans QR (recommandé)
   - Tous produits (régénération)
   - Sélection manuelle
4. **Configuration:**
   - Taille: Moyenne (400×400px)
   - Correction erreur: H (30%)
5. **Lancement génération** → Clic "Générer"
6. **Attente progression** → Barre progression temps réel (SSE optionnel)
7. **Téléchargement ZIP** → QR codes PNG + PDFs (optionnel)
8. **Impression** → Collage sur échantillons showroom

**Points de friction:**
- Lenteur génération (1000+ produits, ~5-10 min)
- Taille fichier ZIP importante (100+ MB)
- Qualité impression (résolution, papier)

## Droits et Permissions

| Rôle | Permissions | Écrans Accessibles |
|------|-------------|--------------------|
| **VISITOR** | Consultation produits, listes d'envies | Catalogue, fiche produit, recherche |
| **SELLER** | + Infos privilégiées (prix, stocks) | + Produits connexes, statistiques basiques |
| **PARTNER** | + Gestion catalogue, marque blanche, QR | + Admin partenaire, import CSV, génération QR |
| **SUPER_USER** | + Multi-tenant, config globale, logs | + Admin74950, création partenaires, monitoring |

**Hiérarchie:**
```
SUPER_USER (Laurent)
    ↓
PARTNER (Sophie)
    ↓
SELLER (Julie)
    ↓
VISITOR (Marc)
```

**Restrictions:**
- VISITOR : Pas d'accès prix privilégiés, pas de modification
- SELLER : Pas de gestion catalogue, uniquement consultation
- PARTNER : Isolé à son catalogue (multi-tenant)
- SUPER_USER : Accès tous partenaires (admin uniquement)

---
**Lignes:** 208/250
**Sources:** personas.md, user-stories-essential.md, business-processes.md
