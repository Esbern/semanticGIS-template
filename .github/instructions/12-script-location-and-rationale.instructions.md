---
description: "Workflow recommendation for script placement and rationale logging. Use when creating Python, shell, SQL, or utility scripts for project workflows."
applyTo: "**/*.{py,sh,sql,md,yml,yaml}"
---

# Script Location And Rationale

Recommended
- Save reusable project scripts in the configured script folder from `.github/workflow-preferences.yaml`.
- Use a clear filename that signals purpose, for example `fetch_osm_data.py` or `build_network_graph.py`.

Required
- Keep scripts inside the workspace boundary.
- Document the overall script logic and intended use in `Design_Rationale.md`.

Fallback
- If the configured script folder is missing, create it and add a short local README.
- If a script is one-off and intentionally not reusable, note that decision in `Design_Rationale.md`.
