---
description: "Workflow recommendation for OpenStreetMap data acquisition and analysis in this template. Use when sourcing OSM for mobility or accessibility tasks."
applyTo: "**/*.{py,ipynb,md}"
---

# OSM Workflow Recommendation

Recommended
- Prefer `OSMnx` as the default OSM acquisition path for Python-based analysis.
- Let library defaults choose the Overpass endpoint unless there is a documented reason to override.
- Use direct `curl` + raw Overpass queries primarily as fallback or for reproducible low-level debugging.

Required
- Store raw binaries in `00_Binary/raw/` first.
- Keep `03_Sanctuary/` logical-only (manifests, lineage, descriptors), with no binary payloads.
- Log provenance in `03_Sanctuary/raw/_manifest.md` and lineage in `03_Sanctuary/Sanctuary_Index.md`.

Fallback
- If OSMnx retrieval fails or rate limits persist, retry with an alternative endpoint and capture the rationale in `Design_Rationale.md`.
