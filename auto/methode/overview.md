# AIDW — AI Documentation Writing

## Pitch

AIDW est une méthode open source pour produire des documents professionnels avec l'aide d'un LLM. Pipeline universel : Markdown → ICML → InDesign → PDF. Compatible avec tout LLM (Claude, GPT, Gemini, LLaMA…) et tout OS (Windows, macOS, Linux).

**Valeur :** [1] Structurer la collaboration humain-LLM sur des projets documentation longs, avec qualité reproductible et pipeline industrialisé. Là où un prompt one-shot produit un brouillon, AIDW produit un document structuré, relu par des personas, corrigé chirurgicalement, et exporté en fichier InDesign lié — prêt pour la mise en page professionnelle. [2] Là où GitHub Copilot génère du code, AIDW génère de la documentation avec une chaîne qualité complète (TOC → écriture → review → corrections → export).

## Audience Cible

- **Primaire :** Développeur ou rédacteur technique qui découvre le dépôt sur GitHub
- **Secondaire :** Contact professionnel intéressé par la méthode

**Profil :** À l'aise avec Git, Markdown, et les LLMs. Pas forcément familier avec InDesign. Cherche à structurer sa production de documentation avec de l'IA.

## Prérequis [4]

- Python 3.10+
- Pandoc 3.0+
- Accès à un LLM (API ou interface web — Claude, GPT, Gemini, LLaMA, etc.)
- InDesign 2024+ (optionnel — uniquement pour la mise en page finale)

## Périmètre

- Prompts workshop (écriture, review, extraction, maintenance)
- Templates (bank.yml, personas, output-styles)
- Agents autonomes (writing-pipeline, coherence-checker)
- Scripts Python (build-icml, extract-pdf, ascii-to-images…) — installation et usage [4]
- Workflows opérationnels (nouveau projet, review, export ICML)
- Installation et configuration (Python, Pandoc, LLM de son choix)

## Hors Périmètre

- La maquette InDesign (AIDW livre l'ICML ; la mise en page graphique reste à l'utilisateur)
- Le contenu des projets clients (AIDW documente la méthode, pas ce qu'on produit avec)
- Les configurations LLM spécifiques (on explique le principe, pas les réglages propres à chaque outil)

## Structure du Document [5]

**Bilingue EN/FR :**
- `README.md` en anglais — entry point GitHub (pitch, quick start, liens vers le guide)
- Chapitres en français — **guide complet de la méthode** (document principal)

Le README est la vitrine ; le guide FR est la référence complète.

## Use Cases Couverts

1. [3] **Évaluer AIDW** : comprendre en 10 minutes si la méthode correspond à son usage (README EN)
2. **Premier projet** : de zéro à un premier document ICML exporté
3. **Maîtrise du pipeline** : prompts, review personas, itérations qualité
4. **Extension** : ajouter un client, un output-style, une persona
5. **Maintenance** : integrity-check, coherence-checker, évolution des prompts

## Contraintes

- Agnostique LLM : aucun LLM imposé, exemples génériques
- Cross-platform : chemins Windows/macOS/Linux documentés
- Langue : README en anglais, guide complet en français
- Ton : communauté open source — direct, exemples concrets, pas corporate

## Livrables

- `README.md` (EN) — entrée GitHub
- Guide AIDW complet (FR) — chapitres exportés en ICML → PDF
