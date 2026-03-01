---
name: review-roleplay
description: Review RPG chapter for resource usage, rules accuracy, TOC compliance, and style conformity
argument-hint: Chapter file to review (e.g., chapitre03.md)
---

# Review Roleplaying Chapter

## Goal

Review a roleplaying chapter to verify all resources (output-style, docs, toc, rules-files) have been properly used, game mechanics are accurate, and the chapter meets quality standards.

## Context

### Required Resources for Review

**Chapter to Review:**
```markdown
@chapitres/$ARGUMENTS
```

**Bank Configuration:**
```yaml
@bank.yml
```

**Output Style (reference):**
```markdown
@<univers>/.output-styles/<output-style>.md
@.claude/output-style.md
```

**TOC (expected content):**
```markdown
@toc.md
```

**Rules System (CRITICAL):**
```markdown
@.claude/rules-files/<systeme-jdr>.md
@rules/regles-locales.md
```

**Universe Documentation:**
```markdown
@<univers>/.docs/terminologie.md
@<univers>/.docs/style-guide.md
```

## Rules

- Review MUST validate ALL game mechanics against rules-files
- NPC stats MUST be valid for the game system
- Score each category objectively
- Issues scored 0-3 (higher = more critical)
- Rules errors are CRITICAL (automatic +2 to severity)
- Review pushes to doctor

## Steps

### Step 1: Load Review Context

1. Load chapter content
2. Load TOC specifications for this chapter
3. Load output-style requirements
4. **CRITICAL:** Load rules-files for validation
5. Load universe terminology
6. Extract KEY_POINT and RULE_CHECK markers (if present)

### Step 2: TOC Compliance Review

Compare chapter against TOC specifications:

| TOC Element | Found in Chapter? | Location | Score |
|-------------|-------------------|----------|-------|
| Title | Yes/No | Line X | 0-3 |
| Synopsis coverage | % | [sections] | 0-3 |
| Key Point 1 | Yes/No/Partial | Line X | 0-3 |
| Key Point 2 | Yes/No/Partial | Line X | 0-3 |
| Key Point 3 | Yes/No/Partial | Line X | 0-3 |
| Locations used | X/Y | [list] | 0-3 |
| NPCs used | X/Y | [list] | 0-3 |
| Encounters defined | X/Y | [list] | 0-3 |
| Tone | Match/Mismatch | [examples] | 0-3 |

### Step 3: Rules Accuracy Review (CRITICAL)

Validate all game mechanics against rules-files:

**Tests and Checks:**

| Test/Check | Rules-File Mechanic | In Chapter | Valid? | Score |
|------------|---------------------|------------|--------|-------|
| [test name] | [correct mechanic] | [as written] | Yes/No | 0-3 |

**NPC Stats:**

| NPC | Stat Block | Valid for System? | Issues | Score |
|-----|------------|-------------------|--------|-------|
| [name] | [stats] | Yes/No | [problems] | 0-3 |

**Difficulty Ratings:**

| Encounter/Test | Difficulty | Appropriate? | Score |
|----------------|------------|--------------|-------|
| [name] | [rating] | Yes/No/High/Low | 0-3 |

**Rewards and Consequences:**

| Element | As Written | Balanced? | Score |
|---------|------------|-----------|-------|
| XP/Rewards | [amount] | Yes/No | 0-3 |
| Consequences | [description] | Proportional? | 0-3 |

### Step 4: Output-Style Conformity

Check adherence to style guide:

| Style Rule | Compliant? | Examples | Score |
|------------|------------|----------|-------|
| Narrative/Rules ratio | [actual] vs [expected] | - | 0-3 |
| GM notes format | Yes/No | [examples] | 0-3 |
| Stat block format | Yes/No | [examples] | 0-3 |
| Read-aloud text | Yes/No | [marked?] | 0-3 |
| Boxed text usage | Yes/No | [appropriate?] | 0-3 |

### Step 5: Terminology and Mechanics Check

Verify correct use of game and universe terms:

| Term | Context | Correct Usage | Issues |
|------|---------|---------------|--------|
| [game term] | [where used] | Yes/No | [if wrong] |
| [universe term] | [where used] | Yes/No | [if wrong] |

Flag:
- Game terms used incorrectly
- Mechanics described wrongly
- Inconsistent rule applications

### Step 6: Calculate Overall Score

Aggregate scores with rules weight:

