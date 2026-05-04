# Troubleshooting

The most common ways things break while you're working through the book, and how to fix each one. Symptom first, then cause, then the actual fix.

## Cannot access this repo after purchase

**Symptom.** You bought the book on Lemonsqueezy, the receipt landed, but you cannot pull or browse `https://github.com/YawLabs/a2a-in-production-companion`. GitHub returns 404 or "you don't have access."

**Cause.** Collaborator access is granted by the LS webhook handler against the GitHub username you provided at checkout. If the username field was blank, mistyped, or the webhook errored, the invite never went out.

**Fix.** Check your email (including spam) for a GitHub invite from YawLabs. If nothing arrived within ~5 minutes of the receipt, email `contact@yaw.sh` from the order email with your GitHub username. Manual invite goes out same day.

This is an access-platform issue rather than a code issue, but it shows up often enough that buyers ask, so it earns a slot here.

## The tag for my chapter does not exist

**Symptom.** You ran `git checkout chapter-7-final` and git says `error: pathspec 'chapter-7-final' did not match any file(s) known to git`. You ran `git tag --list` and the tag is not there.

**Cause.** A2A in Production ships as early access -- not every tag exists from day one. The README and STRUCTURE doc list all planned tags, but the ones cut so far are whichever readers have asked for plus the ones the book launched with.

**Fix.** File an issue on the repo with the chapter number and a short note on what you're trying to do (running the chapter exercises, comparing against your own implementation, etc.). Tags get prioritized by reader pull, not author push -- the more readers ask, the sooner it lands. While you wait, the chapter's prose stands on its own; the tag is the do-the-work surface, not the explanation.

## LLM provider returns 401 / 403

**Symptom.** Any tag that calls an LLM (most of them past chapter 3) fails with a 401 from your provider. Your key works in `curl`.

**Cause.** The script reads the key from an environment variable -- `ANTHROPIC_API_KEY` by default; `OPENAI_API_KEY` or `GOOGLE_API_KEY` when configured for those providers -- and your shell does not have it exported, or you exported it in a different shell.

**Fix.** Export it in the same shell you run the script from:

```bash
export ANTHROPIC_API_KEY=sk-ant-...
```

For longer-lived setup, drop it into a `.env` file at the repo root (already covered by `.gitignore`) and source it before each run, or use `direnv` if you have it.

## Multi-agent fleet starts but router returns 500 on every request

**Symptom.** `docker compose up -d` succeeded, all containers report healthy, but every request to the router returns 500. Logs show the router cannot reach the specialists.

**Cause.** Container-to-container DNS. The router resolves specialists by service name (`research-agent`, `writing-agent`, etc.) which works inside the docker-compose network but fails if you spun up specialists with a different network or a custom name.

**Fix.** Use the compose file the tag ships with -- `docker compose down && docker compose up -d` to recreate the network. If you must run specialists separately, set `RESEARCH_AGENT_URL`, `WRITING_AGENT_URL` (etc., per the tag's README) to the addresses you actually have, with explicit hosts and ports.

## Trace context drops between agents

**Symptom.** chapter 9 (observability bridge): you send a request, you see a span for the router, you see a span for the specialist, but they're not parented to a common trace -- two separate traces in the viewer.

**Cause.** A2A protocol headers carrying the trace context were not propagated across the agent boundary. Either the router did not inject them outbound, or the specialist did not extract them inbound.

**Fix.** Verify both ends use the OpenTelemetry middleware the tag ships with. The router's outbound A2A client must be wrapped; the specialist's inbound A2A server must be wrapped. The tag's README shows the exact wiring; if you swapped the A2A library, the middleware likely doesn't apply and you need to add equivalent header-injection/extraction.

## Costs spike unexpectedly during testing

**Symptom.** You let the chapter 5 swarm pattern run a batch of test requests overnight and woke up to a much larger LLM bill than expected.

**Cause.** Multi-agent systems amplify cost: a single user request becomes N agent calls becomes M model calls. Without budget enforcement (chapter 10) the amplification is invisible until the bill arrives.

**Fix.** Set per-request budget caps in the orchestrator config (chapter 10 walks this through; the primitive is in earlier tags as commented-out config). For testing, point the agents at a cheaper/local model (Haiku, a local Ollama, etc.) before turning them loose on a batch. The architecture is the same; only the per-call cost differs.
