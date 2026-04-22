# SemanticGIS Hub Interface Contract (v1)

This contract defines the boundary between a project created from this template and shared knowledge hosted in semanticgis.dk and semanticgis.org.

## Purpose

A project should not rediscover service type, auth mode, geometry fields, or query execution patterns at runtime. Those belong to shared source contracts on semanticgis hubs and are consumed as inputs by the local project.

## Interface Boundary

### Hub roles (shared, canonical)

- semanticgis.dk is the canonical hub for Danish register and Danish service contracts.
- semanticgis.org is the canonical hub for international and cross-country source contracts.
- OSM and satellite imagery contracts are normally resolved from semanticgis.org, even in a fully Danish project.
- A single project may use both hubs in the same workflow.

### What must live in shared hubs (canonical)

- Dataset capability catalog with service family per dataset (GraphQL, WMS, WMTS, WFS, file download), canonical endpoint URLs, and register codes/versioning.
- Query execution contracts covering pagination style and limits, required temporal arguments, filterability rules, and known error classes.
- Map-ready geometry contracts describing geometry field paths, geometry encoding, native CRS, target CRS, and transformation notes.
- Auth contracts by service type, including GraphQL conventions, map-service conventions, and token-scope caveats.
- Semantic decomposition linking leaf to dataset to service with reusable query anchors.

### What must live in the project template instance (local, execution-specific)

- User intent and scoping, including research question, area/time scope, and assumptions.
- Local credentials and runtime configuration in .env only.
- Binary data and derived artifacts under 00_Binary/.
- Local run lineage and decisions in sanctuary manifests and design rationale logs.
- Experiment outputs such as result tables, GeoJSON, web maps, and narrative exports.

## Startup Handshake (required for each new project)

At project start, the agent should resolve these checks before coding extraction logic:

1. Hub routing check.
2. Dataset capability check.
3. Query contract check.
4. Geometry contract check.
5. Auth contract check.
6. Artifact contract check.

Expected check outputs:

- Hub routing check assigns a source hub per source type (semanticgis.dk or semanticgis.org).
- Dataset capability check confirms service family and endpoint from hub contracts.
- Query contract check confirms pagination, required temporal args, and allowed filters.
- Geometry contract check confirms geometry field and CRS handling for map outputs.
- Auth contract check confirms auth parameter semantics per service.
- Artifact contract check predeclares expected output artifacts and schema in 03_Sanctuary/processed/.

## Minimum Project Intake Record

Create a project-local intake note that stores:

- question_id
- selected_leaf
- selected_dataset
- selected_hub
- selected_service_type
- contract_refs (URLs or local mirrored files)
- planned_outputs

## Anti-Patterns

Do not:

- Infer service type by trial-and-error during core execution.
- Probe unknown geometry fields repeatedly per project.
- Mix auth assumptions across GraphQL and map services.
- Leave output schemas implicit.

## Related Local Files

- 03_Sanctuary/Sanctuary_Index.md
- 03_Sanctuary/raw/_manifest.md
- 03_Sanctuary/intake/query_run_intake.template.json
- 03_Sanctuary/intake/README.md
- 03_Sanctuary/processed/query_artifact_manifest.template.json
- 04_Analytics/Analytical_Recipe.md
