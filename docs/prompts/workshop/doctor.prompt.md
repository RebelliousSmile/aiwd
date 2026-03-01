---
name: doctor
description: Text improvement - grammar, terminology, formatting, enrichments (guided by output-style)
argument-hint: <file> [--remarks "custom notes"] [--dry-run]
version: 1.5
changelog:
  - version: 1.5
    date: 2026-02-14
    changes:
      - "Nouveau Step 1.5 : Cross-Chapter Redundancy Scan (PNJ re-décrits, vocabulaire re-traduit, disclaimers répétés, conseils MJ génériques)"
      - "Tag [REDUNDANCY] dans les corrections"
  - version: 1.4
    date: 2026-02-14
    changes:
      - "Suppression option --mode quick|deep (le doctor corrige tout systématiquement)"
      - "Les priorités 🔴🟡🟢 restent dans le rapport mais ne filtrent plus les corrections"
  - version: 1.3
    date: 2026-02-14
    changes:
      - "Boundary Rules: toute correction persona autorisée, encadrée par l'output-style"
      - "Suppression LaTeX du scope rédactionnel (le doctor travaille sur markdown)"
      - "Chemins .wip/ au lieu de .docs/changelog/"
  - version: 1.2
    date: 2026-02-14
    changes:
      - "Remplacement des fichiers .bak par un changelog cumulatif (.wip/changelog/chapitre<XX>-changelog.md)"
      - "Le changelog trace toutes les corrections appliquées par itération, évitant de diff des fichiers complets"
  - version: 1.1
    date: 2026-02-14
    changes:
      - "Ajout versioning avec changelog dans le frontmatter YAML"
  - version: 1.0
    date: 2026-01-31
    changes:
      - "Version initiale : corrections techniques (grammaire, terminologie, typographie, LaTeX)"
      - "Modes quick (critical only) et deep (full review)"
      - "Backup automatique avec timestamp"
      - "Dry-run pour simulation"
---

# Doctor: Technical Text Improvement

## Goal

Apply all corrections requested by personas: French grammar/typography, terminology, formatting, enrichments (descriptions, encadrés, détails). **Encadré par l'output-style — tout ajout doit respecter le ton et le format définis.**

## Arguments **[2]**

```bash
# Basic usage
doctor.prompt.md chapitres/chapitre01.md

# With remarks (issues personas, corrections spécifiques)
doctor.prompt.md chapitres/chapitre01.md --remarks "Enrichir palette sensorielle, corriger stats Tetsu"

# Dry-run (simulation only)
doctor.prompt.md chapitres/chapitre01.md --dry-run
```

## Context Resources **[10]**

**MUST load all (in order):**

1. **Bank:** `@bank.yml` (document type, univers)
2. **TOC-Chapter:** `@.toc/toc-chapter<XX>.md` (pour récupérer output-style spécifique)
3. **Output-style:** Charger le fichier indiqué dans toc-chapter (ex: `@<client>/.output-styles/user-friendly.md`)
4. **Client docs:** `@<client>/.docs/CLIENT.md`, `@<client>/.docs/glossaire.md`
6. **Typographie:** `@docs/typographie.md` (si existe, sinon règles en dur)
7. **Target file:** `@$ARGUMENTS[0]`
8. **User remarks:** `$ARGUMENTS[--remarks]` (optional)

**Logique output-style:**
- Extraire du toc-chapter la ligne `**Output-style:** <filename>.md`
- Charger ce fichier depuis `<client>/.output-styles/<filename>.md`
- Fallback: si pas de toc-chapter, utiliser `output-style.global` de bank.yml

## Rules **[10]**

