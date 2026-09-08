# Article 2 — working record

Mutable editorial material, not platform authority. See the
[roadmap](../../ROADMAP.md) for lifecycle state and next action, and
[AGENTS.md](../../../AGENTS.md) for the workflow. Manuscript and handoff live in
[draft.md](draft.md) and [review.md](review.md).

## Brief

- **Central question:** What versioned wire profile should cross the first edge,
  and what justifies choosing it?
- **Audience and promise:** carry forward the [series promise](../../ROADMAP.md#promise-and-audience).
  Make the selection reasoning and exact wire representation inspectable,
  separating documented behavior from teaching simplifications.
- **Starting scope:** comparison criteria; customer-source evidence or its
  absence; candidate profile research; selection of a versioned wire contract.
  Treat transport, framing, payload family, and source profile separately.
- **Sequence constraint:** simulator design or implementation cannot precede
  selection of the versioned wire profile.
- **Exclusions:** collector implementation, durable-handoff implementation,
  broker selection, normalization, identity reconciliation, and detection.
- **Evidence needed:** applicable primary export documentation; explicit
  customer-inventory evidence or limitations; comparison rationale; exact bytes
  and their provenance. Determine simulator evidence needs after selection.
- **Continuity:** avoid repeating Article 1's root-governance argument. Carry its
  distinctions forward without implying that a teaching fixture represents the
  market or settles the eventual platform event model.

### Published next-installment promise

From [Article 1](../../article1.md), “Next: choose what crosses the first edge,”
at `article-01:articles/article1.md`:

> In Part 2, we will examine the firewall products and export mechanisms plausible customers are likely to use. We will separate transport, framing, payload family, and source profile; select one versioned teaching contract; and show the exact wire representation our simulator will produce.

> Only then will we design the simulator that exercises it.

Section 5 also promises comparison of enterprise firewall sources, exact bytes,
documented versus simplified behavior, and an experiment whose first fixture
does not become the platform architecture.

### Simulator scope question

- **Question:** After selecting the wire contract, does this installment need
  simulator design, implementation, and/or execution to fulfill its evidence promise?
- **Published context:** Article 1 says, “Only then will we design the simulator
  that exercises it.” This establishes ordering without settling all of Article
  2's eventual implementation scope.
- **Newer instruction:** Geoffrey's approved instrumentation refinement, supplied
  in the implementation request on 2026-09-07, describes Part 2 as selecting the
  contract, showing its exact wire representation, and building a reproducible
  simulator experiment. This records project direction, not an achieved result.
- **Provisional boundary:** proceed through profile research and wire-contract
  selection. Do not design or implement the simulator before selection; do not
  declare simulator implementation excluded from the whole installment.
- **Resolution needed:** after selection, identify the claims that require an
  executable demonstration, the smallest reproducible experiment that supports
  them, and any remaining continuity tension. Record the resulting scope and
  rationale here and reflect it in the roadmap before proceeding. Ask Geoffrey
  if a material correctness or scope ambiguity remains.
- **Resolution:** open; no profile or simulator design has been selected.

## Inherited context

Baseline: annotated `article-01`, root commit
`26a55c1215ca2463e121c42242e60a41ef2961af`. Read the tagged documents as well as
the current root; later changes must not be attributed to this checkpoint.

- **Accepted decision:** [ADR-0001](../../../decisions/0001-establish-the-platform-root.md)
  establishes root authority and service ownership, including reconciliation cost.
- **Accepted next experiment:** [ARCHITECTURE.md](../../../ARCHITECTURE.md)
  records synthetic source, raw collection, and durable handoff boundaries.
  These are capabilities to investigate, not implemented services.
- **Deferrals and non-decisions:** the same architecture leaves transport,
  framing, payload/vendor profile, parsing, normalized schema, broker/storage,
  detection, deployment, tenancy, trust, and scaling topology unselected.
- **Questions:** [OPEN-QUESTIONS.md](../../../OPEN-QUESTIONS.md), OQ-0001–0003
  cover customer environment, transport/framing, and payload/source profile;
  OQ-0004–0008 retain device identity, handoff/retention, operating boundaries,
  parsing/normalization, and the measured scale envelope as unknowns.
- **Risks:** [RISKS.md](../../../RISKS.md), RISK-0001–0006 cover pre-collection
  loss, address/identity confusion, format drift, raw-data exposure, lost
  recoverability, and documentation drift. Synthetic-only local work remains
  the provisional boundary; broader exposure requires the stated risk review.

## Evidence and source trace

Record each material claim with a local evidence label, classification from
AGENTS.md, source location/version/date/section, applicability, limitations, and
the draft claim it supports. Execution evidence additionally needs repository
revision and dirty changes, inputs, environment, commands, results, and durable
artifact paths. Keep primary-source assertions distinct from verified execution.

- **E1 — Authoritative historical evidence:** the Article 1 transcript and tagged
  root documents referenced above establish the prior promise and checkpoint.
  They do not establish vendor behavior or customer prevalence.
- **E2 — Editorial instruction:** Geoffrey's 2026-09-07 refinement, preserved in
  the simulator scope question above, establishes the sequencing constraint and
  scope question. It makes no technical selection or experimental claim.
- **Customer-source evidence:** not yet inventoried. No customer facts asserted.
- **Candidate/vendor research:** not started. No vendor behavior asserted.

## Implementation and experiment findings

No implementation or experiment performed for Article 2. No measurements or
runtime results claimed. After profile selection and scope resolution, record
any required experiment's question, inputs, implementation revision, validation,
observed failures/discrepancies, and limits. Mark unexecuted checks unverified.

## Platform reconciliation

No Article 2 technical decisions or architectural changes. This initial workspace
records editorial scope only; existing architecture, questions, risks, and
ADR-0001 remain unchanged. Future findings must link to affected root changes,
or explain why no change is needed, before manuscript claims are drafted.
