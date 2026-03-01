---
name: init-project
description: Vérifie et initialise l'arborescence d'un projet documentation, migre le contenu existant et prépare les ressources (output-styles, personas, bank.yml)
argument-hint: Chemin vers le projet (ex: "acme-corp/api-documentation") [--integrity-check]
version: 1.5
changelog:
  - version: 1.5
    date: 2026-03-01
    changes:
      - "Mode --integrity-check : audit lecture seule exécutable en cours de cycle de vie"
      - "Step INT : vérifications croisées TOC↔chapitres, bank.yml↔disque, personas bank.yml↔disque, stubs vs vrais contenus, hygène .wip/"
      - "Guard Step 3 : bloc de questions sauté si --integrity-check"
changelog:
  - version: 1.4
    date: 2026-03-01
    changes:
      - "Step 1d : audit personas globaux déplacé ici (disponible pour Q4 dès Step 3)"
      - "Step 2 rapport : CONTENUS DÉTECTÉS affiche maintenant les personas globaux [RÉUTILISABLE]/[ABSENT]"
      - "Step 3C Q4 : référence corrigée 'étape 7a' → 'rapport Step 2 Personas globaux'"
      - "Step 7a : doublon personas globaux supprimé (déjà dans 1d) — table simplifiée"
      - "Step 7b condition : 'absent ou vide' → couvre les cas partiels (dossier existe, archétypes manquants)"
      - "Step 6a : dépendance 5b→6 documentée (5b sauté = sources non dispo = déléguer tone-finder)"
      - "Rapport 8b FAIT : 'extract-pdf.py lancé' → 'Commandes [SHELL] générées' (factuellement correct)"
      - "Rapport 8b À FAIRE : placeholders <id-persona>/<fichier1.pdf> → instructions pour substituer les vraies valeurs"
      - "Quality Checklist : ajout check personas A/B/C couverts"
  - version: 1.3
    date: 2026-03-01
    changes:
      - "Step 5b : extract-pdf.py marqué [SHELL] délégué utilisateur — l'IA génère les commandes prêtes avec les vrais noms de fichiers"
      - "Step 5c : marqué [exécutable en flux] — l'IA lit les .txt et écrit le glossaire directement"
      - "Section À FAIRE 8b : restructurée avec tags [USER]/[AI] et valeurs réelles substituées (plus de placeholders)"
  - version: 1.2
    date: 2026-03-01
    changes:
      - "[1] Step 1b : .claude/ → .output-styles/ (cohérence avec Step 6b surcharge projet)"
      - "[2] Step 8a : git commit déplacé de 'À FAIRE' vers fin Step 8a (exécution automatique)"
      - "[3] Quality Checklist : .gitignore ajouté"
      - "[4] Step 3B : Étape 8 bank.yml ajoutée dans le plan d'action affiché"
      - "[5] Step 3C Q4 : note ajoutée — personas globaux réutilisables rendent Q4 non nécessaire"
  - version: 1.1
    date: 2026-03-01
    changes:
      - "[1][2] Ajout git init, .gitignore (output/*.icml, indesign/, ascii-images/) et git commit final"
      - "[3] Chemin surcharge projet : .claude/output-style.md → .output-styles/<nom-projet>.md"
      - "[4] Heuristiques overview : spec/prd → cahier-des-charges/perimetre (termes français)"
      - "[5] Step 7a : personas globaux existants listés et marqués réutilisables si compatibles"
      - "[6] bank.yml : chapitres-order documenté avec exemple commenté"
      - "[7] Step 0 : .gitignore ajouté comme signal étape 4 déjà faite"
      - "[8] Step 3B : git init/gitignore visible dans le plan d'action affiché"
  - version: 1.0
    date: 2026-03-01
    changes:
      - "Version initiale adaptée au pipeline documentation multi-clients"
      - "Structure client/projet (vs univers/projet du projet parent JdR)"
      - "Types : technical-doc | user-guide | api-doc | process-doc"
      - "Personas documentation : technical-reviewer, clarity-expert, compliance-checker"
      - "Suppression : univers-extract, rules-keeper, terminologie.md, UNIVERS.md"
      - "Ajout : extract-pdf.py pour extraction sources PDFs clients"
---

# Init Project

## Goal

Auditer l'arborescence d'un projet documentation, détecter ce qui est déjà fait, initialiser ce qui manque, et préparer toutes les ressources (CLIENT.md, glossaire, output-styles, personas, bank.yml) en un seul passage sans interruptions inutiles.

## Context

Chemin du projet : `$ARGUMENTS` (sans `--integrity-check`)

Déterminer :
- `<client>` = premier segment du chemin (ex: `acme-corp`)
- `<projet>` = deuxième segment (ex: `api-documentation`)

