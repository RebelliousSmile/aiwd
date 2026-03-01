# Structure des Fichiers de Projet

Documentation de l'arborescence standard d'un projet d'écriture et du rôle de chaque fichier.

---

## Arborescence Type Complète

```
<univers>/<projet>/
│
├── .docs/                              # Documentation de support (canon)
│   ├── sources/                        # Documents de référence (PDFs, images)
│   │   └── <source>.pdf
│   │
│   ├── terminologie.md                 # Termes spécifiques au projet
│   ├── geographie.md                   # Lieux et cartes
│   ├── factions.md                     # Groupes et organisations
│   ├── personnages.md                  # PNJ principaux
│   ├── pj.md                           # Fiches des personnages joueurs
│   ├── creation-personnages.md         # Règles de création PJ
│   ├── document-rules.md               # Règles spécifiques au document
│   ├── scenarios-details.md            # Détails des scénarios (si campagne)
│   └── novels-details.md               # Détails des nouvelles (si recueil)
│
├── .toc/                               # Tables des matières détaillées
│   ├── INDEX.md                        # Index général
│   └── toc-chapter<NN>.md              # TOC par chapitre
│
├── .wip/                               # Artefacts review-pipeline (temporaires)
│   ├── comments/                       # Évaluations personas
│   ├── changelog/                      # Changelogs corrections
│   ├── reports/                        # Doctor + tone-finder reports
│   └── review-summary-<date>.md       # Rapport global
│
├── .git/                               # Dépôt Git (un par projet)
│
├── chapitres/                          # Contenu Markdown (source de vérité)
│   └── chapitre<NN>.md
│
├── output/                             # Fichiers ICML et PDFs générés
│   └── <projet>.icml
│
├── bank.yml                            # Configuration du projet
└── overview.md                         # Document source canon (SEUL fichier de contenu)
```

---

## Fichiers Canon (Racine)

### `overview.md`
**Rôle :** Document source unique contenant tout le contenu finalisé.

- **Seul fichier de contenu à la racine**
- Résultat final du processus créatif (brainstorm → toc → write → review)
- Source de vérité pour les chapitres
- Contenu selon type : synopsis, contexte, PNJ, actes, scènes, aides de jeu

**Cycle de vie :**
```
brainstorm.prompt → itérations → overview.md mature → generate-toc
```

### `bank.yml`
**Rôle :** Configuration du projet pour les prompts workshop.

```yaml
document:
  name: "Nom du projet"
  univers: "<univers>"
  type: "scenario"  # ou "novel", "roleplaying"

docs:
  terminologie: "<projet>/.docs/terminologie.md"
  geographie: "<projet>/.docs/geographie.md"
  factions: "<projet>/.docs/factions.md"
  personnages: "<projet>/.docs/personnages.md"
  projet:
    - ".docs/pj.md"
    - ".docs/document-rules.md"

overview: "overview.md"

toc:
  fichier: ".toc/INDEX.md"
```

---

## Dossier `.docs/`

Documentation de support spécifique au projet. Ces fichiers ne passent pas par le flux créatif — ils sont créés directement ou via extraction.

### Sous-dossier `sources/`

| Dossier | Contenu |
|---------|---------|
| `sources/` | PDFs et images de référence originaux |

### Fichiers Thématiques

| Fichier | Rôle | Origine |
|---------|------|---------|
| `terminologie.md` | Termes, abréviations, vocabulaire spécifique | `univers-extract` ou création |
| `geographie.md` | Lieux, cartes, descriptions spatiales | `univers-extract` ou création |
| `factions.md` | Groupes, organisations, hiérarchies | `univers-extract` ou création |
| `personnages.md` | PNJ principaux et secondaires | `univers-extract` ou création |
| `pj.md` | Fiches des personnages joueurs (prétirés) | Création |
| `creation-personnages.md` | Règles de création de PJ pour ce contexte | Création |
| `document-rules.md` | Règles spécifiques introduites par le document | `univers-extract`, `rules-keeper` ou création |
| `scenarios-details.md` | Détails de scénarios (si campagne multi-scénarios) | `brainstorm` |
| `novels-details.md` | Détails des nouvelles (si recueil multi-nouvelles) | `brainstorm` |

### Catégories d'Extraction (prompt `extract`)

Lors de l'extraction d'un PDF source, le contenu est distribué selon :

| Catégorie | Destination | Contenu |
|-----------|-------------|---------|
| **Lore** | `.docs/` (UNIVERS.md ou thématiques) | Contexte, PNJ, lieux, factions |
| **Terminologie** | `.docs/terminologie.md` | Termes, types, états, abréviations |
| **Règles** | `.rules-files/` (niveau univers) | Mécaniques, tables, tests |
| **Output-Style** | `.output-styles/` (niveau univers) | Ton, format, exemples narratifs |

---

## Dossier `.toc/` (Canon)

Tables des matières détaillées, utilisées pour l'écriture chapitre par chapitre. **Ce dossier fait partie du livrable.**

