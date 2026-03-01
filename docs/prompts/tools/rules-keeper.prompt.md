---
name: rules-keeper
description: Restructure rules files for LLM optimization
argument-hint: [rules-file.md | --all | --update rules-file.md supplement.md]
---

# Rules Keeper Prompt

## Goal

Restructure rules file(s) into optimized format for rapid LLM recall and consistent application.

## When to Use

- Before starting a writing session using a new game system
- When rules files are verbose or poorly structured
- To standardize format across multiple rules files
- To integrate supplements, errata, or complementary rules (`--update`)

**Typical input:** 500-5000 words of raw game rules

**Full documentation:** `docs/memory-bank/rules-keeper-workflow.md`
## Modes

| Mode | Usage | Description |
|------|-------|-------------|
| Default | `rules-file.md` | Restructure single file, create `.original.md` backup |
| All | `--all` | Restructure all files in `docs/rules-files/` |
| Update | `--update base.md supplement.md` | Merge supplement into existing optimized file |
| Local | `--local <projet>` | Génère `<projet>/.docs/document-rules.md` pour les règles spécifiques au document |

## Context

### Target File(s)

**Single file:**
```
docs/rules-files/<filename>.md
```
Example: `docs/rules-files/pbta-simplifie.md`

**All files (--all flag):**
```
docs/rules-files/*.md
```

**Reference:** See existing optimized files in `docs/rules-files/` for format examples.

### Entity Templates

**Base templates:** `docs/templates/rules-entities/`

| Template | File | Usage |
|----------|------|-------|
| Player Character | `player-character.template.md` | PC stat blocks |
| NPC | `npc.template.md` | NPCs with drives/weaknesses |
| Obstacle/Threat | `obstacle.template.md` | Challenges and dangers |
| Asset/Item | `asset.template.md` | Equipment and resources |

**Output location:** Templates are created in `.templates/` at the same level as `.rules-files/`:

```
# If rules file is at univers level:
<univers>/.rules-files/system.md
→ <univers>/.templates/<system>-pc.template.md
→ <univers>/.templates/<system>-npc.template.md
→ <univers>/.templates/<system>-obstacle.template.md
→ <univers>/.templates/<system>-asset.template.md

# If rules file is at project level:
<univers>/<projet>/.rules-files/system.md
→ <univers>/<projet>/.templates/<system>-pc.template.md
→ ...
```

## Rules

- Preserve ALL game mechanics, change only structure
- Create sections: CHEATSHEET + LEXICON + PATTERNS + ENTITY TEMPLATES + FULL REFERENCE + CHANGELOG
- Use consistent patterns across all rules files
- Optimize for pattern matching, not human reading
- Maximum 500 tokens for CHEATSHEET section
- NEVER add mechanics not present in the source

## Output Format

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

### Core Loop
[action] → [resolution] → [outcome]

### Resolution
| Roll | Result | Effect |
|------|--------|--------|
| [X]  | [name] | [what happens] |

### Key Modifiers
- [condition]: [modifier]

### Character Elements
- [element]: [format]

## LEXICON

> English terms used in this file → French equivalents for writing

| English | Français | Context |
|---------|----------|---------|
| [term] | [terme] | [usage] |

## PATTERNS

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

## ENTITY TEMPLATES

> Files created in `.templates/`

### Player Character
See: `.templates/<system>-pc.template.md`

### NPC
See: `.templates/<system>-npc.template.md`

### Obstacle/Threat
See: `.templates/<system>-obstacle.template.md`

### Asset/Item
See: `.templates/<system>-asset.template.md`

## FULL REFERENCE

### Character Creation
1. [Step one]
2. [Step two]
...

### Core Mechanics
[Detailed rules, edge cases, exceptions]

### Advanced Rules
[Optional mechanics, variant rules]

### GM Section
[Running the game, NPC creation, pacing]

## CHANGELOG

| Date | Source | Changes |
|------|--------|---------|
| YYYY-MM-DD | original | Initial restructure |
```
## Example: Restructure Mode

### Input (excerpt from PbtA rules)
```
When you do something risky, roll 2d6 + relevant stat.
On 10+ you succeed. On 7-9 you succeed but with a cost.
On 6- the GM makes a hard move.

Stats are: Cool, Hard, Hot, Sharp, Weird. Range from -1 to +3.
```

### Output

**CHEATSHEET:**
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

**LEXICON:**```markdown
| English | Français | Context |
|---------|----------|---------|
| Roll | Jet | "faire un jet" |
| Success | Réussite | résultat 10+ |
| Partial | Succès partiel | résultat 7-9 |
| Miss | Échec | résultat 6- |
| Hard move | Coup dur | action du MJ |
| Stat | Caractéristique | Cool, Hard, etc. |
| Playbook | Livret | archétype de personnage |
| Move | Action | capacité spéciale |
| Gear | Équipement | objets possédés |
| Harm | Blessure | dégâts subis |
```

**PATTERNS:**
```markdown
### Skill Tests
[Character] tries to [action] under pressure.
→ Roll: 2d6 + [Cool/Hard/Hot/Sharp/Weird]
→ 10+: Success | 7-9: Partial | 6-: Miss
```

**ENTITY TEMPLATES (files created):**

`.templates/pbta-pc.template.md`:
```markdown
# PbtA Player Character Template

