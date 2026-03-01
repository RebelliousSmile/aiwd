# Table des Matières : AIDW — AI Documentation Writing

**Type :** process-doc  
**Client :** auto  
**Document :** AIDW — AI Documentation Writing  

## Vue d'ensemble

AIDW est une méthode open source pour produire des documents professionnels avec l'aide d'un LLM. Le guide documente la méthode complète : installation, premier projet, prompts workshop, review pipeline, export ICML et personnalisation. Compatible tout LLM, tout OS. La structure est bilingue : README en anglais (vitrine GitHub), guide complet en français (référence opérationnelle).

## Thèmes & Objectifs

- Permettre à un tech de passer de zéro à un premier document exporté en moins d'une journée
- Documenter chaque composant de la méthode (prompts, personas, scripts, agents) avec exemples reproductibles
- Couvrir les cinq use cases : évaluation, premier projet, maîtrise, extension, maintenance
- Être agnostique LLM et cross-platform tout au long du guide

## Structure

- **Partie I — Démarrage :** Chapitres 01-03 — Comprendre, installer, produire
- **Partie II — La Méthode :** Chapitres 04-06 — Écrire, réviser, exporter
- **Partie III — Aller plus loin :** Chapitres 07-08 — Personnaliser, maintenir

---

## Chapitres

### Chapitre 01 : Introduction & Vue d'ensemble

**Relecteur TOC :** github-discoverer  
**Output-style :** open-source-guide.md

**Synopsis :** Présentation d'AIDW, sa valeur et ses cas d'usage. Ce chapitre répond immédiatement aux questions du lecteur sceptique : qu'est-ce que c'est, pourquoi pas juste ChatGPT, pour qui, et comment ça marche en un coup d'œil.

**Points clés :**
- [INTRO] AIDW (AI Documentation Writing) — définition, valeur, différenciation vs. prompt one-shot et GitHub Copilot
- [INTRO] Pipeline AIDW — Markdown → ICML → InDesign → PDF (vue macroscopique)
- Prérequis : Python 3.10+, Pandoc 3.0+, accès LLM, InDesign optionnel
- Compatibilité : tout LLM (Claude, GPT, Gemini, LLaMA…), tout OS (Windows, macOS, Linux)
- [INTRO] Architecture du dépôt — structure `<client>/<projet>/`, rôle de `bank.yml`
- Use cases couverts : évaluation, premier projet, maîtrise, extension, maintenance

---

### Chapitre 02 : Installation & Configuration

**Relecteur TOC :** github-discoverer  
**Output-style :** open-source-guide.md

**Synopsis :** Installation pas-à-pas de l'environnement AIDW. Couvre Python, Pandoc, le clone du dépôt et la vérification de l'installation. Cross-platform : commandes Windows, macOS et Linux documentées. Le lecteur repart avec un environnement fonctionnel.

**Points clés :**
- Installation Python 3.10+ (Windows/macOS/Linux) et vérification
- Installation Pandoc 3.0+ et vérification
- Clone du dépôt AIDW, installation des dépendances Python (`pip install -r requirements.txt`)
- [INTRO] Configuration LLM — principe agnostique, exemples avec interface web ou API
- Vérification globale : commande de test, output attendu
- Dépannage : erreurs courantes à l'installation (PATH, version Python, Pandoc introuvable)

---

### Chapitre 03 : Premier Projet

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Walkthrough complet du premier projet AIDW, de la création de la structure client jusqu'à l'export du premier fichier ICML. Chaque étape produit un résultat visible. À la fin de ce chapitre, le lecteur a un document exporté.

**Points clés :**
- [INTRO] Nouveau client — créer `<client>/.docs/CLIENT.md` et `glossaire.md`
- [INTRO] `bank.yml` — configuration minimale d'un projet (champs obligatoires, exemples réels)
- [INTRO] `overview.md` — document source, brief initial du projet
- Exécuter `generate-toc` : du brief à la table des matières
- Écrire le premier chapitre avec `write-technical` ou `write-user-guide`
- [INTRO] `build-icml.py` — premier export ICML (commande minimale, output attendu)
- Résultat attendu : fichier `.icml` prêt pour InDesign

---

### Chapitre 04 : Prompts Workshop — Écriture

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Référence complète des prompts d'écriture. Couvre le flux brainstorm → TOC → écriture → corrections pour les deux types principaux (documentation technique et guide utilisateur). Chaque prompt est documenté avec ses arguments, son output et ses cas d'usage.

