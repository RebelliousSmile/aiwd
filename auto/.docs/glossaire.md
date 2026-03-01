# Glossaire — auto (generationPDF)

## Termes Système

| Terme | Définition |
|-------|-----------|
| **bank.yml** | Fichier de configuration projet — déclare toutes les ressources (docs, personas, output-style, TOC) chargées par les prompts |
| **ICML** | Format InDesign Content Markup Language — fichier lié dans InDesign, généré par `build-icml.py` |
| **output-style** | Fichier Markdown définissant le ton, les conventions et le format d'un type de document |
| **persona** | Définition YAML d'un lecteur fictif utilisé pour l'évaluation qualité des chapitres |
| **prompt workshop** | Fichiers `.prompt.md` dans `docs/prompts/workshop/` — instructions structurées pour Claude |
| **agent** | Fichiers dans `docs/agents/` — orchestrateurs de workflows multi-étapes |
| **chapitre** | Unité structurelle de pagination PAO (`chapitres/chapitre<NN>.md`) |
| **TOC** | Table des matières détaillée (`.toc/INDEX.md` + `toc-chapter<NN>.md`) |
| **review-pipeline** | Cycle d'évaluation et correction : comment → doctor (ou write --feedback) |
| **PAO** | Publication Assistée par Ordinateur — mise en page InDesign |
| **UNIVERS.md** | Ancien nom (JdR) → remplacé par `CLIENT.md` |
| **terminologie.md** | Ancien nom (JdR) → remplacé par `glossaire.md` |

## Abréviations

| Abréviation | Signification |
|-------------|--------------|
| JdR | Jeu de Rôle (legacy — ne plus utiliser dans doc-tech) |
| PAO | Publication Assistée par Ordinateur |
| ICML | InDesign Content Markup Language |
| TOC | Table Of Contents |
| WIP | Work In Progress |