## Mode

**Détecter `--integrity-check` dans `$ARGUMENTS` avant tout.**

| Mode | Comportement |
|------|-------------|
| *(normal)* | Audit → Questions (Step 3) → Création (Steps 4-8) |
| `--integrity-check` | Audit + vérifications croisées (Steps 0-2 + INT) → Rapport → **STOP. Aucune création.** |

**Mode `--integrity-check`** : peut être lancé à n'importe quel moment du cycle de vie du projet pour vérifier qu'aucune cohérence n'a été cassée (après ajout de chapitres, modifications manuelles, mise à jour bank.yml, etc.).

## Rules

- Ne pas écraser l'existant — créer uniquement ce qui manque
- Si le chemin est absent ou invalide : ABORT avec message d'erreur
- bank.yml n'est créé/finalisé qu'en étape 8, une seule fois
- Inline output-style si sources `.docs/sources/*.txt` disponibles ; déléguer à tone-finder sinon
- persona-trainer n'est PAS lancé pendant l'init (pas de feedbacks disponibles)
- Après la validation en step 3, les étapes 4-8 s'enchaînent sans interruption

---

## Steps

### 0. Détection d'État (Reprise)

Vérifier rapidement les outputs caractéristiques de chaque étape :

| Étape | Signal "déjà fait" |
|-------|--------------------|
| 4 — Structure | `bank.yml` existe ET `chapitres/` existe ET `.gitignore` existe [7] |
| 5 — Contenus | `overview.md` non vide |
| 5 — Sources PDF | `.docs/sources/` non vide ET au moins 1 fichier `.txt` extrait |
| 6 — Output-styles | `<client>/.output-styles/` non vide |
| 7 — Personas client | `<client>/.templates/personas/` non vide |
| 7 — Personas projet | `.templates/personas/` non vide |
| 8 — bank.yml finalisé | `bank.yml` contient des entrées `output-style` avec fichiers existants |

```
=== REPRISE : <client>/<projet> ===
  Étape 4 (structure)       : [FAIT / À FAIRE]
  Étape 5 (contenus)        : [FAIT / À FAIRE]
  Étape 6 (output-styles)   : [FAIT / À FAIRE]
  Étape 7 (personas)        : [FAIT / À FAIRE]
  Étape 8 (bank.yml final)  : [FAIT / À FAIRE]
→ Audit détaillé en cours...
```

Les étapes `[FAIT]` sont auditées mais leurs actions sautées, sauf si des éléments internes manquent.

---

### 1. Audit de l'Arborescence

Statuts : `[OK]` / `[MANQUANT]` / `[VIDE]`.

#### 1a. Niveau Client (`<client>/`)

| Chemin | Requis | Rôle |
|--------|--------|------|
| `<client>/.docs/CLIENT.md` | Obligatoire | Infos client, tone, audience cible |
| `<client>/.docs/glossaire.md` | Obligatoire | Termes métier, vocabulaire technique |
| `<client>/.docs/brand-guidelines.md` | Optionnel | Charte graphique, guidelines marque |
| `<client>/.output-styles/` | Obligatoire | Styles d'écriture adaptés au client |
| `<client>/.templates/personas/` | Recommandé | Personas review spécifiques au client |
| `<client>/.templates/` | Optionnel | Templates de contenu réutilisables |

#### 1b. Niveau Projet (`<client>/<projet>/`)

| Chemin | Requis | Rôle |
|--------|--------|------|
| `bank.yml` | Obligatoire | Configuration projet |
| `overview.md` | Obligatoire | Brief et périmètre du document |
| `chapitres/` | Obligatoire | Chapitres Markdown (source de vérité) |
| `output/` | Obligatoire | Exports ICML/PDF |
| `.docs/` | Obligatoire | Documentation support projet |
| `.docs/sources/` | Recommandé | PDFs sources et documents de référence client |
| `.toc/` | Obligatoire | Tables des matières (INDEX.md, toc-chapter*.md) |
| `.output-styles/` | Optionnel | Surcharge style pour ce projet spécifique [3] |
| `.templates/personas/` | Recommandé | Personas spécifiques à ce projet |
| `.wip/comments/` | Recommandé | Évaluations personas (comment.prompt.md) |
| `.wip/changelog/` | Recommandé | Changelogs corrections (doctor.prompt.md) |
| `.wip/reports/` | Recommandé | Rapports review-technical / tone-finder |

#### 1c. Audit de `bank.yml` (si présent)

