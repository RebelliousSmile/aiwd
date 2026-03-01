# TOC Chapitre 03: Consulter le Catalogue Produits

**Projet:** Guide Commercial Showroom CERASCAN  
**Type:** user-guide  
**Longueur estimée:** 2-3 pages  
**Output-style:** `user-friendly.md` (captures interface détaillées, procédures scan QR, pédagogie visuelle)  
**Persona review:** `showroom-staff`, `showroom-owner`, `clarity-expert`

---

## Synopsis

Navigation catalogue produits CERASCAN depuis l'interface commercial : page accueil affiche catégories visuelles (Intérieur/Extérieur, finitions Mat/Brillant/Structuré), barre recherche textuelle haut (nom, référence, fournisseur, autocomplétion). Scanner QR code produit physique showroom : icône appareil photo barre recherche, viser QR code échantillon, ouverture fiche produit <2 secondes. Fiche produit complète : galerie photos HD (zoom pinch), dimensions, finitions, catégorie, prix (public ou professionnel selon rôle), disponibilité stock, produits connexes suggérés. Informations privilégiées compte commercial : prix professionnels visibles (prix public grisé barré), stocks temps réel quantités exactes (vs "Disponible" générique visiteurs), marges calculées automatiquement (ligne "Marge: 28%"). Filtres avancés : panneau latéral (prix, disponibilité, catégories multiples, fournisseurs, combinables). Tri résultats : pertinence, prix croissant/décroissant, disponibilité, fournisseur A-Z. Performance : cache IndexedDB navigation instantanée, synchronisation catalogue automatique, mode hors ligne limité.

---

## Structure Détaillée

### Section 3.1 : Page d'Accueil Catalogue

**Intention :** Orienter le commercial dès la page d'accueil — catégories visuelles et barre recherche au premier regard.

**Contenu clé :**
- **Grilles catégories visuelles :** Tuiles photos Intérieur / Extérieur (premier niveau), sous-catégories finitions (Mat, Brillant, Structuré) accessibles en 2 taps
- **Arborescence :** Maximum 2-3 niveaux (ex: Intérieur → Mural → Finition Mat), navigation fil d'Ariane retour facile
- **Barre recherche :** Toujours visible haut interface (icône loupe + champ texte), priorité absolue consultation rapide pendant service client
- **Recommandation :** Recherche textuelle = consultation rapide visiteur présent, navigation catégories = exploration produits sans client

**Ton :** Orienté, pratique, donne le choix entre 2 modes d'entrée catalogue

**Contraintes :**
- Maximum 4-5 phrases introduction
- ENCADRÉ VISUEL tableau 2 colonnes : Méthode / Quand l'utiliser (Recherche textuelle vs Navigation catégories)
- Capture écran annotée : page accueil avec [1] barre recherche [2] catégorie Intérieur [3] catégorie Extérieur

**Pédagogie visuelle :**
- Encadré ASCII tableau comparatif 2 méthodes accès catalogue

---

### Section 3.2 : Rechercher un Produit par Texte

**Intention :** Expliquer la barre recherche textuelle — saisie libre, autocomplétion, résultats instantanés.

**Contenu clé :**
- **Saisie libre :** Nom produit (ex: "carrelage gris"), référence exacte (ex: "INTFURN01COLL01GRAY60X60"), fournisseur (ex: "Imola")
- **Autocomplétion :** Suggestions pendant saisie (liste déroulante 5-8 résultats), tap suggestion = ouverture fiche directe
- **Résultats :** Grille miniatures photos + noms + prix + badge disponibilité, tri par pertinence par défaut
- **Astuce :** Référence exacte = résultat unique instantané (idéal si commercial connaît la référence)

**Ton :** Pratique, rapide, met en avant l'efficacité pendant service client

**Contraintes :**
- Maximum 4-5 phrases
- ENCADRÉ ASTUCE "💡 Référence produit connue ? Saisir directement → fiche ouverte en 1 tap"
- Pas de capture écran dédiée (combiné avec section 3.1)

---

### Section 3.3 : Scanner un QR Code Échantillon

**Intention :** Décrire procédure scan QR code produit physique showroom — méthode la plus rapide sur le terrain.

**Contenu clé :**
- **Accès scanner :** Icône appareil photo dans barre recherche (ou bouton "Scanner QR" page accueil)
- **Autorisation caméra :** Popup première utilisation "Autoriser l'accès à la caméra ?" → Toujours autoriser
- **Procédure scan :** Viser QR code échantillon (cadrage automatique, pas besoin centrage précis), maintenir 1-2 secondes
- **Résultat :** Ouverture fiche produit automatique <2 secondes, vibration confirmation (si tablette compatible)
- **Si QR code illisible :** Vérifier luminosité, nettoyer caméra, saisir référence manuellement (alternative)

