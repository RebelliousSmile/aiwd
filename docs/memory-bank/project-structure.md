# Structure des Fichiers de Projet — Doc-Tech Multi-Clients

Documentation de l'arborescence standard d'un projet documentation technique et du rôle de chaque fichier.

**Version:** 2.0 (migration JdR → doc-tech multi-clients, mars 2026)

---

## Arborescence Globale

```
generationPDF/
├── docs/                              # Documentation système (prompts, agents, templates)
├── scripts/                           # Scripts Python (build-icml, extract-pdf, etc.)
│
├── <client>/                          # Un dossier par client (ex: acme-corp)
│   ├── .docs/                         # Documentation client (partagée entre projets)
│   │   ├── CLIENT.md                  # Infos client, tone, audience cible
│   │   ├── glossaire.md               # Termes métier, vocabulaire technique
│   │   ├── brand-guidelines.md        # Charte graphique (optionnel)
│   │   └── architecture.md            # Architecture système (optionnel)
│   │
│   ├── .templates/                    # Templates spécifiques client
│   │   └── personas/                  # Personas review client
│   │       └── <id>.yml
│   │
│   ├── .output-styles/                # Styles d'écriture client
│   │   ├── technical-formal.md
│   │   └── user-friendly.md
│   │
│   └── <projet>/                      # Un dossier par projet/document
│       ├── .docs/                     # Documentation spécifique au projet
│       │   ├── sources/               # PDFs, images de référence
│       │   ├── context.md             # Contexte projet (optionnel)
│       │   └── document-rules.md      # Règles spécifiques au document (optionnel)
│       │
│       ├── .toc/                      # Tables des matières (structure canonique)
│       │   ├── INDEX.md               # Index général chapitres
│       │   └── toc-chapter<NN>.md     # TOC détaillée par chapitre
│       │
│       ├── .wip/                      # Artefacts review (temporaires)
│       │   ├── comments/              # Évaluations personas
│       │   ├── changelog/             # Changelogs corrections
│       │   └── reports/               # Review + tone-finder reports
│       │
│       ├── chapitres/                 # Source de vérité Markdown
│       │   └── chapitre<NN>.md        # Unités PAO (un fichier = une unité pagination)
│       │
│       ├── output/                    # Fichiers générés (gitignore sauf releases/)
│       │   ├── <projet>.icml          # Généré par build-icml.py
│       │   ├── ascii-images/          # PNG générés depuis ASCII art
│       │   ├── indesign/              # Sources maquette InDesign
│       │   └── releases/              # Exports PDF versionnés (trackés Git)
│       │
│       ├── bank.yml                   # Configuration projet (prompts)
│       └── overview.md                # Brief initial / document source
```


---

## Niveau Client : `<client>/`

### `.docs/` — Documentation client (obligatoire)

Partagée entre tous les projets du client. Chargée automatiquement par les prompts via `bank.yml`.

| Fichier | Rôle | Obligatoire |
|---------|------|-------------|
| `CLIENT.md` | Contexte client, audience cible, ton & style, contraintes | Oui |
| `glossaire.md` | Termes métier, vocabulaire technique, acronymes | Oui |
| `brand-guidelines.md` | Charte graphique, couleurs, typographie | Non |
| `architecture.md` | Architecture système, stack technique | Non (si doc technique) |

**Générés par :** `client-extract.prompt.md` ou complétés manuellement.

### `.output-styles/` — Styles d'écriture

Adaptés à la charte et au ton du client depuis les templates globaux `docs/output-styles/`.

| Style | Usage |
|-------|-------|
| `technical-formal.md` | Spécifications, architecture, docs API |
| `user-friendly.md` | Guides utilisateur, manuels |
| `developer-docs.md` | Documentation SDK, quick start |
| `process-documentation.md` | Workflows métier, SOP |

**Générés par :** `tone-finder.prompt.md` ou copiés/adaptés depuis `docs/output-styles/`.

### `.templates/personas/` — Personas review client

Personas spécifiques au client, surchargent les personas globaux.
Format : `<id>.yml` (voir `docs/templates/personas/` pour modèles).

---

## Niveau Projet : `<client>/<projet>/`

### `overview.md` — Brief projet

Document source initial : contexte, objectifs, audience, portée du document.

**Cycle de vie :**
`
brainstorm.prompt (itérations) -> overview.md enrichi -> generate-toc
`

### `bank.yml` — Configuration projet

Fichier de configuration chargé par tous les prompts workshop. Déclare toutes les ressources.

`yaml
document:
  name: "Guide Utilisateur API v2"
  client: "acme-corp"
  type: "user-guide"  # technical-doc | user-guide | api-doc | process-doc

output-style:
  global: "acme-corp/.output-styles/user-friendly.md"

docs:
  client: "acme-corp/.docs/CLIENT.md"
  glossaire: "acme-corp/.docs/glossaire.md"
  projet:
    - ".docs/document-rules.md"

toc:
  fichier: ".toc/INDEX.md"

personas:
  global:
    - "docs/templates/personas/technical-reviewer.yml"
    - "docs/templates/personas/clarity-expert.yml"
  client:
    - "acme-corp/.templates/personas/product-owner.yml"
  projet: []

overview: "overview.md"

icml:
  chapitres-source: "chapitres/"
  chapitres-order:
    - "chapitres/chapitre01.md"
    - "chapitres/chapitre02.md"
  output: "output/guide-utilisateur-api.icml"
`

