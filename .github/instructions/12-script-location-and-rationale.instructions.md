---
description: "Workflow recommendation for script placement and rationale logging. Use when creating Python, shell, SQL, or utility scripts for project workflows."
applyTo: "**/*.{py,sh,sql,md,yml,yaml}"
---

# Script Location And Rationale

Recommended
- Save reusable project scripts in the configured script folder from `.github/workflow-preferences.yaml`.
- Use a clear filename that signals purpose, for example `fetch_osm_data.py` or `build_network_graph.py`.
- Use domain-separated script names for major workflows:
	- acquisition stage (for example `fetch_*.py`)
	- analysis stage (for example `analyze_*.py`)
	- visualisation stage (for example `visualize_*.py`)
- Default to user-friendly verbose terminal output unless the user requests quiet mode.
- Prefer configurable logging controls (for example `--log-level`, `--quiet`, `--log-file`) so the same script supports:
	- interactive verbose runs
	- long-running debug sessions with file logs
	- quick low-noise operations

Required
- Keep scripts inside the workspace boundary.
- Document the overall script logic and intended use in `Design_Rationale.md`.
- Keep script responsibilities clear: do not merge data download, analysis, and visualisation into one opaque stage.
- Emit a user-readable completion checkpoint after each stage with output paths and at least one sanity metric.
- Logging behavior must be explicit and configurable; scripts should not hardcode a single verbosity mode.
- Include GIS-user-oriented documentation in script headers or companion docs:
	- workflow purpose in domain language
	- where core logic is implemented (module/function names)
	- assumptions used in spatial filtering, CRS handling, and attribute derivation

Fallback
- If the configured script folder is missing, create it and add a short local README.
- If a script is one-off and intentionally not reusable, note that decision in `Design_Rationale.md`.