| Champ | Obligatoire | Valeur attendue |
|-------|-------------|-----------------|
| `document.name` | Oui | Non vide |
| `document.client` | Oui | Correspond au dossier client |
| `document.type` | Oui | `technical-doc` / `user-guide` / `api-doc` / `process-doc` |
| `output-style.global` | Oui | Fichier existant sur disque |
| `docs.client` | Oui | Fichier CLIENT.md existant |
| `docs.glossaire` | Oui | Fichier glossaire.md existant |
| `overview` | Oui | Fichier existant |
| `toc.fichier` | Oui | Chemin déclaré |
| `personas` | Recommandé | Section présente |
| `icml` | Oui | Section configurée |

**Lire `document.type` depuis bank.yml si présent** — sera réutilisé en step 3 sans redemander.

#### 1d. Détection des Contenus Existants

Scanner simultanément pour alimenter les étapes suivantes.

**Sources overview** — fichiers `.md`/`.txt` à la racine ou dans `.docs/`, hors docs thématiques connus. Heuristique :
1. Nom contenant `overview`, `brief`, `pitch`, `scope`, `cahier-des-charges`, `perimetre` [4]
2. Première ligne = `# <Titre du projet>`
3. En cas d'égalité : fichier le plus long

Afficher : nom, taille, première ligne pour chaque candidat.

**Sources PDFs clients** — fichiers `.pdf` dans `.docs/sources/` ou `.docs/`. Lister ceux qui n'ont pas encore de fichier `.txt` extrait correspondant.