**Points clés :**
- [REF Ch03] Flux d'écriture AIDW — brainstorm → generate-toc → write → doctor
- [INTRO] `brainstorm.prompt.md` — structurer un projet sans brief existant
- [INTRO] `generate-toc.prompt.md` — générer INDEX.md + toc-chapterNN depuis overview.md
- [INTRO] `write-technical.prompt.md` — écriture documentation technique (args, output-style, feedback)
- [INTRO] `write-user-guide.prompt.md` — écriture guide utilisateur accessible
- [INTRO] `doctor.prompt.md` — corrections chirurgicales (grammaire, typo, terminologie, remarks)
- Mode `--feedback` : réécriture informée depuis TOC + retour personas

---

### Chapitre 05 : Prompts Workshop — Review

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Pipeline de review complet. Couvre les personas (lecteurs humains), les outils de vérification (agents et prompts), et les cycles d'itération. Explique le triage : quand appliquer doctor, quand réécrire depuis la TOC.

**Points clés :**
- [INTRO] Personas — lecteurs humains vs. outils de vérification (distinction fondamentale)
- [INTRO] `comment.prompt.md` — évaluation d'un chapitre par un persona (score /20, remarks)
- [REF Ch04] `doctor.prompt.md` — corrections sur base des remarks personas
- [INTRO] `clarity-check.prompt.md` — vérification clarté pour audience cible
- [INTRO] `coherence-checker` (agent) — cohérence terminologique inter-chapitres
- [INTRO] Review pipeline — triage (doctor vs. rewrite --feedback, max 3 itérations)
- Scoring /20 : calibration, plafonds must-have et deal-breaker

---

### Chapitre 06 : Export & Scripts

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Référence complète du pipeline d'export. Couvre `build-icml.py` et ses options, les scripts utilitaires, et le workflow InDesign pour la mise en page finale. Chaque commande documentée avec output attendu.

**Points clés :**
- [REF Ch03] `build-icml.py` — toutes les options (`--no-concat`, `--open`, `--dry-run`, `--chapitres-dir`)
- Pipeline Pandoc → Lua filter → post-traitement ICML (vue technique)
- [INTRO] `extract-pdf.py` — extraction texte depuis PDFs sources
- [INTRO] `ascii-to-images.py` — conversion blocs ASCII art en PNG
- Autres scripts : `normalize-text.py`, `split-pdf.py`, `ocr-to-latex.py`
- Workflow InDesign : import ICML → maquette → export PDF haute définition
- Dépannage export : erreurs Pandoc, ICML malformé, encodage

---

### Chapitre 07 : Extension & Personnalisation

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Adapter AIDW à de nouveaux contextes. Ajouter un client, créer un output-style sur mesure, générer une nouvelle persona, étendre les agents. Ce chapitre s'adresse aux utilisateurs qui maîtrisent la méthode et veulent l'adapter.

**Points clés :**
- [REF Ch03] Nouveau client — structure complète `.docs/`, `.output-styles/`, `.templates/personas/`
- [INTRO] Créer un output-style — à partir du template global, adapter le ton et les conventions
- [INTRO] `generate-persona.prompt.md` — créer une persona lecteur (schéma YAML, critères, scoring)
- [INTRO] `persona-trainer.prompt.md` — calibrer une persona existante sur du feedback réel
- [REF Ch05] Distinction persona (lecteur) / agent (outil de vérification)
- Créer un agent custom — format frontmatter, steps, error handling
- [INTRO] `tone-finder.prompt.md` — extraire un output-style depuis des sources existantes

---

### Chapitre 08 : Maintenance

**Relecteur TOC :** pro-adopter  
**Output-style :** open-source-guide.md

**Synopsis :** Maintenir la méthode à jour et les projets en bonne santé. Couvre les outils d'audit (method-author, coherence-checker), la mise à jour des prompts et la gestion des versions avec Git.

**Points clés :**
- [INTRO] `method-author` (agent) — audit conformité AIDW d'un projet (fichiers, scripts, prompts)
- [REF Ch05] `coherence-checker` — audit régulier inter-chapitres en production
- [INTRO] `upgrade.prompt.md` — mettre à jour les prompts workshop vers une nouvelle version
- [INTRO] `tabula-rasa.prompt.md` — réinitialiser un projet (reset structure, purge .wip)
- Versionning Git — stratégie mono-repo vs. repo par projet
- Cycle de vie d'un projet : du premier commit au PDF final archivé

---

**Source :** `auto/methode/overview.md`  
**Généré :** 2026-03-01
