# Working on Huntly

## Mission

Keep Huntly a dependable local workflow tool. Prefer small, explicit changes
that preserve operator control over discovery, drafting, and delivery.

## Before changing code

1. Read the relevant module and its tests.
2. Check how configuration is loaded from `huntly.yaml` and `.env`.
3. Check whether the behavior crosses the state, agent, integration, or
   dashboard boundary.
4. Preserve existing safety gates and user-visible error reporting.

Do not treat generated product files as source code. Training may replace
`skills/product_knowledge.md` and `skills/competitive_intel.md`.

## Local setup

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e .
python -m pytest -q
```

For dashboard work, run `huntly dashboard` and reload the local page after
editing files in `huntly/web/`.

## Design boundaries

- `huntly/state.py` owns durable application state and migrations.
- `huntly/config.py` owns validation and environment-backed configuration.
- `huntly/brain.py` owns model-provider calls and prompt assembly.
- `huntly/agents/` coordinates work; integrations perform external I/O.
- `huntly/gate.py` is a pre-send safety boundary and must remain deterministic.
- `huntly/web/` is a presentation layer over the existing application API.

Reuse existing helpers instead of duplicating provider, logging, or state
logic. Raise or report actionable failures; do not turn an error into an empty
success response.

## Validation

Use the smallest relevant test selection first, then the complete suite when a
shared module changes:

```bash
python -m pytest -q tests/test_config.py
python -m pytest -q tests/test_pipeline.py tests/test_outbox_native.py
python -m pytest -q
```

When packaging changes, also run:

```bash
HUNTLY_PACKAGE_TESTS=1 python -m pytest -q tests/test_packaging.py
```

Do not commit credentials, local databases, browser state, generated product
knowledge, or build output.
