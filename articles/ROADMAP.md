# Architecting With Evidence — series roadmap

This mutable editorial roadmap owns installment state and next action. It does
not establish platform architecture; see [the root](../README.md) and
[agent contract](../AGENTS.md). Workspaces hold supporting notes and drafts;
published transcripts preserve the historical publication.

## Promise and audience

For experienced engineers, technical leaders, and people using coding agents on
systems whose requirements emerge over time. Use an evolving threat-detection
platform to make architectural judgment inspectable: which evidence creates a
concern, what decision follows, and what could cause reconsideration.

The series does not promise a universal production architecture or preselect the
final system. Customer facts and operating guarantees must be established through
sources, conversations, and measured work, with uncertainty retained.

## Installments

| Installment | Lifecycle state | Central question | Published transcript / checkpoint | Workspace |
| --- | --- | --- | --- | --- |
| 1 — Before the First Service | Published | What must be preserved and recorded before the first service can responsibly begin? | [Article 1](article1.md) / `article-01` | No retrospective workspace |
| 2 — Choose what crosses the first edge (working title) | Drafting | What versioned wire profile should cross the first edge, and what justifies choosing it? | None assigned | [WORK.md](work/article-02/WORK.md) |

Current installment: **2**. Later installments are not scheduled or committed.
Lifecycle requirements live in [AGENTS.md](../AGENTS.md); state is recorded only
in the table above.

## Scope and continuity

Article 1 establishes platform-root authority and the accepted next experiment:
synthetic source → raw collection → durable handoff. It implements no runtime
service and selects no transport, profile, broker, or normalized schema.
Its next action is carried forward into the current installment below.

Article 2 starts with comparison criteria, customer-source evidence or its
explicit absence, candidate profile research, and selection of the versioned
wire contract. Separate transport, framing, payload family, and source profile.
The intended article must show the exact wire representation and distinguish
documentation from teaching simplifications.

Simulator design or implementation must follow wire-profile selection. Whether
Article 2 then designs, implements, and/or executes the simulator remains an
[explicit shaping question](work/article-02/WORK.md#simulator-scope-question).
Collector implementation, durable-handoff implementation, broker selection,
normalization, identity reconciliation, and detection remain excluded.

Use Article 1's preservation and identity distinctions as prerequisites without
retelling its argument. Explain any change to its next-installment promise;
do not let the first fixture imply a universal platform contract or customer
prevalence. The Article 2 workspace records a selected source-shaped basis; root
platform transport and event-model decisions remain unaccepted.

## Next concrete action

FSTO/1 implementation and execution complete. The `tds-firewall-traffic-simulator`
repository implements deterministic 180-byte canonical record generation,
golden-byte verification, UDP send/receive experimental harness, independent
attempt/receipt metadata, and both required scenarios (clean_success,
receiver_unavailable). All 15 tests pass. Evidence artifacts captured. Findings
reconciled to ARCHITECTURE.md, OPEN-QUESTIONS.md, and RISKS.md.

Next action: Draft Article 2 manuscript showing wire-profile selection reasoning,
FSTO/1 contract specification, experimental findings, and preserved limitations.
Do NOT publish, create tags, or push to remote until explicitly authorized.
