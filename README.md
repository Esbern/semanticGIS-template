# SemanticGIS Project Template

This repository is a reusable template for starting a SemanticGIS project with consistent structure, secure credential handling, and intent-first stewardship.

## Included Scaffold

- `Design_Rationale.md`
- `copilot-instructions.md`
- `.gitignore`
- `.env.example`
- `.cache/` — Package cache directory (OSMnx, raster tiles, temporary downloads; gitignored)
- `00_Binary/raw/`, `staging/`, `derived/`
- `01_Scoping/` — Project outline, research questions, stakeholder map
- `02_Modelling/` — Data model diagram, NOIR attribute catalog, ontology
- `03_Sanctuary/` — Logical layer with manifests and lineage
  - `raw/` — Raw data source provenance
  - `processed/` — Processing lineage and NOIR documentation
- `04_Analytics/` — Analytical recipe and analysis plan
- `05_Outputs/` — Output narrative and visualisation spec
- `10_scripts/` — Reusable project-local scripts

## Quick Start

1. Create a new repository from this template.
2. Copy `.env.example` to `.env` and fill local values.
3. Keep `.env` local only (never commit secrets).
4. Start Phase 1 scoping and document assumptions in `Design_Rationale.md`.

### Secrets Handling (Required)

- `.env.example` is the required template for secret/config variable names.
- Real values must be stored only in local `.env` (never in git).
- Do not place secrets in sanctuary manifests, markdown documentation, scripts, or notebooks.

### Data Separation (Required)

- `03_Sanctuary/` is logical-only: manifests, lineage, NOIR attribute semantics, source descriptors, and transformation rationale.
  - `raw/_manifest.md` logs source provenance and original attributes.
  - `processed/` contains processed-layer manifests with attribute-level NOIR levels and transformation detail.
  - `Sanctuary_Index.md` tracks lineage from raw to derived binaries.
- `00_Binary/` is binary-only and local: raw downloads, staging files, derived binary artifacts.
- Binary files remain inside the project workspace for sandboxing, but are gitignored.
- The scaffold includes placeholder `.keep` files so binary folders exist after clone.
- Agents must not write outside the project folder unless explicitly requested by the user.

### NOIR Attribute Documentation (Required)

- For each dataset, document attribute-level NOIR levels (Nominal, Ordinal, Interval, Ratio) in the corresponding processed manifest.
- Link attribute semantics to the ontology in `02_Modelling/Ontology.md`.
- Use `03_Sanctuary/Sanctuary_Index.md` to track which processed manifest covers each transformation.

## Required Environment Variables

- `DATAFORDELER_API_KEY`

## Canonical References

- SemanticGIS Project Bootstrap Manifest (semanticgis.org)
- Intent-First Copilot Instructions Template (semanticgis.org)
- Introduction: The SemanticGIS Manifesto (semanticgis.org)

## Modular AI Workflow Recommendations

This template supports a two-layer instruction model:

1. `copilot-instructions.md` for compact, non-negotiable governance and safety constraints.
2. `.github/instructions/*.instructions.md` for modular workflow recommendations (tool preferences, environment defaults, fallback patterns).

Why this pattern:

- Keeps the global instruction file short and stable.
- Lets users extend or swap recommendations without editing core policy.
- Preserves agent autonomy by expressing defaults as recommendations unless explicitly required.

Included starter packs:

- `.github/instructions/10-python-local-env.instructions.md`
- `.github/instructions/11-python-env-selection-from-config.instructions.md`
- `.github/instructions/12-script-location-and-rationale.instructions.md`
- `.github/instructions/20-osm-workflow-recommendation.instructions.md`
- `.github/instructions/30-recommendation-deviation-log.instructions.md`

Add your own packs by creating new files in `.github/instructions/` with clear `description` and `applyTo` frontmatter.

Organization-level defaults can be configured in `.github/workflow-preferences.yaml`.