**Ton :** Procédure étape par étape, rassurant (autorisation caméra normale)

**Contraintes :**
- Procédure ≤ 4 étapes numérotées
- ENCADRÉ AVERTISSEMENT "⚠️ Première utilisation : autoriser caméra navigateur (ne pas refuser)"
- Capture écran annotée : vue scanner avec [1] icône appareil photo barre recherche [2] QR code dans le viseur [3] résultat fiche produit

---

### Section 3.4 : Lire une Fiche Produit

**Intention :** Décrire structure fiche produit complète — navigation galerie, blocs informations, produits connexes.

**Contenu clé :**
- **Galerie photos HD :** Swipe horizontal 5-10 images, pinch-to-zoom haute qualité, double-tap zoom rapide
- **Bloc informations :** Référence (code unique), nom commercial, dimensions (L × l × h cm), finition, catégorie INT/EXT
- **Prix et disponibilité :** Zone centrale visible (prix gros caractères), badge disponibilité (vert/orange/rouge), détail stock commercial (cf. Section 3.5)
- **Description :** Texte long "Voir plus" dépliable, conseils pose, caractéristiques techniques
- **Produits connexes :** Carrousel horizontal bas page (alternatives, même collection, produits complémentaires sol/mur)

**Ton :** Descriptif, guide le regard de haut en bas fiche produit

**Contraintes :**
- Format liste structurée (pas paragraphes)
- ENCADRÉ INFO "ⓘ Produits connexes : alternatives disponibles immédiatement si le produit est en rupture"
- Capture écran annotée : fiche produit avec [1] galerie photos [2] référence/dimensions [3] prix + badge stock [4] produits connexes

---

### Section 3.5 : Informations Privilégiées Commercial

**Intention :** Mettre en valeur les informations exclusives au rôle commercial (différenciation visiteur/commercial).

**Contenu clé :**
- **Prix professionnel :** Badge "Prix Pro" visible, prix public affiché grisé barré (différentiel immédiat), seul le commercial voit le prix pro
- **Stocks temps réel :** Quantité exacte "12 unités disponibles" (visiteur voit uniquement "Disponible"), mise à jour automatique entrepôt
- **Marge calculée :** Ligne "Marge: 28%" affichée sous prix pro (calcul automatique prix achat → prix vente public), utile argumentation vente
- **Visibilité :** Ces 3 éléments visibles uniquement compte commercial connecté (badge rôle SELLER)

**Ton :** Valorisant, distingue clairement les avantages du compte commercial

**Contraintes :**
- Maximum 4-5 phrases, liste à puces
- ENCADRÉ VISUEL "🔑 Infos exclusives commercial" — tableau 3 lignes : Prix Pro / Stock temps réel / Marge calculée
- Capture écran annotée : zone prix fiche produit avec [1] badge "Prix Pro" [2] prix public barré [3] ligne marge [4] quantité stock exacte

---

### Section 3.6 : Filtres et Tri

**Intention :** Expliquer les filtres avancés et le tri des résultats — combinaison filtres pour affiner rapidement.