| Fichier | Rôle |
|---------|------|
| `INDEX.md` | Table des matières générale du projet |
| `toc-chapter<NN>.md` | TOC détaillée d'un chapitre spécifique |

**Généré par :** `generate-toc.prompt` puis `write-toc-chapter.prompt`

> **Note :** Contrairement aux fichiers de recherche, le contenu de `.toc/` est canon car il structure le document final et peut être référencé dans la documentation.

---

## Dossiers de Contenu

### `chapitres/`
Fichiers Markdown du contenu, un par chapitre. **Source de vérité.**

- Nommage : `chapitre01.md`, `chapitre02.md`, etc.
- Générés par `write-roleplaying.prompt` / `write-novel.prompt`

### `output/`
Fichiers ICML et PDFs générés.

- ICML généré par `build-icml.py`
- Ignoré par Git (PDFs)
- Publié via `publish-one.py`

---

## Dossier `.wip/` (Artefacts de Review)

Fichiers de travail générés par le review-pipeline. Ces artefacts sont des sous-produits de l'évaluation, **pas du canon**. Ils peuvent être supprimés après publication.

```
<projet>/.wip/
├── comments/                    # Évaluations personas (ex: chapitre01-personas.md)
├── changelog/                   # Changelogs corrections (ex: chapitre01-changelog.md)
├── reports/                     # Doctor reports + tone-finder reports
└── review-summary-<date>.md    # Rapport global batch
```

| Sous-dossier | Contenu | Généré par |
|-------------|---------|------------|
| `comments/` | Fichiers `<chapitre>-personas.md` — scores, issues, routing | `comment.prompt.md` |
| `changelog/` | Fichiers `<chapitre>-changelog.md` — historique cumulatif des corrections | `doctor.prompt.md` |
| `reports/` | `doctor-report-*.md`, `tone-finder-report-*.md` | `doctor`, `tone-finder` |
| racine | `review-summary-<date>.md` — rapport global de batch | `review-pipeline` |

---

## Fichiers de Travail (Temporaires)

Ces fichiers sont générés pendant le processus créatif et **n'ont pas vocation à rester**.

### `brainstorm.md` (temporaire)
**Rôle :** Support du prompt `brainstorm`.

- Idées brutes, exploration, options challengées
- Sert à construire/raffiner `overview.md`
- **À supprimer** une fois `overview.md` finalisé
- Le prompt `brainstorm` itère jusqu'à ce que l'overview soit mature

**Critères de suppression :**
- Tous les éléments obligatoires présents dans overview.md
- Plus de questions majeures en suspens
- Dernières itérations = ajustements mineurs

---

## Flux de Vie des Fichiers

```
1. SETUP
   bank.yml ──────────────────────────────────────→ (reste)

2. BRAINSTORM (itératif)
   brainstorm.prompt ←→ overview.md ──────────────→ (reste, source canon)
                    └→ brainstorm.md ─────────────→ (supprimé après)

3. EXTRACTION (si source PDF)
   extract.prompt → .docs/*.md ───────────────────→ (reste)
                 → <univers>/.rules-files/ ───────→ (reste)
                 → <univers>/.output-styles/ ─────→ (reste)

4. STRUCTURE
   generate-toc.prompt → .toc/INDEX.md ───────────→ (reste)
   write-toc-chapter.prompt → .toc/toc-chapter*.md → (reste)

5. SUPPORT (création manuelle)
   → .docs/pj.md ─────────────────────────────────→ (reste)
   → .docs/document-rules.md ─────────────────────→ (reste)

6. ÉCRITURE
   write-*.prompt → chapitres/*.md ───────────────→ (reste, source de vérité)

7. EXPORT
   build-icml.py → output/*.icml ─────────────────→ (reste, publié via InDesign)
```

---

## Déclaration dans `bank.yml`

Tous les fichiers `.docs/` doivent être référencés dans `bank.yml` :

```yaml
docs:
  # Fichiers thématiques (niveau projet)
  terminologie: "<projet>/.docs/terminologie.md"
  geographie: "<projet>/.docs/geographie.md"
  factions: "<projet>/.docs/factions.md"
  personnages: "<projet>/.docs/personnages.md"

  # Fichiers spécifiques au projet
  projet:
    - ".docs/pj.md"
    - ".docs/document-rules.md"
    - ".docs/creation-personnages.md"
```

---

## Bonnes Pratiques

1. **Un seul fichier canon** — Tout le contenu finalisé dans `overview.md`
2. **Support dans `.docs/`** — Documentation de jeu séparée du contenu narratif
3. **Sources dans `.docs/sources/`** — PDFs et images de référence
4. **Pas de fichiers de travail** — Supprimer `brainstorm.md` une fois le projet mature
5. **Déclaration exhaustive** — Tous les fichiers `.docs/` référencés dans `bank.yml`
6. **TOC dans `.toc/`** — Tables des matières détaillées séparées de la racine
7. **Git propre** — Ignorer `output/` (ICML et PDFs)

**Template bank.yml :** `docs/templates/bank.yml`
