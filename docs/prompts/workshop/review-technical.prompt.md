---
name: review-technical
description: Review technical documentation section for accuracy, clarity, and completeness
argument-hint: Section file to review (e.g., section03.md)
version: 1.0
changelog:
  - version: 1.0
    date: 2026-02-28
    changes:
      - "Adaptation de review-chapter pour documentation technique"
      - "Critères : précision technique, clarté, exhaustivité, audience"
---

# Review Technical Documentation Section

## Goal

Review a written technical documentation section to verify all resources (output-style, docs, toc) have been properly used and the section meets quality standards for technical documentation.

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
@<client>/.output-styles/<output-style>.md
@.claude/output-style.md
```

**TOC (expected content):**
```markdown
@.toc/INDEX.md
@.toc/toc-chapter<XX>.md
```

**Client Documentation:**
```markdown
@<client>/.docs/CLIENT.md
@<client>/.docs/glossaire.md
```

## Rules

- Review MUST compare section against ALL loaded resources
- Score each category objectively
- Issues scored 0-3 (higher = more critical)
- Focus on technical accuracy and clarity
- Generate actionable fix list
- Review pushes to doctor

## Steps

### Step 1: Load Review Context

1. Load chapter content
2. Load TOC specifications for this chapter
3. Load output-style requirements
4. Load client glossary
5. Extract KEY_POINT markers from chapter (if present)

### Step 2: TOC Compliance Review

Compare chapter against TOC specifications:

| TOC Element | Found in Chapter? | Location | Score |
|-------------|-------------------|----------|-------|
| Title | Yes/No | Line X | 0-3 |
| Synopsis coverage | % | [subsections] | 0-3 |
| Key Point 1 | Yes/No/Partial | Line X | 0-3 |
| Key Point 2 | Yes/No/Partial | Line X | 0-3 |
| Key Point 3 | Yes/No/Partial | Line X | 0-3 |
| Technical elements | X/Y | [list] | 0-3 |
| Audience level | Match/Mismatch | [assessment] | 0-3 |
| Length | Short/OK/Long | [word count] | 0-3 |

**Score Meaning:**
- 0: Perfect match
- 1: Minor deviation (acceptable)
- 2: Significant deviation (needs attention)
- 3: Missing or wrong (must fix)

### Step 3: Technical Accuracy Review

Check technical content accuracy:

| Element | Status | Issues | Score |
|---------|--------|--------|-------|
| Code examples | Valid/Invalid | [errors] | 0-3 |
| Diagrams | Correct/Incorrect | [issues] | 0-3 |
| Parameters/specs | Accurate/Inaccurate | [errors] | 0-3 |
| Version info | Current/Outdated | [details] | 0-3 |
| Links/references | Working/Broken | [count] | 0-3 |

### Step 4: Clarity & Accessibility Review

Evaluate clarity for target audience:

| Criterion | Assessment | Examples | Score |
|-----------|------------|----------|-------|
| Language level | Appropriate/Too complex/Too simple | [passages] | 0-3 |
| Jargon handling | Defined/Undefined | [terms] | 0-3 |
| Structure | Clear/Confusing | [issues] | 0-3 |
| Examples | Present/Missing/Insufficient | [count] | 0-3 |
| Instructions | Clear/Ambiguous | [passages] | 0-3 |
| Visual aids | Adequate/Missing | [diagrams/screenshots] | 0-3 |

### Step 5: Completeness Review

Verify all necessary information is present:

| Element | Present | Missing | Score |
|---------|---------|---------|-------|
| Prerequisites | Yes/No | [what's missing] | 0-3 |
| Context/background | Yes/No | [gaps] | 0-3 |
| All use cases | Yes/No | [missing cases] | 0-3 |
| Error handling | Yes/No | [missing scenarios] | 0-3 |
| Next steps | Yes/No | [unclear path] | 0-3 |

### Step 6: Style Conformity

Check adherence to output-style guide:

| Style Rule | Compliant? | Examples | Score |
|------------|------------|----------|-------|
| Heading hierarchy | Yes/No | [issues] | 0-3 |
| List formatting | Yes/No | [issues] | 0-3 |
| Code block syntax | Yes/No | [issues] | 0-3 |
| Table formatting | Yes/No | [issues] | 0-3 |
| Tone consistency | Yes/No | [passages] | 0-3 |

### Step 7: Glossary Compliance

Verify correct use of client-specific terms:

| Term | Correct Usage | Occurrences | Issues |
|------|---------------|-------------|--------|
| [term from glossaire.md] | Yes/No | X | [if wrong] |

Flag:
- Terms used incorrectly
- Missing required terminology
- Inconsistent spelling/formatting
- Undefined jargon

### Step 8: Calculate Overall Score

Aggregate scores:

```
TOC Compliance:       X/24 (8 items x 3)
Technical Accuracy:   X/15 (5 items x 3)
Clarity:              X/18 (6 items x 3)
Completeness:         X/15 (5 items x 3)
Style Conformity:     X/15 (5 items x 3)
Glossary:             X/[N*3]

