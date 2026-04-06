# Scripts

Use this folder for reusable project scripts.

Guidelines
- Keep scripts task-focused and named by intent.
- Prefer domain separation in multi-stage workflows:
	- `fetch_*.py` for acquisition/download
	- `analyze_*.py` for analysis/modeling
	- `visualize_*.py` for maps/charts/exports
- Prefer idempotent behavior where practical.
- Document script purpose and logic in `Design_Rationale.md`.
- After each stage, print a short completion summary with output paths and one sanity metric.

Documentation expectations (GIS users)
- Explain the workflow logic and why each stage exists.
- Point to where key task logic lives (file and function/section).
- For OSM scripts, document query/tag scope, network assumptions, and clipping/boundary rules.

Logging expectations
- Default output should be verbose enough for users to validate progress.
- Provide a quiet mode for simple runs.
- Provide file logging for long/debug runs (prefer `.cache/logs/`).
- Recommended CLI switches: `--log-level`, `--quiet`, `--log-file`.

Examples
- `fetch_osm_data.py`
- `prepare_network_layers.py`
- `run_accessibility_analysis.py`