```
TOC Compliance:      X/27 (9 items x 3)
Rules Accuracy:      X/[N*3] ** WEIGHTED x2 **
Style Conformity:    X/15 (5 items x 3)
Terminology:         X/[N*3]

Total Issues Score:  X/[max]
Quality Rating:      [A/B/C/D/F]

** Rules errors add +2 to issue severity **
```

**Rating Scale:**
- A (0-10%): Excellent, rules accurate, ready for doctor
- B (10-25%): Good, minor rules clarifications needed
- C (25-50%): Acceptable, some rules fixes required
- D (50-75%): Poor, significant rules errors
- F (75%+): Unacceptable, rules broken, rewrite required

### Step 7: Generate Issues Table

| # | Category | Issue | Severity | Type | Fix Required | Line |
|---|----------|-------|----------|------|--------------|------|
| 1 | Rules | [desc] | 3 | CRITICAL | [action] | [line] |
| 2 | TOC | [desc] | 1 | Minor | [action] | [line] |

**Severity Modifiers:**
- Rules errors: Base + 2
- Balance issues: Base + 1
- Narrative issues: Base + 0

### Step 8: Decide Action

**IF Rating A or B:**
```
APPROVE for doctor review
Route to: workshop/doctor.prompt.md
```

**IF Rating C with NO rules errors:**
```
Request minor fixes
List specific changes needed
WAIT FOR USER
```

**IF Rating C with rules errors OR Rating D:**
```
Request rules fixes FIRST
May require re-checking rules-files
WAIT FOR USER to fix rules issues
```

**IF Rating F:**
```
Request rewrite
Rules system not properly applied
Recommend re-running write-roleplaying
WAIT FOR USER decision
```

## Output Format

```markdown
# Review Report: [Chapter Title] (RPG)

**File:** `chapitres/[filename]`
**Reviewed:** [date]
**Rating:** [A/B/C/D/F] ([score]%)
**Rules Accuracy:** [X]% valid

---

## Executive Summary

[2-3 sentences summarizing review findings, highlighting rules status]

---

## TOC Compliance

| Element | Status | Score |
|---------|--------|-------|
| Synopsis | [status] | [0-3] |
| Key Points | [X/Y covered] | [0-3] |
| NPCs | [X/Y present] | [0-3] |
| Encounters | [X/Y defined] | [0-3] |

**TOC Score:** [X/Y]

---

## Rules Accuracy (CRITICAL)

| Category | Valid | Invalid | Score |
|----------|-------|---------|-------|
| Tests/Checks | [X] | [Y] | [0-3] |
| NPC Stats | [X] | [Y] | [0-3] |
| Difficulty | [X] | [Y] | [0-3] |
| Balance | [X] | [Y] | [0-3] |

**Rules Score:** [X/Y] (weighted x2)

### Rules Issues (if any)

| Issue | Expected (rules-file) | Written | Fix |
|-------|----------------------|---------|-----|
| [issue] | [correct] | [wrong] | [action] |

---

## Style Conformity

| Rule | Status | Score |
|------|--------|-------|
| Narrative/Rules ratio | [actual] | [0-3] |
| Stat blocks | [status] | [0-3] |
| GM notes | [status] | [0-3] |

**Style Score:** [X/Y]

---

## Issues to Fix

| # | Category | Issue | Severity | Priority |
|---|----------|-------|----------|----------|
| 1 | RULES | [issue] | [3+2=5] | CRITICAL |
| 2 | TOC | [issue] | [1] | Low |

---

## Recommendation

**Action:** [Approve/Fix Rules/Revise/Rewrite]

**Next Step:**
- IF Approve: `workshop/doctor.prompt.md [chapter]`
- IF Fix Rules: Apply rules fixes FIRST, re-run review
- IF Revise/Rewrite: `workshop/write-roleplaying.prompt.md [chapter]`
```

## Transition

**On Approval (A/B rating with no critical rules errors):**
```
Route to: workshop/doctor.prompt.md
Arguments: [chapter]
```

## Quality Checklist

- [ ] All TOC elements verified
- [ ] ALL game mechanics validated against rules-files
- [ ] NPC stats verified
- [ ] Difficulty ratings appropriate
- [ ] Balance checked
- [ ] Output-style rules checked
- [ ] Terminology validated
- [ ] Score calculated correctly
- [ ] Rules issues flagged as CRITICAL
- [ ] Recommendation is clear