**CRITICAL:**
- ✅ PRESERVE original meaning and intent
- ✅ PRESERVE narrative voice and style
- ✅ FOLLOW output-style conventions for all additions (ton, format, niveau de détail)
- ❌ NEVER use personas (that's comment.prompt.md)
- ❌ NEVER invent content not supported by TOC, terminologie, or UNIVERS.md

**Apply:**
1. French typography (guillemets «», tirets cadratins —, espaces insécables)
2. French grammar and orthography (accents é è ê à ù, conjugaison)
3. Terminologie per terminologie.md (strict)
4. Output-style conventions (dialogue format, emphasis, lists)

**Document:**
- ALL changes with line number + reason

## Boundary Rules — Output-Style comme Garde-Fou

**Le doctor peut exécuter toutes les corrections demandées par les personas**, y compris des ajouts de contenu (descriptions, encadrés, détails). La contrainte n'est pas la taille mais **la conformité à l'output-style**.

### Ce que le doctor PEUT faire

- Corriger stats, terminologie, orthographe, typographie
- Ajouter des exemples concrets si demandé par les personas
- Insérer des encadrés (> **Astuce**, > **Attention**, > **Note**)
- Enrichir des procédures, ajouter des critères de vérification
- Reformuler des paragraphes pour clarté ou accessibilité

### Garde-fous obligatoires

1. **Output-style** : Tout ajout DOIT respecter le ton, le format et le niveau de détail définis dans l'output-style chargé en contexte
2. **Sources** : Tout contenu ajouté DOIT être fondé sur la TOC, glossaire.md, CLIENT.md ou les --remarks. Ne jamais inventer de contenu sans source
3. **Cohérence** : Les ajouts doivent s'intégrer au tissu narratif existant (pas de blocs détachés du contexte)
4. **Changelog** : Chaque ajout documenté avec source et justification

### Quand router vers write-* --feedback

Le doctor est toujours tenté en premier. Le routing vers write-* --feedback est déclenché par **les scores personas**, pas par la taille des corrections :

- **≥2 personas plafonnées ≤11/20** par must-haves structurels → rewrite --feedback
- **Consensus qui ne progresse plus après doctor** (Δ < 1.0 entre itérations) → envisager rewrite

Le doctor ne se limite pas : il fait toutes les corrections possibles. C'est le review-pipeline qui décide si le résultat est suffisant.

## Correction Priority **[3]**

| Priority | Category | Examples |
|----------|----------|----------|
| 🔴 **CRITICAL** | Grammar, orthography, terminology | Fautes, accents manquants, *saidin* écrit *saidar* |
| 🟡 **IMPORTANT** | Typography, Markdown formatting | Guillemets droits ", format dialogue non conforme |
| 🟢 **OPTIONAL** | Style optimization, flow | Phrase trop longue, transition faible |

**Le doctor corrige TOUT** (🔴 + 🟡 + 🟢). Les priorités servent au rapport, pas au filtrage.

## Steps **[1-4-7]**

### 1. Load & Parse

1. Load all context resources (bank.yml → output-style)
2. Identify document type (`technical-doc`, `user-guide`, `api-doc`, `process-doc`)
3. Parse user remarks if provided

### 1.5 Cross-Chapter Redundancy Scan (chapitres 02+)

**SKIP pour le chapitre 01.**

Avant de scanner le texte, charger les chapitres précédents dans `chapitres/` et construire un inventaire :

1. **Termes déjà définis** : si le chapitre courant ré-explique un terme déjà introduit → correction 🟡 : garder le terme seul, renvoi au chapitre d'introduction
2. **Procédures déjà documentées** : si une procédure est redécrite intégralement alors qu'elle est déjà dans un chapitre antérieur → correction 🟡 : remplacer par renvoi court
3. **Avertissements répétés** : si un encadré ⚠️ Attention répète un avertissement posé plus tôt → correction 🟡 : supprimer le doublon
4. **Introductions génériques** : si des formulations d'introduction identiques apparaissent dans plusieurs chapitres → correction 🟢 : différencier

Ces corrections s'ajoutent à la liste du Step 2 avec le tag `[REDUNDANCY]`.

### 2. Scan Text **[5-6]**

Build correction list by priority:

**🔴 Critical:**
- Fautes d'orthographe/grammaire
- Erreurs de terminologie (terminologie.md)
- Accents manquants (é è ê ë à â ù û ô î ï ç œ æ)

**🟡 Important:**
- Typographie française (« » — … espaces insécables)
- Format Markdown non conforme (:::regle, :::secret, blockquotes)
- Format dialogue non conforme

**🟢 Optional:**
- Phrases trop longues (>40 mots)
- Répétitions
- Transitions faibles

### 3. Apply User Remarks **[2]**

If `--remarks` provided, add to correction list with priority 🔴.

### 4. Generate Corrections **[7]**

For each issue (by priority):

```markdown
**#N - [Priority] [Category]** (Line XX)
- Avant: `[texte original]`
- Après: `[texte corrigé]`
- Raison: [explication courte]
```

### 5. Save or Simulate **[9]**

**IF `--dry-run`:**
- Show corrections list only
- NO file modification

**ELSE:**
1. **NE PAS créer de fichier .bak** — utiliser le changelog à la place
2. Apply corrections to original file
3. Append corrections to changelog: `.wip/changelog/chapitre<XX>-changelog.md`
4. Report statistics

**Changelog cumulatif :** Toutes les corrections appliquées sont ajoutées au fichier changelog du chapitre. Ce fichier accumule les entrées par itération, permettant de voir l'historique complet des modifications sans avoir à diff des fichiers complets.

```markdown
<!-- .wip/changelog/chapitre02-changelog.md -->

# Changelog — Chapitre 02

## Itération 1 (2026-02-14) — doctor quick

| # | Priorité | Ligne | Avant | Après | Raison |
|---|----------|-------|-------|-------|--------|
| 1 | 🔴 | 42 | `Bokujin` | `Sumujin` | Romanisation corrigée |
| 2 | 🟡 | 58 | `"texte"` | `« texte »` | Guillemets français |

---

## Itération 2 (2026-02-14) — doctor deep

| # | Priorité | Ligne | Avant | Après | Raison |
|---|----------|-------|-------|-------|--------|
| 1 | 🟢 | 103 | `[phrase longue]` | `[phrase scindée]` | Clarté |
```

## Output Format **[4-8]**

```markdown
# 🩺 Doctor Report

**File:** `[chemin]`
**Mode:** [quick/deep]
**Type:** [technical-doc/user-guide/api-doc/process-doc]

---

## 📊 Synthèse

| Priorité | Trouvés | Corrigés |
|----------|---------|----------|
| 🔴 Critical | X | X |
| 🟡 Important | X | X |
| 🟢 Optional | X | X |
| **Total** | X | X |

---

## ✏️ Corrections Appliquées

**#1 - 🔴 Orthographe** (L.42)
- Avant: `Le saidin est corrompu`
- Après: `Le *saidin* est corrompu`
- Raison: Terminologie client → correction glossaire

**#2 - 🟡 Typographie** (L.58)
- Avant: `"Cliquez sur Ok", dit le bouton.`
- Après: `« Cliquez sur Ok ».`
- Raison: Guillemets français + espace insécable

**#3 - 🟢 Style** (L.103)
- Avant: `[phrase 45 mots...]`
- Après: `[phrase scindée en 2...]`
- Raison: Clarté (phrase >40 mots)

---

## 📁 Fichiers

- Changelog: `.wip/changelog/chapitre01-changelog.md` (entrée ajoutée)
- Corrigé: `chapitres/chapitre01.md`

---

## 🎯 Prochaine Étape

Pour feedback qualitatif personas:
`comment.prompt.md chapitres/chapitre01.md clarity-expert`
```

## Règles Typographiques Françaises **[1-5]**

| Élément | Incorrect | Correct |
|---------|-----------|---------|
| Guillemets | "texte" | « texte » (avec espaces insécables) |
| Tiret dialogue | - Texte | — Texte (cadratin) |
| Points de suspension | ... | … (caractère unique) |
| Apostrophe | l'homme | l'homme (typographique, pas droit) |
| Espaces insécables | Avant : ; ! ? » | Après « Avant : ; ! ? » |

**Accents obligatoires:**
é è ê ë à â ù û ô î ï ç œ æ

## Quality Checklist **[Final]**

- [ ] Contexte chargé (bank.yml → output-style)
- [ ] Output-style respecté (ton, format, niveau de détail)
- [ ] Terminologie vérifiée (terminologie.md)
- [ ] Typographie française appliquée
- [ ] Remarques utilisateur traitées
- [ ] Redundancy scan inter-chapitres effectué (chapitres 02+)
- [ ] Changelog mis à jour (si non dry-run)
