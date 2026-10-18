# Huntly

<p align="center">
  <strong>LOCAL INTELLIGENCE · CONTROLLED OUTREACH · ZERO BLACK BOXES</strong>
</p>

<p align="center">
  <a href="https://github.com/atharhive/Huntly/actions/workflows/tests.yml">
    <img src="https://img.shields.io/github/actions/workflow/status/atharhive/Huntly/tests.yml?branch=main&style=for-the-badge&label=CI" alt="CI status">
  </a>
  <a href="https://github.com/atharhive/Huntly">
    <img src="https://img.shields.io/github/stars/atharhive/Huntly?style=for-the-badge&color=00d9ff&label=STARS" alt="GitHub stars">
  </a>
  <img src="https://img.shields.io/badge/status-beta-8b5cf6?style=for-the-badge" alt="Beta status">
  <img src="https://img.shields.io/badge/python-3.11%2B-3776ab?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.11 or newer">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Claude-native-111827?style=flat-square&logo=anthropic&logoColor=white" alt="Claude native">
  <img src="https://img.shields.io/badge/Codex-opt--in-111827?style=flat-square&logo=openai&logoColor=white" alt="Codex opt-in">
  <img src="https://img.shields.io/badge/FastAPI-dashboard-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI dashboard">
  <img src="https://img.shields.io/badge/SQLite-state-003b57?style=flat-square&logo=sqlite&logoColor=white" alt="SQLite state">
  <img src="https://img.shields.io/badge/Playwright-browser-2ead33?style=flat-square&logo=playwright&logoColor=white" alt="Playwright browser automation">
  <img src="https://img.shields.io/badge/Docker-ready-2496ed?style=flat-square&logo=docker&logoColor=white" alt="Docker ready">
  <img src="https://img.shields.io/badge/pytest-tested-0a9edc?style=flat-square&logo=pytest&logoColor=white" alt="Tested with pytest">
</p>

<p align="center">
  <code>DISCOVER</code> → <code>PROFILE</code> → <code>QUALIFY</code> → <code>WRITE</code> → <code>REVIEW</code> → <code>RESPOND</code>
</p>

Huntly is a local sales-workflow engine for teams that want research, message
drafting, delivery, and reply handling in one inspectable process.

It is deliberately file-oriented:

- `huntly.yaml` describes the operator, offer, audience, and operating limits.
- `prompts/` contains the agent instructions.
- `skills/` contains reusable sales knowledge and generated product context.
- `data/` contains the SQLite state created at runtime.
- `huntly/web/` contains the dashboard served by the local process.

## Start here

### 1. Create an environment

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e .
```

### 2. Add local credentials

```bash
cp .env.example .env
```

Fill only the integrations you intend to use. Keep `.env` local and never add
it to source control.

### 3. Describe the offer

Edit `huntly.yaml`, or train from a public product site:

```bash
huntly train https://example.com
```

Review the generated files in `skills/` before sending anything.

### 4. Inspect the system

```bash
huntly status
huntly signals
huntly discover --estimate
```

Estimation commands do not start a paid discovery run. Confirm the signals and
provider choices before collecting data.

### 5. Run the local dashboard

```bash
huntly dashboard
```

The dashboard is available at `http://localhost:5555`. It is intended for the
same machine that runs Huntly.

## Operating model

Each heartbeat performs the smallest useful next action:

1. check quiet hours, provider health, budget, and sending limits;
2. process replies and delivery events;
3. drain approved outbound messages;
4. write campaigns for deliverable contacts;
5. discover and profile new companies;
6. record the result in SQLite.

The order is deterministic. A model is used for research interpretation,
message composition, and reply classification, not for deciding whether safety
limits should be bypassed.

Start the loop with:

```bash
huntly run
```

Use the dashboard or `huntly outbox` to review pending messages when approval
mode is enabled.

## Providers

Email can be connected through the provider configured in `huntly.yaml`.
Native Gmail and SMTP modes use the local outbox and approval workflow.
Instantly remains available for installations that already use it.

The thinking provider is selected explicitly under `brain`. Huntly does not
silently switch providers when the selected one is unavailable. Claude is the
normal default; Codex can be selected when its supported CLI and login are
available; Gemini can be selected with a `GEMINI_API_KEY` from Google AI
Studio. Gemini calls use plain text only and do not receive tools.

## Guardrails

Huntly is designed to make outbound work reviewable:

- quiet hours and daily limits are enforced before sending;
- unverified guesses are not treated as deliverable addresses;
- native providers stage outgoing messages in the outbox;
- replies and bounces can stop future messages;
- discovery estimates spend before a provider is called;
- local SQLite records the decisions and observations.

These controls reduce operational risk; they do not replace applicable
anti-spam, privacy, or marketing law.

## Useful commands

```text
huntly setup                 configure the local profile
huntly train <url>           learn product and audience context
huntly status                show pipeline and provider state
huntly usage                 show model usage recorded by Huntly
huntly signals               review or confirm collection signals
huntly discover              collect companies from configured sources
huntly export                write deliverable contacts to CSV
huntly outbox                review, approve, or reject staged messages
huntly sending pause         stop outbound delivery
huntly sending resume        resume outbound delivery
```

## Repository map

| Path | Purpose |
| --- | --- |
| `huntly/` | Application code and integrations |
| `huntly/agents/` | Scout, writer, sender, handler, and analyst workers |
| `huntly/collectors/` | Discovery and company profiling |
| `huntly/models/` | Data models used by the state layer |
| `huntly/web/` | Dashboard assets |
| `prompts/` | Runtime prompt templates |
| `skills/` | Runtime sales knowledge |
| `tests/` | Automated behavior tests |

## Development

Run the suite from the project environment:

```bash
python -m pytest -q
```

The project has no required external service for its unit tests. Integration
credentials and browser binaries are only needed for the workflows that use
them.
