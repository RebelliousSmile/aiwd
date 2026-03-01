---
name: review-chapter
description: Review narrative chapter for resource usage, TOC compliance, and style conformity
argument-hint: Chapter file to review (e.g., chapitre03.md)
---

# Review Chapter

## Goal

Review a written chapter to verify all resources (output-style, docs, toc) have been properly used and the chapter meets quality standards.

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

**Universe Documentation:**
```markdown
@<univers>/.docs/terminologie.md
@<univers>/.docs/style-guide.md
```

## Rules

- Review MUST compare chapter against ALL loaded resources
- Score each category objectively
- Issues scored 0-3 (higher = more critical)
- Challenge assumptions, less is more
- Generate actionable fix list
- Review pushes to doctor

## Steps

### Step 1: Load Review Context

1. Load chapter content
2. Load TOC specifications for this chapter
3. Load output-style requirements
4. Load universe terminology
5. Extract KEY_POINT markers from chapter (if present)

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
| Characters used | X/Y | [list] | 0-3 |
| Tone | Match/Mismatch | [examples] | 0-3 |
| Length | Short/OK/Long | [word count] | 0-3 |

**Score Meaning:**
- 0: Perfect match
- 1: Minor deviation (acceptable)
- 2: Significant deviation (needs attention)
- 3: Missing or wrong (must fix)

### Step 3: Output-Style Conformity

Check adherence to style guide:

| Style Rule | Compliant? | Examples | Score |
|------------|------------|----------|-------|
| Dialogue format | Yes/No | [lines] | 0-3 |
| Description density | Yes/No | [assessment] | 0-3 |
| POV consistency | Yes/No | [issues] | 0-3 |
| Tense consistency | Yes/No | [issues] | 0-3 |
| Paragraph length | Yes/No | [avg] | 0-3 |
| Quote usage | Yes/No | [count] | 0-3 |

### Step 4: Terminology Check

Verify correct use of universe-specific terms:

| Term | Correct Usage | Occurrences | Issues |
|------|---------------|-------------|--------|
| [term from terminologie.md] | Yes/No | X | [if wrong] |

Flag:
- Terms used incorrectly
- Missing required terminology
- Inconsistent spelling/formatting

### Step 5: Calculate Overall Score

Aggregate scores:

```
TOC Compliance:      X/27 (9 items x 3)
Style Conformity:    X/18 (6 items x 3)
Terminology:         X/[N*3]

Total Issues Score:  X/[max]
Quality Rating:      [A/B/C/D/F]
```

**Rating Scale:**
- A (0-10%): Excellent, ready for doctor review
- B (10-25%): Good, minor fixes needed
- C (25-50%): Acceptable, several fixes needed
- D (50-75%): Poor, major revision needed
- F (75%+): Unacceptable, rewrite required

### Step 6: Generate Issues Table

| # | Category | Issue | Severity | Fix Required | Line |
|---|----------|-------|----------|--------------|------|
| 1 | [cat] | [description] | 0-3 | [action] | [line] |
| 2 | ... | ... | ... | ... | ... |

### Step 7: Decide Action

**IF Rating A or B:**
```
APPROVE for doctor review
Route to: workshop/doctor.prompt.md
```

**IF Rating C:**
```
Request minor fixes
List specific changes needed
WAIT FOR USER to apply fixes or request auto-fix
```

**IF Rating D or F:**
```
Request major revision
Recommend re-running write-novel/write-roleplaying
WAIT FOR USER decision
```

## Output Format

```markdown
# Review Report: [Chapter Title]

**File:** `chapitres/[filename]`
**Reviewed:** [date]
**Rating:** [A/B/C/D/F] ([score]%)

---

## Executive Summary

[2-3 sentences summarizing review findings]

---

## TOC Compliance

| Element | Status | Score |
|---------|--------|-------|
| Synopsis | [status] | [0-3] |
| Key Points | [X/Y covered] | [0-3] |
| Locations | [X/Y used] | [0-3] |
| Characters | [X/Y present] | [0-3] |
| Tone | [match level] | [0-3] |

**TOC Score:** [X/Y]

---

## Style Conformity

| Rule | Status | Score |
|------|--------|-------|
| Dialogue | [status] | [0-3] |
| POV | [status] | [0-3] |
| Tense | [status] | [0-3] |

**Style Score:** [X/Y]

---

## Terminology

**Correct:** [X] terms
**Issues:** [Y] terms

| Term | Issue | Line |
|------|-------|------|
| [term] | [problem] | [line] |

---

## Issues to Fix

| # | Issue | Severity | Action |
|---|-------|----------|--------|
| 1 | [issue] | [0-3] | [fix] |

---

## Recommendation

**Action:** [Approve/Fix/Revise/Rewrite]

**Next Step:**
- IF Approve: `workshop/doctor.prompt.md [chapter]`
- IF Fix: Apply fixes, re-run review
- IF Revise/Rewrite: `workshop/write-novel.prompt.md [chapter]`
```

## Transition

**On Approval (A/B rating):**
```
Route to: workshop/doctor.prompt.md
Arguments: [chapter]
```

## Quality Checklist

- [ ] All TOC elements verified
- [ ] Output-style rules checked
- [ ] Terminology validated
- [ ] Score calculated correctly
- [ ] Issues are actionable
- [ ] Recommendation is clear
