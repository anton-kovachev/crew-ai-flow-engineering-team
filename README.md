# Flow Engineering Team

An AI engineering pipeline built with [CrewAI Flows](https://docs.crewai.com/en/concepts/flows). Give it an application brief and it coordinates specialized crews to design, implement, and validate a small application, leaving every generated artifact in a shared sandbox.

## What It Does

The default flow builds a trading-simulation account-management application. Its state can be customized with an application name, description, audience, and stack preference.

```text
Application brief
			 |
			 v
Design crew -> Backend crew -> Frontend crew -> Validation crew
			 |              |               |                |
			 +--------------+---------------+----------------+
															|
															v
									 sandbox/ artifacts and manifest
```

Each run writes the design specification, implementation summaries, test reports, validation sign-off, and `flow_manifest.json` beneath `sandbox/`.

## Prerequisites

- Python 3.10 through 3.13
- [uv](https://docs.astral.sh/uv/)
- An [OpenRouter](https://openrouter.ai/) API key

## Setup

```bash
git clone https://github.com/anton-kovachev/flow-engineering-team.git
cd flow-engineering-team
cp .env.example .env
```

Set `OPENROUTER_API_KEY` in `.env`. The default base URL is already configured for OpenRouter.

Install the project dependencies:

```bash
uv sync
```

The crew configuration uses OpenRouter model IDs in `src/flow_engineering_team/crews/*/config/agents.yaml`. Change those values to select another supported model.

## Run The Flow

Run the default trading-simulation brief:

```bash
uv run crewai run
```

Or provide a custom brief as JSON:

```bash
uv run run_with_trigger '{
	"app_name": "Inventory Console",
	"app_description": "An internal tool for tracking warehouse inventory and reorder thresholds.",
	"target_audience": "warehouse operations staff",
	"stack_hint": "FastAPI backend and a simple browser UI"
}'
```

Review the generated results in `sandbox/`, beginning with `sandbox/flow_manifest.json`. The validation report marks the run as accepted only when its verdict includes `approved` and does not request revision.

## Project Layout

```text
src/flow_engineering_team/
	main.py                  # Flow state, stages, and CLI entry points
	crews/                   # Design, backend, frontend, and validation crews
	tools/                   # File and sandbox tools available to crews
	sandbox.py               # Shared artifact directory helpers
sandbox/                   # Generated specifications, reports, and application output
.env.example               # Required OpenRouter configuration template
```

## Development

Run the test suite with:

```bash
uv run pytest
```

Create a flow visualization with:

```bash
uv run crewai flow plot
```

## Security

Never commit `.env` or API keys. `.env` is intentionally ignored; use `.env.example` to document required configuration without exposing credentials.

