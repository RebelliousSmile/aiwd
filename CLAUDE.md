# Projet: Génération de Documents PAO (Multi-Clients)

## Vue d'Ensemble

Système de génération de documentation professionnelle pour clients multiples. Pipeline markdown → ICML → InDesign → PDF avec templates modulaires et documentation isolée par client.

**Cas d'usage :**
- Documentation technique (spécifications, architecture, API)
- Guides utilisateur (manuels, tutoriels, procédures)
- Documentation processus (workflows métier, SOP)
- Contenu narratif (romans, scénarios JdR) — legacy supporté

---

## Structure du Projet

```
generationPDF/
├── CLAUDE.md                          # Ce fichier
├── .claude/                           # Git config, settings
├── scripts/                           # Scripts Python (build ICML)
├── docs/
│   ├── prompts/workshop/              # Système de prompts
│   ├── templates/                     # Templates (bank.yml, personas)
│   ├── output-styles/                 # Styles de sortie globaux
│   │   ├── technical-formal.md        # Specs, architecture
│   │   ├── user-friendly.md           # Guides utilisateur
│   │   ├── developer-docs.md          # Docs API/SDK
│   │   └── process-documentation.md   # Workflows métier
│   └── memory-bank/                   # Détails techniques
│
├── <client>/                          # Clients (acme-corp, tech-solutions, etc.)
│   ├── .docs/                         # Documentation client
│   │   ├── CLIENT.md                  # Infos client, tone, audience
│   │   ├── glossaire.md               # Termes métier
│   │   └── brand-guidelines.md        # Charte graphique (optionnel)
│   ├── .templates/                    # Templates spécifiques client
│   ├── .output-styles/                # Styles de sortie client
│   │   ├── technical-formal.md
│   │   └── user-friendly.md
│   └── <projet>/                      # Projets documentation
│       ├── .docs/                     # Documentation projet
│       │   ├── pj.md                  # Personnages joueurs
│       │   ├── document-rules.md      # Règles spécifiques
│       │   └── sources/               # PDFs de référence
│       ├── .toc/                      # Tables des matières (canon)
│       │   ├── INDEX.md
│       │   └── toc-chapter<NN>.md
│       ├── .wip/                      # Artefacts review-pipeline (temporaires)
│       │   ├── comments/              # Évaluations personas
│       │   ├── changelog/             # Changelogs corrections
│       │   └── reports/               # Doctor + tone-finder reports
│       ├── chapitres/                 # Source de vérité (markdown)
│       ├── output/                    # Fichiers InDesign et exports
│       │   ├── *.icml                 # Générés par build-icml.py (gitignore)
│       │   ├── ascii-images/          # PNG ASCII art (gitignore)
│       │   ├── [assets]/              # Assets spécialisés : deck/, fiches/… (gitignore)
│       │   ├── indesign/              # Sources maquette InDesign (gitignore)
│       │   └── releases/              # Exports PDF finaux, versionnés (trackés)
│       ├── bank.yml                   # Config projet
│       ├── overview.md                # Document source (seul fichier de contenu)
│       └── .git/
```

**Structure détaillée des projets:** `docs/memory-bank/project-structure.md`

**Univers actifs:** `archipels`, `wot`, `ecryme`, `demonslayer`

---

## Philosophie : Chapitre = Unité PAO

**Principe fondamental :** Un chapitre est une **unité structurelle de pagination**, pas un genre littéraire.

- `chapitres/chapitre01.md` peut contenir :
  - Une section "Installation" (doc technique)
  - Un chapitre "Le Réveil" (roman)
  - Une procédure "Onboarding RH" (processus)
  - Une référence "API Endpoints" (doc développeur)

**Séparation des responsabilités :**
```
chapitres/          ← Structure physique (PAO/InDesign)
output-styles/      ← Conventions d'écriture (ton, format)
prompts/            ← Méthode de génération (processus)
personas/           ← Critères qualité (validation)
```

---

## Système de Prompts Workshop

Flux de travail pour l'écriture structurée.

### Flux Principal (Documentation Technique)

```
brainstorm → generate-toc → write-technical → doctor → build-icml
                          → write-user-guide → doctor → build-icml
```

**Pipeline complet (industrialisé) :**
```
.toc/toc-chapter<NN>.md → write-technical → doctor → chapitres/chapitre<NN>.md → build-icml.py → .icml
```

