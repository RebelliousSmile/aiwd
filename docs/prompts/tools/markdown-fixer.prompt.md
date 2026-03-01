---
name: markdown-fixer
description: Validate and fix markdown syntax for Scrivener 3 import compatibility
argument-hint: <file.md>
---

# Markdown Fixer Prompt (Scrivener 3)

## Goal

Validate markdown file syntax and fix issues to ensure compatibility with Scrivener 3's markdown import feature ("Convert Markdown to Rich Text"), producing publication-ready output.

## Context

### Target File

```markdown
@$FILE
```

### Scrivener 3 Markdown Support

Scrivener 3 has limited markdown conversion (see Scrivener Manual §23-24). Only these elements are reliably converted:

**Supported:**

- Headers (`#`, `##`, etc.) - used for document splitting via "Import and Split"
- Bold (`**text**`)
- Italic (`*text*` or `_text_`)
- Bold+Italic (`***text***` or `**_text_**`)
- Links (`[text](url)`) - title attribute ignored
- Images (`![alt](path)`)
- Unordered lists (`-` or `*`) - nested up to 2 levels only
- Ordered lists (`1.`, `2.`, etc.)
- Inline code (`` `code` ``)
- Line breaks (two trailing spaces or `\` at end of line)

**Not supported (will appear as raw text):**

- Tables
- Task lists (`- [ ]`) — also inappropriate for publication
- Code blocks (fenced with triple backticks)
- Blockquotes (`>`)
- Footnotes
- Front matter (YAML `---`)
- Horizontal rules (`---`, `***`, `___`)
- Clickable images (`[![alt](img)](url)`)
- Nested lists beyond 2 levels (flattened by Scrivener)

**Warning:** Converting unsupported elements may result in loss of visual formatting. Review all conversions carefully.

## Rules

- Fix syntax only, NEVER modify content meaning
- Preserve document structure and hierarchy
- Use ONLY Scrivener 3 compatible syntax
- **Produce publication-ready output** — no technical markers, labels, or meta-comments
- When converting unsupported elements, preserve semantic meaning over visual appearance
- Offer multiple conversion options when ambiguous (Option A is always recommended)
- Report all changes before applying
- Ask user validation before writing

## Steps

### Step 1: Parse and Identify

Parse file and identify all syntax elements, including nested structures.

### Step 2a: Validate Supported Syntax

Check supported elements for correct syntax:

| Element | Valid Pattern | Common Issues |
|---------|---------------|---------------|
| Headers | `# ` with space | Missing space after `#` |
| Bold | `**text**` | Unbalanced `**` markers |
| Italic | `*text*` | Unbalanced `*` markers |
| Bold+Italic | `***text***` | Mixed marker styles |
| Links | `[text](url)` | Missing parentheses |
| Images | `![alt](path)` | Missing `!` prefix |
| Lists | `- ` or `1. ` | Missing space after marker |
| Inline code | `` `code` `` | Unbalanced backticks |
| Line breaks | `  ` or `\` at EOL | Inconsistent style |

**Special characters to escape:**

Characters `*`, `_`, `[`, `]`, `(`, `)`, `` ` `` must be escaped with `\` when used literally (not as formatting).

Example: `prices are 5\* quality` → displays as `prices are 5* quality`

### Step 2b: Convert Unsupported Elements

For each conversion, **Option A is recommended** unless user specifies otherwise.

All conversions must produce **publication-ready text** — no technical labels or bracketed comments.

---

**Tables** → Convert according to complexity:

**Règle principale : Approche hybride selon le nombre de colonnes**

- **Tables simples (2-3 colonnes)** → Convertir en listes (Option A)
- **Tables complexes (4+ colonnes)** → Garder en markdown (Option E)

---

- **Option A (recommended for simple tables):** Narrative list format
  ```
  Before: | Name  | Age |
          |-------|-----|
          | Alice | 30  |
          | Bob   | 25  |

  After:  - Alice : 30 ans
          - Bob : 25 ans
  ```

  Ideal for: rating tables (★★★), XP rewards, simple key-value pairs.

- Option B: Prose format
  ```
  After:  Alice (30 ans) et Bob (25 ans).
  ```

- **Option E (recommended for complex tables):** Keep as markdown
  ```
  When: Table has 4+ columns OR contains structured data important for layout
  Action: Preserve table as-is in markdown format

  Rationale:
  - Complex tables don't convert well to lists (unreadable)
  - Markdown table syntax `| A | B | C | D |` remains readable as plain text
  - Important for LaTeX conversion (will become \begin{tabular})
  - User can manually format in Scrivener if needed

  Example (keep as-is):
  | PNJ | Rôle | Lieu | Motivation |
  |-----|------|------|------------|
  | Aldric | Aubergiste | Pont de Pierre | Protéger son commerce |
  ```

- Option C: Keep ALL tables as-is (LaTeX-first workflow)
  ```
  When: All markdown files are destined for LaTeX conversion (via Pandoc or md-to-tex)
  Action: Preserve all tables — they will be converted to \begin{tabular}
  ```

- Option D: HTML tables (if Scrivener supports HTML rendering)
  ```
  Note: Check Scrivener settings first. Most versions convert HTML to plain text.
  ```

**Decision tree:**
1. Count table columns
2. If 2-3 columns → Option A (convert to list)
3. If 4+ columns → Option E (keep as markdown)

---

**Task lists** → Flag as inappropriate for publication, then convert to simple list:

```
Before: - [ ] Todo item
        - [x] Done item

After:  - Todo item
        - Done item

Note: Task lists are working documents, not publication content.
      Ask user to review if these items belong in the final document.
```

---

**Code blocks** → Convert to:

- **Option A (recommended):** Indented monospace block (for technical publications)
  ```
  Before: ```python
          def hello():
              print("Hi")
          ```

  After:  def hello():
              print("Hi")
  ```

- Option B: Italic block with context (for narrative publications)
  ```
  After:  *Le programme affiche simplement « Hi » à l'écran.*
  ```

---

**Blockquotes** → Convert to:

- **Option A (recommended):** Italic with quotation marks
  ```
  Before: > This is a quote
          > across multiple lines

  After:  *« This is a quote across multiple lines »*
  ```

- Option B: Em-dash prefix (for dialogue or attributions)
  ```
  After:  — This is a quote across multiple lines
  ```

---

**Footnotes** → Convert to inline parenthetical:

```
Before: Here is text[^1] and more[^note].

        [^1]: First footnote
        [^note]: Named footnote

After:  Here is text (First footnote) and more (Named footnote).
```

Note: For long footnotes (>50 chars), consider moving to end of paragraph or as a separate sentence.

---

**Front matter** → Remove silently:

```
Before: ---
        title: My Doc
        author: Name
        ---

After:  (removed — no visible trace in document)
```

---

**Horizontal rules** → Convert to text separator:

- **Option A (recommended):** Blank line with centered asterisks
  ```
  Before: ---
          (or *** or ___)

  After:  * * *
  ```

- Option B: Blank lines only (minimal)
  ```
  After:  (two blank lines)
  ```

Note: Distinguish from front matter by position (front matter only at file start with valid YAML).

---

**Clickable images** → Simplify to image only:

```
Before: [![Alt text](image.png)](https://example.com)

After:  ![Alt text](image.png)
```

Note: If the link is essential, add it as a caption or footnote.

---

**Deeply nested lists (>2 levels)** → Flatten to 2 levels:

```
Before: - Level 1
          - Level 2
            - Level 3
              - Level 4

After:  - Level 1
          - Level 2
        - Level 3
          - Level 4
```

---

**Nested formatting in lists:**

```
Before: - Item with **bold** and *italic*
        - Item with [link](url)

After:  (preserved as-is — these combinations ARE supported)
```

### Step 3: Build Fix List

```
SYNTAX FIXES:
  Line X: [issue] → [fix]

CONVERSIONS:
  Line Y: [element type] → [conversion applied]

REMOVED:
  Line Z: [element] removed (front matter, etc.)

WARNINGS:
  Line W: [element inappropriate for publication — review needed]
```

### Step 4: Show Summary

```
Found N issues:
├─ X syntax errors (auto-fixable)
├─ Y unsupported elements (converted)
├─ Z elements removed
└─ W warnings requiring review

Potential information loss:
- [list any semantic losses, e.g., "table structure", "footnote numbering"]
```

### Step 5: User Confirmation

Ask: "Apply fixes? [Y/n/select/preview]"

- **Y** → apply all (Option A for conversions)
- **n** → abort
- **select** → choose which fixes and which options
- **preview** → show final result before applying

### Step 6: Apply Fixes

Apply approved fixes in order:
1. Syntax corrections
2. Character escaping
3. Element conversions
4. Element removals

### Step 7: Verify

Verify result is publication-ready:

- No unsupported Markdown elements remain
- All markers are balanced
- Special characters properly escaped
- Document structure preserved
- No lists nested beyond 2 levels
- No technical labels or meta-comments visible
- Text flows naturally for reading
