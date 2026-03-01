# Rules Keeper - Guide Complet

## Concept

Restructurer les fichiers de règles JdR pour une mémorisation rapide par les LLMs. Transforme des règles verboses en format optimisé avec cheatsheet, patterns d'actions, et templates d'entités.

```
règles brutes → CHEATSHEET + PATTERNS + ENTITY TEMPLATES + FULL REFERENCE
```

---

## Quand Utiliser

- Avant une session d'écriture avec un nouveau système de jeu
- Quand les fichiers de règles sont verbeux ou mal structurés
- Pour standardiser le format entre plusieurs fichiers de règles

**Input typique :** 500-5000 mots de règles brutes

---

## Invocation

```bash
# Fichier unique (cree automatiquement .original.md)
@docs/prompts/tools/rules-keeper.prompt.md docs/rules-files/pbta-simplifie.md

# Tous les fichiers
@docs/prompts/tools/rules-keeper.prompt.md --all

# Integrer un supplement dans un fichier existant
@docs/prompts/tools/rules-keeper.prompt.md --update base.md supplement.md
```

## Modes

| Mode | Usage | Description |
|------|-------|-------------|
| Default | `rules-file.md` | Restructure, cree `.original.md` |
| All | `--all` | Restructure tous les fichiers |
| Update | `--update base.md supplement.md` | Fusionne supplement dans base |

### Mode Update

Pour integrer des supplements, errata ou regles complementaires :

1. Lit le fichier de base (deja optimise) et le supplement
2. Classifie le contenu : nouvelles regles, modifications, extensions
3. Detecte les conflits et demande arbitrage
4. Fusionne avec changelog
5. Met a jour les templates si necessaire

---

## Structure de Sortie

```markdown
---
name: [system-name]
description: [one-line summary]
mechanics: [dice|cards|diceless|hybrid]
complexity: [light|medium|crunchy]
source-language: [fr|en]
validated: [true|false]
validated-date: [YYYY-MM-DD]
backup: [true|false]
---

# [System Name]

## CHEATSHEET
[Quick reference, < 500 tokens]

## LEXICON
[English → French term mapping for writing]

## PATTERNS
[Action templates: combat, skill, social]

## ENTITY TEMPLATES
[PC, NPC, obstacles, assets]

## FULL REFERENCE
[Detailed rules organized by category]
```

### Section LEXICON

Mapping des termes anglais (optimisés LLM) vers français (rédaction) :

```markdown
## LEXICON

### Core Terms
| English | Français | Context |
|---------|----------|---------|
| Roll | Jet / Lancer | "faire un jet de d100" |
| Success | Réussite | "réussite critique" |

### Characteristics
| English | Français | Abbreviation |
|---------|----------|--------------|
| Strength | Force | FOR |
```

Catégories recommandées :
- Core Terms (termes de base)
- Characteristics (caractéristiques)
- Health & Damage (santé et dégâts)
- Combat States (états de combat)
- Skills (compétences)
- System-specific terms (termes spécifiques au jeu)

---

## Sections Détaillées

### CHEATSHEET (< 500 tokens)

| Élément | Format | Max |
|---------|--------|-----|
| Core Loop | `[action] → [resolution] → [outcome]` | 1 ligne |
| Resolution | Table avec seuils | 5 rows |
| Key Modifiers | Liste à puces | 5 items |
| Character Elements | Liste à puces | 5 items |

### PATTERNS

Templates pour les types d'actions :

```
### Combat Actions
[Character] attacks [target] with [method].
→ Roll: [dice] + [modifier]
→ Success: [threshold]+
→ Effect: [damage/outcome]

### Skill Tests
[Character] attempts [action] using [skill/stat].
→ Roll: [dice] vs [difficulty]
→ Outcomes: [failure] / [partial] / [success]

### Social Encounters
[Character] tries to [persuade/deceive/intimidate] [NPC].
→ Leverage: [what they want/fear]
→ Roll: [relevant stat/skill]
→ Stakes: [on success] / [on failure]
```

### ENTITY TEMPLATES

Référence les templates personnalisables :

| Template | Fichier |
|----------|---------|
| Player Character | `docs/templates/rules-entities/player-character.template.md` |
| NPC | `docs/templates/rules-entities/npc.template.md` |
| Obstacle/Threat | `docs/templates/rules-entities/obstacle.template.md` |
| Asset/Item | `docs/templates/rules-entities/asset.template.md` |

### FULL REFERENCE

Organisation par catégorie :
- Character Creation
- Core Mechanics
- Advanced Rules
- GM Section

---

## Templates d'Entités

