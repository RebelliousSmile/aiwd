# Client: CERASCAN (EFFICERAM)

## Contexte Métier

**Secteur:** Distribution céramique B2B2C
**Activité:** Solution SaaS multi-tenant de transformation numérique pour showrooms céramique

CERASCAN est développée par EFFICERAM (Euro Ceramiche France), marque commerciale visant la transformation numérique des showrooms de céramique. La solution permet aux visiteurs de scanner des QR codes sur les échantillons pour accéder instantanément aux fiches produits complètes sur leur smartphone.

**Modèle économique:** SaaS multi-tenant avec marque blanche pour chaque partenaire distributeur.

## Audience Cible

### Utilisateurs Finaux

| Type | Profil | Besoins |
|------|--------|---------|
| **Visiteur Showroom** | Client final, mobile-first, cherche autonomie | Infos produit instantanées (scan QR → fiche), comparaison, listes d'envies |
| **Vendeuse Showroom** | Personnel vente, accompagnement client | Accès infos privilégiées (prix, stocks), outils vente améliorés |
| **Admin Partenaire** | Propriétaire showroom locataire | Gestion catalogue, personnalisation marque blanche, statistiques |
| **Super Utilisateur** | Développeur/Patron EFFICERAM | Administration multi-tenant, monitoring, configuration globale |

### Parties Prenantes

- **Décideurs:** Direction EFFICERAM, propriétaires showrooms partenaires
- **Équipes techniques:** Développeurs Node.js/MySQL, responsables infrastructure
- **Support:** Service client EFFICERAM, support technique partenaires

## Contraintes Projet

### Techniques
- **Multi-tenant strict:** Isolation données par partenaire (tables MySQL séparées)
- **Performance mobile:** Catalogue 3000+ produits, cache IndexedDB requis (10-50ms)
- **QR codes massifs:** Génération par lots (jusqu'à 5000 QR codes)
- **Marque blanche:** Personnalisation complète (logo, couleurs, domaine) par partenaire
- **PWA offline:** Service worker, cache assets, fonctionnement hors ligne

### Métier
- **Secteur céramique:** Nomenclature spécifique (Int/Ext, finitions, dimensions)
- **Showroom physique:** QR codes imprimés sur échantillons
- **B2B2C:** Partenaires distributeurs + clients finaux
- **Rotation catalogue:** Imports CSV fréquents, activation/désactivation produits
- **Analytics:** Suivi consultation produits, statistiques par fournisseur

### Réglementaires
- **RGPD:** Données utilisateurs (listes d'envies), consentement cookies
- **Accessibilité:** Mobile-first, navigation tactile optimisée
- **Hébergement:** OVH France (souveraineté données)

## Objectifs Documentation

**Document principal:** Guide technique développeur + Guide utilisateur admin/partenaire
**Livrables attendus:** 
- Markdown pour développeurs (architecture, API, patterns)
- PDF pour utilisateurs finaux (guides utilisateur, installation)
**Délais:** Aucun délai strict (documentation vivante)

## Tone & Style

**Ton attendu:** Technique et précis pour développeurs, accessible et pédagogique pour admins/partenaires
**Niveau technique:** 
- Développeurs: Expert (Node.js, MySQL, multi-tenant, patterns SOLID)
- Admins: Intermédiaire (concepts web, CSV, QR codes)
- Partenaires: Débutant (tutoriels pas-à-pas, captures d'écran)

**Exemples à privilégier:** 
- Code JavaScript (patterns, services, Alpine.js)
- Requêtes SQL (multi-tenant, composite IDs)
- Captures d'écran UI (guides utilisateur)
- Diagrammes architecture (PlantUML, Mermaid)

---
**Lignes:** 78/250
**Màj:** 2026-02-28
**Sources:** README.md, architecture-core.md, personas.md, GUIDE-UTILISATEUR-CERASCAN.md