**Contenus déjà extraits** — fichiers `.txt` dans `.docs/sources/` (résultats d'`extract-pdf.py`).

**Personas globaux réutilisables** [1] — scanner `docs/templates/personas/`. Pour chaque fichier trouvé, noter `id` et `name`. Marquer `[RÉUTILISABLE]` ceux dont l'`id` correspond aux archétypes standard (`technical-reviewer`, `clarity-expert`, `compliance-checker`). Cette information sera utilisée à Step 3 Q4 pour savoir si la génération de personas client est nécessaire.

---

### 2. Rapport d'Audit

```
=== AUDIT : <client>/<projet> ===

NIVEAU CLIENT
  [OK/MANQUANT/VIDE]  <client>/.docs/CLIENT.md
  [OK/MANQUANT]       <client>/.docs/glossaire.md
  [OK/MANQUANT]       <client>/.docs/brand-guidelines.md    (optionnel)
  [OK/VIDE]           <client>/.output-styles/
  [OK/MANQUANT]       <client>/.templates/personas/

NIVEAU PROJET
  [OK/MANQUANT]  bank.yml               (document.type: <valeur ou "?">)
  [OK/MANQUANT]  overview.md            (N l.)
  [OK/VIDE]      chapitres/
  [OK/MANQUANT]  .docs/sources/
  [OK/MANQUANT]  .templates/personas/
  [OK/MANQUANT]  .wip/ + sous-dossiers
  ...

BANK.YML
  [OK/MANQUANT]  document.type = "<valeur>"
  [OK/MANQUANT]  output-style.global → ...
  [OK/MANQUANT]  docs.client, docs.glossaire
  ...

CONTENUS DÉTECTÉS
  Overview candidate  : <fichier> (N l.) — "<première ligne>"
  PDFs sources        : <liste ou "aucun">
  Extraits .txt       : <liste ou "aucun">
  Personas globaux    : [RÉUTILISABLE] <ids> / [ABSENT] <ids manquants>

RÉSUMÉ
  Obligatoires manquants : N
  Recommandés manquants  : N
  bank.yml incomplet     : oui/non
```

Si tout est déjà fait : `[OK] Projet entièrement initialisé. Suite : @docs/prompts/workshop/brainstorm.prompt.md $ARGUMENTS`

---

### INT. Vérifications d'Intégrité *(mode --integrity-check uniquement)*

> **Uniquement si `--integrity-check` détecté.** Sinon : passer directement à Step 3.

Vérifications croisées en lecture seule. Cinq domaines.

#### INT.1 — bank.yml ↔ Fichiers sur disque

Pour chaque chemin référencé dans bank.yml, vérifier que le fichier existe réellement :

| Champ bank.yml | Fichier attendu | Statut |
|----------------|-----------------|--------|
| `output-style.global` | `<chemin>` | [EXISTS / MANQUANT] |
| `output-style.projet` | `<chemin>` | [EXISTS / MANQUANT / NON DÉCLARÉ] |
| `docs.client` | `<chemin>` | [EXISTS / MANQUANT] |
| `docs.glossaire` | `<chemin>` | [EXISTS / MANQUANT] |
| `personas.global[*]` | `<chemin>` | [EXISTS / MANQUANT] |
| `personas.client[*]` | `<chemin>` | [EXISTS / MANQUANT] |
| `personas.projet[*]` | `<chemin>` | [EXISTS / MANQUANT] |
| `toc.fichier` | `<chemin>` | [EXISTS / MANQUANT] |

#### INT.2 — TOC ↔ Chapitres

Croiser `.toc/INDEX.md` avec le contenu de `chapitres/` :

- **Chapitres dans INDEX.md mais absents de `chapitres/`** → [ORPHAN-TOC]
- **Fichiers dans `chapitres/` mais absents de INDEX.md** → [ORPHAN-CHAPITRE]
- **Chapitres dans `bank.yml chapitres-order` mais absents de `chapitres/`** → [ORPHAN-BANK]
- **Fichiers `chapitres/*.md` vides (0 octets ou < 10 lignes)** → [VIDE]

#### INT.3 — Qualité des Contenus Clés

Distinguer stubs (créés par init-project) des vrais contenus :

| Fichier | Indicateur stub | Statut |
|---------|-----------------|--------|
| `CLIENT.md` | Contient `[À compléter]` ou moins de 30 lignes réelles | [STUB / OK] |
| `glossaire.md` | Contient `[Terme métier 1]` ou moins de 3 termes définis | [STUB / OK] |
| `overview.md` | Contient `[À compléter]` ou moins de 20 lignes réelles | [STUB / OK] |
| `<client>/.output-styles/*.md` | Contient `[extrait réel]` ou moins de 50 lignes | [STUB / OK] |

#### INT.4 — Hygiènes .wip/

- **Fichiers `.wip/comments/<chapitre>-*.md` sans correspondant dans `chapitres/`** → [ORPHAN-COMMENT] (chapitre supprimé ou renommé)
- **Fichiers `.wip/changelog/<chapitre>-*.md` sans correspondant dans `chapitres/`** → [ORPHAN-CHANGELOG]
- **Sous-dossiers `.wip/` manquants** (comments / changelog / reports) → [MANQUANT]

#### INT.5 — Cohérence bank.yml Interne

- `document.client` correspond au `<client>` déduit du chemin → [OK / INCOHÉRENT]
- `document.type` est une valeur valide (`technical-doc|user-guide|api-doc|process-doc`) → [OK / INVALIDE]
- `icml.chapitres-source` est un dossier existant → [OK / MANQUANT]
- `icml.output` a une extension `.icml` → [OK / INVALIDE]

#### INT — Rapport Final

```
=== INTEGRITY CHECK : <client>/<projet> ===
Date : YYYY-MM-DD

INT.1 — bank.yml ↔ disque
  [OK]       output-style.global    → <client>/.output-styles/technical-formal.md
  [MANQUANT] personas.client[0]     → <client>/.templates/personas/missing.yml
  ...

INT.2 — TOC ↔ chapitres
  [OK]          chapitre01.md, chapitre02.md, chapitre03.md
  [ORPHAN-TOC]  chapitre04.md  ← dans INDEX.md mais absent de chapitres/
  [ORPHAN-CHAPITRE]  annexeB.md  ← dans chapitres/ mais absent de INDEX.md
  [VIDE]        chapitre03.md  ← fichier présent mais < 10 lignes

INT.3 — Qualité contenus
  [STUB]  CLIENT.md       ← contient [À compléter]
  [OK]    glossaire.md    ← 12 termes définis
  [STUB]  overview.md     ← 8 lignes réelles

INT.4 — Hygiènes .wip/
  [OK]          .wip/comments/, .wip/changelog/, .wip/reports/
  [ORPHAN-COMMENT]  .wip/comments/chapitre04-personas.md  ← chapitre04 supprimé

INT.5 — Cohérence bank.yml
  [OK]          document.client = acme-corp
  [INVALIDE]    document.type = "tech-doc"  ← valeur non reconnue (→ technical-doc)

RÉSUMÉ
  Critiques  [rouge]  : N (MANQUANT fichiers référencés, valeurs invalides)
  Avertissements [orange] : N (STUB contenus, ORPHAN)
  OK [vert]  : N

RECOMMANDATIONS
  1. [CRITIQUE] Corriger document.type → "technical-doc" dans bank.yml
  2. [CRITIQUE] Créer ou corriger le chemin personas manquant
  3. [AVERT]    Compléter CLIENT.md et overview.md (stubs détectés)
  4. [AVERT]    Supprimer .wip/comments/chapitre04-personas.md (orphan)
  5. [AVERT]    Ajouter annexeB.md dans .toc/INDEX.md ou supprimer de chapitres/
```

**→ STOP. Mode --integrity-check : aucune modification effectuée.**

---

### 3. Bloc de Questions Unique

> **Skip si `--integrity-check` détecté** → rapport INT déjà affiché, fin du prompt.

**Toutes les décisions sont prises ici. Les étapes 4-8 s'enchaînent sans interruption.**

#### 3A — Type de document (si non connu)

Si `document.type` absent de bank.yml ou bank.yml manquant :

```
Q1 — Type de document ?
     → technical-doc   (spécifications, architecture, documentation technique)
     → user-guide      (guides utilisateur, manuels, tutoriels)
     → api-doc         (documentation API, endpoints, SDKs)
     → process-doc     (workflows métier, SOP, procédures internes)
     (conditionne output-styles et personas)
```

Sinon : utiliser la valeur lue à l'étape 1c, Q1 sautée.

#### 3B — Plan d'action

Afficher avec le type connu :

```
=== PLAN D'ACTION  (type: <type>) ===

ÉTAPE 4 — Structure
  Dossiers à créer      : [liste ou "aucun"]
  CLIENT.md stub        : [à créer (nouveau client) / déjà présent]
  glossaire.md stub     : [à créer / déjà présent]
  overview.md           : [créer vide / migrer depuis <fichier> / déjà OK]
  .gitignore            : [à créer / déjà présent] [8]
  git init              : [à lancer / déjà présent] [8]
  bank.yml              : sera finalisé à l'étape 8

ÉTAPE 5 — Contenus
  Overview ← <fichier>  : [oui / rien à migrer]
  extract-pdf.py        : [<PDFs détectés> / aucun]

ÉTAPE 6 — Output-Styles  (type: <type>)
  Styles client         : → <type-style> adapté au client (Q2 confirmera)
  Style projet          : → oui / non selon Q3

ÉTAPE 7 — Personas
  Client (proposition)  :
    A) technical-reviewer — exactitude technique, sécurité, cohérence architecture
    B) clarity-expert    — compréhensibilité, accessibilité, jargon, audience
    C) <profil déduit du type et du domaine client>
  Note : globaux [RÉUTILISABLE] (rapport Step 2) peuvent déjà couvrir A/B/C → Q4 = non
  Projet                : → selon Q5 (description audience)

ÉTAPE 8 — bank.yml
  Créer depuis template  : type=<type>, output-style, docs, icml [4]
```

#### 3C — Questions

```
Q2 — Style d'écriture à créer pour ce client ?
     (peut en combiner plusieurs selon types de docs produits)
     → technical-formal      (spécifications, architecture)
     → user-friendly         (guides utilisateur, manuels)
     → developer-docs        (API, SDK, quick start)
     → process-documentation (workflows, SOP)

Q3 — Style projet spécifique (surcharge du style client) ?
     → oui / non
     (utile si ce projet a des contraintes de format très spécifiques)

Q4 — Personas client à créer ?
     [Voir rapport Step 2 "Personas globaux" — si tous A/B/C [RÉUTILISABLE] → Q4 = non]
     Proposition (3 archétypes — modifier les descriptions si besoin) :
       A) technical-reviewer — exactitude technique, sécurité, cohérence, termes précis
       B) clarity-expert     — compréhensibilité, accessibilité, jargon adapté audience
       C) <Profil déduit du domaine> — ex: compliance-checker (conformité, exhaustivité)
          ou domain-expert (connaissances métier spécifiques au client)
     → oui (les 3 tels quels)
     → modifier: [décrire les changements souhaités]
     → non

Q5 — Personas projet pour <projet> ?
     → non
     → oui : <décrire l'audience en quelques mots>
             ex: "développeur découvrant l'API", "opérateur RH lisant les SOP"

→ Valider et lancer ? (oui / ajuster / non)
```

Si **non** : fin, aucune modification.
Si **ajuster** : corriger les items concernés, puis relancer la validation.
Si **oui** : exécuter les étapes 4-8 sans interruption.

---

### 4. Structure de Base

Créer les éléments manquants confirmés.

**Dossiers** : créer tous les dossiers manquants (vides), y compris `.docs/sources/` et `.wip/{comments,changelog,reports}`.

**`<client>/.docs/CLIENT.md`** (si client nouveau — dossier `<client>/.docs/` absent ou CLIENT.md manquant) :

```markdown
# Client : <Nom Client>

## Contexte
[À compléter — secteur d'activité, produit/service, positionnement]

## Audience Cible
- **Primaire :** [profil, niveau technique, objectif]
- **Secondaire :** [profil, niveau technique, objectif]

## Ton & Style
- [Ton souhaité : formel/accessible/technique/pédagogique]
- [Conventions spécifiques]
- [Ce qu'il faut éviter]

## Contraintes
- [Contrainte 1 : ex. pas de credentials réelles dans les exemples]
- [Contrainte 2]

## Contacts
- [Interlocuteur principal]
```

**`<client>/.docs/glossaire.md`** (si absent) :

```markdown
# Glossaire : <Nom Client>

| Terme | Définition | Notes |
|-------|-----------|-------|
| [Terme métier 1] | [Définition précise] | [Usage attendu dans les docs] |
| [Terme métier 2] | [Définition précise] | |

## Termes à Éviter

| Éviter | Préférer | Raison |
|--------|---------|--------|
| [terme ambigu] | [terme précis] | [explication] |
```

**`overview.md`** (si manquant et aucune source à migrer) :

```markdown
# [Titre du Document]

## Objectif
[À compléter — quel problème ce document résout-il ?]

## Audience
[À compléter — qui va lire ce document, avec quel niveau de connaissance ?]

## Périmètre
### Couvert
- [Sujet 1]

### Non couvert
- [Sujet exclu]

## Structure
[À définir — chapitres envisagés]

## Ton & Style
[Style : technical-formal / user-friendly / developer-docs / process-documentation]

## Livrables
- Format principal : [PDF / web / ICML]
```

**bank.yml** : ne pas créer ici — finalisé à l'étape 8.

**`.gitignore`** (si absent) [2] :
```
output/*.icml
output/ascii-images/
output/indesign/
```

**Git** [1] : si `.git/` absent dans le dossier projet : `git init && git add . && git commit -m "Init: <projet>"`.

---

### 5. Migration des Contenus

Chaque sous-étape est indépendante — évaluer séparément.

#### 5a. Overview

Si source candidate identifiée ET (`overview.md` absent ou vide) : extraire l'essence (objectif, audience, périmètre, structure) depuis le fichier source sélectionné par l'heuristique 1d. Intégrer dans `overview.md`. Ne pas supprimer le fichier source.

Sinon : sauter 5a.

#### 5b. Extraction PDFs Sources

**`extract-pdf.py` est une commande shell — non exécutable dans le flux.** L'IA liste les PDFs trouvés et génère les commandes prêtes à copier-coller.

Si PDFs clients détectés dans `.docs/sources/` sans fichier `.txt` extrait correspondant :

```
[SHELL — À exécuter par l'utilisateur]
python scripts/extract-pdf.py .docs/sources/mon-fichier.pdf --output .docs/sources/mon-fichier.txt
```

Substituer les noms réels détectés à l'étape 1d. L'extension `.pdf` devient `.txt` (ex: `rapport-2024.pdf` → `rapport-2024.txt`). Marquer en "À FAIRE [USER]" dans le rapport 8b.

Sinon : sauter 5b.

#### 5c. Enrichissement Glossaire (si extractions disponibles)

**Exécutable en flux** — l'IA lit les `.txt` extraits directement.

Si `.docs/sources/*.txt` existent (extractions PDF) ET glossaire.md est vide ou stub :

Scanner les extractions pour détecter des termes métier récurrents (majuscules, acronymes, formulations spécifiques au domaine). Écrire directement dans `<client>/.docs/glossaire.md` un glossaire initial avec les termes détectés, à compléter par le client.

Sinon : sauter 5c.

---

### 6. Output-Styles

#### 6a. Styles client

Pour chaque style confirmé (Q2) absent de `<client>/.output-styles/` :

**Convention de nommage :** `<type-style>.md` (ex: `technical-formal.md`, `user-friendly.md`)

**Sélection des sources selon le type :**

| Type Style | Sources prioritaires dans `.docs/sources/` |
|------------|--------------------------------------------|
| `technical-formal` | Spécifications, architecture existante, API docs, changelogs |
| `user-friendly` | Guides utilisateur existants, FAQs, manuels |
| `developer-docs` | READMEs, API references, quick start guides |
| `process-documentation` | SOP, procédures, workflows, runbooks |

Sélectionner 2-3 fichiers correspondants dans `.docs/sources/`. **Si les PDFs sources existent mais n'ont pas encore été extraits (5b sauté) : traiter comme "aucune source disponible" → déléguer à tone-finder.** Si aucune source disponible : déléguer à tone-finder.

**Si sources disponibles — écriture directe (sans validation intermédiaire) :**

Extraire depuis les sources :

| Élément | À analyser |
|---------|------------|
| Structure | Ratio prose/listes, usage des tableaux, découpe en sections |
| Ton | Registre (formel/courant), adresse au lecteur (vous/tu/on), présence d'exemples |
| Typographie | Guillemets, gras, italiques et leurs usages spécifiques |
| Termes clés | Termes avec capitalisation obligatoire, acronymes normalisés |
| Exemples | Extraire 2 passages réels représentatifs : un procédural, un descriptif |

Rédiger directement `<client>/.output-styles/<type-style>.md` :

```markdown
# Output Style: <Nom Client> — <Type Style>
**Source :** [fichiers analysés]
**Version :** 1.5

## Philosophie d'écriture
[Paragraphe synthétisant le style fondamental]

### Principes fondamentaux
1. **[Principe]** — [justification extraite des sources]

## Structure des sections
### Paragraphes / Listes / Tableaux / Encadrés
[Description + exemple réel extrait des sources]

## Conventions de formatage
### Typographie
### Termes à capitaliser / normaliser
### Éléments de code / interface
[Convention d'affichage des noms de boutons, menus, chemins]

## Bon usage / Mauvais usage
### Bon usage
[Extrait réel des sources, représentatif du style attendu]
### Mauvais usage
[Contre-exemple construit + liste des problèmes]

## Checklist
- [ ] [Critère spécifique issu de l'analyse — pas générique]
```

**Si aucune source disponible — déléguer :**
```bash
@docs/prompts/workshop/tone-finder.prompt.md <client> --only <type-style>
```

#### 6b. Style projet (surcharge)

Si Q3 = oui : créer `.output-styles/<nom-projet>.md` [3] :

```markdown
# Output Style: <Nom Projet> — Surcharge Projet
**Hérite de :** `<client>/.output-styles/<type-style>.md`
**Version :** 1.5

## Spécificités de ce Projet
[Ce qui rend ce projet stylistiquement distinct des autres docs du client]

## Règles Modifiées ou Précisées
[Uniquement les règles qui diffèrent — supprimer cette section si rien ne diffère]

## Conventions Supplémentaires
[Termes propres au projet, format spécifique, sujets sensibles, contraintes PAO]

## Checklist Supplémentaire
- [ ] [Critère spécifique à ce projet]
```

En l'absence de règle dans ce fichier, le style client s'applique intégralement.

---

### 7. Personas

#### 7a. Audit des trois niveaux

| Niveau | Chemin | Rôle |
|--------|--------|------|
| Global | `docs/templates/personas/` | Audité en Step 1d — résultats dans rapport Step 2 |
| Client | `<client>/.templates/personas/` | Personas spécifiques au client/domaine |
| Projet | `.templates/personas/` | Personas spécifiques à ce projet |

Lister les fichiers client et projet trouvés avec `id` et `name`. Les personas globaux réutilisables sont déjà connus depuis Step 1d.

#### 7b. Personas client

Si Q4 ≠ non et au moins un archétype A/B/C n'est pas couvert par un persona global `[RÉUTILISABLE]` ni déjà présent dans `<client>/.templates/personas/` :

Générer chaque archétype confirmé (avec les modifications saisies à Q4) via :

```bash
@docs/prompts/workshop/generate-persona.prompt.md "<description de l'archétype>"
```

Sauvegarder dans `<client>/.templates/personas/<id>.yml`.

**Descriptions à passer selon les archétypes :**

- **Archétype A (technical-reviewer)** : `"Expert technique vérifiant l'exactitude, la sécurité et la cohérence architecturale de la documentation [domaine client]. Vérifie que chaque assertion est testable, que les valeurs sont précises, que les exemples sont fonctionnels. Deal-breaker : assertion non vérifiable, exemple de code incorrect, valeur ambiguë."`

- **Archétype B (clarity-expert)** : `"Expert en accessibilité et compréhensibilité documentaire. Audience cible : [profil audience du client]. Vérifie que le jargon est défini au premier usage, que les procédures sont suivables sans ambiguïté, que le niveau de langue est adapté. Deal-breaker : terme non défini utilisé dans une procédure critique, étape avec plusieurs actions simultanées."`

- **Archétype C** :
  - Si domaine compliance détecté (réglementaire, certifications, audit) : `"Compliance checker vérifiant l'exhaustivité, la traçabilité et la conformité aux standards du domaine [domaine]. Vérifie que rien de critique n'est omis, que chaque exigence référencée est documentée."`
  - Si domaine technique très spécialisé : `"Expert domaine [domaine client] vérifiant la pertinence métier, l'absence d'approximations, la cohérence avec les pratiques du secteur."`
  - Sinon (fallback) : `"Utilisateur final découvrant l'outil/le processus sans formation préalable. Vérifie l'accessibilité, la courbe d'entrée, la clarté des prérequis."`

#### 7c. Personas projet

Si Q5 = non : sauter cette section.

Si Q5 = description fournie : générer via :

```bash
@docs/prompts/workshop/generate-persona.prompt.md "<description fournie en Q5>"
```

Sauvegarder dans `.templates/personas/<id>.yml`.

**persona-trainer n'est pas lancé ici.** Recommandé dans le rapport final après le premier cycle de review (comment.prompt.md).

---

### 8. Finalisation bank.yml + Rapport

#### 8a. bank.yml

**Si bank.yml n'existe pas — créer depuis le template :**

```yaml
document:
  name: "<nom — à compléter>"
  client: "<client>"
  type: "<type>"   # technical-doc | user-guide | api-doc | process-doc

output-style:
  global: "<client>/.output-styles/<type-style>.md"
  # projet: ".output-styles/<nom-projet>.md"  # si créé à l'étape 6b [3]

docs:
  client: "<client>/.docs/CLIENT.md"
  glossaire: "<client>/.docs/glossaire.md"
  # brand: "<client>/.docs/brand-guidelines.md"    # si présent
  # architecture: "<client>/.docs/architecture.md"  # si présent
  projet:
    # - ".docs/context.md"
    # - ".docs/document-rules.md"

overview: "overview.md"

toc:
  fichier: ".toc/INDEX.md"

personas:
  global:
    # Pré-remplir avec les IDs [RÉUTILISABLE] détectés en Step 1d, ex :
    # - "docs/templates/personas/technical-reviewer.yml"
    # - "docs/templates/personas/clarity-expert.yml"
  client:
    - "<client>/.templates/personas/<id>.yml"
    # ...
  projet:
    - ".templates/personas/<id>.yml"   # si créé à l'étape 7c

icml:
  chapitres-source: "chapitres/"
  chapitres-order: []   # [6] Laisser vide = ordre alphabétique. Exemple : ["chapitre01.md", "chapitre02.md", "annexeA.md"]
  output: "output/<nom-projet>.icml"

metadata:
  auteur: ""
  date-creation: "<YYYY-MM-DD>"
  version: "0.1"
  statut: "draft"
```

**Si bank.yml existe déjà — mettre à jour uniquement :**
- Section `output-style` : ajouter les clés créées à l'étape 6
- Section `personas` : ajouter les fichiers créés à l'étape 7
- Ne pas modifier le reste sauf champ vide ou placeholder

**Commit final [2] :** `git add -A && git commit -m "Init complet : output-styles, personas, bank.yml"`

#### 8b. Rapport Final

```
=== INITIALISATION TERMINÉE : <client>/<projet> ===

FAIT
  Structure     : chapitres/  output/  .docs/sources/  .toc/
                  .wip/{comments,changelog,reports}
  CLIENT.md     : [créé stub / déjà présent]
  glossaire.md  : [créé stub / déjà présent]
  overview.md   : [créé vide / migré depuis <fichier>]
  bank.yml      : [créé / mis à jour] (type: <type>)

  Contenus      : extract-pdf.py [Commandes [SHELL] générées pour <N> PDFs / ignoré]
                  glossaire     [enrichi depuis extractions / ignoré]

  Output-styles : <client>/.output-styles/<type-style>.md  v1.0  [inline / tone-finder]
                  .output-styles/<nom-projet>.md  [surcharge / absent] [3]

  Personas      : <client>/.templates/personas/ → [N] créés / [N] réutilisés depuis global [5]
                  .templates/personas/          → [N] créés via generate-persona

À FAIRE

  [USER] Compléter CLIENT.md (si stub) :
    Ouvrir : <client>/.docs/CLIENT.md
    → renseigner : audience, ton, contraintes

  [USER] Compléter glossaire.md (si stub) :
    Ouvrir : <client>/.docs/glossaire.md
    → valider / compléter les termes proposés

  [USER] Extraction PDFs sources (si PDFs détectés en Step 1d) :
    [Copier-coller les commandes générées en Step 5b avec les noms de fichiers réels]

  [USER] Compléter bank.yml :
    Ouvrir : bank.yml
    → document.name = "<titre réel>"
    → metadata.auteur = "<nom>"
    → icml.chapitres-order = [] ou ["chapitre01.md", ...]

  [AI — après 1er cycle de review] persona-trainer (substituer les IDs réels de Step 7) :
    @docs/prompts/workshop/persona-trainer.prompt.md <id-persona-généré-step7>
      --feedback-files ".wip/comments/*.md"
    [Répéter pour chaque persona créé en Step 7b/7c]

PROCHAINE ÉTAPE
  @docs/prompts/workshop/brainstorm.prompt.md $ARGUMENTS
```

## Quality Checklist

- [ ] Chemin `<client>/<projet>` valide et existant
- [ ] CLIENT.md présent (réel ou stub) dans `<client>/.docs/`
- [ ] glossaire.md présent (réel ou stub) dans `<client>/.docs/`
- [ ] overview.md non vide
- [ ] Au moins 1 output-style dans `<client>/.output-styles/`
- [ ] bank.yml complet (type, output-style, docs, icml)
- [ ] Personas client générés (ou existants)
- [ ] Au moins 1 persona disponible par archétype A/B/C (global réutilisable ou client généré)
- [ ] `.wip/` avec les 3 sous-dossiers créés
- [ ] `.gitignore` présent [3]
- [ ] Aucun fichier existant écrasé
