---
name: extract
description: Extract and distribute PDF content into project resources
argument-hint: <project-path> <source-document>
---

# Extract PDF Content Prompt

## Goal

Extract content from a PDF and distribute it into appropriate project resources.

## Context

### Project Configuration

```yaml
@$PROJECT/bank.yml
```

### Source Path

`$SOURCE` 
## Rules

- Output in user's preferred language
- Preserve original content faithfully
- NEVER invent content not in source
- Verify each extracted file against source
- Ask user validation before writing files
- **Store intermediate results in `docs/extraction/<source-name>/`**
- **One chunk per session for large PDFs**

---

## Phase A: Setup (Session 1)

### Step A.1: Validate Environment

1. Check `$PROJECT/bank.yml` exists
   - IF missing → STOP, ask user to create or specify project
2. Extract fields from bank.yml:
   - `client` (required)
   - `document.name` (for reference)
3. Verify `$SOURCE` file exists and is readable
4. Store source basename: `<source-name>` = filename without extension

5. **Validate required tools:**
   ```bash
   # Cross-platform check
   python -c "import shutil; print('[OK] pdftotext' if shutil.which('pdftotext') else '[WARN] pdftotext not found')"
   python -c "import shutil; print('[OK] tesseract' if shutil.which('tesseract') else '[WARN] tesseract not found')"
   ```
   - IF both missing → STOP, ask user to install at least one

6. **Error conditions:**
   - Missing bank.yml → STOP with clear message
   - Missing $SOURCE → STOP with clear message
   - Unreadable PDF → STOP, suggest checking file

### Step A.2: Create Extraction Workspace

```bash
# Cross-platform (Python)
python -c "from pathlib import Path; Path('docs/extraction/<source-name>/chunks').mkdir(parents=True, exist_ok=True)"
python -c "from pathlib import Path; Path('docs/extraction/<source-name>/raw').mkdir(parents=True, exist_ok=True)"
python -c "from pathlib import Path; Path('docs/extraction/<source-name>/classified').mkdir(parents=True, exist_ok=True)"
```

### Step A.3: Estimate and Split (PDF only)

IF `$SOURCE` is a PDF file:

1. **Get PDF info**:
   ```bash
   python scripts/split-pdf.py "$SOURCE" --info
   ```

2. **Estimate context fit** (limite conservative: 50000 chars):
   ```bash
   python scripts/split-pdf.py "$SOURCE" --estimate --context-limit 50000
   ```

3. **Evaluate**:

   | Chunks Needed | Action |
   |---------------|--------|
   | 1 | Proceed to Phase B in same session |
   | 2+ | Split PDF, create progress.md, END session |

4. **IF splitting required**:

   a. Show estimation:
      ```
      Ce PDF contient ~X caracteres (Y pages).
      Limite par session: 50000 caracteres (~20 pages).
      Decoupage: Z chunks de N pages.
      ```

   b. Execute split:
      ```bash
      python scripts/split-pdf.py "$SOURCE" --pages-per-chunk 20 --output-dir "docs/extraction/<source-name>/chunks"
      ```

      c. Create progress file with full context:

      ```markdown
      # Extraction Progress: <source-name>

      ## Context
            - **Source**: $SOURCE
      - **Project**: $PROJECT
      - **Client**: <client from bank.yml>
      - **Document**: <document.name from bank.yml>
      - **Created**: YYYY-MM-DD

      ## Tools Available

      - pdftotext: [yes/no]
      - tesseract: [yes/no]

      ## Chunks Status

      | Chunk | Pages | Chars | Status | Date |
      |-------|-------|-------|--------|------|
      | chunk_01.pdf | 1-20 | ~X | pending | - |
      | chunk_02.pdf | 21-40 | ~X | pending | - |
      | ... | ... | ... | pending | - |

      ## Classification Limits
            Max per file: 30000 chars. Split if exceeded.

      ## Resume Command

      Nouvelle session:
      ```
      Reprendre: docs/extraction/<source-name>/progress.md
      ```
      ```

   d. **END SESSION**

---

## Phase B: Extract Chunks

> **IMPORTANT**: Traiter tous les chunks pending en séquence dans la même session si le contexte le permet.
> Lire `progress.md` pour le contexte complet.
> Passer à Phase C dès que tous les chunks sont `done`.

### Step B.0: Load Context

1. Read `docs/extraction/<source-name>/progress.md`
2. Extract:
   - `Client` → for classification destinations
   - `Project` → for final distribution
   - `Tools Available` → for extraction method
3. Identify next pending chunk

4. **Error conditions:**
   - No pending chunks → go to Phase C
   - progress.md missing → STOP, corrupted state

### Step B.1: Extract Text

```bash
pdftotext -layout "docs/extraction/<source-name>/chunks/chunk_XX.pdf" -
```

IF garbled (>30% non-printable):
```bash
tesseract "docs/extraction/<source-name>/chunks/chunk_XX.pdf" stdout -l fra
```

Save raw text:
```bash
# → docs/extraction/<source-name>/raw/chunk_XX.txt
```

### Step B.2: Preview Classification

1. Show first 500 chars of extracted text
2. Identify main sections visually
3. **Pour le chunk 1 uniquement**: présenter le plan de classification et demander confirmation [Y/n]
4. **Pour les chunks suivants**: enchaîner sans demander (le pattern est validé)

IF user wants adjustments → note them before classifying

