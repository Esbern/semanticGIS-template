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
- `03_Sanctuary`: local raw/processed project data
- `04_Analytics`: analytical recipe and analysis plan
- `05_Outputs`: narrative and visual specs

## Safety Rules

- Ask before destructive actions.
- Do not expose restricted data in outputs.
- Treat `.env` as local-only.
