# SemanticGIS Intent-First Copilot Instructions

## Priorities

1. Clarify user intent before creating code or downloads when intent is ambiguous.
2. Prefer semantic alignment over tool convenience.
3. Record assumptions and decisions in `Design_Rationale.md`.
4. Keep public source manifests external (semanticgis.dk).
5. Keep logical sanctuary content separate from binary data storage.
6. Never print, persist, or commit real credentials.

## Project Structure Contract

- `00_Binary`: local binary data root (gitignored) with `raw/`, `staging/`, `derived/`
- `01_Scoping`: outline, stakeholder map, research questions
- `02_Modelling`: ontology, NOIR attribute catalog, data model diagram
- `03_Sanctuary`: logical manifests and lineage (`raw/` + `processed/` descriptors, no binary payloads)
- `04_Analytics`: analytical recipe and analysis plan
- `05_Outputs`: narrative and visual specs

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
- Raw binaries must never be overwritten by processed outputs.

### Stage 2 — Processed

- Derive processed binaries from `00_Binary/raw/` into `00_Binary/derived/` (optionally via `00_Binary/staging/`).
- Apply reprojection, column selection, geometry normalisation, and filter rules in this stage.
- Each processed dataset must be traceable in `03_Sanctuary/processed/` and `03_Sanctuary/Sanctuary_Index.md`.
- The Sanctuary Index must record: raw manifest reference, transformation applied, processed manifest reference, and binary path.

### Enforcement rule for AI-generated scripts

Any fetch or download script must:
1. Save raw binaries to `00_Binary/raw/` first.
2. Update `03_Sanctuary/raw/_manifest.md` for provenance.
3. Transform into `00_Binary/derived/` in a separate step (may be the same script, but clearly staged).
4. Append a provenance row to `03_Sanctuary/raw/_manifest.md`.

If a script writes binary payloads into `03_Sanctuary/` or outside the workspace without explicit user request, it violates this protocol.

## Safety Rules

- Ask before destructive actions.
- Do not expose restricted data in outputs.
- Treat `.env` as local-only.
- Secrets policy: keep credentials, tokens, connection passwords, and private DSNs in `.env` only. Use `.env.example` as the required template for which variables must be provided.
- Never store real secrets in manifests, markdown files, scripts, notebooks, or committed config files.
- Write-boundary policy: do not write files outside the project workspace unless the user explicitly requests it.