**Template complet :** `docs/templates/bank.yml`

---

## Dossier `.docs/` (projet)

Documentation de support spécifique au projet.

| Fichier/Dossier | Rôle | Origine |
|-----------------|------|---------|
| `sources/` | PDFs, images de référence | Manuel ou `extract.prompt.md` |
| `context.md` | Contexte projet, historique | Création manuelle |
| `document-rules.md` | Règles propres à ce document | Création manuelle |

---

## Dossier `.toc/`

Tables des matières détaillées. Structure canonique du document.

| Fichier | Rôle | Généré par |
|---------|------|------------|
| `INDEX.md` | Index général, résumés chapitres | `generate-toc.prompt.md` |
| `toc-chapter<NN>.md` | TOC détaillée d'un chapitre | `write-toc-chapter.prompt.md` |

**Règle :** Les fichiers `.toc/` font partie du livrable. Ils ne sont pas temporaires.

---

## Dossier `chapitres/` — Source de vérité

Contenu Markdown final, organisé en unités PAO.

**Philosophie :** Un chapitre = unité structurelle de pagination (pas un genre littéraire).

**Généré par :** `write-technical.prompt.md`, `write-user-guide.prompt.md`

---

## Dossier `.wip/` — Artefacts de review

Sous-produits du review-pipeline. **Temporaires**, supprimables après publication.

| Sous-dossier | Contenu | Généré par |
|-------------|---------|------------|
| `comments/` | `chapitre<NN>-comment.md` — scores personas, issues, routing | `comment.prompt.md` |
| `changelog/` | `chapitre<NN>-v*.md` — historique corrections | `doctor.prompt.md` |
| `reports/` | `review-*.md`, `tone-finder-*.md` | `review-technical`, `tone-finder` |

---

## Dossier `output/`

Fichiers générés. La plupart ignorés par Git (sauf `releases/`).

| Contenu | Description | Git |
|---------|-------------|-----|
| `<projet>.icml` | Export InDesign, généré par `build-icml.py` | Ignore |
| `ascii-images/` | PNG depuis blocs ASCII art | Ignore |
| `indesign/` | Sources maquette InDesign (.indd) | Ignore |
| `releases/` | Exports PDF finaux, versionnés | Tracke |

---

## Flux de Vie des Fichiers

`
1. INITIALISATION
   init-project.prompt -> Structure + bank.yml

2. EXTRACTION SOURCES (si PDF client)
   client-extract.prompt -> <client>/.docs/CLIENT.md, glossaire.md
   tone-finder.prompt    -> <client>/.output-styles/

3. BRIEF
   brainstorm.prompt <-> overview.md

4. STRUCTURE
   generate-toc.prompt      -> .toc/INDEX.md
   write-toc-chapter.prompt -> .toc/toc-chapter*.md

5. ECRITURE
   write-technical.prompt   -> chapitres/*.md (source de verite)
   write-user-guide.prompt  -> chapitres/*.md (source de verite)

6. REVIEW (iteratif)
   review-technical.prompt  -> .wip/reports/     (temporaire)
   comment.prompt           -> .wip/comments/    (temporaire)
   doctor.prompt            -> chapitres/*.md (patch)
                            -> .wip/changelog/   (temporaire)

7. EXPORT
   build-icml.py -> output/<projet>.icml          (ignore Git)
   InDesign      -> output/releases/<version>.pdf  (tracke Git)
`

---

## Types de Documents

| Type | Description | Prompts principaux |
|------|-------------|-------------------|
| `technical-doc` | Specifications, architecture, documentation technique | `write-technical` |
| `user-guide` | Guides utilisateur, manuels, tutoriels | `write-user-guide` |
| `api-doc` | Documentation API, endpoints, SDKs | `write-technical` |
| `process-doc` | Workflows metier, SOP, procedures | `write-technical` |

---

## Bonnes Pratiques

1. **Isoler les clients** — Jamais de fichiers d'un client dans le dossier d'un autre
2. **bank.yml complet** — Toutes ressources declarees, commentaires sur les optionnels
3. **`.docs/` client vs projet** — CLIENT.md/glossaire au niveau client, contexte au niveau projet
4. **`.wip/` temporaire** — Supprimer apres publication ; ne pas committer dans les releases
5. **`releases/` tracke** — Seuls les PDFs finaux vont dans Git
6. **Un chapitre = unite PAO** — La granularite des `chapitres/*.md` suit la maquette InDesign

**Documentation connexe :**
- Pipeline ICML : `docs/memory-bank/indesign-workflow.md`
- Prompts disponibles : `docs/prompts/workshop/README.md`
- Template bank.yml : `docs/templates/bank.yml`