**Pipeline review (itératif) :**
```
chapitres/chapitre<NN>.md → comment (personas) → TRIAGE:
  - Issues patchables → doctor (corrections chirurgicales)
  - Issues structurelles → write-technical/user-guide --feedback (réécriture depuis TOC)
  - Patterns systémiques → tone-finder (output-style v+1)
→ re-comment → LOOP (max 3 itérations)
```

**Legacy (contenu narratif) :**
```
.toc/toc-chapter<NN>.md → write-novel/write-roleplaying → doctor → chapitres/chapitre<NN>.md → build-icml.py
```

### Prompts Disponibles

#### Documentation Technique (Primaire)

| Prompt | Usage |
|--------|-------|
| `brainstorm` | `@docs/prompts/workshop/brainstorm.prompt.md <projet>` |
| `generate-toc` | `@docs/prompts/workshop/generate-toc.prompt.md <source.md>` |
| `write-technical` | `@docs/prompts/workshop/write-technical.prompt.md <n> [--feedback <file>]` |
| `write-user-guide` | `@docs/prompts/workshop/write-user-guide.prompt.md <n> [--feedback <file>]` |
| `review-technical` | `@docs/prompts/workshop/review-technical.prompt.md chapitre<NN>.md` |
| `doctor` | `@docs/prompts/workshop/doctor.prompt.md chapitre<NN>.md` |
| `tone-finder` | `@docs/prompts/workshop/tone-finder.prompt.md <client> [sources]` |

#### Contenu Narratif (Legacy)

| Prompt | Usage |
|--------|-------|
| `write-novel` | `@docs/prompts/workshop/write-novel.prompt.md <n> [--feedback <file>]` |
| `write-roleplaying` | `@docs/prompts/workshop/write-roleplaying.prompt.md <n> [--feedback <file>]` |
| `review-chapter` | `@docs/prompts/workshop/review-chapter.prompt.md chapitre<NN>.md` |
| `review-roleplay` | `@docs/prompts/workshop/review-roleplay.prompt.md chapitre<NN>.md` |

#### Utilitaires

| Prompt | Usage |
|--------|-------|
| `research` | `@docs/prompts/workshop/research.prompt.md "sujet"` |
| `upgrade` | `@docs/prompts/workshop/upgrade.prompt.md` |
| `tabula-rasa` | `@docs/prompts/workshop/tabula-rasa.prompt.md <projet>` |

### Agents Autonomes

Agents orchestrant des workflows complets, exécutables en parallèle.

| Agent | Usage |
|-------|-------|
| `writing-pipeline` | `@docs/agents/writing-pipeline.md <chapitres> [--parallel]` |
| `memory-manager` | `@docs/agents/memory-manager.md <fichier>` |

**Exemple : écrire tous les chapitres en parallèle**
```bash
# Lance 6 agents simultanément pour chapitres 02-07
@docs/agents/writing-pipeline.md 2-7 --parallel
```

### Configuration: bank.yml

```yaml
document:
  name: "Documentation API v2"
  client: "acme-corp"
  type: "technical-doc"  # technical-doc | user-guide | api-doc | process-doc

output-style:
  global: "acme-corp/.output-styles/technical-formal.md"

docs:
  client: "acme-corp/.docs/CLIENT.md"
  glossaire: "acme-corp/.docs/glossaire.md"

toc:
  fichier: ".toc/INDEX.md"

icml:
  chapitres-source: "chapitres/"
  output: "output/api-documentation.icml"
```

**Documentation complète:** `docs/README.md`

**Types de documents :**
- `technical-doc` : Spécifications, architecture, documentation technique
- `user-guide` : Guides utilisateur, manuels, tutoriels
- `api-doc` : Documentation API, endpoints, SDKs
- `process-doc` : Workflows métier, SOP, procédures
- `novel` / `roleplaying` : Contenu narratif (legacy)

---

## Workflows Principaux

### Nouveau Projet Documentation

```bash
# 1. Créer structure client (si nouveau client)
mkdir <nom-client>
cd <nom-client>
mkdir .docs .output-styles

# Créer .docs/CLIENT.md, .docs/glossaire.md
# Copier output-styles depuis docs/output-styles/

# 2. Créer projet
mkdir <nom-projet>
cd <nom-projet>

# 3. Créer structure projet
mkdir chapitres output .docs .toc .wip

# 4. Créer bank.yml
cp ../../../docs/templates/bank.yml .
# Éditer avec les valeurs du projet

# 5. Créer overview.md (brief initial)

# 6. Git (optionnel)
git init && git add . && git commit -m "Initial commit"
```

