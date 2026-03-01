# Output Style: Open Source Guide

**Usage:** Guides de méthode open source, documentation communauté, README GitHub  
**Audience:** Développeurs et tech découvrant le projet sur GitHub ou par recommandation  
**Ton:** Direct, pragmatique, communauté — ni corporate, ni académique

---

## Principes Généraux

1. **Show, don't tell** — Une commande vaut mille mots
2. **Sceptique bienvenu** — Anticiper "pourquoi pas juste X ?" avant que le lecteur parte
3. **Zéro jargon maison non défini** — Tout terme interne expliqué à la 1ère occurrence
4. **Reproductible sans contexte** — Le lecteur suit sans avoir lu autre chose avant
5. **Bilingue assumé** — Structure EN pour GitHub, contenu FR pour le guide détaillé

---

## Structure Type

1. **Accroche** (3 lignes max) : Quoi, pour qui, différenciation immédiate
2. **Quick start** : Commandes réelles, résultat visible en <5 min
3. **Concepts clés** : Vocabulaire propre à la méthode, expliqué dès la 1ère mention
4. **Guide pas-à-pas** : Un objectif par section, commandes avant explications
5. **Référence** : Options, paramètres, fichiers de config — exhaustif
6. **Troubleshooting** : Erreurs courantes identifiées par le message exact

---

## Format Commandes

### Conventions

```bash
# [OK] — Chemin réel, paramètres réels
python scripts/build-icml.py --project "acme-corp/api-docs" --dry-run

# [KO] — Abstrait, inutilisable
python build.py --project <votre-projet>
```

### Structure d'un bloc de commandes

```bash
# 1. Contexte (ce que fait la commande)
python scripts/build-icml.py --project "acme-corp/api-docs"

# Output attendu :
# [INFO] Chapitres trouvés : chapitre01.md, chapitre02.md
# [OK] ICML généré : output/api-docs.icml
```

### Chemins de fichiers

- Toujours relatifs à la racine du projet
- Jamais de `~` ou de chemins absolus dans les exemples
- Format POSIX dans les exemples (`scripts/build-icml.py`), note Windows si nécessaire

---

## Ton

- **Tutoiement** dans les guides pratiques — c'est une communauté, pas un corporate
- **Vouvoiement** absent — même dans les sections formelles
- **Phrases courtes** — max 20 mots, une idée par phrase
- **Pas de formules creuses** : éviter "grâce à notre solution innovante", "de façon optimale"
- **Honnêteté sur les limites** — mentionner ce qui est hors périmètre, les prérequis réels

---

## Bilingue EN/FR

### Point d'entrée GitHub (EN)

```markdown
# AIDW — AI Documentation Writing

A structured workflow for producing professional documentation with LLMs.

> Works with any LLM. Requires Python 3.10+ and Pandoc 3.0+.

**Quick start** · **Guide (FR)** · **Contributing**
```

### Contenu du guide (FR)

- Chapitres rédigés en français
- Termes techniques anglais conservés tels quels (`bank.yml`, `build-icml.py`)
- Traduction des concepts propres à la méthode en français

---

## Différenciation — Répondre aux objections implicites

Anticiper dans l'introduction :

| Objection | Réponse attendue |
|-----------|-----------------|
| "Pourquoi pas juste ChatGPT ?" | Structure reproductible, versionnée, multi-doc |
| "C'est compliqué à setup ?" | Quick start en 5 minutes |
| "Ça marche avec quel LLM ?" | Tous — agnostique par design |

---

## Anti-patterns à éviter

- ❌ Sections sans exemple concret
- ❌ Workflow décrit sans commande associée
- ❌ Prérequis mentionnés en milieu de guide (toujours en début)
- ❌ Ton corporate ("dans le cadre de cette documentation")
- ❌ Screenshots sans texte alternatif descriptif
- ❌ Exemple avec données inventées non réalistes (`client: "my-client"`)

---

**Version:** 1.0  
**Date:** 2026-03-01  
**Projet:** AIDW — AI Documentation Writing
