# Analyse Personas: Comment Utiliser ce Guide (v1.1 calibrated)

**Date:** 2026-02-28 | **Chapitre:** 01 | **Personas:** 1 (product-owner v1.1)

---

## Section 1: TECHNICAL ISSUES → doctor.prompt.md

### Typographie
Aucun problème détecté. Guillemets «», tirets —, accents corrects.

### Grammaire
Aucun problème détecté.

### Terminologie
- [ ] 🔴 **Terminologie "comptoir showroom" non contextualisée** — Ligne 5 : "comptoir showroom" utilisé sans définition (comptoir d'accueil client ? espace vente ? zone caisse ?). Documentation technique doit préciser LIEU EXACT : "comptoir d'accueil showroom où vous renseignez clients".
- [ ] 🔴 **Terminologie "plastifiable" sans procédure** — Ligne 5 : "format plastifiable" sans contexte d'utilisation pratique. COMMENT plastifier ? (pochette A4 ? plastifieuse ? prestataire externe ?). Must-have manquant : instructions sans FORMAT EXACT résultat attendu.
- [ ] 🔴 **Forward-reference "Scanner QR code"** — Ligne 19 : exemple Astuce Rapide "Scanner QR code directement depuis barre recherche" mais QR codes NON introduits Ch01 (arrivent Ch03). Forward-reference sans contexte minimal. Deal-breaker v1.1 : exemple référençant fonctionnalité non documentée.
- [ ] 🔴 **Forward-reference "Mode hors ligne"** — Ligne 20 : exemple Attention "Mode hors ligne limité consultation" mais mode hors ligne NON documenté Ch01 (arrive Ch06 ?). Forward-reference sans définition minimale. Exemple non reproductible.
- [ ] 🟡 **Format coordonnées propriétaire manquant** — Ligne 37 : "Coordonnées affichées page connexion (email + téléphone)" mais FORMAT EXACT non précisé. Commercial voit email SEUL ? téléphone SEUL ? formulaire contact ? Must-have v1.1 : FORMAT EXACT résultats attendus manquant.

### Formatting
Aucun problème détecté.

### Devil's Advocate (🟢 suggestions)
- [ ] 🟢 **Product Owner v1.1**: Ligne 5 "formation 30 minutes" non contextualisée. Formation sur QUOI précisément ? (connexion ? catalogue complet ? tout le guide ?). Suggestion : "formé 30 min par propriétaire (connexion, navigation catalogue basique)".
- [ ] 🟢 **Product Owner v1.1**: Ligne 13 "index en fin de document" mais Ch01 ne précise pas QUAND (page combien ? après Ch06 ?). Suggestion : "index termes clés après Chapitre 6 (page X)".
- [ ] 🟢 **Product Owner v1.1**: Section "Ressources Complémentaires" lignes 57-58 mentionne longueurs autres guides (22-30 pages, 53-65 pages) mais pas QUAND consulter. Suggestion : "Si propriétaire vous demande aide import CSV, Guide Partenaire Ch02 détaille procédure".

**Priorités:** 🔴 Haute: 5 | 🟡 Moyenne: 0 | 🟢 Basse: 3

**Routing:** 5 corrections 🔴 critiques → **doctor.prompt.md chapitre01.md** (30-45 min corrections must-haves)

---

## Section 2: SYSTEMIC PATTERNS → tone-finder.prompt.md

### Pattern 1: Forward-References Sans Contexte

- **Occurrences:** 2 instances critiques (ligne 19 "Scanner QR code", ligne 20 "Mode hors ligne")
- **Impact:** Clarté -3pts (exemples non reproductibles), Satisfaction -2pts (confusion lecteur)
- **Source:** Product Owner v1.1 (deal-breaker "exemple référençant fonctionnalité non documentée")
- **Action output-style:**
  ```yaml
  conventions-document:
    - "Exemples conventions UNIQUEMENT fonctionnalités déjà introduites Ch01-courant"
    - "Si forward-reference nécessaire, ajouter contexte minimal : 'Scanner QR code (détaillé Ch03)'"
    - "Alternative : exemples génériques sans référence fonctionnalités futures"
  ```

**Recommandation:** Pattern détecté 2× Ch01 (seuil 3× non atteint) → **Surveiller Ch02-06**. Si forward-references récurrentes ≥3 chapitres → `tone-finder.prompt.md --improve-from-feedback`

---

## Section 3: QUALITATIVE SCORES

### Persona: Product Owner v1.1 (global, calibrated)

| Critère | Score | Poids | Pondéré | Evidence |
|---------|-------|-------|---------|----------|
| Engagement | 17/20 | 0.15 | 2.55 | Ton direct professionnel ("Vous êtes commercial..."), structure logique 5 sections, transition finale engageante |
| Clarté | 12/20 | 0.40 | 4.80 | **PLAFOND MUST-HAVE**: 2 forward-references sans contexte (lignes 19-20 QR code/mode hors ligne), terminologie métier floue (ligne 5 "comptoir showroom" sans définition, "plastifiable" sans procédure), format résultats manquant (ligne 37 coordonnées format exact non précisé). Vocabulaire général accessible mais exemples non reproductibles. |
| Immersion | 15/20 | 0.15 | 2.25 | Contexte professionnel présent (showroom, formation 30 min), atmosphère documentation utilisateur, conventions document explicites (💡, ⚠️, gras, italique) |
| Satisfaction | 14/20 | 0.30 | 4.20 | Documentation Ch01 structure correcte (objectif, modes lecture, conventions, contacts, ressources) MAIS forward-references dégradent utilisabilité pratique (commercial lit exemple ligne 19 "Scanner QR code" sans comprendre contexte avant Ch03). Longueur respectée. |
| **TOTAL** | - | 1.0 | **13.80/20** | - |

**Must-haves:** ❌ 2 manquants
- ❌ "Exemples conventions référencent UNIQUEMENT contenu déjà documenté" → Lignes 19-20 forward-references QR code + mode hors ligne non documentés Ch01
- ❌ "Instructions avec FORMAT EXACT résultats" → Ligne 37 coordonnées propriétaire format non précisé (email seul? téléphone? formulaire?)
- **Score plafonné 14/20 maximum** (must-have manquant)

**Deal-breakers:** ❌ 1 présent
- ❌ "Exemple référençant fonctionnalité non documentée" → Lignes 19-20 exemples conventions mentionnent QR code + mode hors ligne AVANT introduction (Ch03/Ch06)
- **Score plafonné 11/20 maximum** (deal-breaker présent)

**Score final = MIN(13.80, 14, 11) = 11/20**

**Ajustement score Clarté :** 12/20 initial → 11/20 après plafonds (deal-breaker impacte consensus)

#### Craft Checklist (Documentation Technique v1.1)

| # | Question | Réponse |
|---|----------|---------|
| DT1 | **Terminologie définie** : Tous termes techniques/métier définis avant usage ? | ❌ Partiellement. Conventions (💡, ⚠️, gras, italique) définies lignes 19-23 OK. Termes métier NON définis : "comptoir showroom" ligne 5 (où exactement ?), "plastifiable" ligne 5 (comment ?), "formation 30 min" ligne 5 (sur quoi ?). |
| DT2 | **Procédures claires** : Chaque instruction explique quand/comment utiliser ? | ⚠️ Partiellement. "Comment Lire" modes lecture OK. "Contacts Support" hiérarchie quand contacter OK MAIS format résultats manquant ligne 37 (email? téléphone?). |
| DT3 | **Exemples concrets reproductibles** : Exemples SANS forward-references ? | ❌ NON. Lignes 19-20 exemples conventions réf. QR code + mode hors ligne NON documentés Ch01. Exemple non reproductible (lecteur Ch01 ne comprend pas contexte avant Ch03/Ch06). Deal-breaker v1.1. |
| DT4 | **Contexte d'utilisation** : Fonctionnalités avec quand/comment utiliser PRATIQUE ? | ⚠️ Partiellement. Contacts support cas usage détaillés (lignes 31-44) OK. Terminologie métier sans contexte pratique ("comptoir showroom" où ? "plastifiable" comment ?). |
| DT5 | **Références croisées** : Liens sections/ressources valides ? | ✅ Oui. "Chapitre 2" ligne 62 valide, "Guide Partenaire/Administrateur" lignes 57-58 contextualisés. |

#### Points Forts

- **Hiérarchie contacts support excellente** : Structure Propriétaire/Administrateur avec cas usage détaillés (lignes 31-44), insistance "pas contact direct commercial" bloc Attention (lignes 50-51)
- **Typographie française impeccable** : Guillemets «», tirets —, accents corrects, espaces insécables respectées
- **Conventions document explicites** : 5 symboles listés avec exemples (lignes 19-23), facilite décodage chapitres suivants

#### Points d'Amélioration (CRITIQUES)

- [ ] 🔴 **Supprimer forward-references exemples conventions** : Lignes 19-20 remplacer exemples QR code/mode hors ligne par exemples génériques OU ajouter contexte minimal "(détaillé Ch03)". Deal-breaker v1.1.
- [ ] 🔴 **Définir termes métier** : Ligne 5 "comptoir showroom" → "comptoir d'accueil showroom où vous renseignez clients", "plastifiable" → "protégé pochette plastique A4 (fournie propriétaire)".
- [ ] 🔴 **Préciser format coordonnées** : Ligne 37 "Coordonnées affichées page connexion (email + téléphone)" → préciser si email SEUL visible OU téléphone OU les deux.
- [ ] 🟢 **Contextualiser formation 30 min** : Ligne 5 "formé 30 min" → "formé 30 min (connexion, navigation catalogue basique)".
- [ ] 🟢 **Préciser index** : Ligne 13 "index fin document" → "index termes clés après Chapitre 6".

---

## CONSENSUS: 11/20

| Persona | Score | Poids | Pondéré | Source |
|---------|-------|-------|---------|--------|
| Product Owner v1.1 | 11/20 | 1.0 | 11.00 | Global (calibrated) |
| **CONSENSUS** | - | 1.0 | **11/20** | - |

**Interprétation :** 11/20 = **Non utilisable (must-haves manquants)**. Deal-breaker présent (forward-references lignes 19-20 QR code + mode hors ligne sans contexte). Must-haves manquants (exemples non reproductibles, format résultats non précisé ligne 37, terminologie métier floue ligne 5). Corrections 🔴 critiques obligatoires (30-45 min) avant publication.

**Calibration validée :** v1.0 notait 17.15/20 (trop indulgent), v1.1 détecte correctement 11/20 avec deal-breaker forward-references.

---

## ROUTING DECISIONS

**Section 1 (Technical):**
- 5 corrections 🔴 critiques (forward-references, terminologie, formats) → **doctor.prompt.md chapitre01.md** (30-45 min corrections must-haves)
- 3 suggestions 🟢 Devil's Advocate → **Intégrer après corrections 🔴**

**Section 2 (Systemic):**
- Pattern forward-references 2× Ch01 → **Surveiller Ch02-06**
- Si récurrent ≥3 chapitres → `tone-finder.prompt.md --improve-from-feedback`

**Section 3 (Iteration):**
- Consensus 11/20 < target 15/20 → **CORRECTIONS OBLIGATOIRES**
- Deal-breaker présent → **BLOQUANT publication**

**Action:**
- 🔴 **OBLIGATOIRE : Corrections critiques Ch01** (supprimer forward-references, définir terminologie, préciser formats)
- ⏸️ **PAUSE Ch02** jusqu'à corrections Ch01 validées
- 🔄 **Re-comment après corrections** pour valider 15+/20
