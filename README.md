# a2a-in-production-companion

Paid companion repo for the book **A2A in Production** by Yaw Labs. Volume IV of the Yaw Labs Production Series. Status: **early access.**

This repo holds the canonical end-of-chapter checkpoints for the multi-agent systems built throughout the book. Each checkpoint is preserved as a git tag so you can clone the repo, check out the tag for whichever chapter you're working through, and compare it against your own work.

Book link: https://yaw.sh/a2a-in-production

## Early access

A2A in Production is shipping as early access. The book launches with a subset of chapters drafted and adapts/adds chapters as the production-incident base accumulates. Your purchase includes free updates to v1.0 and beyond -- the same companion repo, the same access, every new chapter and checkpoint as they land.

What that means for this repo:

- **Some tags exist on day one** -- the chapters that ship in early access have their checkpoints cut.
- **Most tags fill in over time** -- as new chapters land, their `chapter-N-final` tags get cut. The full planned set is below.
- **Tags fill in by reader pull** -- if a tag you need isn't here yet, file an issue and it gets prioritized. Author push is the backstop, not the schedule.

## How the book uses this repo

The book uses tagged checkpoints at `chapter-N-final` for hands-on chapters where there's a concrete artifact to build (an A2A protocol server/client, a router orchestrator, an auth-aware federation, an observability bridge across agent hops, and so on). Some chapters (notably 1, 12) are conceptual or forward-looking and don't carry their own code tag -- the chapter content stands on its own.

Planned tag set (filled in over time):

```
chapter-2-final     (placeholder -- pending chapter)
chapter-3-final     A2A protocol: server + client end-to-end
chapter-4-final     orchestration: router pattern over two specialist agents
chapter-5-final     orchestration: supervisor + swarm
chapter-6-final     auth across agent boundaries (auth context propagation)
chapter-7-final     write-permission semantics + provenance trail
chapter-8-final     federated state across agents
chapter-9-final     observability bridge: trace across agent hops
chapter-10-final    cost + latency budgets + circuit breaking
chapter-11-final    error propagation + partial failure handling
```

`git checkout <tag>` to land on that chapter's final state. See `STRUCTURE.md` for what lives at each tag and which tags are cut today vs pending.

## Access

Access to this repo is granted on book purchase. The book is sold via Lemonsqueezy at https://yaw.sh/a2a-in-production. After purchase, the LS webhook handler grants the buyer's GitHub account collaborator access automatically -- no manual step. If access does not appear within a few minutes of purchase, email `contact@yaw.sh` with the order email.

## How to use it

```bash
git clone https://github.com/YawLabs/a2a-in-production-companion
cd a2a-in-production-companion
git checkout chapter-3-final
```

From there, follow `SETUP.md` for chapter-specific install and run steps, then diff against your own checkout in another window.

## License

MIT for the reference code at the tags. The book text itself is sold separately and is not in this repo.

## Publisher

Yaw Labs.

- Book: https://yaw.sh/a2a-in-production
- Email: contact@yaw.sh
- Issues: file them on this repo for problems with the reference code, or to request that a not-yet-cut tag get prioritized.

If the book itself is the issue (a chapter is unclear, a code listing does not match the repo, an exercise step is broken), email `contact@yaw.sh`.
