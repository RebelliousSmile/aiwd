---
name: research
description: Cross-reference web research with existing documentation, Perplexity-style
argument-hint: Research topic or question
---

# Research: Cross-Referenced Investigation

## Goal

Perform comprehensive web research on a topic, cross-reference multiple sources, and compare findings with existing project documentation to produce a structured research report.

## Context

### Existing Documentation

```markdown
@<univers>/.docs/UNIVERS.md
@<univers>/.docs/terminologie.md
@<univers>/.docs/lieux.md
@<univers>/.docs/personnages.md
```

### Project-Specific Docs

```markdown
@docs/*.md
```

### Research Topic

```text
$ARGUMENTS
```

## Rules

- Perform minimum 3 distinct web searches
- Cross-reference at least 3 different sources
- Compare ALL findings with existing docs
- Flag contradictions explicitly
- Output report using template
- Save report to `docs/research/`
- Research pushes to evaluate

## Steps

### Step 1: Parse Research Request

1. Analyze the research topic
2. Identify key concepts to investigate
3. Break down into 3-5 searchable queries
4. Prioritize official/authoritative sources

### Step 2: Define Search Strategy

Create search matrix:

| Query | Purpose | Expected Sources |
|-------|---------|------------------|
| [query 1] | [what we're looking for] | [type of source] |
| [query 2] | [what we're looking for] | [type of source] |
| [query 3] | [what we're looking for] | [type of source] |

**Search Angles:**
- Official/canonical sources (books, official sites)
- Community knowledge (wikis, forums)
- Academic/analytical (essays, reviews)
- Visual references (if applicable)

### Step 3: Execute Web Searches

For each query:

1. Perform web search
2. Extract key information
3. Note source URL and credibility
4. Tag information type (fact, opinion, speculation)

```yaml
search_result:
  query: "<requête>"
  source: "<URL>"
  credibility: "high|medium|low"
  type: "official|community|academic|other"
  key_findings:
    - "<finding 1>"
    - "<finding 2>"
  quotes:
    - text: "<citation>"
      context: "<contexte>"
```

### Step 4: Cross-Reference Sources

Compare findings across sources:

| Topic | Source A | Source B | Source C | Consensus |
|-------|----------|----------|----------|-----------|
| [topic] | [finding] | [finding] | [finding] | [agree/disagree] |

**Flag:**
- Contradictions between sources
- Information found in only one source
- Speculative vs confirmed facts

### Step 5: Compare with Existing Docs

For each finding, check against project documentation:

| Finding | In Existing Docs? | Match? | Action |
|---------|-------------------|--------|--------|
| [finding] | Yes/No | Yes/Partial/No | Update/Add/Ignore |

**Categories:**
- **Confirms:** Finding matches existing docs
- **Extends:** Finding adds new detail to existing docs
- **Contradicts:** Finding conflicts with existing docs
- **New:** Finding not in existing docs

### Step 6: Generate Research Report

Save to `docs/research/<date>-<topic>.md` using template:

```markdown
# Research Report: [Topic]

**Date:** [YYYY-MM-DD]
**Researcher:** Claude (workshop/research.prompt.md)
**Status:** Draft | Review | Validated

---

## Executive Summary

[2-3 paragraphs summarizing key findings]

---

## Research Questions

1. [Question 1]
2. [Question 2]
3. [Question 3]

---

## Sources Consulted

| # | Source | Type | Credibility | URL |
|---|--------|------|-------------|-----|
| 1 | [name] | [type] | [level] | [url] |
| 2 | [name] | [type] | [level] | [url] |
| 3 | [name] | [type] | [level] | [url] |

---

## Key Findings

### Finding 1: [Title]

**Sources:** [1, 2]
**Confidence:** High | Medium | Low

[Description of finding]

**Evidence:**
> "[Quote from source]" - Source X

### Finding 2: [Title]
...

---

## Cross-Reference Analysis

### Consensus Points
- [Point with multiple source agreement]

### Contradictions
| Topic | Source A Says | Source B Says | Resolution |
|-------|---------------|---------------|------------|
| [topic] | [claim] | [claim] | [which is correct and why] |

### Gaps
- [Information not found]

---

## Comparison with Existing Documentation

### Confirmed
- [Existing doc element confirmed by research]

### Extensions
| Existing Info | New Detail | Source | Recommendation |
|---------------|------------|--------|----------------|
| [current] | [new] | [#] | Update/Note |

### Contradictions
| Existing Doc Says | Research Says | Resolution |
|-------------------|---------------|------------|
| [current] | [new] | [which to trust] |

### New Information
- [Info to potentially add to docs]

---

## Recommendations

1. **Update [doc]:** [specific change]
2. **Add to [doc]:** [new content]
3. **Verify:** [uncertain finding requiring more research]
4. **Ignore:** [unreliable or irrelevant finding]

---

## Next Steps

Route to: `workshop/evaluate.prompt.md`
Purpose: Evaluate project with updated research

---

## Appendix: Raw Search Results

[Optional: detailed search data]
```

### Step 7: Save and Report

1. Save report to `docs/research/<date>-<topic>.md`
2. List documentation updates recommended
3. Provide confidence assessment

## Transition

After research report is complete:

```
Route to: workshop/evaluate.prompt.md
Arguments: [project path]
Purpose: Re-evaluate project with new research
```

## Quality Checklist

- [ ] Minimum 3 searches performed
- [ ] Minimum 3 sources consulted
- [ ] Cross-reference table complete
- [ ] Existing docs compared
- [ ] Contradictions flagged
- [ ] Recommendations actionable
- [ ] Report saved to docs/research/