**Structure des dossiers:**
- `chapitres/` = Source de vérité (markdown) — unités structurelles PAO
- `output/` = Fichiers ICML et PDFs générés
- `.toc/` = Tables des matières (structure canonique)
- `.wip/` = Artefacts review (comments, reports)

### Export ICML (pipeline unique)

```
chapitres/*.md ──→ build-icml.py ──→ .icml ──→ InDesign ──→ PDF HD
```

```bash
# Export ICML pour InDesign
python scripts/build-icml.py --project "<client>/<nom-projet>"

# Options
--project, -p         # Chemin du projet (obligatoire)
--output-name, -o     # Nom du ICML sans extension
--no-concat           # Un ICML par chapitre
--open                # Ouvrir après génération
--dry-run             # Simulation sans génération
--chapitres-dir DIR   # Override dossier chapitres (défaut: chapitres/)
```

**Documentation complète :** `docs/memory-bank/indesign-workflow.md`

**Pipeline complet :**
1. Markdown source (`chapitres/*.md`)
2. Conversion Pandoc → ICML via filtre Lua
3. Post-traitement : injection styles, couleurs, polices
4. Import InDesign → maquette graphique
5. Export PDF haute définition

---

## Documentation Client

Chaque client a sa documentation dans `<client>/.docs/`:

- `CLIENT.md` - Infos client, tone, audience cible (obligatoire)
- `glossaire.md` - Termes métier, vocabulaire technique (obligatoire)
- `brand-guidelines.md` - Charte graphique, guidelines marque (optionnel)
- `architecture.md` - Architecture système (si doc technique, optionnel)

**Exemple CLIENT.md :**
```markdown
# Client: ACME Corporation

## Contexte
Éditeur SaaS B2B, outils collaboration entreprise.

## Audience Cible
- **Primaire:** Développeurs intégrant l'API (niveau intermédiaire à expert)
- **Secondaire:** Admins système configurant l'infrastructure

## Ton & Style
- Technique mais accessible
- Éviter jargon marketing
- Code examples prioritaires (show, don't tell)
- Formats préférés: JSON, Python, cURL

## Contraintes
- Pas de credentials réelles dans exemples
- Toujours documenter breaking changes
- Rate limits obligatoires pour chaque endpoint
```

---

## Versionnage Git

**Recommandation :** Versionner tout le répertoire `generationPDF/` dans un seul repo Git.

**Avantages :**
- Historique complet des modifications (tous clients)
- Gestion simplifiée (un seul remote)
- Partage facile des scripts/templates entre projets

**Alternative :** Un repo Git par projet (dans `<client>/<projet>/.git/`) — supporté mais optionnel.

---

## Scripts Disponibles

| Script | Description |
|--------|-------------|
| `build-icml.py` | Convertit les chapitres Markdown en ICML (InDesign) via Pandoc |
| `ascii-to-images.py` | Convertit les blocs ASCII art en PNG pour le pipeline ICML |
| `generate-mockups.py` | Génère des mockups UI annotés (login, menu, popup) depuis `mockups.yml` |
| `extract-pdf.py` | Extrait le texte de PDFs sources pour analyse/intégration |
| `split-pdf.py` | Découpe un PDF en fichiers individuels par page |
| `ocr-to-latex.py` | Convertit des images/scans en texte via OCR (Tesseract) |
| `normalize-text.py` | Normalise l'encodage et les caractères d'un fichier |
| `coherence-extract.py` | Extrait des éléments cohérents entre chapitres |
| `generate-deck.py` | Génère des cartes de jeu (legacy JdR) |

**Documentation:** `scripts/README.md`

**Scripts supprimés (non pertinents pour doc technique) :**
- `git-sync.py` — versionner tout le repo en un seul remote
- `install-butler.py`, `publish-*.py`, `itch-checklist.py` — publication itch.io (legacy JdR)

---

## Règles d'Or

1. **Langue Française** - Toujours utiliser les accents et caractères spéciaux français (é, è, ê, ë, à, â, ù, û, ô, î, ï, ç, œ, æ, «», —, etc.) dans tous les fichiers générés
2. **Client Actif** - Clarifier le client et type de doc au début de session
3. **bank.yml** - Chaque projet doit avoir sa configuration complète
4. **Documentation Client** - Charger depuis `<client>/.docs/` (CLIENT.md, glossaire.md)
5. **Pipeline unique ICML** - `build-icml.py` est le seul export ; InDesign produit le PDF final
6. **Chapitre = Unité PAO** - Peu importe le contenu, un chapitre = unité structurelle de pagination
7. **Extraction PDF** - NE JAMAIS inventer de contenu, uniquement extraire ce qui est dans le document source
8. **Précision Technique** - Documentation technique = zéro ambiguïté, tout doit être testable/vérifiable

