---
name: write-toc
description: Generate detailed table of contents with chapter synopses, locations and characters
argument-hint: Path to bank.yml or project brief description
---

# Write Table of Contents

## Goal

Create a comprehensive table of contents (TOC) for a writing project, including chapter synopses, key points, locations, and characters for each section.

## Context

### Bank Configuration

```yaml
@$ARGUMENTS/bank.yml
```

### Universe Documentation

```markdown
@<univers>/.docs/UNIVERS.md
```

## Rules

- Output in user's preferred language
- Each chapter MUST have: title, synopsis, key points
- Locations and characters are optional per chapter
- LESS IS MORE: focus on essential story beats
- Save TOC to `toc.md` in project root before displaying
- TOC guides ALL subsequent writing

## Steps

### Step 1: Understand Project Scope

1. Load bank.yml configuration
2. Identify document type (novel, roleplaying, scenario)
3. Determine target length (chapters, word count estimates)
4. Note any existing outline or brief

### Step 2: Brainstorm Structure with User

Interactive dialogue to define high-level structure:

1. Propose main narrative arcs (3-5 acts/parts)
2. Discuss pacing: action vs exposition ratio
3. Identify key turning points
4. Validate overall structure

**WAIT FOR USER APPROVAL** before detailing chapters

### Step 3: Detail Each Chapter

For each chapter, define:

```yaml
chapter:
  number: X
  title: "<titre-chapitre>"
  synopsis: |
    <2-4 phrases décrivant l'action principale>
  key_points:
    - <point clé 1: événement ou révélation>
    - <point clé 2: développement personnage>
    - <point clé 3: avancement intrigue>
  locations:
    - name: "<nom-lieu>"
      description: "<courte description si nouveau>"
  characters:
    - name: "<nom-personnage>"
      role: "<rôle dans ce chapitre>"
      first_appearance: true/false
  tone: "<atmosphère dominante>"
  estimated_length: "<court|moyen|long>"
```

### Step 4: Validate Consistency

Check for:

1. **Continuity**: Characters appear logically
2. **Pacing**: No chapter overloaded, no filler
3. **Locations**: Travel time realistic
4. **Arcs**: Each character arc progresses
5. **Hooks**: Each chapter ends with tension

List any issues found.

**WAIT FOR USER APPROVAL** on fixes

### Step 5: Generate TOC File

Create `toc.md` with full structure:

```markdown
# Table des Matières: [Nom du Projet]

**Type:** [novel|roleplaying|scenario]
**Univers:** [nom univers]
**Chapitres:** [nombre total]
**Longueur estimée:** [pages/mots]

---

## Structure Narrative

### Acte I: [Titre Acte]
- Chapitres X à Y
- Arc principal: [description]

### Acte II: [Titre Acte]
...

---

## Chapitres Détaillés

### Chapitre 1: [Titre]

**Synopsis:**
[Synopsis détaillé]

**Points clés:**
- [ ] [Point 1]
- [ ] [Point 2]

**Lieux:**
- [Lieu 1]: [description]

**Personnages:**
- [Personnage]: [rôle]

**Ton:** [atmosphère]
**Longueur:** [estimation]

---

### Chapitre 2: [Titre]
...
```

### Step 6: Save and Confirm

1. Save TOC to `$ARGUMENTS/toc.md`
2. Display saved file path
3. Show summary statistics:
   - Total chapters
   - Unique locations
   - Character count
   - Estimated total length

## Transition

After TOC approval, route to appropriate writing prompt:

**IF type = novel OR guide:**
```
Route to: workshop/write-novel.prompt.md
```

**IF type = roleplaying OR scenario:**
```
Route to: workshop/write-roleplaying.prompt.md
```

## Quality Checklist

- [ ] Every chapter has clear purpose
- [ ] No orphan characters (introduced but forgotten)
- [ ] Locations used consistently
- [ ] Tone variation prevents monotony
- [ ] Key points are checkable (can verify in text)
- [ ] Synopsis sufficient for writing without ambiguity
