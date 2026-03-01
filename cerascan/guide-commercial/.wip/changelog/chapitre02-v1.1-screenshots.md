# Changelog: Chapitre 02 v1.0 → v1.1

**Date:** 2026-02-28  
**Type:** Doctor (corrections chirurgicales)  
**Trigger:** Review personas CERASCAN v1.1 — Must-have manquant (captures écran)

---

## Corrections Appliquées

### CORRECTION BLOQUANTE #1 : Captures Écran Interface Mobile/Tablette

**Impact:** Must-have manquant → Plafond 11/20 appliqué TOUTES personas  
**Localisation:** Section 2.4 Page Connexion (L95-130), Section 2.5 Déconnexion (L143-158)

**Problème:**
- Descriptions textuelles seules : "Menu profil [icône utilisateur coin haut droite]" (L149)
- Reconnaissance visuelle rapide IMPOSSIBLE pour commercial showroom
- Reproduction interface IMPOSSIBLE pour propriétaire showroom

**Solution Appliquée:**

**1. Ajout Figure 2.1 : Page Connexion (L97-112)**
```markdown
![Page Connexion CERASCAN](screenshots/guide-commercial-ch02-page-connexion.png)
*Figure 2.1 : Page connexion quotidienne CERASCAN — (1) Champ Email, (2) Champ Mot de passe avec icône œil, (3) Case "Rester connecté", (4) Bouton "Se connecter"*
```

**Annotations requises (commentaire HTML):**
- (1) Champ Email avec exemple *julie@showroom-ceramique.fr*
- (2) Champ Mot de passe avec icône œil masquer/afficher
- (3) Case "Rester connecté" cochable
- (4) Bouton "Se connecter" bleu/vert visible
- Flèches rouges pointant chaque élément numéroté

**2. Ajout Figure 2.2 : Menu Profil Ouvert (L145-156)**
```markdown
![Menu Profil CERASCAN](screenshots/guide-commercial-ch02-menu-profil.png)
*Figure 2.2 : Menu profil ouvert — (1) Icône utilisateur 👤 coin haut droite, (2) Menu déroulant, (3) Bouton "Déconnexion"*
```

**Annotations requises (commentaire HTML):**
- (1) Icône utilisateur 👤 coin haut droite (à droite logo CERASCAN)
- (2) Dropdown menu profil déroulé
- (3) Bouton "Déconnexion" dans le menu
- Contexte: Menu ouvert après clic sur icône utilisateur

**3. Ajout Figure 2.3 : Popup Confirmation Déconnexion (L158-167)**
```markdown
![Popup Confirmation Déconnexion](screenshots/guide-commercial-ch02-popup-deconnexion.png)
*Figure 2.3 : Popup confirmation déconnexion — (1) Message confirmation, (2) Bouton "Annuler", (3) Bouton "Valider"*
```

**Annotations requises (commentaire HTML):**
- (1) Message "Êtes-vous sûr(e) de vouloir vous déconnecter ?"
- (2) Bouton "Annuler" gris
- (3) Bouton "Valider" bleu/rouge

**Impact projeté:** +7-8 points TOUTES personas (11.00 → 18-19/20)

---

### CORRECTION MINEURE #2 : Précision Menu Profil Emoji + Positionnement

**Impact:** Clarity +1pt showroom-staff, +0.5pt showroom-owner  
**Localisation:** Section 2.5 Déconnexion, encadré workflow (L169-181)

**Problème:**
- Description textuelle floue : "Menu profil [icône utilisateur coin haut droite]"
- Icône pas décrite (forme, couleur, symbole)

**Solution Appliquée:**
```markdown
# AVANT (L149 v1.0)
Menu profil [icône utilisateur coin haut droite]

# APRÈS (L171 v1.1)
Menu profil [icône utilisateur 👤 coin haut droite, à droite logo CERASCAN]
```

