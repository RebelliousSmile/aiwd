---
name: evaluate
description: Evaluate and refine a document project, then route to appropriate writing prompt
argument-hint: Path to bank.yml or document directory
---

# Evaluate Document Project

## Goal

Evaluate a writing project through interactive dialogue, identify its type and requirements, then route to the appropriate writing prompt (write-novel or write-roleplaying).

## Context

### Bank Configuration

```yaml
@$ARGUMENTS/bank.yml
```

## Rules

- Output in user's preferred language
- Evaluate project scope BEFORE recommending writing approach
- Challenge ambiguities and edge cases
- Validate resources availability before proceeding
- LESS IS MORE: focus on essential requirements

## Steps

### Step 1: Load and Parse Bank

1. Read `bank.yml` from provided path
2. Verify all referenced resources exist
3. List missing or incomplete resources
4. IF critical resources missing: STOP and report

### Step 2: Analyze Project Type

Determine document type based on bank.yml configuration:

| Type | Indicators | Route |
|------|------------|-------|
| `novel` | No rules-files, narrative focus | write-novel |
| `roleplaying` | rules-files present, game mechanics | write-roleplaying |
| `scenario` | rules-files + structured encounters | write-roleplaying |
| `guide` | technical/reference content | write-novel |

### Step 3: Evaluate Project Completeness

Score each category (0-3):

| Category | Score | Criteria |
|----------|-------|----------|
| Output-style | 0-3 | Style guide loaded, conventions clear |
| Documentation | 0-3 | Universe docs complete, terminology defined |
| TOC | 0-3 | Structure exists, chapters outlined |
| Templates | 0-3 | LaTeX templates available, compilation ready |
| Rules (if applicable) | 0-3 | Game system defined, mechanics clear |

**Scoring:**
- 0: Missing entirely
- 1: Partial, unusable as-is
- 2: Present but needs refinement
- 3: Complete and ready

### Step 4: Identify Gaps

For each score < 3:

1. List specific missing elements
2. Propose resolution path:
   - Create missing resource
   - Use template/default
   - Request user input

**WAIT FOR USER APPROVAL** on gap resolution

### Step 5: Refine Project Requirements

Make this iteration until no more questions:

1. Detail project requirements in bullet points
2. List inconsistencies and ambiguities
3. Check for edge cases (chapter count, scope, tone)
4. Replay steps 2-3 until no more questions
5. Display final project requirements

**WAIT FOR USER APPROVAL**

### Step 6: Route to Writing Prompt

Based on type determination:

**IF type = novel OR guide:**
```
Route to: workshop/write-novel.prompt.md
Arguments: $ARGUMENTS
```

**IF type = roleplaying OR scenario:**
```
Route to: workshop/write-roleplaying.prompt.md
Arguments: $ARGUMENTS
```

## Output Format

```markdown
# Evaluation Report: [Project Name]

## Project Type: [novel|roleplaying|scenario|guide]

## Resource Scores

| Resource | Score | Status |
|----------|-------|--------|
| Output-style | X/3 | [status] |
| Documentation | X/3 | [status] |
| TOC | X/3 | [status] |
| Templates | X/3 | [status] |
| Rules | X/3 | [status or N/A] |

**Total: XX/15 (or XX/12 if no rules)**

## Gaps Identified

1. [Gap 1]: [Resolution]
2. [Gap 2]: [Resolution]

## Project Requirements Summary

- [Requirement 1]
- [Requirement 2]
- ...

## Recommendation

Route to: **[write-novel|write-roleplaying]**
Confidence: [High|Medium|Low]
Reason: [Brief explanation]
```

## Transition

After user approval, execute the appropriate writing prompt with the validated bank configuration.