### Step B.3: Classify Content

Scan text and tag sections:

| Category | Keywords/Patterns | File |
|----------|-------------------|------|
| ClientInfo | "présentation", "à propos", "historique", "contexte", "mission", "valeurs" | `classified/client-info.md` |
| Terminology | "glossaire", "définition:", terme=explication, "signifie" | `classified/terminology.md` |
| Style | "ton", "style", "écriture", "éviter", "charte", "couleur", "police", "logo" | `classified/style.md` |
| Architecture | "architecture", "API", "endpoint", "composant", "flux", "spécification", "procédure", "étape" | `classified/architecture.md` |
| Structure | "chapitre", "partie", "sommaire", "plan", "section" | `classified/structure.md` |
| Templates | patterns récurrents, exemples type, modèles de documents | `classified/templates.md` |

**Append with YAML front matter:**
```markdown
---
chunk: XX
pages: N-M
extracted: YYYY-MM-DD
---

[content here]

---
```

**Size check:**
- After appending, check file size
- IF file > 30000 chars → split into `lore-1.md`, `lore-2.md`, etc.

### Step B.4: Update Progress

1. Update `progress.md`:
   - Mark chunk as `done`
   - Add today's date
   - Note character count

2. Show summary:
   ```
   Chunk XX: Y sections, Z chars
   Progress: X/N (XX%)
   Next: chunk_YY
   ```

3. **Si d'autres chunks sont pending → retourner à B.0 pour le chunk suivant**
4. **Si tous les chunks sont done → passer à Phase C**

---

## Phase C: Distribute

> Execute when ALL chunks are `done` in progress.md

### Step C.0: Validate Completion

1. Read `progress.md`
2. Check all chunks are `done`
3. IF any `pending` → STOP, list missing

4. Calculate total classified content size
5. IF total > 80000 chars → warn user, process in batches

### Step C.1: Merge Classified Files

For each file in `classified/`:

1. Read content
2. Remove YAML front matter markers 3. Deduplicate identical sections
4. Prepare merged version

### Step C.2: Prepare Rollback

**Stash current state before writing (cross-platform):**

```bash
# git -C works on Windows and Unix
git -C "<client>" stash push -m "pre-extraction-<source-name>"
git -C "<project>" stash push -m "pre-extraction-<source-name>"
```

Store stash refs for potential rollback.

### Step C.3: Distribute to Destinations

| Source | Destination | Strategy |
|--------|-------------|----------|
| `client-info*.md` | `<client>/.docs/CLIENT.md` | append with `---` separator |
| `terminology*.md` | `<client>/.docs/glossaire.md` | merge by term |
| `style*.md` | `<client>/.output-styles/<source>.md` | create new |
| `architecture*.md` | `<client>/.docs/architecture.md` | create/append |
| `structure*.md` | `<project>/.toc/INDEX.md` | create/update |
| `templates*.md` | `<client>/.templates/` | append |

For each:
1. Show preview (first 500 chars + diff if exists)
2. **WAIT FOR USER APPROVAL**
3. Write file

### Step C.4: Validate or Rollback

**Ask user:** "Valider les modifications ? [Y/n/diff]"

IF `Y` (validate):
```bash
git -C "<client>" add .docs/ .output-styles/ .templates/
git -C "<client>" commit -m "Extract: <source-name>"
git -C "<project>" add .
git -C "<project>" commit -m "Extract: <source-name>"
# Drop stash (no longer needed)
git -C "<client>" stash drop
git -C "<project>" stash drop
```

IF `n` (rollback):
```bash
git -C "<client>" checkout .
git -C "<client>" stash pop
git -C "<project>" checkout .
git -C "<project>" stash pop
```
→ Files restored to pre-extraction state

IF `diff` (review):
```bash
git -C "<client>" diff
git -C "<project>" diff
```
→ Show changes, ask again

### Step C.5: Generate Report

```markdown
# Extraction Report: <source-name>

## Summary

- Source: [path]
- Project: [path]
- Univers: [name]
- Chunks: [N]
- Total chars: [X]

## Distribution

| Category | Sections | Chars | Destination | Action |
|----------|----------|-------|-------------|--------|
| ClientInfo | X | Y | .docs/CLIENT.md | appended |
| ... | ... | ... | ... | ... |

## Coverage

- Extracted: X chars
- Total: Y chars
- Coverage: XX%
```

### Step C.6: Cleanup

```bash
# Cross-platform (Python)
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/chunks', ignore_errors=True)"
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/raw', ignore_errors=True)"
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/classified', ignore_errors=True)"
python -c "from pathlib import Path; from datetime import date; Path('docs/extraction/<source-name>/progress.md').rename(f'docs/extraction/<source-name>/DONE-{date.today()}.md')"
```

**Final message:**
```
Extraction complete!
Files: [list]
Archive: docs/extraction/<source-name>/DONE-YYYYMMDD.md
```

---

## Quick Reference

```bash
# New extraction
@docs/prompts/workshop/extract.prompt.md <project> <source.pdf>

# Resume (after /clear or new session)
Reprendre: docs/extraction/<source-name>/progress.md

# Check progress
cat docs/extraction/<source-name>/progress.md
```

## Lightweight Resume

Pour reprendre sans recharger tout le contexte:
```
Reprendre extraction <source-name>, chunk N
```

Le prompt lira uniquement progress.md et le chunk specifié.