### Templates de Base

```
docs/templates/rules-entities/
├── README.md
├── player-character.template.md
├── npc.template.md
├── obstacle.template.md
└── asset.template.md
```

### Templates Adaptés (Output)

Les templates adaptés sont créés dans `.templates/` au même niveau que `.rules-files/` :

```
# Niveau univers
<univers>/.rules-files/system.md
→ <univers>/.templates/<system>-pc.template.md
→ <univers>/.templates/<system>-npc.template.md
→ <univers>/.templates/<system>-obstacle.template.md
→ <univers>/.templates/<system>-asset.template.md

# Niveau projet
<univers>/<projet>/.rules-files/system.md
→ <univers>/<projet>/.templates/<system>-pc.template.md
→ ...
```

### Personnalisation

Chaque template de base contient :
1. **Format** : Structure de base
2. **Placeholders** : Description de chaque champ
3. **Customization Notes** : Adaptations par système

Exemple d'adaptation pour PbtA :

```markdown
### Player Character
**[Name]** - [Playbook]
Stats: Cool: [+X], Hard: [+X], Hot: [+X], Sharp: [+X], Weird: [+X]
Moves: [move1], [move2]...
Gear: [item1], [item2]...
```

---

## Workflow Complet

1. **Read** : Lire le fichier de règles source
2. **Handle Errors** : Vérifier que c'est bien un fichier de règles
3. **Extract** : Identifier les mécaniques core
4. **Build CHEATSHEET** : Résumé < 500 tokens
5. **Build PATTERNS** : Templates d'actions
6. **Build ENTITY TEMPLATES** : Adapter les templates de base
7. **Build FULL REFERENCE** : Organiser le contenu détaillé
8. **Validate** : Vérifier la complétude
9. **Diff Preview** : Montrer les changements
10. **Confirm** : Demander validation
11. **Write** : Écrire le fichier mis à jour

---

## Validation Checklist

- [ ] Backup `.original.md` existe
- [ ] CHEATSHEET < 500 tokens
- [ ] LEXICON couvre tous les termes spécifiques
- [ ] Toutes les mécaniques de dés présentes
- [ ] Tous les seuils de succès/échec préservés
- [ ] Toutes les règles de modificateurs préservées
- [ ] Aucune mécanique ajoutée qui n'était pas dans la source
- [ ] Templates d'entités adaptés au système
- [ ] Fichiers templates créés dans `.templates/`
- [ ] Exemples autonomes (pas de référence externe)

---

## Règles Importantes

| Règle | Description |
|-------|-------------|
| Préserver | TOUTES les mécaniques, changer seulement la structure |
| Ne jamais ajouter | Mécaniques non présentes dans la source |
| Optimiser pour | Pattern matching LLM, pas lecture humaine |
| Limite CHEATSHEET | 500 tokens maximum |

---

## Fichiers de Règles Existants

```
docs/rules-files/
├── pbta-simplifie.md
├── zombiology-d100.md
└── ...
```

---

## Exemple d'Utilisation

### Input

```
When you do something risky, roll 2d6 + relevant stat.
On 10+ you succeed. On 7-9 you succeed but with a cost.
On 6- the GM makes a hard move.

Stats are: Cool, Hard, Hot, Sharp, Weird. Range from -1 to +3.
```

### Output (CHEATSHEET)

```markdown
### Core Loop
Describe action → Roll 2d6+stat → Apply outcome

### Resolution
| Roll  | Result  | Effect                    |
|-------|---------|---------------------------|
| 10+   | Success | Full success, no cost     |
| 7-9   | Partial | Success with complication |
| 6-    | Miss    | GM makes a hard move      |

### Key Modifiers
- Stats range: -1 to +3

### Character Elements
- Stats: Cool, Hard, Hot, Sharp, Weird
```

---

## Intégration avec Extraction

Après une extraction PDF, les règles sont placées dans `docs/rules-files/`. Utilisez rules-keeper pour les optimiser :

```bash
# 1. Extraction
python scripts/extract-pdf.py zombiology source.pdf

# 2. Optimisation des règles extraites
@docs/prompts/tools/rules-keeper.prompt.md docs/rules-files/source.md
```

---

## Dépannage

| Problème | Solution |
|----------|----------|
| Fichier pas reconnu comme règles | Vérifier présence de mécaniques (dés, seuils) |
| CHEATSHEET trop long | Réduire à 5 items max par section |
| Mécaniques manquantes | Relire la source, vérifier l'extraction |
| Templates inadaptés | Personnaliser les templates de base dans `docs/templates/rules-entities/` |