---

## Ajout Nouveau Client

1. Créer dossier: `<nouveau-client>/`
2. Docs obligatoires:
   - `<nouveau-client>/.docs/CLIENT.md` (infos client, tone, audience)
   - `<nouveau-client>/.docs/glossaire.md` (termes métier)
3. Output-styles: copier depuis `docs/output-styles/` vers `<nouveau-client>/.output-styles/`
   - Adapter selon besoins (charte client, ton spécifique)
4. Templates (optionnel): `<nouveau-client>/.templates/`

---

## Dépannage

### Pandoc non trouvé

```bash
# Vérifier installation
pandoc --version

# Windows: installer depuis pandoc.org
# Linux: apt install pandoc
# macOS: brew install pandoc
```

### Build ICML échoue

```bash
# Mode debug
python scripts/build-icml.py --project "<client>/<projet>" --dry-run

# Vérifier structure
ls chapitres/  # Doit contenir *.md
cat bank.yml   # Vérifier config icml.chapitres-source
```

### Caractères spéciaux mal encodés

```bash
# Normaliser encodage
python scripts/normalize-text.py <fichier>
```

---

## Préférences Globales

(Issues de `~/.claude/CLAUDE.md`)

- **Gestionnaire de paquets:** `pnpm` (au lieu de npm)
- **Checklist:** 1) Lire CLAUDE.md, 2) Vérifier docs univers, 3) Chercher dans code

---

## Bonnes Pratiques Scripts

**IMPORTANT:** Pas d'emojis dans les scripts PowerShell/Python - utiliser des préfixes texte: `[OK]`, `[ERROR]`, `[INFO]`.

---

## Output-Styles Disponibles

| Style | Usage | Audience | Ton |
|-------|-------|----------|-----|
| `technical-formal` | Spécifications, architecture, docs API | Développeurs, architectes | Objectif, précis, formel |
| `user-friendly` | Guides utilisateur, manuels, tutoriels | Utilisateurs finaux (tous niveaux) | Accessible, pédagogique, encourageant |
| `developer-docs` | Documentation SDK, quick start, intégrations | Développeurs pratiques | Code-first, exemples concrets |
| `process-documentation` | Workflows métier, SOP, procédures internes | Équipes opérationnelles | Directif, action-oriented |

**Localisation :**
- Globaux : `docs/output-styles/`
- Par client : `<client>/.output-styles/` (adaptés selon charte)

---

## Personas Review Disponibles

### Documentation Technique

| Persona | Focus | Sévérité |
|---------|-------|----------|
| `technical-reviewer` | Exactitude technique, sécurité, cohérence architecture | Stricte (bloque si erreurs critiques) |
| `clarity-expert` | Compréhensibilité, accessibilité, jargon | Modérée (bloque si incompréhensible) |
| `compliance-checker` | Exhaustivité, conformité style, traçabilité TOC | Stricte (bloque si gaps critiques) |

### Contenu Narratif (Legacy)

| Persona | Focus | Sévérité |
|---------|-------|----------|
| `casual-reader` | Engagement, fluidité, immersion | Modérée |
| `mj-curieux` | Utilisabilité JdR, clarté mécanique | Stricte |
| `fan-wot` | Cohérence univers, respect canon | Modérée |

**Localisation :** `docs/templates/personas/`

---

**Version:** 6.0
**Date:** 2026-02-28
**Changelog 6.0:** Adaptation pour documentation technique multi-clients. Nouveaux prompts (write-technical, write-user-guide, review-technical). Nouveaux output-styles (4 types docs). Nouveaux personas (technical-reviewer, clarity-expert, compliance-checker). Suppression git-sync, scripts itch.io. Philosophie unifiée : chapitre = unité PAO (contenu agnostique). Legacy JdR/romans supporté.
**Changelog 5.0:** Migration vers pipeline unique ICML/InDesign. Suppression LaTeX (latex-partials/, main.tex, .sty, md-to-tex, build-pdf, export-sty-specs). Pipeline unique : chapitres/*.md → build-icml.py → .icml → InDesign → PDF HD.
**Changelog 4.0:** Mode --feedback pour write-novel/write-roleplaying (réécriture informée depuis TOC + feedback personas). review-pipeline v4.0 (triage doctor vs rewrite). Pipeline review documenté dans Flux Principal.
