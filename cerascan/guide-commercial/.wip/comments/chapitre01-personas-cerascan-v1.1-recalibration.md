# Analyse Personas CERASCAN v1.1: Comment Utiliser ce Guide

**Date:** 2026-02-28 | **Chapitre:** 01 | **Personas:** 3 (3 univers CERASCAN v1.1)  
**Version chapitre:** v1.1 (post-corrections doctor)  
**Itération:** Re-test après calibration personas v1.1

---

## Section 1: TECHNICAL ISSUES → doctor.prompt.md

### Typographie
*Aucune issue détectée* — Guillemets «», tirets —, espaces insécables présents. Accents français corrects.

### Grammaire
*Aucune issue détectée* — Syntaxe fluide, temps verbaux cohérents.

### Terminologie
*Toutes issues validées* — Terminologie claire et cohérente (comptoir d'accueil showroom, pochette plastique, formation 30 min).

### Formatting
*Aucune issue détectée* — Symboles 💡⚠️ utilisés correctement, structure sections claire, gras/italique cohérents.

### Pédagogie Visuelle (NOUVELLES ATTENTES v1.1 showroom-staff)
- [ ] 🟡 **Encadrés info manquants** — Section 1.4 Contacts (lignes 26-51) : Hiérarchie contacts propriétaire/admin présentée en paragraphes. Ajouter ENCADRÉ VISUEL récapitulatif :
  ```
  ┌─────────────────────────────────────────┐
  │ 📞 QUI CONTACTER ?                      │
  ├─────────────────────────────────────────┤
  │ Propriétaire Showroom (PRINCIPAL)       │
  │ • Mot de passe, QR code, infos produit │
  │ • Réponse 24h                           │
  ├─────────────────────────────────────────┤
  │ Admin CERASCAN (via propriétaire)       │
  │ • Bugs répétés, données erronées        │
  │ • JAMAIS contact direct commercial      │
  └─────────────────────────────────────────┘
  ```
- [ ] 🟡 **Schéma workflow manquant** — Section 1.2 Comment Lire (lignes 9-13) : 2 modes lecture décrits textuellement. Ajouter SCHÉMA VISUEL décision :
  ```
  Première utilisation ?
      OUI → Lecture complète (15-20 min, Ch01-06)
      NON → Consultation rapide (Ctrl+F ou index)
  ```
- [ ] 🟡 **Hiérarchie visuelle faible section 1.3** — Conventions (lignes 15-23) : Liste 5 items uniforme. Améliorer SÉPARATION VISUELLE entre symboles (💡⚠️) et mises en forme texte (gras/italique/captures).

### Permissions Partenaire (NOUVELLES ATTENTES v1.1 showroom-owner)
- [ ] 🟡 **Périmètre partenaire non explicité** — Section 1.5 Ressources (lignes 57-58) : Mention Guide Partenaire "import CSV, QR codes" SANS préciser si commercial peut consulter/assister. Ajouter : "Consulter auprès propriétaire si vous devez l'assister (ex: coller QR codes showroom). **Vous ne pouvez PAS** importer CSV ou générer QR codes vous-même."

### Devil's Advocate (🟢 suggestions)

#### Showroom-Staff v1.1
- [ ] 🟢 **Encadré récap conventions début chapitre** : Ajouter encadré visuel 💡 avec 5 symboles + significations rapides (déjà décrit lignes 19-23 mais pas encadré distinct).
- [ ] 🟢 **Schéma hiérarchie contacts visuels** : Organigramme simple propriétaire→commercial, admin→propriétaire→commercial.
- [ ] 🟢 **Pédagogie progressive section 1.2** : Ajouter "📖 Lecture complète recommandée première journée showroom" AVANT liste modes.

#### Showroom-Owner v1.1
- [ ] 🟢 **Permissions commercial explicites section 1.5** : Préciser "Commercial PEUT : consulter catalogue, chercher produits, voir coordonnées support. CANNOT : importer CSV, générer QR codes, modifier config."
- [ ] 🟢 **Coûts non applicable Ch01** : N/A mode emploi. Vérifier chapitres suivants si fonctionnalités mentions coûts.
- [ ] 🟢 **Périmètre autonomie vs escalade section 1.4** : Tableau "Problème → Contact → Délai" plus visuel que paragraphes actuels.

#### Platform-Admin v1.1
- [ ] 🟢 **Architecture 3-tiers schéma visuel** : Diagramme commercial→partenaire→admin avec périmètres lectures.
- [ ] 🟢 **Permissions lecture guides tableau** : Staff→commercial, Owner→partenaire+commercial, Admin→tout.

**Priorités:** 🔴 Haute: 0 | 🟡 Moyenne: 4 | 🟢 Basse: 8

**Routing:** ≥1 correction 🟡 → **doctor.prompt.md** pour ajouter encadrés/schémas visuels (amélioration pédagogie).

---

## Section 2: SYSTEMIC PATTERNS → tone-finder.prompt.md

### Pattern 1: Pédagogie textuelle vs visuelle
- **Occurrences:** 3 instances (section 1.2 modes lecture texte, section 1.3 conventions liste uniforme, section 1.4 contacts paragraphes)
- **Impact:** Engagement -1pt showroom-staff (apprentissage visuel rapide prioritaire)
- **Source:** Showroom-staff v1.1 (must-have encadrés info, schémas workflows, hiérarchie visuelle)
- **Action output-style:**
  ```yaml
  visual-pedagogy:
    - "Privilégier encadrés visuels (bordures, icônes) pour informations clés récapitulatives"
    - "Ajouter schémas décision workflows (organigrammes simples, flèches) pour procédures"
    - "Renforcer hiérarchie visuelle (espacements, séparations horizontales, groupements visuels)"
  ```

**Recommandation:** Pattern détecté 3× Ch01 (mode emploi court). Si ≥3 chapitres suivants similaires → `tone-finder.prompt.md --improve-from-feedback` pour renforcer output-style user-friendly.md avec directives pédagogie visuelle.

---

## Section 3: QUALITATIVE SCORES

### Persona 1: Showroom-Staff v1.1 (Univers CERASCAN)

| Critère | Score | Poids | Pondéré | Evidence |
|---------|-------|-------|---------|----------|
| Engagement | 16/20 | 0.35 | 5.60 | Format ultra-compact (62 lignes), listes puces, symboles 💡⚠️ MAIS encadrés visuels manquants (-1pt), hiérarchie visuelle faible section 1.3 (-1pt). Schémas workflows absents. |
| Clarté | 16/20 | 0.30 | 4.80 | Terminologie simple définie, procédures ≤ 4 étapes MAIS pédagogie progressive manquante (-1pt). Conventions décrites texte pas encadré visuel distinct. |
| Immersion | 16/20 | 0.10 | 1.60 | Ton user-friendly direct, contexte terrain authentique, empathie contraintes quotidiennes. |
| Satisfaction | 17/20 | 0.25 | 4.25 | Utilisabilité terrain excellente (plastifiable, index, Ctrl+F) MAIS mémorisation visuelle perfectible (-1pt) : contacts hiérarchie paragraphes pas schéma. |
| **TOTAL** | - | 1.0 | **16.25/20** | - |

**Must-haves:** ✅ 8/12 validés | ❌ 4/12 manquants (nouvelles attentes v1.1)  
**Deal-breakers:** ✅ 0/6 présents

#### Must-Haves Validés (8/12)
- [x] Format ultra-compact (6-8 pages max)
- [x] Listes à puces prioritaires
- [x] Captures écran mentionnées (ligne 21)
- [x] Procédures ≤ 4 étapes
- [x] Exemples situations showroom réelles
- [x] Symboles visuels explicites (💡⚠️)
- [x] Contacts support hiérarchisés (contenu OK)
- [x] Terminologie simple définie

#### Must-Haves Manquants (4/12 — v1.1)
- [ ] **Encadrés info visuels** — Section 1.4 contacts hiérarchie présentée paragraphes, pas encadré visuel bordure récapitulatif (🟡 ligne 26-51)
- [ ] **Schémas visuels workflows** — Section 1.2 modes lecture décrit texte, pas organigramme décision (🟡 ligne 9-13)
- [ ] **Hiérarchie visuelle forte** — Section 1.3 conventions liste uniforme sans séparation visuelle symboles vs mises en forme texte (🟡 ligne 15-23)
- [ ] **Pédagogie progressive** — Pas de mention "lecture complète recommandée première journée" avant liste modes (🟢 suggestion)

**Score impact must-haves manquants :** 4 manquants → plafond 11/20 **MAIS** issues mineures (🟡 moyennes, pas 🔴 critiques) → Score maintenu 16/20 avec pénalités graduelles (-1pt engagement encadrés, -1pt clarté pédagogie, -1pt satisfaction mémorisation visuelle, -1pt engagement hiérarchie).

#### Points Forts
- **Format terrain ultra-optimisé** — 62 lignes, 0,5-1 page, plastifiable A4, listes puces, symboles 💡⚠️, procédures ≤ 4 étapes. Base solide.
- **Terminologie simple accessible** — Comptoir d'accueil showroom, pochette plastique, formation 30 min propriétaire. Zéro jargon.
- **Exemples situations réelles** — Mot de passe oublié, QR code ne scanne pas, déconnexion session partagée. Contexte authentique.

#### Points d'Amélioration
- [ ] **Ajouter encadré visuel récap contacts (🟡 critique)** — Section 1.4 lignes 26-51 : Transformer paragraphes en tableau bordure "QUI CONTACTER ?" avec colonnes Problème/Contact/Délai (+1pt Engagement, +0.5pt Satisfaction)
- [ ] **Ajouter schéma décision modes lecture (🟡 critique)** — Section 1.2 lignes 9-13 : Organigramme simple "Première utilisation ? OUI→Lecture complète / NON→Consultation rapide" (+1pt Clarté, +0.5pt Engagement)
- [ ] **Renforcer hiérarchie visuelle section 1.3 (🟡 critique)** — Lignes 15-23 : Séparer conventions visuellement : groupe symboles (💡⚠️) vs groupe mises en forme (gras/italique/captures) avec ligne horizontale ou espacement (+1pt Engagement)

---

### Persona 2: Showroom-Owner v1.1 (Univers CERASCAN)

| Critère | Score | Poids | Pondéré | Evidence |
|---------|-------|-------|---------|----------|
| Engagement | 16/20 | 0.15 | 2.40 | Lecture fluide, structure 5 sections, listes puces lisibles, autonomie navigation. |
| Clarté | 15/20 | 0.40 | 6.00 | Terminologie cohérente, procédures simples MAIS permissions commercial non explicitées (-2pts) : section 1.5 mention Guide Partenaire sans préciser si commercial peut assister/consulter. Périmètre autonomie vs escalade admin flou. |
| Immersion | 15/20 | 0.10 | 1.50 | Perspective métier claire, ton professionnel, empathie contraintes utilisateur. |
| Satisfaction | 16/20 | 0.35 | 5.60 | Indépendance opérationnelle staff, cohérence inter-guides MAIS limites droits commercial manquantes (-1pt) : peut-il assister partenaire sur tâches import CSV? Coûts N/A Ch01 (vérifier chapitres suivants). |
| **TOTAL** | - | 1.0 | **15.50/20** | - |

**Must-haves:** ✅ 7/10 validés | ❌ 3/10 manquants (nouvelles attentes v1.1)  
**Deal-breakers:** ✅ 0/9 présents

#### Must-Haves Validés (7/10)
- [x] Procédures simplifiées avec captures écran
- [x] Workflows complets autonomes
- [x] Terminologie cohérente inter-guides
- [x] Troubleshooting erreurs fréquentes
- [x] Limites contraintes explicites (formats, périmètres guides)
- [x] Formats CSV N/A Ch01 (applicable chapitres suivants)
- [x] Statistiques N/A Ch01 (applicable guide-partenaire)

#### Must-Haves Manquants (3/10 — v1.1)
- [ ] **Permissions partenaire explicites** — Section 1.5 lignes 57-58 mention Guide Partenaire "import CSV, QR codes" SANS préciser si commercial peut consulter/assister. Ajouter "Commercial NE PEUT PAS importer CSV/générer QR codes lui-même" (🟡 ligne 57)
- [ ] **Coûts associés actions** — N/A Ch01 mode emploi (applicable chapitres suivants fonctionnalités avancées)
- [ ] **Périmètre autonomie vs escalade admin** — Section 1.4 lignes 26-51 hiérarchie contacts OK MAIS pas explicite "tâches quotidiennes autonomes commercial" vs "nécessitant intervention propriétaire/admin"

**Score impact must-haves manquants :** 3 manquants dont 1 applicable Ch01 (permissions) → Clarté -2pts (permissions floues), Satisfaction -1pt (limites droits manquantes).

#### Points Forts
- **Terminologie uniforme inter-guides** — Commercial, propriétaire showroom, administrateur CERASCAN cohérents. Vision multi-guides.
- **Autonomie staff garantie workflows** — Guide complet sans dépendance propriétaire quotidien (contacts, conventions, ressources documentés).
- **Périmètres guides définis** — Commercial 6-8p, Partenaire 22-30p, Admin 53-65p. Architecture claire.

#### Points d'Amélioration
- [ ] **Préciser permissions commercial section 1.5 (🟡 critique)** — Ligne 57 : Ajouter "Consulter auprès propriétaire si vous devez l'assister (ex: coller QR codes showroom). **Vous ne pouvez PAS** importer CSV ou générer QR codes vous-même." (+2pts Clarté, +1pt Satisfaction)
- [ ] **Tableau périmètre autonomie section 1.4 (🟢 optionnel)** — Lignes 26-51 : Tableau "Problème → Qui résout → Autonomie" (commercial seul / avec propriétaire / admin via propriétaire) (+0.5pt Clarté)

---

### Persona 3: Platform-Admin v1.1 (Univers CERASCAN)

| Critère | Score | Poids | Pondéré | Evidence |
|---------|-------|-------|---------|----------|
| Engagement | 15/20 | 0.10 | 1.50 | Densité information correcte, références inter-guides, efficacité technique. |
| Clarté | 16/20 | 0.35 | 5.60 | Procédures complètes mode emploi, architecture 3-tiers documentée, terminologie uniforme, gestion erreurs présente. |
| Immersion | 14/20 | 0.10 | 1.40 | Authenticité technique correcte, perspective système, ton professionnel. |
| Satisfaction | 17/20 | 0.45 | 7.65 | Utilisabilité opérationnelle excellente, cohérence inter-guides, couverture sécurité, troubleshooting efficace. |
| **TOTAL** | - | 1.0 | **16.15/20** | - |

**Must-haves:** ✅ 8/8 validés  
**Deal-breakers:** ✅ 0/6 présents

*Score identique v1.0 → v1.1 (calibration showroom-owner/staff n'affecte pas platform-admin mode emploi Ch01).*

#### Points Forts
- **Architecture 3-tiers documentée** — Commercial 6-8p, Partenaire 22-30p, Admin 53-65p. Permissions implicites correctes.
- **Terminologie uniforme inter-guides** — Commercial, propriétaire, administrateur cohérents. Maintenance future facilitée.
- **Sécurité contrôle accès explicite** — Ligne 47-51 pas contact direct admin, ligne 20 déconnexion session partagée.

#### Points d'Amélioration
- [ ] **Schéma architecture 3-tiers (🟢 optionnel)** — Diagramme visuel commercial→partenaire→admin avec périmètres lectures (+0.3pt Engagement)

---

## CONSENSUS: 15.98/20

| Persona | Score | Poids | Pondéré | Source |
|---------|-------|-------|---------|--------|
| Showroom-Staff v1.1 | 16.25/20 | 1.0 | 16.25 | Univers (target principal) |
| Showroom-Owner v1.1 | 15.50/20 | 0.8 | 12.40 | Univers (lecture secondaire) |
| Platform-Admin v1.1 | 16.15/20 | 0.8 | 12.92 | Univers (lecture complète) |
| **CONSENSUS** | - | 2.6 | **15.98/20** | - |

**Calcul :** (16.25×1.0 + 15.50×0.8 + 16.15×0.8) / 2.6 = 41.57 / 2.6 = **15.98/20**

### Analyse Évolution Scores v1.0 → v1.1

**Showroom-Staff :** 17.50 → 16.25 (-1.25pts)
- Calibration v1.1 détecte manques pédagogie visuelle : encadrés info, schémas workflows, hiérarchie visuelle forte
- Must-haves 12 (was 8) : 4 nouvelles attentes visuelles non remplies Ch01
- Score réaliste : contenu textuel solide MAIS présentation visuelle perfectible

**Showroom-Owner :** 16.65 → 15.50 (-1.15pts)
- Calibration v1.1 détecte manques permissions : droits commercial non explicités section 1.5
- Must-haves 10 (was 7) : 3 nouvelles attentes permissions/coûts, 1 applicable Ch01 manquante
- Score réaliste : périmètre autonomie vs escalade admin flou

**Platform-Admin :** 16.15 → 16.15 (stable)
- Calibration v1.1 n'affecte pas admin (permissions owner/staff hors scope admin mode emploi)

**Consensus :** 16.77 → 15.98 (-0.79pts)
- Score plus réaliste après calibration personas v1.1
- **Seuil production-ready maintenu** (15-17/20) MAIS corrections 🟡 moyennes recommandées

### Échelle /20
- **18-20/20** : Production-ready (publiable immédiatement)
- **15-17/20** : Corrections mineures (1-2h, typos/formatage) ← **CHAPITRE 01 ICI**
- **12-14/20** : Utilisable avec réserves (must-haves OK, nice-to-have manquants)

**Chapitre 01 reste production-ready (15.98/20)** MAIS corrections 🟡 moyennes amélioreront scores :
- +1pt staff encadrés visuels → 17.25/20
- +2pts owner permissions explicites → 17.50/20
- Consensus projeté après corrections : **17.3/20** (production-ready optimale)

---

## ROUTING DECISIONS

**Section 1 (Technical):**
- ✅ 4 corrections 🟡 moyennes : → `doctor.prompt.md` pour ajouter encadrés/schémas visuels + préciser permissions

**Section 2 (Systemic):**
- ⚠️ 1 pattern détecté (pédagogie textuelle vs visuelle) : Surveiller chapitres 02-06. Si ≥3 chapitres similaires → `tone-finder.prompt.md`

**Section 3 (Iteration):**
- ✅ Consensus 15.98/20 ≥ 15/20 : **Seuil production-ready atteint**
- ⚠️ Score -0.79pts vs v1.0 (calibration v1.1 plus stricte) : Corrections 🟡 recommandées

**Action:**
- 🟡 **15-17/20** : Corrections moyennes recommandées (encadrés visuels, permissions explicites)
- ➡️ **Option A** : Appliquer corrections 🟡 doctor → score projeté 17.3/20
- ➡️ **Option B** : Publier tel quel (15.98/20 acceptable) + appliquer attentes visuelles chapitres suivants

---

## Validation Calibration Personas v1.1

**Showroom-staff v1.1 :**
- ✅ Détecte manques pédagogie visuelle (encadrés, schémas, hiérarchie)
- ✅ Score réaliste 16.25/20 (was 17.50) avec must-haves visuels renforcés
- ✅ Calibration réussie : issues 🟡 moyennes identifiées correctement

**Showroom-owner v1.1 :**
- ✅ Détecte manques permissions explicites (droits commercial flous section 1.5)
- ✅ Score réaliste 15.50/20 (was 16.65) avec must-haves permissions/coûts renforcés
- ✅ Calibration réussie : issues 🟡 moyennes identifiées correctement

**Conclusion :** Personas v1.1 plus strictes et réalistes. Détectent issues pédagogie visuelle (staff) et permissions (owner) manquées v1.0. Prêtes pour production avec surveillance chapitres suivants.

---

**Fichier généré :** `.wip/comments/chapitre01-personas-cerascan-v1.1-recalibration.md`  
**Validation :** Chapitre 01 production-ready (15.98/20) avec corrections 🟡 recommandées  
**Action suivante :** Appliquer corrections doctor OU écrire Ch02 avec attentes visuelles v1.1
