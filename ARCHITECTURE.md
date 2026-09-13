# Platform architecture — current through Article 2

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

- The **reproducible firewall-shaped source** is implemented in
  `tds-firewall-traffic-simulator` as FSTO/1 (FortiOS-Shaped Traffic
  Observation v1). It delivers a deterministic 180-byte FortiOS-shaped record
  via UDP to a local experimental receiver. It supports two validated scenarios:
  clean_success (byte preservation verified) and receiver_unavailable (send
  without receipt confirmed). This is a teaching contract; it is NOT a claim
  about real FortiOS output or production network behavior.
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

## Teaching fixtures

### FSTO/1 - FortiOS-Shaped Traffic Observation v1

**Repository:** `tds-firewall-traffic-simulator`
**Status:** Implemented
**Canonical SHA-256:** `62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653`

FSTO/1 is a deterministic 180-byte teaching contract shaped after FortiOS 7.4.8
Traffic/forward session-end logs (LOG_ID_TRAFFIC_END_FORWARD). It provides:

- Deterministic canonical record generation (fixed eventtime, IPs, ports)
- UDP send with attempt metadata
- Experimental receiver harness with receipt metadata
- Three-way time distinction: source eventtime, attempt time, receipt time
- Golden-byte verification via pytest

**Validated scenarios:**
1. `clean_success`: One send → one receipt with byte preservation
2. `receiver_unavailable`: Send succeeds but no receipt observed

**Explicit limitations:**
- Local loopback only (127.0.0.1), not real network
- Synthetic teaching record, not real FortiOS output
- Experimental receiver harness, not production collector
- No claim about production firewall behavior or real network loss rates

**Language selection:** Python accepted for experimental tooling focused on
readability and reproducible evidence. This does NOT select Python for future
RAW-COLLECTION service (provisional preference: Go for continuous network
intake, controlled concurrency/allocation).

## Stable boundaries

- `PLATFORM-ROOT`: platform intent and cross-service coherence.
- `RAW-COLLECTION`: receipt and preservation of incoming bytes with available
  observations; no payload interpretation.
- `DURABLE-HANDOFF`: separation of receipt from later interpretation; concrete
  guarantees remain unknown.
- `SYNTHETIC-SOURCE`: reproducible source-shaped input for controlled learning.
