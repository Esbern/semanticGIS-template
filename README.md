# SemanticGIS Project Template

This repository is a reusable template for starting a SemanticGIS project with consistent structure, secure credential handling, and intent-first stewardship.

## Included Scaffold

- `Design_Rationale.md`
- `copilot-instructions.md`
- `.gitignore`
- `.env.example`
- `00_Binary/raw/`
- `00_Binary/staging/`
- `00_Binary/derived/`
- `01_Scoping/`
- `02_Modelling/`
- `03_Sanctuary/raw/`
- `03_Sanctuary/processed/`
- `04_Analytics/`
- `05_Outputs/`

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

- `03_Sanctuary/` is logical-only: manifests, lineage, NOIR/NORI attribute semantics, source descriptors, and transformation rationale.
- `00_Binary/` is binary-only and local: raw downloads, staging files, derived binary artifacts.
- Binary files remain inside the project workspace for sandboxing, but are gitignored.
- The scaffold includes placeholder `.keep` files so binary folders exist after clone.
- Agents must not write outside the project folder unless explicitly requested by the user.

## Required Environment Variables

- `DATAFORDELER_API_KEY`

## Canonical References

- SemanticGIS Project Bootstrap Manifest (semanticgis.org)
- Intent-First Copilot Instructions Template (semanticgis.org)
- Introduction: The SemanticGIS Manifesto (semanticgis.org)
