# SemanticGIS Intent-First Copilot Instructions

## Priorities

1. Clarify user intent before creating code or downloads when intent is ambiguous.
2. Prefer semantic alignment over tool convenience.
3. Record assumptions and decisions in `Design_Rationale.md`.
4. Keep public source manifests external (semanticgis.dk).
5. Keep logical sanctuary content separate from binary data storage.
6. Never print, persist, or commit real credentials.

## Project Structure Contract

- `.cache/`: ephemeral package caches (OSMnx, raster tiles, temporary downloads; gitignored)
- `00_Binary`: local binary data root (gitignored) with `raw/`, `staging/`, `derived/`
- `01_Scoping`: outline, stakeholder map, research questions
- `02_Modelling`: ontology, NOIR attribute catalog, data model diagram
- `03_Sanctuary`: logical manifests and lineage (`raw/` + `processed/` descriptors, no binary payloads)
- `04_Analytics`: analytical recipe and analysis plan
- `05_Outputs`: narrative and visual specs
- `10_scripts`: reusable project-local scripts (see `workflow-preferences.yaml` for default folder)

## Binary-Logical Separation Protocol

All projects must keep logic and binaries separate.
Never store binary dataset payloads in `03_Sanctuary`.

### Logical Layer (`03_Sanctuary/`)

- `03_Sanctuary/raw/` stores raw-source manifests and provenance descriptors.
- `03_Sanctuary/processed/` stores processed-layer manifests and transformation lineage.
- Logical sanctuary content may include source references (URLs, table names, connector aliases), but no secrets.

### Binary Layer (`00_Binary/`)

- `00_Binary/raw/` stores unmodified binary source data downloaded or extracted for local processing.
- `00_Binary/staging/` stores intermediate binary artifacts.
- `00_Binary/derived/` stores project-local, analysis-ready binary artifacts.
- All `00_Binary/` contents are local and gitignored except placeholder `.keep` files.

### Stage 1 — Raw

- Store the unmodified source download in `00_Binary/raw/` exactly as received.
- No reprojection, no column filtering, no geometry transformation.
- Preserve the original CRS and all original attributes.
- For each raw dataset, update or create `03_Sanctuary/raw/_manifest.md` with:
  - Filename
  - Local binary path under `00_Binary/raw/`
  - Source (URL, API, or register name)
  - Download date
  - Original CRS
  - Download tool / method
  - Licence
  - Notes (e.g., list of original attributes as they appear in the raw source)
- Raw binaries must never be overwritten by processed outputs.
- Do not document NOIR levels at the raw stage; NOIR assignment occurs during processing.

### Stage 2 — Processed

- Derive processed binaries from `00_Binary/raw/` into `00_Binary/derived/` (optionally via `00_Binary/staging/`).
- Apply reprojection, column selection, geometry normalisation, and filter rules in this stage.
- Create a processed manifest in `03_Sanctuary/processed/[dataset]_manifest.md` for each derived dataset. Include:
  - Transformation applied (e.g., CRS change, column selection, filtering rules).
  - Attribute-level NOIR levels (Nominal, Ordinal, Interval, Ratio) for all output columns.
  - Link to raw manifest row that fed this transformation.
  - Link to ontology definitions in `02_Modelling/Ontology.md`.
  - Binary path under `00_Binary/derived/`.
- Update `03_Sanctuary/Sanctuary_Index.md` to record the lineage row.
- The Sanctuary Index must record: raw manifest reference, transformation applied, processed manifest reference, NOIR coverage, and binary path.

### Enforcement rule for AI-generated scripts

Any fetch or download script must:
1. Save raw binaries to `00_Binary/raw/` first.
2. Update `03_Sanctuary/raw/_manifest.md` for provenance.
3. Transform into `00_Binary/derived/` in a separate step (may be the same script, but clearly staged).
4. Append a provenance row to `03_Sanctuary/raw/_manifest.md`.

Script responsibility boundaries must be explicit:
1. Data acquisition logic belongs in fetch/download scripts only.
2. Analytical computation belongs in analysis scripts only.
3. Mapping/chart rendering belongs in visualisation scripts only.
4. If a single orchestrator is used, it must call these stage scripts and keep stage boundaries visible.

User validation checkpoints are required after each stage:
1. Print or return a concise completion summary with key output paths.
2. Include a basic sanity check signal (for example, row count, feature count, CRS, or file-exists status).
3. Stop on stage failure and report the failing stage clearly.

Script documentation for GIS users is required:
1. Explain workflow logic in domain terms (not only implementation details).
2. State where each core task is implemented (file and function/section).
3. For OSM workflows, document task logic such as key tag filters, network type assumptions, and boundary rules.

Logging behavior must support different run contexts:
1. Default to informative/verbose terminal progress for GIS users unless quiet mode is requested.
2. Allow redirecting logs to a file for long runs or debugging.
3. Allow low-noise execution for quick operations.
4. Implementation details are flexible (for example `logging` module, structured logger, or equivalent), but mode selection must be explicit.

If a script writes binary payloads into `03_Sanctuary/` or outside the workspace without explicit user request, it violates this protocol.

## Python Environment Governance

- Always use a project-local Python environment (`.venv`, `micromamba`, `conda`, or similar).
- Never install packages into the global or system Python.
- Consult `.github/workflow-preferences.yaml` for preferred manager and priority order.
- Record environment decisions in `Design_Rationale.md` when they materially affect reproducibility.

## Modular Instruction Packs

Workflow recommendations live in `.github/instructions/*.instructions.md`. These express defaults as recommendations, preserving agent autonomy. Only items marked **Required** in those packs are hard constraints. Agents may deviate from **Recommended** defaults and should log the rationale in `Design_Rationale.md`.

## Safety Rules

- Ask before destructive actions.
- Do not expose restricted data in outputs.
- Treat `.env` as local-only.
- Secrets policy: keep credentials, tokens, connection passwords, and private DSNs in `.env` only. Use `.env.example` as the required template for which variables must be provided.
- Never store real secrets in manifests, markdown files, scripts, notebooks, or committed config files.
- Write-boundary policy: do not write files outside the project workspace unless the user explicitly requests it.
