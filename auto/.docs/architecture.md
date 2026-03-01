# Architecture — AIDW (generationPDF)

## Stack Technique

| Couche | Technologie | Version |
|--------|-------------|---------|
| Scripting | Python | 3.10+ (requis) |
| Conversion docs | Pandoc | 3.0+ (requis) |
| Filtre Pandoc | Lua | intégré Pandoc |
| Bibliothèques Python | PyYAML, Pillow (PIL) | latest (optionnel pour PIL) |
| Rendu diagrammes | Mermaid CLI (`mmdc`) | 11.4.2 via pnpm |
| Gestion paquets Node | pnpm | latest |
| Sortie finale | InDesign | 2024+ (optionnel) |

## Pattern Architectural

**Pipeline linéaire par étapes** (pas de framework MVC, pas de serveur, pas d'UI) :

```
Markdown (chapitres/) → Pandoc + Lua filter → ICML → InDesign → PDF
```

Chaque composant est indépendant et interchangeable :
- Les prompts sont des fichiers `.prompt.md` (pas de code)
- Les scripts Python sont des outils CLI autonomes
- La configuration est déclarative (`bank.yml`)
- Les templates sont des fichiers Markdown ou YAML

## Modules Principaux

| Module | Responsabilité | Dossier |
|--------|---------------|---------|
| Scripts Python | Conversion, extraction, génération | `scripts/` |
| Prompts Workshop | Instructions LLM structurées | `docs/prompts/workshop/` |
| Agents | Workflows multi-étapes | `docs/agents/` |
| Templates | bank.yml, personas, output-styles | `docs/templates/` |
| Output Styles | Conventions d'écriture par type | `docs/output-styles/`, `<client>/.output-styles/` |
| Personas | Profils lecteurs pour review | `docs/templates/personas/` |
| Collector | Kit extraction info projet | `docs/aidw-collector/` |
| Pandoc filter | Filtre Lua pour ICML | `scripts/pandoc/` |

## Scripts Python — Détail

| Script | Arguments clés | Description |
|--------|---------------|-------------|
| `build-icml.py` | `--project`, `--output-name`, `--no-concat`, `--open`, `--dry-run`, `--chapitres-dir` | Pipeline principal Markdown → ICML via Pandoc |
| `extract-pdf.py` | `<fichier.pdf>` | Extraction texte depuis PDFs sources |
| `ascii-to-images.py` | `<fichier.md>` | Blocs ASCII art → PNG (pour ICML) |
| `import-screenshot.py` | `<image>` | Normalise et place une capture au bon endroit |
| `generate-mockups.py` | `<config>` | Génère des mockups interface depuis spécifications |
| `normalize-text.py` | `<fichier>` | Normalise l'encodage et les caractères spéciaux |
| `split-pdf.py` | `<fichier.pdf>` | Découpe un PDF en pages individuelles |
| `ocr-to-latex.py` | `<image>` | OCR image → texte (Tesseract) |
| `coherence-extract.py` | `<projet>` | Extrait éléments cohérents inter-chapitres |

## Flux de Données Principal

```
overview.md + bank.yml
    ↓ [LLM + generate-toc]
.toc/INDEX.md + toc-chapterNN.md
    ↓ [LLM + write-technical/write-user-guide]
chapitres/chapitreNN.md
    ↓ [LLM + comment → doctor]
chapitres/chapitreNN.md (corrigé)
    ↓ [build-icml.py + Pandoc + Lua]
output/*.icml
    ↓ [InDesign manuel]
output/releases/*.pdf
```

## Modèles de Données Clés

Pas de base de données. Les "données" sont des fichiers texte avec conventions strictes.

| Entité | Format | Champs importants | Notes |
|--------|--------|-------------------|-------|
| `bank.yml` | YAML | `document`, `output-style`, `docs`, `toc`, `personas`, `icml` | Config racine de tout projet |
| Persona | YAML | `name`, `profile`, `expectations.must_have`, `deal_breakers`, `criteria` (weights /1.0), `scoring_calibration` | Score /20 |
| Output-style | Markdown | Sections : principes, format, ton, anti-patterns | Chargé par les prompts d'écriture |
| Chapitre | Markdown | Headers H1-H4, tableaux, blocs code, ASCII art | Unité de pagination PAO |
| toc-chapter | Markdown | Sections, termes clés, ressources visuelles, notes écriture | Entrée pour write-technical |

## Dépendances Python

```
# Détectées dans build-icml.py et autres scripts
PyYAML       → lecture bank.yml
Pillow       → traitement images (ascii-to-images.py) — optionnel
pytesseract  → OCR (ocr-to-latex.py) — optionnel
pdfplumber / PyMuPDF → extraction PDF (extract-pdf.py)
```

Pas de `requirements.txt` dans le dépôt — **[GAP à corriger]** : les dépendances sont documentées dans `scripts/README.md` mais sans fichier pip standard.
