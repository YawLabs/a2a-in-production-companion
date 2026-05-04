# Structure

What lives at each tag. Use this as a map when diffing your own work against the reference.

**Code checkpoints fill in over time.** The repo's content tracks book launch and reader feedback. If a chapter's tag is missing today and you need it, file an issue; tags are prioritized by reader pull, not author push.

Chapters 1 and 12 do not get their own checkpoint -- chapter 1 sets the stage and chapter 12 is forward-looking. Their content stands on its own without a code artifact.

## `chapter-3-final`

A2A protocol: server + client end-to-end.

A reference A2A server that exposes a small set of agent capabilities and a client that calls them. Includes the protocol envelope, capability discovery, and a worked request/response cycle. Every later chapter builds on this shape -- if the protocol layer is unfamiliar, this is the tag to start at.

## `chapter-4-final`

Orchestration: router pattern over two specialist agents.

A router agent that receives tasks and dispatches to one of two specialists (a "research" agent and a "writing" agent in the reference, but the shape generalizes). Demonstrates capability declaration, the routing decision surface, and how the router gets out of the way once dispatch is done.

## `chapter-5-final`

Orchestration: supervisor + swarm.

Two more orchestration shapes. The supervisor pattern: one agent owns the task, breaks it into subtasks, delegates to peer agents, integrates the results. The swarm pattern: multiple agents work the same task in parallel, an aggregator merges. Same specialist agents as chapter 4, different orchestrator on top.

## `chapter-6-final`

Auth across agent boundaries (auth context propagation).

The router and specialists from chapter 4-5 grow auth context. A request enters the router with a user identity; that identity is propagated through to specialists; specialists enforce per-user scope on whatever state they touch. Includes a worked OIDC integration and a "service-to-service auth between agents" alternative.

## `chapter-7-final`

Write-permission semantics + provenance trail.

Multi-agent systems write to shared state (a memory store, a database, a queue). This tag introduces explicit write-permission grants between agents and a provenance trail that records "agent A wrote this, on behalf of user U, at time T, in service of task X." Provenance is the substrate for the observability chapter and the trust story below.

## `chapter-8-final`

Federated state across agents.

The state question generalized: when each agent has its own local store and they need to share, how do you do it without re-writing every store as a single shared service? Three patterns -- broadcast, request-on-demand, and CRDT-style merge -- with a working implementation of each over the chapter 4-5 fleet.

## `chapter-9-final`

Observability bridge: trace across agent hops.

OpenTelemetry trace context propagation across A2A calls. A single user request crosses three agents and produces one trace with spans on every hop, including the routing decision, each specialist's tool calls, and the final integration step. Includes a small viewer for the traces produced.

## `chapter-10-final`

Cost + latency budgets + circuit breaking.

Budget enforcement across agent boundaries. The router carries a cost ceiling per request; each hop debits against it; specialist work that would blow the budget is rejected before it starts. Circuit breakers trip when a specialist degrades, isolating the failure from the rest of the fleet.

## `chapter-11-final`

Error propagation + partial failure handling.

Multi-agent systems fail in shapes single-agent systems don't: one specialist hangs, one returns garbage, two agents disagree. This tag handles each case end to end: timeout-and-fallback, validation-with-retry, and a disagreement-resolution pattern. The chapter is about which failure modes are worth designing for; this tag is the primitives.
