# Platform architecture — Article 1 checkpoint

## Authority and scope

This root is authoritative for platform-level intent, accepted cross-service
boundaries, decisions, risks, and unknowns. No runtime service exists in this
checkpoint. Later service repositories will remain authoritative for their own
implementation, tests, and build instructions.

## Maturity vocabulary

- **Implemented:** verified in this repository or an explicitly referenced
  implementation.
- **Accepted next experiment:** the smallest decision approved for the next
  slice, not a production commitment.
- **Proposed:** a plausible future direction without an accepted commitment.
- **Unknown:** important information not yet established.

## First observable flow

```text
reproducible firewall-shaped source
                 |
                 | raw transport input
                 v
         raw collection boundary
                 |
                 | received bytes
                 | + available receipt observations
                 v
           durable handoff
```

### Current boundary statements

- The **reproducible firewall-shaped source** is an accepted next experiment.
  It is synthetic and must support repeatable bounded runs. Its concrete
  transport and payload profile are unknown.
- The **raw collection boundary** is an accepted next experiment. It preserves
  received bytes and available receipt observations. It does not parse,
  normalize, enrich, or classify.
- The **durable handoff** boundary is part of the accepted next experiment. Its
  product, delivery behavior, retention, replay, ordering, and scaling
  properties remain unknown.
- The platform-level architecture and agent contract are implemented as
  Markdown in this repository.

## Explicit non-decisions

This checkpoint does not select a transport, framing convention, payload family,
vendor or model, parser, normalized event schema, broker, storage system,
detection service, deployment model, tenancy model, trust boundary, or scaling
topology. Those may appear later as questions, risks, or experiments when the
series earns them.

## Stable boundaries

- `PLATFORM-ROOT`: platform intent and cross-service coherence.
- `RAW-COLLECTION`: receipt and preservation of incoming bytes with available
  observations; no payload interpretation.
- `DURABLE-HANDOFF`: separation of receipt from later interpretation; concrete
  guarantees remain unknown.
- `SYNTHETIC-SOURCE`: reproducible source-shaped input for controlled learning.
