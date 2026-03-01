# TOC Chapitre 02: Se Connecter

**Projet:** Guide Commercial Showroom CERASCAN  
**Type:** user-guide  
**Longueur estimée:** 1-2 pages  
**Output-style:** `user-friendly.md` (procédure simple, captures écran annotées, pédagogie visuelle)  
**Persona review:** `showroom-staff`, `showroom-owner`, `platform-admin`

---

## Synopsis

Procédure connexion application CERASCAN : URL fournie propriétaire (https://partner808.cerascan.fr) ou QR code accès rapide comptoir. Identifiants commercial : email + mot de passe temporaire fournis formation 30 min propriétaire. Premier login obligatoire changement mot de passe sécurité. Page connexion : champs email/mot de passe, bouton "Se connecter", case "Rester connecté" (tablette fixe recommandé, smartphone personnel déconseillé). Connexion réussie : redirection catalogue produits. Déconnexion : menu profil icône utilisateur haut droite, bouton "Déconnexion". Changer session commercial : si plusieurs commerciaux partagent tablette, déconnexion obligatoire (traçabilité consultations). Problèmes courants : mot de passe oublié (propriétaire réinitialise), compte bloqué 5 tentatives (propriétaire débloque), URL erronée (vérifier email propriétaire).

---

## Structure Détaillée

### Section 2.1 : Accéder à l'Application

**Intention :** Expliquer les 2 moyens d'accès application (URL navigateur ou QR code) et avantages chacun.

**Contenu clé :**
- **URL navigateur :** https://{partenaire}.cerascan.fr fournie email propriétaire, saisir manuellement barre adresse smartphone/tablette, ajouter favoris navigateur (accès 1 clic quotidien)
- **QR code accès rapide :** Collé comptoir showroom, scanner appareil photo smartphone/tablette (pas besoin app dédiée), ouvre navigateur préconfiguré URL directement (<2 secondes)
- **Recommandation :** QR code quotidien (rapidité), URL favoris navigateur backup (si QR code illisible/absent)

**Ton :** Pratique, orienté rapidité quotidienne

**Contraintes :**
- Maximum 4-5 phrases introduction, puis ENCADRÉ VISUEL tableau 2 colonnes (URL vs QR code)
- Capture écran annotée : QR code exemple collé comptoir + smartphone cadrage scan

**Pédagogie visuelle (must-have showroom-staff v1.1) :**
- Encadré ASCII bordure tableau comparatif URL vs QR code (colonnes : Méthode / Quand utiliser / Durée)
- Schéma workflow : "Accès application" → 2 chemins (URL ou QR code) → Page connexion

---

### Section 2.2 : Identifiants Commercial

**Intention :** Expliquer provenance identifiants (créés par propriétaire) et format (email + mot de passe temporaire).

**Contenu clé :**
- **Email professionnel showroom :** Ex: julie@showroom-ceramique.fr (pas email personnel), fourni propriétaire lors formation 30 min
- **Mot de passe temporaire :** Ex: "CERASCAN2024!" (générique sécurisé), obligatoire changement premier login
- **Création identifiants :** Propriétaire interface partenaire section "Utilisateurs" (commercial NE PEUT PAS créer compte lui-même)

**Ton :** Informatif, précise origine identifiants

**Contraintes :**
- Maximum 3-4 phrases, format liste à puces
- ENCADRÉ INFO "ⓘ Identifiants fournis par propriétaire showroom lors formation 30 min"
- Pas de capture écran (interface partenaire hors périmètre commercial)

**Permissions explicites (must-have showroom-owner v1.1) :**
- **Commercial PEUT :** Utiliser identifiants fournis, changer mot de passe après premier login
- **Commercial NE PEUT PAS :** Créer compte lui-même, réinitialiser mot de passe seul (contacter propriétaire)

---

### Section 2.3 : Premier Login Sécurisé

**Intention :** Décrire procédure premier login avec changement mot de passe obligatoire (sécurité + mémorisation personnelle).

**Contenu clé :**
- **Page "Changer mot de passe" :** Apparaît automatiquement premier login (pas de redirection catalogue)
- **Champs obligatoires :** Mot de passe temporaire (fourni propriétaire), nouveau mot de passe (choix commercial, min 8 caractères), confirmation nouveau mot de passe (identique)
- **Validation :** Message "Mot de passe changé avec succès, connexion automatique" → redirection catalogue produits
- **Sécurité :** Nouveau mot de passe personnel (mémorisation facile commercial, ne PAS partager)

**Ton :** Rassurant, pédagogique étape par étape

**Contraintes :**
- Procédure ≤ 4 étapes (must-have showroom-staff v1.1)
- ENCADRÉ VISUEL workflow : "Premier login" → "Page changement mot de passe" → "Validation" → "Catalogue"
- Capture écran annotée : page "Changer mot de passe" avec champs numérotés [1] temporaire [2] nouveau [3] confirmation

**Pédagogie progressive (must-have showroom-staff v1.1) :**
- Étape 1 simple (saisir temporaire) → Étape 2 complexe (choisir nouveau sécurisé) → Étape 3 confirmation

---

### Section 2.4 : Page Connexion

**Intention :** Décrire éléments page connexion (champs, boutons, case "Rester connecté") et usage quotidien.

**Contenu clé :**
- **Champs saisie :** Email (ex: julie@showroom-ceramique.fr), mot de passe (masqué, icône œil afficher/masquer)
- **Bouton "Se connecter" :** En bas formulaire, validation identifiants
- **Case "Rester connecté" :** Tablette showroom fixe (RECOMMANDÉ, pas redemander identifiants chaque jour), smartphone personnel (DÉCONSEILLÉ, risque sécurité si perte/vol), durée session 30 jours (reconnexion automatique après)
- **Connexion réussie :** Redirection catalogue produits automatique (pas de page intermédiaire)

**Ton :** Explicatif, distingue tablette fixe vs smartphone personnel

**Contraintes :**
- Maximum 5-6 phrases
- ENCADRÉ AVERTISSEMENT "⚠️ Case 'Rester connecté' : Tablette fixe → OUI | Smartphone personnel → NON"
- Capture écran annotée : page connexion avec champs [1] email [2] mot de passe [3] case "Rester connecté" [4] bouton "Se connecter"

**Hiérarchie visuelle (must-have showroom-staff v1.1) :**
- Grouper éléments formulaire (champs + bouton) séparés visuellement de case "Rester connecté" (espacement, ou ligne horizontale)

---

### Section 2.5 : Se Déconnecter

**Intention :** Expliquer procédure déconnexion (menu profil) et quand l'utiliser (tablette partagée).

**Contenu clé :**
- **Accès menu profil :** Icône utilisateur coin haut droite interface (toujours visible)
- **Bouton "Déconnexion" :** Clic → popup confirmation "Êtes-vous sûr(e) ?" → validation → redirection page connexion
- **Quand se déconnecter :** Tablette partagée plusieurs commerciaux (obligatoire traçabilité consultations), fin journée showroom (sécurité), avant prêt tablette visiteur (confidentialité infos privilégiées)

**Ton :** Pratique, insiste traçabilité tablette partagée

**Contraintes :**
- Maximum 4-5 phrases
- ENCADRÉ INFO "💡 Tablette partagée ? Déconnexion obligatoire après chaque session (traçabilité consultations)"
- Capture écran annotée : menu profil déroulé avec bouton "Déconnexion" surligné

**Pédagogie visuelle :**
- Schéma workflow simple : "Menu profil" [icône] → "Déconnexion" [bouton] → "Confirmation" [popup] → "Page connexion"

---

### Section 2.6 : Problèmes Connexion Courants

**Intention :** Lister erreurs fréquentes (mot de passe oublié, compte bloqué, URL erronée) avec résolutions rapides.

**Contenu clé :**
- **Mot de passe oublié :** Contacter propriétaire showroom (réinitialisation interface partenaire section "Utilisateurs"), pas de lien "Mot de passe oublié" page connexion (commercial ne gère pas compte seul)
- **Compte bloqué après 5 tentatives :** Message "Compte temporairement bloqué, contacter propriétaire showroom", déblocage propriétaire interface partenaire, sécurité anti-intrusion
- **URL application erronée :** Vérifier URL email propriétaire (ex: https://partner808.cerascan.fr), pas https://cerascan.fr générique (erreur 404)
- **Internet instable showroom :** Page connexion charge lentement (attendre 10-15 secondes), si échec répété contacter propriétaire (vérifier réseau Wi-Fi showroom)

**Ton :** Troubleshooting direct, solutions claires

**Contraintes :**
- Format TABLEAU VISUEL 3 colonnes : Problème | Cause | Solution
- Maximum 4 problèmes courants (les plus fréquents quotidien)
- Chaque solution mentionne contact propriétaire (hiérarchie support claire)

**Pédagogie visuelle :**
- Encadré ASCII tableau bordure avec 4 lignes (1 par problème)

---

## Notes d'Écriture

### Pédagogie Visuelle (Intégration v1.1)

- **Encadrés ASCII bordure obligatoires :** Section 2.1 (tableau URL vs QR code), Section 2.4 (avertissement case "Rester connecté"), Section 2.5 (astuce tablette partagée), Section 2.6 (tableau troubleshooting)
- **Schémas workflows :** Section 2.1 (accès application 2 chemins), Section 2.3 (premier login 4 étapes), Section 2.5 (déconnexion workflow)
- **Hiérarchie visuelle forte :** Sections courtes 4-6 phrases max, espacements généreux entre sections, séparations visuelles groupes logiques (champs formulaire vs case "Rester connecté")
- **Pédagogie progressive :** Section 2.3 premier login (simple→complexe), sections ordonnées logiquement (accès → identifiants → premier login → page connexion → déconnexion → problèmes)

### Permissions Explicites (showroom-owner v1.1)

- **Section 2.2 Identifiants :** Préciser "Commercial NE PEUT PAS créer compte seul, identifiants créés par propriétaire interface partenaire"
- **Section 2.6 Problèmes :** Toutes résolutions passent par propriétaire showroom (réinitialisation, déblocage), commercial ne gère pas compte seul

### Format Ultra-Compact

- **Longueur cible :** 1-2 pages (environ 400-800 mots)
- **Captures écran :** 3 maximum (QR code comptoir, page connexion annotée, menu profil déconnexion)
- **Procédures ≤ 4 étapes :** Section 2.3 premier login (4 étapes numérotées)
- **Listes à puces prioritaires :** Éviter paragraphes > 5 lignes, privilégier listes/encadrés/tableaux

### Terminologie Cohérente

- **Propriétaire showroom** (pas "Sophie", pas "votre patron")
- **Commercial** / **Vendeuse showroom** (pas "Julie", pas "utilisateur")
- **Tablette showroom fixe** vs **smartphone personnel commercial**
- **Interface partenaire** (outil propriétaire, hors périmètre commercial)

### Exemples Situations Réelles

- Email professionnel : *julie@showroom-ceramique.fr* (pas *julie.dupont@gmail.com*)
- URL application : *https://partner808.cerascan.fr* (format concret)
- Mot de passe temporaire : *CERASCAN2024!* (exemple réaliste)
- QR code collé comptoir showroom (contexte physique authentique)

---

## Points Clés (depuis INDEX.md)

- Accès application : URL https://{partenaire}.cerascan.fr (fournie email propriétaire), QR code accès rapide collé comptoir showroom (scan ouvre navigateur direct), ajouter favoris navigateur smartphone/tablette (accès 1 clic quotidien)
- Identifiants commercial : Email professionnel showroom (ex: julie@showroom-ceramique.fr), mot de passe temporaire fourni propriétaire (ex: "CERASCAN2024!"), créés par propriétaire interface partenaire section "Utilisateurs"
- Premier login sécurisé : Page "Changer mot de passe" apparaît automatiquement premier login, champs (mot de passe temporaire, nouveau mot de passe, confirmation), validation affiche message "Mot de passe changé, connexion automatique"
- Case "Rester connecté" : Tablette showroom fixe (recommandé, pas redemander identifiants chaque jour), smartphone personnel commercial (déconseillé, sécurité données), durée session 30 jours (reconnexion automatique après 30 jours inactivité)
- Déconnexion : Menu profil icône utilisateur (coin haut droite interface), bouton "Déconnexion" (confirmation), redirection page connexion, nécessaire si plusieurs commerciaux partagent tablette (traçabilité)
- Problèmes connexion courants : Mot de passe oublié (contacter propriétaire showroom, réinitialisation interface partenaire), compte bloqué après 5 tentatives échouées (déblocage propriétaire), URL application erronée (vérifier URL email propriétaire)

---

## Quality Checklist

- [ ] Synopsis respecté (accès application, identifiants, premier login, page connexion, déconnexion, problèmes courants)
- [ ] Longueur cible atteinte (1-2 pages = 400-800 mots environ)
- [ ] Sections structurées courtes (4-6 phrases max par section)
- [ ] **Encadrés ASCII bordure visuels** : Section 2.1 (tableau URL/QR), Section 2.4 (avertissement case), Section 2.5 (astuce tablette), Section 2.6 (tableau troubleshooting)
- [ ] **Schémas workflows** : Section 2.1 (accès 2 chemins), Section 2.3 (premier login 4 étapes), Section 2.5 (déconnexion)
- [ ] **Procédures ≤ 4 étapes** : Section 2.3 premier login (numérotées 1-4)
- [ ] **Hiérarchie visuelle forte** : Espacements généreux, séparations groupes logiques
- [ ] **Pédagogie progressive** : Sections ordonnées logiquement (simple→complexe)
- [ ] **Permissions explicites** : Section 2.2 identifiants (NE PEUT PAS créer compte seul), Section 2.6 (résolutions via propriétaire)
- [ ] Captures écran annotées mentionnées (QR code comptoir, page connexion, menu profil)
- [ ] Terminologie cohérente (propriétaire showroom, commercial, tablette fixe vs smartphone personnel, interface partenaire)
- [ ] Exemples situations réelles (email *julie@showroom-ceramique.fr*, URL *https://partner808.cerascan.fr*, QR code comptoir)
- [ ] Ton user-friendly ultra-synthétique (direct, pratique, pas de théorie)
- [ ] Troubleshooting intégré (section 2.6 problèmes courants avec résolutions)
- [ ] Contacts support hiérarchisés (propriétaire showroom résolutions, pas admin)

---

**Date génération :** 2026-02-28  
**Générée depuis :** INDEX.md Chapitre 02 (guide-commercial)  
**Intégration :** Attentes personas v1.1 (pédagogie visuelle, permissions explicites, format ultra-compact)