**[Name]** - [Playbook]

Stats: Cool: [+X], Hard: [+X], Hot: [+X], Sharp: [+X], Weird: [+X]
Moves: [move1], [move2]...
Gear: [item1], [item2]...
Hx: [character]: [+X/-X]
```

`.templates/pbta-npc.template.md`:
```markdown
# PbtA NPC Template

**[Name]** - [Role]

Drive: [motivation]
Weakness: [exploitable flaw]
Moves: [GM move1], [GM move2]
```

`.templates/pbta-obstacle.template.md`:```markdown
# PbtA Obstacle Template

**[Name]** - [Type]

Countdown: [segments] (if applicable)
Trigger: [what activates it]
Stakes: [success] / [failure]
```

`.templates/pbta-asset.template.md`:```markdown
# PbtA Asset Template

**[Name]** - [Tag]

Bonus: [+1 forward, +1 ongoing, etc.]
Limitation: [1-use, requires X, etc.]
```

**FULL REFERENCE (excerpt):**
```markdown
### Character Creation
1. Choose a playbook
2. Assign stats: distribute +2, +1, +1, 0, -1
3. Select 2 starting moves from playbook
4. Pick gear from playbook list

### Core Mechanics
- Roll 2d6 + stat for all moves
- 10+ = full success, 7-9 = partial, 6- = miss
- On miss, GM makes a move (soft or hard)
```

## Example: Update Mode
### Input

**Base file (already optimized):** `pbta.md`
**Supplement:** `pbta-advanced-moves.md`
```
Advanced Moves (available at level 6+):
- Impossible Reflexes: When you act under fire, on 12+ you transcend the danger entirely.
- Merciless: When you do harm, do +1harm.
```

### Process

```
ANALYSIS:
- New mechanic: 12+ result tier
- Extended mechanic: +1harm bonus
- No conflicts detected

MERGE PREVIEW:
# CHEATSHEET
  Resolution table: add row for 12+

# PATTERNS
  + ### Advanced Combat
  + [Character] with Merciless attacks [target].
  + → Roll: 2d6 + Hard
  + → Damage: +1harm

# FULL REFERENCE
  + ### Advanced Moves
  + Available at level 6+
  + - Impossible Reflexes: 12+ = transcend danger
  + - Merciless: +1harm on all attacks
