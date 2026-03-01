---
name: publish-itch
description: Publish PDF to itch.io using butler and project configuration
argument-hint: project="<univers>/<nom-projet>" [version="vX.Y"]
---

# Publish to itch.io

## Goal

Publish a PDF project to itch.io using the butler CLI and project configuration from `itch-projects.json`.

## Prerequisites

- Butler CLI installed (`.claude/scripts/butler/butler.exe`)
- Butler authenticated (`butler login` done once)
- Project exists on itch.io
- Project configured in `.claude/itch-projects.json`

## Arguments

- **project**: Path to project (required)
- **version**: Version string (optional, auto-detected from git tag)
- **message**: Release message (optional)

## Steps

### Step 1: Load Project Configuration

Read `.claude/itch-projects.json` and find project by `path`.

**If found:** Use existing config (username, itch_project)
**If not found:** Ask user to add project to config

### Step 2: Detect Version

1. Check for git tag: `git describe --tags --abbrev=0`
2. If no tag: use `latest-YYYY-MM-DD`
3. Or use `$ARGUMENTS_VERSION` if provided

### Step 3: Verify PDF Exists

Check `<project>/output/*.pdf`

If missing: Run `build-pdf` first

### Step 4: Execute publish-one.ps1

```powershell
.\.claude\scripts\publish-one.ps1 -Project "<project>" [-Version "<version>"] [-Message "<message>"]
```

The script automatically:
1. Reads itch-projects.json
2. Detects version from git tag
3. Uploads via butler with `--if-changed`
4. Reports URL

### Step 5: Report Success

- Version published
- PDF size
- itch.io URL
- Upload status (changed/unchanged)

## Examples

```
/publish-itch project="archipels/carnets_de_voyage"
/publish-itch project="demonslayer/fleurdumal" version="v1.2"
```

## Configuration File

**File:** `.claude/itch-projects.json`

```json
{
  "username": "tnnka",
  "projects": [
    {
      "path": "univers/nom-projet",
      "itch_project": "nom-sur-itch-io",
      "title": "Titre Complet",
      "collection": "univers"
    }
  ]
}
```

## Workflow

```
1. Work on project
2. /build-pdf project="..."
3. git tag v1.0 -m "Release"
4. /publish-itch project="..."
5. Verify on https://tnnka.itch.io/<project>
```
