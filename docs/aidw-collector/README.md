# AIDW Collector

A lightweight kit to drop into any project and extract structured information for AIDW documentation.

## What it does

`client-extract` analyzes your project (code + existing docs) and produces a structured extraction report organized around AIDW's expected files:

- **[CLIENT.md]** — client/product context, audience, tone, constraints
- **[glossaire.md]** — technical vocabulary found in the codebase and docs
- **[overview.md]** — project purpose, scope, use cases
- **[bank.yml]** — suggested AIDW configuration

The report is intermediate: a human or LLM then creates the actual AIDW files from it.

## Usage

### 1. Copy this folder to your project

```bash
cp -r aidw-collector/ /path/to/your/project/
```

### 2. Fill in `collect.yml`

Edit `aidw-collector/collect.yml` with basic project information before running the prompt.

### 3. Run the extraction prompt

With your LLM (any — Claude, GPT, Gemini, etc.):

```
@aidw-collector/prompts/client-extract.prompt.md
```

The prompt will analyze your project structure and existing docs, then produce `aidw-collector/output/extraction-report.md`.

### 4. Use the report

Open `extraction-report.md` and use each section to create your AIDW files:

```
generationPDF/
└── <client>/
    ├── .docs/
    │   ├── CLIENT.md       ← from [CLIENT.md] section
    │   └── glossaire.md    ← from [glossaire.md] section
    └── <projet>/
        ├── bank.yml        ← from [bank.yml] section
        └── overview.md     ← from [overview.md] section
```

## Requirements

- Any LLM with file access (Claude, GPT-4, Gemini, etc.)
- No Python, no Pandoc required at this stage

## Output

`aidw-collector/output/extraction-report.md` — structured report, ready to be transformed into AIDW files.
