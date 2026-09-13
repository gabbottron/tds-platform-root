# Open questions — current through Article 2

Unanswered questions are recorded here rather than resolved by convention.
Each entry states why it matters, what it could affect, the provisional boundary
that lets work continue, evidence needed, and current status.

## OQ-0001 — First customer and source environment

- **Question:** Which customer environment and source conditions should shape the
  first real integration?
- **Why it matters:** Devices, operators, deployment constraints, and data
  obligations determine useful boundaries.
- **May affect:** source profile, onboarding, deployment, security, scale, and
  ownership.
- **Provisional boundary:** use synthetic source-shaped input locally.
- **Evidence needed:** customer conversations and a representative environment.
- **Status:** open.

## OQ-0002 — Transport and framing

- **Question:** How should source bytes reach the collection boundary, and how is
  one event delimited?
- **Why it matters:** Transport changes loss, ordering, pressure, and available
  receipt evidence.
- **May affect:** collector behavior, handoff, scaling, and operational cost.
- **Provisional boundary:** transport-neutral raw collection boundary.
- **Evidence needed:** source inventory, operating requirements, and controlled
  failure experiments.
- **Status:** partial finding. FSTO/1 uses UDP with one-record-per-datagram
  (180-byte boundary preserved). Validated: successful send does NOT guarantee
  receipt (receiver_unavailable scenario). This is a bounded teaching finding,
  not a general transport selection. Real FortiOS syslog behavior, TCP
  considerations, and production network loss patterns remain unknown.

## OQ-0003 — Payload family and source profile

- **Question:** Which event family, vendor/model, software version, and export
  format should be the first fixture?
- **Why it matters:** A concrete fixture is needed for reproducible work, but it
  must not become the platform event model by accident.
- **May affect:** simulator, parser boundary, compatibility, and replay.
- **Provisional boundary:** preserve source-shaped bytes before interpretation.
- **Evidence needed:** authoritative format documentation, likely customer
  source inventory, and safely reproducible fixtures.
- **Status:** implemented. FSTO/1 (FortiOS-Shaped Traffic Observation v1)
  provides a deterministic 180-byte teaching contract shaped after FortiOS
  7.4.8 Traffic/forward session-end logs (LOG_ID_TRAFFIC_END_FORWARD). This is
  a synthetic teaching fixture, NOT real FortiOS output. It supports repeatable
  bounded experiments with golden-byte verification. Real FortiOS variability,
  field ordering, escaping, and multi-line behavior remain unknown.

## OQ-0004 — Device identity

- **Question:** How can each source be identified across address, location,
  hardware, and software changes?
- **Why it matters:** Address alone can split or merge histories incorrectly.
- **May affect:** onboarding, reconciliation, provenance, and investigations.
- **Provisional boundary:** preserve observed source information without silently
  asserting continuity.
- **Evidence needed:** source-provided identifiers and replacement/failover
  scenarios across models.
- **Status:** open.

## OQ-0005 — Durable handoff and retention

- **Question:** What durability, delivery, retention, replay, and ordering
  guarantees are necessary after collection?
- **Why it matters:** Consumer failure and later interpretation changes must not
  erase accepted evidence.
- **May affect:** handoff implementation, storage, cost, and recovery.
- **Provisional boundary:** durable handoff is a required capability; product is
  unselected.
- **Evidence needed:** event rates, burst sizes, consumer fan-out, recovery and
  replay scenarios.
- **Status:** open.

## OQ-0006 — Deployment, trust, tenant, and operating boundaries

- **Question:** Where does collection run, who operates each boundary, who
  responds to integration changes, and how are customer boundaries protected?
- **Why it matters:** Network exposure, credentials, access, residency, and
  support responsibilities depend on the operating model.
- **May affect:** security, deployment, tenancy, retention, support, and
  ownership.
- **Provisional boundary:** local single-environment learning deployment.
- **Evidence needed:** customer requirements and operational/security review.
- **Status:** open.

## OQ-0007 — Parser and normalized-event boundary

- **Question:** What interpretation and normalized representation are shared
  across downstream capabilities?
- **Why it matters:** Format drift must not corrupt raw evidence or couple every
  service to one parser implementation.
- **May affect:** parsing, storage, detection, enrichment, and replay.
- **Provisional boundary:** interpretation occurs after durable handoff.
- **Evidence needed:** multiple representative payloads and downstream use cases.
- **Status:** open.

## OQ-0008 — Evidence-backed scale envelope

- **Question:** What devices, rates, payload sizes, lag, loss, retention, and
  recovery targets must the platform support?
- **Why it matters:** Scaling and infrastructure choices require workload
  evidence rather than invented numbers.
- **May affect:** collector topology, handoff, storage, and operating cost.
- **Provisional boundary:** bounded synthetic runs with declared inputs.
- **Evidence needed:** customer workload observations and measured experiments.
- **Status:** open.
