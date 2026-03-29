# api

This directory will contain the REST API layer for zbal, including route definitions, request/response schemas, authentication middleware, and configuration endpoints.

## Planned functionality

- `POST /signals` — submit new signals for processing and attribution
- `GET /attributions/{signal_id}` — retrieve attribution results for a signal
- `GET /actors` — list known actor profiles in the lattice
- `PUT /config` — update runtime parameters (false-flag thresholds, scoring weights)

An OpenAPI specification will be published here once the API is implemented.