**Contenu clé :**
- **Accès filtres :** Icône entonnoir (ou bouton "Filtrer") → panneau latéral gauche
- **Filtres disponibles :** Prix (sliders min/max), disponibilité (checkboxes : Disponible / Rupture / Sur commande), catégories multiples (checkboxes INT/EXT + finitions), fournisseurs (liste alphabétique checkboxes)
- **Filtres combinables :** Ex: Intérieur + Mat + Disponible + Imola = résultats croisés instantanés
- **Réinitialiser :** Bouton "Réinitialiser filtres" bas panneau (tout effacer d'un tap)
- **Tri :** Menu déroulant "Trier par" haut liste (Pertinence, Prix ↑, Prix ↓, Disponibilité, Fournisseur A-Z)

**Ton :** Pratique, empowerment commercial (trouver rapidement le bon produit)

**Contraintes :**
- Format liste structurée
- ENCADRÉ EXEMPLE "Exemple terrain : visiteur veut carrelage intérieur mat disponible chez Imola → 3 filtres en 15 secondes"
- Pas de capture écran dédiée (capture dans Section 3.1 ou note renvoi)

---

## Ressources Visuelles

| ID | Section | Type | Description | Statut |
|----|---------|------|-------------|--------|
| `ch03-catalogue-accueil` | 3.1 | Mockup | Page accueil catalogue avec grille catégories, barre recherche, fil d'Ariane | À générer |
| `ch03-scanner-qr` | 3.3 | Mockup | Vue scanner QR code avec viseur cadrage + fiche produit résultat | À générer |
| `ch03-fiche-produit` | 3.4 + 3.5 | Mockup | Fiche produit complète avec galerie, infos, prix pro/marge/stock, connexes | À générer |

**Note renderers :** Les types `catalog`, `product-detail`, `qr-scanner` ne sont pas encore dans `generate-mockups.py`.
Options :
1. Ajouter renderers `catalog` et `product-detail` au script → mockups générés automatiquement
2. Utiliser `import-screenshot.py` quand l'interface réelle est disponible

**Recommandation :** Créer mockups manuels (Pillow) pour ch03 → spec dans `mockups.yml` ci-dessous.

**Bloc YAML pour `mockups.yml` (à ajouter après ch02 entries) :**

```yaml
  # Chapitre 03 — Consulter le Catalogue Produits
  - id: "ch03-catalogue-accueil"
    type: catalog
    output: "chapitres/screenshots/guide-commercial-ch03-catalogue-accueil.png"
    catalog:
      title: "Catalogue Produits"
      categories:
        - {name: "Intérieur", icon: "INT", count: "1 243 produits"}
        - {name: "Extérieur", icon: "EXT", count: "657 produits"}
      search_placeholder: "Rechercher un produit, une référence..."
      breadcrumb: "Accueil > Catalogue"
    annotations:
      - {id: 1, element: search}
      - {id: 2, element: category_int}
      - {id: 3, element: category_ext}

  - id: "ch03-fiche-produit"
    type: product-detail
    output: "chapitres/screenshots/guide-commercial-ch03-fiche-produit.png"
    product:
      reference: "INTFURN01COLL01GRAY60X60"
      name: "Carrelage Gris Mat Intérieur"
      dimensions: "60 × 60 × 0.9 cm"
      finish: "Mat"
      category: "INT"
      price_public: "45.90 €"
      price_pro: "28.50 €"
      stock: "12 unités disponibles"
      margin: "Marge: 37.9%"
      availability: "disponible"
    annotations:
      - {id: 1, element: gallery}
      - {id: 2, element: reference}
      - {id: 3, element: price_pro}
      - {id: 4, element: stock}
      - {id: 5, element: margin}

  - id: "ch03-scanner-qr"
    type: qr-scanner
    output: "chapitres/screenshots/guide-commercial-ch03-scanner-qr.png"
    scanner:
      state: "scanning"
      label: "Visez le QR code de l'échantillon"
    annotations:
      - {id: 1, element: camera_icon}
      - {id: 2, element: viewfinder}
```

**Refs markdown pour `write-user-guide 3` :**

```markdown
![Page d'accueil catalogue CERASCAN](screenshots/guide-commercial-ch03-catalogue-accueil.png)
*Page accueil catalogue : [1] Barre recherche [2] Catégorie Intérieur [3] Catégorie Extérieur*

![Fiche produit avec infos privilégiées commercial](screenshots/guide-commercial-ch03-fiche-produit.png)
*Fiche produit : [1] Galerie photos [2] Référence [3] Prix professionnel [4] Stock temps réel [5] Marge calculée*

![Scanner QR code échantillon showroom](screenshots/guide-commercial-ch03-scanner-qr.png)
*Scanner QR code : [1] Icône appareil photo [2] Viseur cadrage*
```

**Commandes à lancer avant écriture :**

```bash
# Quand renderers catalog/product-detail/qr-scanner ajoutés à generate-mockups.py :
python scripts/generate-mockups.py --project "cerascan/guide-commercial" --id "ch03-catalogue-accueil"
python scripts/generate-mockups.py --project "cerascan/guide-commercial" --id "ch03-fiche-produit"
python scripts/generate-mockups.py --project "cerascan/guide-commercial" --id "ch03-scanner-qr"
```

---

## Notes d'Écriture

### Pédagogie Visuelle

- **Encadrés ASCII bordure obligatoires :** Section 3.1 (tableau 2 méthodes accès), Section 3.5 (infos exclusives commercial 3 lignes), Section 3.6 (exemple terrain filtres combinés)
- **Procédure scan QR :** Section 3.3 — procédure 4 étapes numérotées maximum
- **Hiérarchie visuelle :** Section 3.4 fiche produit — liste structurée (pas paragraphe), regard guidé haut → bas fiche
- **Différenciation commercial/visiteur :** Section 3.5 — mettre en avant visuellement (encadré "🔑 Infos exclusives")

### Infos Privilégiées : Ton Valorisant

- Prix pro, stocks temps réel, marges = avantage compte commercial à valoriser (pas juste "fonctionnalité")
- Formulation : "En tant que commercial, vous voyez..." (pas "le commercial peut voir...")
- Marge calculée = outil argumentation vente (reformuler en bénéfice : "savoir immédiatement si le produit est rentable")

### Format Ultra-Compact

- **Longueur cible :** 2-3 pages (environ 700-1100 mots)
- **Captures écran :** 3 maximum (accueil catalogue, fiche produit infos pro, scanner QR)
- **Sections courtes :** 5-6 phrases maximum par section
- **Listes à puces prioritaires :** Éviter paragraphes > 4 lignes

### Terminologie Cohérente

- **Commercial** / **Vendeuse showroom** (pas "l'utilisateur", pas "vous")
- **QR code** (pas "code QR", pas "flashcode")
- **Fiche produit** (pas "page produit", pas "détail produit")
- **Prix professionnel** (pas "prix B2B", pas "prix revendeur")
- **Stock temps réel** (pas "stock live", pas "inventaire")
- **Produits connexes** (pas "suggestions", pas "recommandations")
- **Référence** (pas "SKU", pas "code produit")

### Exemples Situations Réelles

- Référence produit : *INTFURN01COLL01GRAY60X60* (format réaliste)
- Prix public : *45.90 €* / Prix pro : *28.50 €* / Marge : *37.9%*
- Stock : *"12 unités disponibles"* (quantité exacte, pas "Disponible")
- Recherche textuelle : *"carrelage gris mat Imola"* (saisie naturelle)
- Exemple filtres combinés : *Intérieur + Mat + Disponible + Imola*

---

## Points Clés (depuis INDEX.md)

- Navigation catégories : Page accueil grilles visuelles (Intérieur/Extérieur tuiles photos), sous-catégories finitions (Mat, Brillant, Structuré), arborescence 2-3 niveaux profondeur
- Recherche textuelle rapide : Barre recherche haut interface (icône loupe), recherche nom produit, référence exacte, fournisseur, suggestions automatiques pendant saisie
- Scanner QR code : Icône appareil photo barre recherche, autoriser accès caméra navigateur (première utilisation), viser QR code échantillon, ouverture fiche produit instantanée (<2 secondes)
- Fiche produit structure : Galerie photos HD (swipe horizontal, zoom pinch), bloc informations (référence, nom, dimensions, finition, catégorie), prix, disponibilité stock, produits connexes bas page
- Infos privilégiées commerciaux : Prix professionnels (badge "Prix Pro", prix public grisé barré), stocks temps réel (quantité exacte vs visiteur "Disponible"), marges calculées (ligne "Marge: 28%")
- Filtres avancés : Panneau filtres latéral gauche (prix, disponibilité, catégories multiples, fournisseurs), bouton "Réinitialiser filtres"
- Tri résultats : Menu déroulant "Trier par" (Pertinence, Prix croissant, Prix décroissant, Disponibilité, Fournisseur A-Z)

---

## Quality Checklist

- [ ] Synopsis respecté (navigation, recherche, scan QR, fiche produit, infos privilégiées, filtres/tri)
- [ ] Longueur cible atteinte (2-3 pages = 700-1100 mots environ)
- [ ] Sections courtes (5-6 phrases max par section)
- [ ] **Encadrés ASCII bordure visuels** : Section 3.1 (2 méthodes accès), Section 3.5 (infos exclusives), Section 3.6 (exemple terrain)
- [ ] **Procédure scan QR** : Section 3.3 — 4 étapes numérotées, avertissement autorisation caméra
- [ ] **Hiérarchie visuelle** : Section 3.4 fiche produit — liste structurée haut → bas
- [ ] **Différenciation commercial/visiteur** : Section 3.5 — encadré "🔑 Infos exclusives commercial" valorisant
- [ ] Captures écran annotées mentionnées (accueil catalogue, scanner QR, fiche produit infos pro)
- [ ] Terminologie cohérente (QR code, fiche produit, prix professionnel, référence, stock temps réel)
- [ ] Exemples situations réelles (référence INTFURN01..., prix 45.90€/28.50€, marge 37.9%, stock 12 unités)
- [ ] Exemple filtres combinés concret (Intérieur + Mat + Disponible + Imola)
- [ ] Ton user-friendly valorisant (avantages commercial explicites, pas technique neutre)
- [ ] (Step 2.7) 3 mockups planifiés avec IDs, chemins output et refs markdown
- [ ] (Step 2.7) YAML mockups.yml prêt à intégrer
- [ ] (Step 2.7) Renderers catalog/product-detail/qr-scanner identifiés comme manquants dans generate-mockups.py

---

**Date génération :** 2026-03-01  
**Générée depuis :** INDEX.md Chapitre 03 (guide-commercial)  
**Prochaine étape :** Ajouter renderers ch03 dans `generate-mockups.py`, puis `write-user-guide 3`
