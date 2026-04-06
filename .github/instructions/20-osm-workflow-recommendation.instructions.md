---
description: "Workflow recommendation for OpenStreetMap data acquisition and analysis in this template. Use when sourcing OSM for mobility or accessibility tasks."
applyTo: "**/*.{py,ipynb,md}"
---

# OSM Workflow Recommendation

Recommended
- Prefer `OSMnx` as the default OSM acquisition path for Python-based analysis.
- Configure OSMnx to cache into the project `.cache/` folder: `ox.settings.cache_folder = ".cache/"`.
- Let library defaults choose the Overpass endpoint unless there is a documented reason to override.
- Use direct `curl` + raw Overpass queries primarily as fallback or for reproducible low-level debugging.
- Keep OSM workflows split into stage scripts where practical:
	- fetch/download
	- analysis/modeling
	- visualisation/export
- Deliver OSM workflows stage-by-stage by default, not as a pre-authored full pipeline.

Required
- Store raw binaries in `00_Binary/raw/` first.
- Keep `03_Sanctuary/` logical-only (manifests, lineage, descriptors), with no binary payloads.
- Log provenance in `03_Sanctuary/raw/_manifest.md` and lineage in `03_Sanctuary/Sanctuary_Index.md`.
- Do not generate downstream OSM stage scripts before the prior stage has completed and been validated.
- Wait for user confirmation between OSM stages unless the user explicitly requests full pipeline creation.
- After each OSM stage, emit a user validation checkpoint including output paths and at least one sanity metric.
- Document OSM task logic in user-facing terms, including:
	- key tag filters or query scope
	- network type assumptions (walk, bike, drive)
	- geometry/boundary clipping rules
	- where this logic is implemented in script files

Fallback
- If OSMnx retrieval fails or rate limits persist, retry with an alternative endpoint and capture the rationale in `Design_Rationale.md`.
