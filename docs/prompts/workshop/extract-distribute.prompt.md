---
name: extract-distribute
description: Merge extracted content and distribute to project destinations
argument-hint: <progress-file>
---

# Distribute Extracted Content

## Context

### Progress File

```markdown
@$PROGRESS
```

## Goal

Merge all classified content, distribute to destinations, with git stash rollback.

---

## Step 0: Load Context & Validate

1. Parse progress file:
   - `Source` → original PDF path
   - `Project` → project path
   - `Client` → destination client

2. Verify ALL chunks have status `done`
   - IF any `pending` → STOP, list missing chunks

3. List files in `classified/`:
   ```bash
   dir /b "docs\extraction\<source-name>\classified"   # Windows
   ls "docs/extraction/<source-name>/classified"       # Unix
   ```

4. Calculate total size:
   - IF total > 80000 chars → warn, suggest batch processing

---

## Step 1: Merge Classified Files

For each file in `classified/`:

1. Read entire content
2. Remove YAML front matter blocks:
   ```
   ---
   chunk: XX
   pages: N-M
   extracted: YYYY-MM-DD
   ---
   ```
3. Deduplicate identical sections
4. Store merged content in memory

---

## Step 2: Prepare Rollback (Git Stash)

**Cross-platform git commands:**

```bash
# Stash client changes
git -C "<client>" stash push -m "pre-extraction-<source-name>"

# Stash project changes (if different repo)
git -C "<project>" stash push -m "pre-extraction-<source-name>"
```

Note: `git -C <path>` works on both Windows and Unix.

Store stash state for potential rollback.

---

## Step 3: Preview Distributions

For each category with content:

| Classified | Destination | Action |
|------------|-------------|--------|
| `client-info*.md` | `<client>/.docs/CLIENT.md` | append |
| `terminology*.md` | `<client>/.docs/glossaire.md` | merge |
| `style*.md` | `<client>/.output-styles/<source>.md` | create |
| `architecture*.md` | `<client>/.docs/architecture.md` | create/append |
| `structure*.md` | `<project>/.toc/INDEX.md` | create/update |
| `templates*.md` | `<client>/.templates/` | append |

**For each distribution:**

1. Show preview:
   ```
   === [Category] → [Destination] ===
   Action: [create | append | merge]
   Size: X chars
   Preview (500 chars):
   ---
   [content preview]
   ---
   ```

2. IF file exists, show what will change:
   ```bash
   git -C "<path>" diff --no-index "<existing>" "<new>"
   ```

3. **WAIT FOR USER APPROVAL** per category

---

## Step 4: Write Files

For each approved distribution:

1. Ensure parent directory exists:
   ```bash
   # Python (cross-platform)
   python -c "from pathlib import Path; Path('<dir>').mkdir(parents=True, exist_ok=True)"
   ```

2. Write content using Write tool

3. Log action

---

## Step 5: Validate or Rollback

**Ask user:** "Valider toutes les modifications ? [Y/n/diff]"

### IF `Y` (validate):

```bash
# Commit universe changes
git -C "<client>" add .docs/ .output-styles/ .templates/
git -C "<client>" commit -m "Extract: <source-name>"

# Commit project changes
git -C "<project>" add .
git -C "<project>" commit -m "Extract: <source-name>"

# Drop stashes (no longer needed)
git -C "<client>" stash drop
git -C "<project>" stash drop
```

### IF `n` (rollback):

```bash
# Restore client
git -C "<client>" checkout .
git -C "<client>" stash pop

# Restore project
git -C "<project>" checkout .
git -C "<project>" stash pop
```

→ All files restored to pre-extraction state

### IF `diff` (review changes):

```bash
git -C "<client>" diff
git -C "<project>" diff
```

→ Show all changes, then ask again

---

## Step 6: Generate Report

```markdown
# Extraction Report: <source-name>

## Summary

- Source: [original PDF path]
- Project: [project path]
- Client: [client name]
- Chunks processed: [N]
- Total characters: [X]

## Distribution

| Category | Sections | Chars | Destination | Action |
|----------|----------|-------|-------------|--------|
| ClientInfo | X | Y | .docs/CLIENT.md | appended |
| Terminology | X | Y | .docs/glossaire.md | merged |
| Style | X | Y | .output-styles/... | created |
| Architecture | X | Y | .docs/architecture.md | created |
| Structure | X | Y | .toc/INDEX.md | created |
| Templates | X | Y | .templates/... | appended |

## Git Commits

- `<client>`: [commit hash] "Extract: <source-name>"
- `<project>`: [commit hash] "Extract: <source-name>"

## Coverage

- Extracted: X chars
- Total source: Y chars
- Coverage: XX%

## Files Modified

1. `<path>` - [action]
2. ...
```

---

## Step 7: Cleanup

**Remove extraction workspace (cross-platform):**

```bash
# Python (works on Windows and Unix)
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/chunks', ignore_errors=True)"
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/raw', ignore_errors=True)"
python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>/classified', ignore_errors=True)"
```

**Rename progress to archive:**

```bash
# Python (cross-platform)
python -c "from pathlib import Path; from datetime import date; Path('docs/extraction/<source-name>/progress.md').rename(f'docs/extraction/<source-name>/DONE-{date.today()}.md')"
```

---

## Final Message

```
Extraction complete!

Fichiers crees/modifies:
- [list with paths]

Commits:
- <client>: [hash]
- <project>: [hash]

Archive: docs/extraction/<source-name>/DONE-YYYY-MM-DD.md

Pour supprimer l'archive:
  python -c "import shutil; shutil.rmtree('docs/extraction/<source-name>')"
```