```

### Output (changes only)

**CHEATSHEET (updated):**
```markdown
### Resolution
| Roll  | Result     | Effect                    |
|-------|------------|---------------------------|
| 12+   | Transcend  | Beyond success (advanced) |
| 10+   | Success    | Full success, no cost     |
| 7-9   | Partial    | Success with complication |
| 6-    | Miss       | GM makes a hard move      |
```

**CHANGELOG (updated):**
```markdown
| Date | Source | Changes |
|------|--------|---------|
| 2026-01-31 | pbta-advanced-moves.md | Added 12+ tier, advanced moves section |
| 2026-01-15 | original | Initial restructure |
```

## Steps
1. **Backup original file:**
   - Copy `<file>.md` to `<file>.original.md` (same directory)
   - If `.original.md` already exists, skip (preserve first version)
   - Log: "Backup created: <file>.original.md"

2. **Detect mode:**
   - If `--update`: go to step 14 (Update Mode)
   - If `--local`: go to step 23 (Local Mode)
   - Otherwise: continue with Restructure Mode

### Restructure Mode (steps 3-13)

3. **Read target rules file(s)**

4. **Handle errors early:**
   - If file not found: list available files in `docs/rules-files/`
   - If not a rules file: "This doesn't appear to be a game rules file. Expected: dice mechanics, resolution tables, or character creation rules."
   - If mechanics unclear: ask user for clarification before proceeding

5. **Extract core mechanics:**
   - Resolution system (dice, cards, etc.)
   - Success/failure thresholds
   - Modifier rules
   - Character components

6. **Build CHEATSHEET:**
   - Core loop in one line
   - Resolution table (max 5 rows)
   - Key modifiers list (max 5 items)
   - Character elements (max 5 items)

7. **Build LEXICON section:**
   - Extract all game-specific terms from source
   - Create English → French mapping tables
   - Group by category (Core, Characteristics, Combat, etc.)
   - Include context/usage examples
   - Essential for French writing with English-optimized rules

8. **Build PATTERNS section:**
   - Template for each action type (combat, skill, social)
   - Minimal working examples
   - Copy-paste ready formats

9. **Build ENTITY TEMPLATES section:**
   - Read base templates from `docs/templates/rules-entities/`
   - Adapt placeholders to this system's mechanics
   - Add system-specific fields (see Customization Notes in each template)
   - Remove irrelevant fields
   - **Create template files** in `.templates/` (same level as `.rules-files/`):
     - `<system>-pc.template.md`
     - `<system>-npc.template.md`
     - `<system>-obstacle.template.md`
     - `<system>-asset.template.md`

10. **Build FULL REFERENCE:**
    - Organize by category (character, mechanics, advanced, GM)
    - Remove redundancy with CHEATSHEET
    - Standardize headings
    - Add internal links

11. **Validate:**
    - [ ] Backup `.original.md` exists
    - [ ] CHEATSHEET < 500 tokens (estimate ~4 chars/token)
    - [ ] LEXICON covers all game-specific terms
    - [ ] All dice mechanics from original are present
    - [ ] All success/failure thresholds match original
    - [ ] All modifier rules are preserved
    - [ ] No mechanics added that weren't in source
    - [ ] Entity templates adapted to system specifics
    - [ ] Template files created in `.templates/`
    - [ ] Examples can be run standalone

12. **Show diff preview:**
    ```diff
    - [old line]
    + [new line]
    ```
    Show first 20 significant changes.

13. **Confirm and write:**
    - Ask: "Apply restructure? [Y/n]"
    - Write updated file with CHANGELOG entry: "Initial restructure"

### Update Mode (steps 14-22)
14. **Read both files:**
    - Base file: existing optimized rules (must have CHEATSHEET/PATTERNS/FULL REFERENCE structure)
    - Supplement file: new rules to integrate

15. **Handle errors:**    - If base file not optimized: "Base file must be in rules-keeper format. Run restructure first."
    - If supplement empty: "Supplement file is empty or unreadable."
    - If supplement not rules: "Supplement doesn't contain game mechanics."

16. **Classify supplement content:**
    - **New mechanics**: rules not present in base
    - **Modified mechanics**: rules that change existing values/thresholds
    - **Extended mechanics**: rules that add options to existing systems

17. **Detect conflicts:**
    - Compare resolution tables, thresholds, modifiers
    - Flag any value that differs between base and supplement

    ```
    CONFLICT DETECTED:
    - Base: "Critical on 10+"
    - Supplement: "Critical on doubles"
    → Which version to keep? [base/supplement/both]
    ```

18. **Merge strategy:**

    | Content Type | Action |
    |--------------|--------|
    | New mechanics | Add to appropriate section |
    | Modified (no conflict) | Update in place |
    | Modified (conflict) | Ask user for arbitration |
    | Extended | Append to existing entry |

19. **Update sections:**
    - CHEATSHEET: Only if core loop or resolution changes
    - LEXICON: Add new terms from supplement
    - PATTERNS: Add new action types, update existing
    - ENTITY TEMPLATES: Update template files if new fields
    - FULL REFERENCE: Merge by category, maintain organization

20. **Add changelog entry:**
    ```markdown
    ## CHANGELOG

    | Date | Source | Changes |
    |------|--------|---------|
    | YYYY-MM-DD | supplement.md | Added [X], modified [Y] |
    ```

21. **Show merge preview:**
    ```diff
    # CHEATSHEET (no changes)

    # PATTERNS
    + ### New Action Type
    + [new pattern]

    # FULL REFERENCE
    - Old value: X
    + New value: Y
    ```

22. **Confirm and write:**
    - Ask: "Apply merge? [Y/n]"
    - Write updated file

### Local Mode (steps 23-27)

Génère un fichier `.docs/document-rules.md` pour les règles spécifiques à un document (scénario, campagne).

23. **Identifier le projet cible:**
    - Chemin : `<univers>/<projet>/`
    - Vérifier que `bank.yml` existe dans le projet

24. **Analyser les sources:**
    - Lire le contenu source fourni (ou `overview.md` si non spécifié)
    - Extraire les règles qui sont **spécifiques à ce document** :
      - Nouvelles mécaniques introduites
      - Variantes des règles système
      - Capacités uniques de PJ/PNJ
      - Objets spéciaux avec règles

25. **Distinguer des règles système:**
    - Règles système → `docs/rules-files/` ou `<univers>/.rules-files/`
    - Règles document → `<projet>/.docs/document-rules.md`

    | Type | Destination | Exemple |
    |------|-------------|---------|
    | Règle système (générique) | `.rules-files/` | "Jets de dés 2d6" |
    | Règle document (locale) | `.docs/document-rules.md` | "Sérum de Ren : +1 en combat pendant 1 scène" |

26. **Générer le fichier:**
    ```markdown
    # Règles Spécifiques: [Nom Document]

    ## Résumé

    [Liste courte des mécaniques ajoutées par ce document]

    ## Mécaniques

    ### [Mécanique 1]

    **Type:** [capacité / objet / état / ...]
    **Déclencheur:** [quand s'applique]
    **Effet:** [ce qui se passe]
    **Durée:** [permanente / temporaire / scène]

    ### [Mécanique 2]

    ...

    ## Objets Spéciaux

    | Nom | Effet | Charges/Usage |
    |-----|-------|---------------|
    | [objet] | [effet] | [limites] |

    ## Interactions avec les Règles Système

    - [Règle système] + [règle document] = [résolution]

    ---
    **Document:** [nom projet]
    **Règles système:** [référence au fichier .rules-files/]
    **Màj:** [date]
    ```

27. **Mettre à jour bank.yml:**
    - Ajouter `.docs/document-rules.md` dans la section `docs.projet`