**Justification:**
- Emoji 👤 aide reconnaissance visuelle rapide (cohérent symboles visuels guide)
- Positionnement "à droite logo" améliore repérage spatial interface

---

### CORRECTION MINEURE #3 : Clarification Durée Blocage Compte

**Impact:** Clarity +0.5pt platform-admin (précision technique)  
**Localisation:** Section 2.6 Troubleshooting, tableau ligne "Compte bloqué" (L180-187)

**Problème:**
- Durée blocage non mentionnée (30 minutes standard sécurité)
- Déblocage "interface" ambigu (automatique vs manuel ?)

**Solution Appliquée:**
```markdown
# AVANT (L180-182 v1.0)
│  Compte bloqué               │ 5 tentatives          │ Contacter proprié-   │
│  "Compte temporairement      │ échouées              │ taire showroom →     │
│  bloqué"                     │ (sécurité)            │ Déblocage interface  │

# APRÈS (L180-187 v1.1)
│  Compte bloqué               │ 5 tentatives          │ Contacter proprié-   │
│  "Compte temporairement      │ échouées              │ taire showroom →     │
│  bloqué"                     │ (sécurité)            │ Déblocage manuel     │
│                              │ Blocage 30 min        │ (aucun déblocage     │
│                              │                       │ automatique)         │
```

**Justification:**
- Admin plateforme apprécierait temporalité blocage (30 min standard sécurité)
- Clarification déblocage manuel uniquement (pas automatique après 30 min)
- Évite confusion attente passive vs contact propriétaire immédiat

---

## Résumé Impact Corrections

| Correction | Impact | Personas Affectées | Gain Projeté |
|------------|--------|-------------------|--------------|
| #1 Captures écran (BLOQUANTE) | Must-have manquant résolu | Toutes (showroom-staff, showroom-owner, platform-admin) | +7-8pts |
| #2 Emoji menu profil | Clarity visuelle | showroom-staff (+1pt), showroom-owner (+0.5pt) | +0.75pt |
| #3 Durée blocage compte | Clarity technique | platform-admin (+0.5pt) | +0.4pt |

**Score Consensus Projeté:** 11.00/20 (v1.0) → **18-19/20** (v1.1)

---

## Fichiers Captures Écran Requis

**Dossier:** `cerascan/guide-commercial/screenshots/`

| Fichier | Description | Annotations |
|---------|-------------|-------------|
| `guide-commercial-ch02-page-connexion.png` | Page connexion quotidienne mobile/tablette | (1) Champ Email, (2) Champ Mot de passe + icône œil, (3) Case "Rester connecté", (4) Bouton "Se connecter" |
| `guide-commercial-ch02-menu-profil.png` | Menu profil ouvert coin haut droite | (1) Icône utilisateur 👤, (2) Menu déroulant, (3) Bouton "Déconnexion" |
| `guide-commercial-ch02-popup-deconnexion.png` | Popup confirmation déconnexion | (1) Message confirmation, (2) Bouton "Annuler", (3) Bouton "Valider" |

**Format:**
- Résolution: Min 800px largeur (lisibilité mobile/tablette)
- Format: PNG (compression sans perte)
- Annotations: Flèches rouges + numéros cercles rouge (1), (2), (3)
- Contexte: Interface mobile/tablette showroom prioritaire (pas desktop)

---

## État Validation

**Statut:** ⏳ En attente captures écran réelles  
**Prochaine étape:** 
1. Créer 3 captures écran interface CERASCAN mobile/tablette
2. Annoter avec flèches/numéros selon spécifications
3. Placer dans `cerascan/guide-commercial/screenshots/`
4. Re-run `comment.prompt.md` chapitre02.md v1.1 (validation personas)

**Score Consensus Attendu:** 18-19/20 (production-ready)

---

**Version:** 1.1  
**Date:** 2026-02-28  
**Type:** Doctor (corrections chirurgicales)  
**Lignes modifiées:** ~30 lignes (ajouts placeholders images + 2 corrections texte)
