# Déploiement — AIDW (generationPDF)

## Type de Déploiement

**Tooling local — pas de serveur, pas de déploiement cloud.** AIDW est un ensemble d'outils CLI locaux. L'utilisateur clone le dépôt et travaille directement.

## Environnements

| Env | Description | Particularités |
|-----|-------------|---------------|
| local | Machine du rédacteur | Environnement unique — pas de staging/prod |

## Prérequis Système

| Composant | Version minimale | Obligatoire | Notes |
|-----------|-----------------|-------------|-------|
| Python | 3.10+ | Oui | Pour tous les scripts |
| Pandoc | 3.0+ | Oui | Pipeline ICML principal |
| Node.js / pnpm | LTS | Non | Uniquement pour Mermaid CLI |
| InDesign | 2024+ | Non | Mise en page finale uniquement |
| Tesseract | 5.0+ | Non | Uniquement pour `ocr-to-latex.py` |
| Git | 2.x | Recommandé | Versionning du dépôt |

## Installation

```bash
# 1. Cloner le dépôt
git clone <url-depot> generationPDF
cd generationPDF

# 2. Installer les dépendances Python
pip install pyyaml pillow pdfplumber  # + pytesseract si OCR

# 3. Installer Pandoc (selon OS)
# Windows : https://pandoc.org/installing.html
# macOS   : brew install pandoc
# Linux   : apt install pandoc

# 4. Installer Mermaid CLI (optionnel — pour les diagrammes)
pnpm install

# 5. Vérifier
python scripts/build-icml.py --help
pandoc --version
```

## Variables d'Environnement

| Variable | Rôle | Obligatoire | Format attendu |
|----------|------|-------------|---------------|
| `PATH` | Doit inclure Python et Pandoc | Oui | Configuré par les installeurs |

**Pas de fichier `.env`** — aucune variable secrète requise. Le dépôt ne contient aucune clé API ou credential.

## Dépendances & Intégrations Externes

| Service / Outil | Rôle | Obligatoire | Notes |
|-----------------|------|-------------|-------|
| LLM (Claude, GPT, Gemini, LLaMA…) | Exécuter les prompts workshop | Oui | Agnostique — interface web ou API |
| InDesign 2024+ | Mise en page finale, export PDF | Non | ICML importé manuellement |
| GitHub | Hébergement dépôt, vitrine README | Non | Recommandé pour projets publics |
| Pandoc | Conversion Markdown → ICML | Oui | Installé localement |

## Dossiers Ignorés par Git

```
output/*.icml
output/ascii-images/
output/indesign/
node_modules/
scripts/_temp/
scripts/__pycache__/
```

Les PDFs finaux (`output/releases/`) et les chapitres Markdown sont **versionnés**.

## [À COMPLÉTER]

- Pas de CI/CD défini dans le dépôt (pas de `.github/workflows/` actif)
- Pas de script d'installation automatisé (`install.sh` / `install.ps1`)
- Pas de `requirements.txt` — **GAP identifié** : ajouter pour simplifier l'onboarding
