# Setup

Getting from a cold clone to a working checkpoint.

The companion repo's checkpoints span Python (orchestration glue, observability bridges) and TypeScript (the reference A2A server + client from chapter 3 onward). Each tag's `README.md` is the source of truth for that checkpoint's specific tooling -- this doc covers the shared shape.

## Prerequisites

- **Python 3.11 or newer** for the orchestration patterns and observability bridges (chapters 4 onward have Python options). `python --version` should print `3.11` or higher.
- **Node 20 or newer** for the reference A2A server + client (chapter 3 onward) and TypeScript orchestration shapes. `node -v` should print `v20.x` or higher.
- **Docker** for running multiple agents locally without polluting your host (chapter 4 and later spin up multiple specialist agents in parallel). `docker --version` should work.
- **An LLM provider key.** Reference implementations default to Anthropic (`ANTHROPIC_API_KEY`); the orchestration code is provider-agnostic and works with OpenAI, Gemini, or local models with minor configuration.

Chapter-specific extras (an OAuth identity provider for the auth chapters, an OpenTelemetry collector for the observability chapter, etc.) are called out in the per-tag README.

## Clone the repo

```bash
git clone https://github.com/YawLabs/a2a-in-production-companion
cd a2a-in-production-companion
```

## Check out the tag for your chapter

The `main` branch holds documentation only -- the actual code lives at the tags. Pick the checkpoint matching the chapter you're working through:

```bash
git checkout chapter-3-final   # or chapter-4-final, chapter-5-final, ...
```

You should now see the source tree for that chapter's artifact.

If the tag does not exist yet, see the note in `README.md`: checkpoints fill in over time. File an issue for the one you want and it gets prioritized.

## Install dependencies

The exact install step depends on the tag. Each checkpoint's top-level `README.md` says which to run.

For Python-flavoured tags (orchestration, observability bridges):

```bash
python -m venv .venv
source .venv/bin/activate     # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
```

For Node-flavoured tags (A2A server/client, TypeScript orchestration):

```bash
npm install
```

## Run the local agent fleet (chapter 4 and later)

The orchestration chapters need multiple agents running simultaneously. The repo ships a `docker-compose.yml` at chapter-4-final and later that spins up the specialist agents and a router on configurable ports.

```bash
docker compose up -d
```

This starts the agent fleet on `localhost:8080` (router) with specialists on adjacent ports. The exact port map is in the tag's README.

## Send a request through the orchestrator

At chapter-4-final and later you have a running multi-agent system. Send a request to the router:

```bash
curl -X POST http://localhost:8080/tasks \
  -H 'Content-Type: application/json' \
  -d '{"input": "your task here"}'
```

You should get JSON back showing which specialist(s) handled the task and the final answer. If you see real routing decisions in the response, you are wired up correctly.

If anything fails, jump to `TROUBLESHOOTING.md`.

## Cleaning up

When you switch to a different checkpoint, stop the agent fleet (the agent shapes may differ between tags) and remove the venv if you are switching between Python and Node tags:

```bash
docker compose down -v
deactivate     # if you were in a venv
```

Then check out the new tag and repeat the install + ingest steps.
