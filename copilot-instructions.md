# SemanticGIS Intent-First Copilot Instructions

## Priorities

1. Clarify user intent before creating code or downloads when intent is ambiguous.
2. Prefer semantic alignment over tool convenience.
3. Record assumptions and decisions in `Design_Rationale.md`.
4. Keep public source manifests external (semanticgis.dk).
5. Keep local sanctuary focused on sanitized project outputs.
6. Never print, persist, or commit real credentials.

## Project Structure Contract

- `01_Scoping`: outline, stakeholder map, research questions
- `02_Modelling`: ontology, NOIR attribute catalog, data model diagram
- `03_Sanctuary`: local raw/processed project data (see Two-Stage Sanctuary Protocol below)
- `04_Analytics`: analytical recipe and analysis plan
- `05_Outputs`: narrative and visual specs

## Two-Stage Sanctuary Protocol

All data passing through `03_Sanctuary` must follow a strict two-stage pipeline.
Never write directly to `processed/` without first archiving a raw copy.

### Stage 1 — Raw (`03_Sanctuary/raw/`)

- Store the unmodified source download exactly as received.
- No reprojection, no column filtering, no geometry transformation.
- Preserve the original CRS and all original attributes.
- For each raw file, update or create `03_Sanctuary/raw/_manifest.md` with:
  - Filename
  - Source (URL, API, or register name)
  - Download date
  - Original CRS
  - Download tool / method
  - Licence
- Raw files must never be derived from, or overwritten by, processed outputs.

### Stage 2 — Processed (`03_Sanctuary/processed/`)

- Derive only from files already present in `03_Sanctuary/raw/`.
- Apply reprojection, column selection, geometry normalisation, and filter rules here.
- Each processed file must be traceable back to one or more raw files via the Sanctuary Index (`03_Sanctuary/Sanctuary_Index.md`).
- The Sanctuary Index must record: raw source filename, transformation applied, processed output filename.

### Enforcement rule for AI-generated scripts

Any fetch or download script must:
1. Save the raw response to `03_Sanctuary/raw/` first.
2. Transform into `03_Sanctuary/processed/` in a separate step (may be the same script, but clearly staged).
3. Append a provenance row to `03_Sanctuary/raw/_manifest.md`.

If a script writes directly to `processed/` without a corresponding raw file, it violates this protocol.

## Safety Rules

- Ask before destructive actions.
- Do not expose restricted data in outputs.
- Treat `.env` as local-only.
