# ADR-0001 — Establish the platform root

- **Status:** accepted for Article 1 checkpoint
- **Date:** 2026-09-01
- **Stable reference:** `ADR-0001`

## Context

The threat-detection field study will eventually span independently versioned
services and repositories. No service repository naturally owns the truth about
the whole platform. If implementation begins first, a locally reasonable
service can silently become the architecture and hide unresolved assumptions
from later agents and collaborators.

## Decision

Establish `tds-platform-root` as the authoritative repository for platform-level
intent, the current cross-service model, accepted boundaries, decisions, risks,
and unknowns. Future service repositories remain authoritative for their own
implementation, tests, and build instructions. Every later cross-service change
must reconcile the root with the affected implementation before it is considered
coherent.

## Alternatives considered

- **Start with the first service repository:** faster initial code, but its local
  concerns would become platform assumptions by default.
- **Keep architecture only in conversations or diagrams:** low setup cost, but
  agents cannot reliably recover the reasoning or uncertainty later.
- **Create a complete multi-repository governance system now:** potentially
  powerful, but premature and costly before the workflow has repeated.
- **Use a monorepository for root context and all future services:** simplifies
  atomic changes initially, but obscures the independent ownership and
  versioning behavior this field study intends to examine.

## Consequences

The root gives humans and agents a durable place to find context and prevents
the first implementation from silently deciding the whole system. It also
introduces synchronization work, review overhead, and a new risk of documentation
drifting from implementation. The root must remain small and must not duplicate
service internals.

## Costs and risks

Maintainers must update the root when a platform boundary changes and preserve
the distinction between evidence, inference, decision, proposal, risk, and
unknown. If that discipline is not maintained, the root can create false
confidence rather than coherence.

## Reconsideration conditions

Reconsider this decision if repeated work demonstrates that the root cannot be
kept coherent, if another repository arrangement gives readers and agents better
cross-service context with less drift, or if measured coordination cost exceeds
its value. Any replacement must preserve an inspectable platform-level authority.