Total Issues Score:   X/[max]
Quality Rating:       [A/B/C/D/F]
```

**Rating Scale:**
- A (0-10%): Excellent, ready for doctor review
- B (10-25%): Good, minor fixes needed
- C (25-50%): Acceptable, several fixes needed
- D (50-75%): Poor, major revision needed
- F (75%+): Unacceptable, rewrite required

### Step 9: Generate Issues Table

| # | Category | Issue | Severity | Fix Required | Line |
|---|----------|-------|----------|--------------|------|
| 1 | [cat] | [description] | 0-3 | [action] | [line] |
| 2 | ... | ... | ... | ... | ... |

**Categories:**
- TOC: Missing key points, off-topic content
- Technical: Incorrect code, outdated info, broken links
- Clarity: Jargon, ambiguous instructions, missing examples
- Completeness: Missing prerequisites, gaps, no error handling
- Style: Formatting, tone, hierarchy
- Glossary: Wrong terms, inconsistent usage

### Step 10: Decide Action

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
Recommend re-running write-technical/write-user-guide
WAIT FOR USER decision
```

## Output Format

```markdown
# Review Report: [Chapter Title]

**File:** `chapitres/[filename]`
**Reviewed:** [date]
**Rating:** [A/B/C/D/F] ([score]%)
**Audience:** [technical/end-user/mixed]

---

## Executive Summary

[2-3 sentences summarizing review findings, key strengths and critical issues]

---

## TOC Compliance

| Element | Status | Score |
|---------|--------|-------|
| Synopsis | [status] | [0-3] |
| Key Points | [X/Y covered] | [0-3] |
| Technical elements | [X/Y present] | [0-3] |
| Audience level | [match level] | [0-3] |

**TOC Score:** [X/Y]

---

## Technical Accuracy

| Element | Status | Score |
|---------|--------|-------|
| Code examples | [status] | [0-3] |
| Diagrams | [status] | [0-3] |
| Parameters/specs | [status] | [0-3] |
| Version info | [status] | [0-3] |

**Technical Score:** [X/Y]

---

## Clarity & Accessibility

| Criterion | Assessment | Score |
|-----------|------------|-------|
| Language level | [status] | [0-3] |
| Jargon handling | [status] | [0-3] |
| Examples | [status] | [0-3] |
| Instructions | [status] | [0-3] |

**Clarity Score:** [X/Y]

---

## Completeness

| Element | Present | Score |
|---------|---------|-------|
| Prerequisites | [Y/N] | [0-3] |
| Context | [Y/N] | [0-3] |
| Use cases | [Y/N] | [0-3] |
| Error handling | [Y/N] | [0-3] |

**Completeness Score:** [X/Y]

---

## Glossary & Terminology

**Correct:** [X] terms
**Issues:** [Y] terms

| Term | Issue | Line |
|------|-------|------|
| [term] | [problem] | [line] |

---

## Issues to Fix

| # | Category | Issue | Severity | Action |
|---|----------|-------|----------|--------|
| 1 | [cat] | [issue] | [0-3] | [fix] |

**Critical (3):** [count] issues
**Major (2):** [count] issues
**Minor (1):** [count] issues

---

## Strengths

- [Point fort 1]
- [Point fort 2]
- [Point fort 3]

---

## Recommendation

**Action:** [Approve/Fix/Revise/Rewrite]

**Rationale:** [Brief explanation of decision]

**Next Step:**
- IF Approve: `workshop/doctor.prompt.md chapitre<XX>.md`
- IF Fix: Apply fixes, re-run review
- IF Revise/Rewrite: `workshop/write-technical.prompt.md <XX>` OU `workshop/write-user-guide.prompt.md <XX>`
```

## Transition

**On Approval (A/B rating):**
```
Route to: workshop/doctor.prompt.md
Arguments: chapitre<XX>.md
```

## Quality Checklist

- [ ] All TOC elements verified
- [ ] Technical accuracy checked (code, specs, versions)
- [ ] Clarity assessed for target audience
- [ ] Completeness verified (prerequisites, context, use cases)
- [ ] Output-style rules checked
- [ ] Glossary terms validated
- [ ] Score calculated correctly
- [ ] Issues are actionable and prioritized
- [ ] Recommendation is clear and justified
