# Changelog Corrections: Chapitre 01 v1.2

**Date:** 2026-02-28  
**Source feedback:** `.wip/comments/chapitre01-personas-cerascan-v1.1-recalibration.md`  
**Personas:** showroom-staff v1.1, showroom-owner v1.1, platform-admin v1.1  
**Score avant:** 15.98/20 (production-ready avec corrections moyennes)  
**Score cible:** 17.3/20 (production-ready optimale)

---

## Corrections Appliquées (4/4 = 100%)

### 1. Section 1.2 — Schéma décision modes lecture (🟡 showroom-staff)
**Avant:**
```markdown
- **Lecture complète** : Parcourez les 6 chapitres linéairement si vous découvrez l'application (15-20 minutes de lecture totale).
- **Consultation rapide** : Utilisez le sommaire interactif du PDF ou la fonction recherche (Ctrl+F) pour trouver une information précise (ex : "mot de passe oublié").
```

**Après:**
```markdown
┌────────────────────────────────────────────┐
│ 📖 COMMENT UTILISER CE GUIDE ?             │
├────────────────────────────────────────────┤
│  Première utilisation ?                   │
│  ✓ OUI → Lecture complète (15-20 min)     │
│  ✗ NON → Consultation rapide (Ctrl+F)     │
└────────────────────────────────────────────┘
```

**Raison:** Must-have showroom-staff v1.1 "schémas visuels workflows". Organigramme décision simple avec encadré bordure ASCII pour mémorisation visuelle rapide. (+1pt Clarté, +0.5pt Engagement)

---

### 2. Section 1.3 — Hiérarchie visuelle conventions (🟡 showroom-staff)
**Avant:**
```markdown
- **💡 Astuce Rapide** — ...
- **⚠️ Attention** — ...
- **Captures écran annotées** — ...
- **Texte en gras** — ...
- *Texte en italique* — ...
```

**Après:**
```markdown
**Symboles visuels :**
- **💡 Astuce Rapide** — ...
- **⚠️ Attention** — ...

**Mises en forme texte :**
- **Captures écran annotées** — ...
- **Texte en gras** — ...
- *Texte en italique* — ...
```

**Raison:** Must-have showroom-staff v1.1 "hiérarchie visuelle forte". Séparation groupes symboles (💡⚠️) vs mises en forme texte (gras/italique/captures). Espacements généreux. (+1pt Engagement)

---

### 3. Section 1.4 — Encadré visuel contacts support (🟡 showroom-staff)
**Avant:**
```markdown
### Propriétaire Showroom (contact principal)

**Quand contacter :**
- Questions quotidiennes (mot de passe oublié, compte bloqué)
...

### Administrateur CERASCAN (via propriétaire uniquement)
```

**Après:**
```markdown
┌─────────────────────────────────────────────────────┐
│ 📞 QUI CONTACTER ?                                  │
├─────────────────────────────────────────────────────┤
│ PROPRIÉTAIRE SHOWROOM (contact principal)          │
│ Quand : Questions quotidiennes...                   │
│ Comment : Coordonnées page connexion (24h)         │
├─────────────────────────────────────────────────────┤
│ ADMINISTRATEUR CERASCAN (via propriétaire)         │
│ Quand : Problèmes complexes...                      │
│ Comment : UNIQUEMENT via propriétaire              │
└─────────────────────────────────────────────────────┘
```

**Raison:** Must-have showroom-staff v1.1 "encadrés info visuels (récapitulatifs clés)". Tableau bordure ASCII hiérarchie contacts avec séparation visuelle propriétaire/admin. Reconnaissance rapide comptoir showroom. (+1pt Engagement, +0.5pt Satisfaction)

---

### 4. Section 1.5 — Permissions commercial explicites (🟡 showroom-owner)
**Avant:**
```markdown
- **Guide Partenaire CERASCAN** : Import catalogue CSV, génération et impression QR codes, placement showroom (22-30 pages). Consulter auprès du propriétaire showroom si vous devez l'assister sur ces tâches.
```

**Après:**
```markdown
- **Guide Partenaire CERASCAN** : Import catalogue CSV, génération et impression QR codes, placement showroom (22-30 pages). Consulter auprès du propriétaire showroom si vous devez l'assister sur ces tâches (par exemple : coller QR codes produits showroom).  
  **⚠️ Vous ne pouvez PAS** importer CSV ou générer QR codes vous-même — ces actions sont réservées au propriétaire showroom.
```

**Raison:** Must-have showroom-owner v1.1 "permissions partenaire explicites (PEUT faire vs NE PEUT PAS)". Précise périmètre autonomie commercial : assistance physique OK (coller QR codes), actions techniques interdites (import CSV, génération QR). (+2pts Clarté, +1pt Satisfaction)

---

## Résumé

**Corrections appliquées :** 4/4 (100%)
- ✅ Schéma décision modes lecture (encadré ASCII workflow)
- ✅ Hiérarchie visuelle conventions (groupes séparés)
- ✅ Encadré visuel contacts support (tableau bordure)
- ✅ Permissions commercial explicites (PEUT vs NE PEUT PAS)

**Impact scores estimé :**
- **Showroom-staff v1.1 :** 16.25 → **17.50/20** (+1.25pts : engagement +2pts, clarté +1pt, satisfaction +0.5pt, pénalité hiérarchie -0.25pt compensée)
- **Showroom-owner v1.1 :** 15.50 → **17.50/20** (+2pts : clarté +2pts permissions, satisfaction +1pt périmètre, pénalité -1pt compensée)
- **Platform-admin v1.1 :** 16.15 → **16.50/20** (+0.35pts : engagement +0.3pt schéma architecture implicite)

**Consensus estimé :** 15.98 → **17.30/20** (production-ready optimale)

---

## Validation Pédagogie Visuelle

**Encadrés ASCII bordure :**
- ✅ Section 1.2 : Organigramme décision modes lecture
- ✅ Section 1.4 : Tableau récapitulatif contacts support

**Hiérarchie visuelle renforcée :**
- ✅ Section 1.3 : Groupes séparés symboles vs mises en forme texte

**Permissions explicites :**
- ✅ Section 1.5 : PEUT (assister physiquement) vs NE PEUT PAS (actions techniques)

**Pattern pédagogie visuelle établi pour chapitres 02-06 :**
- Encadrés ASCII pour workflows/décisions/récapitulatifs
- Hiérarchie visuelle groupes logiques (espacements, séparations)
- Permissions explicites PEUT/NE PEUT PAS pour rôles

---

**Action suivante :** Re-comment avec personas v1.1 pour validation score 17+/20
