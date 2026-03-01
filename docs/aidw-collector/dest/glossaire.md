# Glossaire — AIDW (generationPDF)

| Terme | Définition | Source |
|-------|-----------|--------|
| **AIDW** | AI Documentation Writing — nom de la méthode | `auto/methode/overview.md`, `CLAUDE.md` |
| **bank.yml** | Fichier de configuration projet — déclare toutes les ressources (docs, personas, output-style, TOC, icml) chargées par les prompts | `docs/templates/bank.yml`, tout projet |
| **ICML** | InDesign Content Markup Language — format de fichier lié InDesign, généré par `build-icml.py` | `scripts/build-icml.py` |
| **output-style** | Fichier Markdown définissant le ton, les conventions et le format d'un type de document | `docs/output-styles/`, `auto/.output-styles/` |
| **persona** | Profil YAML d'un lecteur humain fictif utilisé pour l'évaluation qualité des chapitres (score /20) | `docs/templates/personas/*.yml` |
| **prompt workshop** | Fichiers `.prompt.md` dans `docs/prompts/workshop/` — instructions structurées pour le LLM | `docs/prompts/workshop/` |
| **agent** | Fichiers `.md` dans `docs/agents/` — orchestrateurs de workflows multi-étapes | `docs/agents/` |
| **chapitre** | Unité structurelle de pagination PAO (`chapitres/chapitre<NN>.md`) — peut contenir tout type de contenu | Convention AIDW |
| **TOC** | Table des matières détaillée — INDEX.md + toc-chapterNN.md dans `.toc/` | `auto/methode/.toc/` |
| **toc-chapter** | Fiche détaillée d'un chapitre : sections, termes clés, ressources visuelles, notes d'écriture | `auto/methode/.toc/toc-chapter01.md` |
| **review-pipeline** | Cycle d'évaluation et correction : `comment` (personas) → triage → `doctor` ou `write --feedback` | `docs/agents/review-pipeline.md` |
| **doctor** | Prompt de corrections chirurgicales — applique les `remarks` produits par `comment` | `docs/prompts/workshop/doctor.prompt.md` |
| **PAO** | Publication Assistée par Ordinateur — mise en page InDesign | Convention métier |
| **client** | Répertoire de premier niveau (`cerascan/`, `auto/`) — regroupe tous les projets d'un même commanditaire | Structure dépôt |
| **overview.md** | Document source d'un projet — brief initial, use cases, structure suggérée | Tout projet AIDW |
| **aidw-collector** | Kit portable (à copier sur un projet cible) pour extraire toute l'information nécessaire à la documentation | `docs/aidw-collector/` |
| **collect.yml** | Config du collector — scope, extensions, fichiers à produire | `docs/aidw-collector/collect.yml` |
| **open-source-guide** | Output-style pour projets communauté — tutoiement, show/don't tell, différenciation concurrents | `auto/.output-styles/open-source-guide.md` |
| **dest/** | Répertoire de sortie du collector — fichiers prêts à copier dans `<client>/.docs/` | `docs/aidw-collector/dest/` |
| **wip** | Work In Progress — dossier `.wip/` contenant les artefacts de review (commentaires, changelogs, rapports) | `.wip/comments/`, `.wip/changelog/`, `.wip/reports/` |

## Termes à valider avec les stakeholders

- **`method-author`** : agent de vérification conformité — nom peut porter à confusion (semble être un persona alors que c'est un outil)
- **`coherence-checker`** vs. **`clarity-check`** : distinction pas toujours claire pour les nouveaux utilisateurs
- **`process-doc`** : type de document utilisé pour AIDW — est-ce le bon terme ou faudrait-il `method-doc` ?
