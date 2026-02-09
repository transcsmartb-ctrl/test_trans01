# Construction Safety API Schemas

This repository contains the API surface and JSON Schemas for a construction safety platform.  
It defines OAuth endpoints, core API endpoints, normative registries, and JSON Schema definitions for devices, software, measurements, and alerts.

## Folder structure

- `oauth/`  
  - `token.json`, `authorize.json`: Example or canonical request/response payloads for OAuth token and authorization endpoints.

- `api/`  
  - `mediaToken.json`: Example or canonical payload for media token.  
  - `v1/basic-info/`  
    - `orgInfo.json`, `zoneInfo.json`: Example payloads for organisation and zone information.  
  - `v1/category4s.json`, `v1/devices.json`, `v1/software.json`, `v1/measurements.json`, `v1/alerts.json`: Example payloads for key API resources.  
  - `v1/alerts/media.json`: Example payload for alert media.

- `.well-known/`  
  - `schema-registry.json`: Entry point URL list for schema registry discovery (used by clients to find JSON Schemas).

- `docs/`  
  - `index.html`: Human‑readable API documentation.

- `registry/`  
  - `current`: Symlink or redirect to the latest registry version.  
  - `v1/catalog.json`: Master catalog of all available schemas in this version.  
  - `v1/normative/`: Normative lists (authoritative enums)  
    - `categories.json`, `alert-types.json`, `measurement-types.json`, `device-types.json`.  
  - `v1/schemas/`: JSON Schemas for all resources  
    - `oauth/`: `token.schema.json`  
    - `mediaToken.schema.json`  
    - `basic-info/`: `orgInfo.schema.json`, `zoneInfo.schema.json`  
    - `category4s.schema.json`  
    - `devices/` and `devices/category4s/`: device base schema and per‑category specialisations (smart‑monitoring-device, tower-crane, mobile-plant-operation, ai-monitoring, confined-space, lock-key, scaffold-coupler, mewp, others).  
    - `software/` and `software/category4s/`: software base schema and per‑category specialisations (e‑permit, digitalised-tracking-system, vr-training, others).  
    - `measurements.json` plus `measurements/category4s/*`: measurement base schema and per‑category specialisations.  
    - `alerts.json` plus `alerts/category4s/*`: alert base schema and per‑category specialisations (including e‑permit and digitalised-tracking-system alert schemas).

## How clients should use the schemas

1. Retrieve `.well-known/schema-registry.json` from the API domain to discover the registry base URL.  
2. Call `registry/current` (or a fixed version like `registry/v1/catalog.json`) to list available schemas and their URIs.  
3. Resolve the specific JSON Schema (for example, a device or alert type) using the catalog entries.  
4. Validate request/response payloads against the corresponding JSON Schema before sending/accepting them.

## Versioning

- `registry/current` should always point to the latest stable version (e.g. `registry/v1`).  
- New breaking versions should be added as `registry/v2`, `v3`, etc., with their own `catalog.json` and `normative` lists, keeping previous versions immutable.

## Contributing

- Propose changes to schemas via pull requests.  
- For new device/software/measurement/alert categories, add:  
  - An entry in the appropriate `normative/*.json` list.  
  - A new schema file under the corresponding `schemas/.../category4s/` folder.  
- Keep example payloads under `api/` in sync with the JSON Schemas in `registry/v*/schemas/`.
