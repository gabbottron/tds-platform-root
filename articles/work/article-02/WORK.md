# Article 2 — working record

Research and evidence working material, not publication authority. See the
[roadmap](../../ROADMAP.md) for lifecycle state and next action, and
[AGENTS.md](../../../AGENTS.md) for the workflow. Manuscript and handoff live in
[review.md](review.md). `draft.md`, `jane-draft-01.md`, and `jane-draft-02.md`
are temporary editorial working material; the sole Article 2 publication
candidate is [articles/article2.md](../../article2.md).

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
- **Published context:** Article 1 says, "Only then will we design the simulator
  that exercises it." This establishes ordering without settling all of Article
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
- **Resolution (E7, 2026-09-10):** Article 2 will include design, implementation,
  and execution of the smallest reproducible simulator experiment demonstrating
  the `FSTO/1` teaching contract and attempt-versus-receipt distinction. The
  experiment must establish deterministic generation of canonical bytes,
  golden-byte verification, source-attempt and receiver-receipt observations,
  byte preservation, and a clean-success case. One optional controlled
  discrepancy (send without receipt) may demonstrate the attempt-vs.-receipt
  distinction. Experimental receiver is a minimal test harness, not production
  collector or durable handoff. Implementation authorization detailed in E8 below.

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
  scope question. At the time recorded, it made no technical selection or
  experimental claim.
- **E3 — Editorial instruction, 2026-09-10:** bounded first-edge research only.
  This pass ends at joint review of taxonomy, criteria, evidence, and candidates.
  Selection/recommendation, technical design, simulator scope resolution, and
  manuscript prose are not authorized by this research task.
- **E4 — Shaping judgment, 2026-09-10:** documentation-led teaching selection
  may proceed without customer inventory, but may not make a customer-prevalence
  claim. The direct firewall-to-collector-controlled boundary is the primary
  Article 2 contract. P3–P5 remain contrasts, not finalists. The historical
  repositories are reference only and must not influence selection or become
  series implementation evidence or architecture authority. Simulator scope
  remains open until after exact contract shaping.
- **E5 — Accepted Article 2 next-experiment decision, 2026-09-10:** select F1,
  FortiGate/FortiOS 7.4.8 forward-traffic session-end over UDP, as the
  source-shaped basis for the first versioned teaching contract. This is based
  on direct-edge teaching value and documented behavior, not customer prevalence,
  market dominance, or historical implementation convenience. PAN-OS TLS remains
  a rejected-for-now contrast. The exact simulator contract is not finalized;
  missing vendor behavior must be labeled as teaching simplification or runtime
  unknown. This decision is recorded in the Article 2 workspace and does not
  alter platform authority or create an ADR.
- Customer inventory and external documentation findings follow below.

## Research boundary and repository evidence — 2026-09-10

**Observed repository fact (R1):** checkout
`/Users/gabbott/git/gabbottron/tds-platform-root`, actual branch `article-2`
(the request called it `article-02`), HEAD
`2e97282e60c5bff59d838b6d9b2e614f20495f46`. Initial index and working tree
were clean, with no untracked files. No branch was renamed or switched.
Annotated `article-01` object `1c9d682caea85a49e68e53f2fd979f18948f26e3`
peels to `26a55c1215ca2463e121c42242e60a41ef2961af`.

Read the requested root documents, all three workspace files, roadmap, published
transcript, and tagged README, agent contract, architecture, questions, risks,
and ADR-0001. The transcript is unchanged from the tag. Inspected the complete
`git diff article-01 --`: inherited changes are editorial instrumentation,
README clarification, expanded agent workflow, and `.gitignore`, not platform
technical decisions. No service repository or implementation revision is
registered; none is an affected implementation in this task. Other sibling
projects were not treated as evidence for this platform.

**Customer evidence inventory (R2):** searched tracked content with
`rg -n -i 'customer|forti|palo|cisco|juniper|syslog|netflow|ipfix|inventory|service.repo' .`,
inspected `git ls-files`, untracked files, `git log --all --name-status`, and
history changes with `git log --all -G 'customer|Forti|Palo|Cisco|Juniper|NetFlow|IPFIX|syslog|inventory'`.
All four reachable commits were accounted for: initial README `2fa9fc3`,
Article 1 seed `26a55c1`, instrumentation `fb0a527`, and merge `2e97282`.
There are no deleted inventory files in that history. The initial README and
all subsequently introduced documents were inspected. This covers local
recorded refs, not remote-only history, private conversations, or other repos.

| Evidence category | Result and consequence |
| --- | --- |
| Direct customer evidence | None found: no named customer's product, deployment, firmware, transport, format, volume, or operational constraint. |
| Repository evidence | Article 1 §2 uses hypothetical examples, including a Boca office and appliance replacement. §5 explicitly anticipates missing inventory. OQ-0001–0003 request evidence; they do not supply it. |
| External market evidence | None collected. Product documentation establishes support within its scope, not market share or likely customer use. |
| Inference | A synthetic teaching comparison can proceed under the existing provisional boundary. It cannot establish the first customer integration. |
| Complete absence within searched scope | No actual/likely-customer source inventory or provenance for such an inventory. No source-specific customer facts can be carried into selection. |

**Continuity question:** Article 1 promises attention to plausible customers.
Without conversations or inventory, a later choice would need to be explicitly a
documentation-led teaching choice. Geoffrey must judge whether that qualification
fulfills the promise or whether customer discovery must precede selection.

## Following an observation to the collection boundary

This is a path model for research, not a service design. Evidence labels S, V,
and A below resolve to primary-source records later in this file. Descriptions
of collector implications are **inferences** from those sources and the current
architecture; none is a verified implementation. Illustrative scenarios are
**teaching simplifications**, not vendor sample output or proposed fixtures.

### 1. What happened, and what did the firewall choose to describe?

A firewall applies rules to network traffic. A rule states which traffic a
policy permits or blocks; a logging choice determines which observations get
recorded/exported. Those are separate responsibilities (V4, A1, V2).
A browser connection may involve many network packets, yet a firewall can
export a summary at connection end. Alternatively it can report a blocked
attempt or an administrative change. Thus silence at collection can mean no
matching activity, disabled/filtered logging, delayed export, or loss; receipt
alone cannot distinguish those explanations. These are possibilities, not a
claim that every product has every behavior.

Terminology at this step:

- **Observation:** information the source reports about activity it could see.
  **Event:** the occurrence being described, or a vendor's explicitly named
  event category. One event need not produce exactly one exported record.
- **Record:** a source-format data unit describing an observation. A flow
  summary, a log entry, and a later update are records with different contracts.
  A **payload family** answers what is described: traffic, threat/alert,
  configuration/audit, authentication, or device operation. It does not answer
  how bytes travel. Only the families evidenced in each source are attributed
  to that source.
- A **flow** groups traffic sharing defined properties over time. A **session**
  is tracked conversation state in the relevant firewall; neither means one
  packet. **Stateful** inspection considers tracked context, while a
  **stateless** rule considers traffic without that session context. These are
  sufficient teaching descriptions for the AWS logging eligibility distinction
  (A1); full rule-engine evaluation is unnecessary here.
- **Allow**, **deny/drop**, and **reset/reject** broadly concern permitting,
  withholding, or actively terminating/refusing traffic. Precise action names
  and log triggers are vendor-specific (V6, A1). A logged drop is not evidence
  that the export was dropped. An alert is a source-reported condition, not an
  accepted platform threat verdict. Retain these qualifications in the article;
  detailed rule ordering and signature taxonomies are unnecessary.

A **packet** here always means an Internet Protocol (IP) network-layer unit,
qualified as either *observed traffic packet* or *export-path IP packet*.
It never stands for an exported event. A payload field counting packets counts
observed traffic in its documented scope, not messages received by our platform
(V3, V6). A binary NetFlow document's capitalized “Export Packet” is an overloaded
application unit; below it is called a NetFlow export message (S8).

### 2. Who prepares and sends the exported description?

**Appliance** means a dedicated firewall device; **virtual firewall** means a
software deployment serving that role. The exact product/build remains part of
the source profile. A **management server** configures/operates devices; a
**log server** stores/processes their observations. Check Point's exporter runs
on such a server (V9), introducing an earlier hop before external collection.
A **relay** forwards syslog from an originator (S4/S5). A managed cloud logging
service can instead publish records to storage or a service destination (A1–A4).
These are distinct provenance paths, not interchangeable physical sources.

**Inference:** a collector can observe its immediate peer or delivery service;
that does not establish the original device, completeness of an earlier hop, or
continuity across replacement. A source identifier inside a record is a claim
made by that source. Cryptographically authenticating an export connection
is a different observation. A later **security event pipeline** is the sequence
of systems that receive, store, and interpret these records; its downstream
formats are outside this survey. Management-console workflows, clustering
algorithms, and downstream analytics are unnecessary to explain this distinction.

### 3. Which layer answers which wire question?

| Separate layer | Question and investigation vocabulary | Why it matters / evidence |
| --- | --- | --- |
| Network and addressing | IP version 4 or 6 carries export traffic between addresses. IP headers identify network endpoints, not a durable device identity. | IP packet structure and fragmentation: S1. A sender can be a relay: S5. Observed traffic addresses inside a record belong to another path. |
| Transport | User Datagram Protocol (UDP) carries discrete datagrams; Transmission Control Protocol (TCP) provides an ordered byte stream within a connection. | A UDP datagram contains transport header plus payload. A TCP segment carries part of a stream; a read need not match a send. S2/S3. |
| Connection/delivery | Who initiates, how failures appear, and whether pressure slows sending. | Backpressure means a receiver/path limits how quickly the sender can move bytes. It does not prescribe what a firewall does with newly generated logs. S3, S9; vendor queues need evidence. |
| Transport security | Transport Layer Security (TLS) protects an authenticated connection according to configured trust checks. | Protection of a hop is separate from delivery completeness and payload identity. S7. Vendor user interface “SSL” (Secure Sockets Layer) must be checked for the actual TLS version (V4). |
| Framing | How the receiver locates the end of an application message. A **frame** here means the framing representation around a message, not an Ethernet frame. | Datagram boundaries, a byte-count prefix, and a terminator are different contracts. S5–S7. Neither TCP segments nor TLS records delimit syslog messages. |
| Message envelope/header | A **message** is an application-protocol unit submitted for export; an **envelope** is descriptive wrapper metadata. “Header” names a specific protocol's leading fields. | Syslog priority/time/host information is outside vendor message content. Binary export and Hypertext Transfer Protocol (HTTP) response headers have different jobs. S4/S8, A5. |
| Payload encoding/record format | Text spelling, field separators, quoting, character encoding, or binary layout. | Key-value text, comma-separated values (CSV), JavaScript Object Notation (JSON), and template-defined binary data differ independently of transport. V1–V6, S8–S12. |
| Payload family | Which kind of observation the record describes. | Traffic and audit records can share transport but describe different responsibilities. V5, V9, A1. |
| Exact source profile | Vendor, product/deployment, software/build or document revision, export mode, logging/filter choices, and format configuration. | A standard explains a convention; only applicable vendor evidence ties it to a product. All candidates below remain incomplete comparison profiles. |

IP can carry a UDP datagram using fragmentation: one application message need
not equal one export-path IP packet. IPv4 and IPv6 fragmentation rules differ
(S1); that qualification matters, but extension-header layouts do not. Ordinary
socket receipt is not packet capture. Which local address, interface, timestamp,
or truncation indication a future implementation exposes depends on its
operating system and receive application programming interface (API); no local capability was tested.

### 4. Where does the message end, and what is inside it?

**Syslog** is a family of logging protocols/formats for moving source reports
to receivers. A syslog envelope helps identify and categorize a report without
standardizing every vendor's content. Request for Comments (RFC) 3164 describes legacy practice;
RFC 5424 specifies a versioned header, structured-data area, and optional
message body (S4). “BSD” (Berkeley Software Distribution) and “IETF” (Internet
Engineering Task Force) are vendor format labels here, not transports.
**Facility** is a logging category and **severity** a source-assigned importance
level; together they form the syslog priority value. Neither is our threat score.
**Structured data** in RFC 5424 is a specifically delimited metadata area;
arbitrary key-value text in a vendor body is not automatically that area.

In a TCP stream, **octet counting** prefixes a message with its byte length and
a space. **Terminator framing** ends it with a delimiter such as a line feed.
A newline inside content can conflict with delimiter framing (S6). For a
conceptual three-byte body `abc`, `3 abc` demonstrates counting only; it is
not a valid complete syslog fixture. Counting encoded bytes rather than display
characters is essential. Exact prefix and escaping behavior remain profile
facts to prove. Cipher suites and TCP congestion algorithms are unnecessary
for this article; message boundaries and failure limits must remain.

Encoding does not select framing:

| Form, place, and responsibility | Observable distinction and editorial limit |
| --- | --- |
| Human-readable text / key-value body | Labels and quoted values make fields visible; spaces inside a value need a source-specific rule. Fortinet's downloaded raw example demonstrates this form, not a complete syslog datagram (V3). |
| CSV-like vendor body | Positional fields and escaping carry the record. A comma inside a field is not necessarily a separator. Generic CSV documentation (S11) does not prove a vendor follows it; PAN documents its own comma-separated format (V5). Avoid presenting the field list as a platform schema. |
| JSON body | Structured text uses objects, arrays, and typed values (S12). It supplies no socket-level message boundary. Amazon Web Services (AWS) wraps engine output with service fields (A2); JSON named `netflow` is not binary NetFlow v9. |
| Common Event Format (CEF) | A security-record convention with a pipe-separated header and key-value extension, optionally carried inside syslog (S10). It addresses integration vocabulary, not reliability. PAN requires custom configuration for CEF; Fortinet exposes it as an export format (V1/V2/V4). Those are not identical source profiles. |
| Log Event Extended Format (LEEF) | IBM's integration format has its own header and attribute delimiters; v1 uses tabs and v2 can declare another delimiter (S10). Check Point documents LEEF export (V9/V10). A LEEF header is not the syslog header, and CEF escaping is not LEEF escaping. Full mapping catalogs are unnecessary. |
| Binary template/data records | A template describes field IDs and lengths so later data can be interpreted. NetFlow and IP Flow Information Export (IPFIX) headers and sets group these records (S8/S9). Template availability affects replay even when the data bytes survive. Field registry enumeration is unnecessary. |

### 5. What can receipt establish? Bounded mechanism taxonomy

The following families distinguish **documented support** from standards-only
plausibility (notably IPFIX here), not measured customer
prevalence. No family is called common in current customer use. A stronger
support-prevalence claim needs a defined product population and a systematic
version/mode inventory. A customer-prevalence claim needs dated, representative
customer deployment/configuration data with sampling limits. Historical wording
in a protocol specification is not that evidence.

**T1 — Syslog-family export over UDP (S2/S5; V1/V2/V4).**
One RFC 5426 syslog message occupies one UDP datagram payload; the message may
already be truncated. This is a convention to verify for vendor modes, not a
universal property of every UDP exporter. No connection handshake, application
receipt acknowledgment, ordering guarantee, or receiver-driven flow control
is supplied. Loss, reordering, duplicate/replayed input, and forgery remain
possible. Addresses/ports and receipt time may be observed; header host/time
remain source claims. Fragment loss can lose the whole datagram. A returned
send call cannot establish remote receipt (inference from S2/S5). Bare UDP
provides no confidentiality. **Raw-boundary pressure:** distinguish delivered
bytes from attempted records and from source-side truncation; never infer
complete history from successful receipts. **Needs experiment:** vendor datagram
packing, size limits, source queue/drop behavior, local receive truncation,
reordering/loss under controlled failures, and any sender diagnostics.

**T2 — Syslog-family export over clear TCP (S3/S6; V1/V4/V9).**
Messages cross an initiated connection as stream bytes. Length-prefixed or
terminator framing supplies boundaries separately. TCP retries transport data,
preserves byte order within that connection, and can slow the sender or report
connection failure. It does not acknowledge durable application receipt.
Reconnect/retry behavior can create application duplicates or gaps; it does
not inherit one continuous message order across connections. These are
inferences about the application boundary, not measured vendor retries.
Bare TCP exposes content; peer address/ports, connection lifecycle, byte counts,
and local time may be observable. **Raw-boundary pressure:** split/coalesced
reads and incomplete final frames must not be confused with vendor record
boundaries. **Needs experiment:** actual framing variant, buffering, failure
latency, reconnect policy, application retransmission, and pressure effects
on source logging. V1's word “reliable” does not settle these questions.

**T3 — Syslog with TLS over a connection (S7; V4/V9).**
The receiver sees framed application bytes after decryption, not stable TLS
ciphertext fixtures. RFC 5425 specifies octet-counted syslog; a vendor's TLS
checkbox alone does not establish compliance with that framing. Authentication,
confidentiality, and integrity depend on certificate/trust configuration; a
certificate identifies the authenticated peer, not every origin behind it.
Connection security neither accounts for earlier losses nor proves persistence.
TCP pressure/failure limits from T2 still apply. **Raw-boundary pressure:** keep
security observations distinct from source identity and application acceptance.
**Needs experiment:** actual framing, certificate rejection/client authentication,
negotiated version, reconnect, buffering, and failure behavior. Do not infer
security from a port number or modernize documented TLS versions silently.

**T4 — Binary flow export (S8/S9; V7/V8).**
NetFlow v9 and IP Flow Information Export (IPFIX) describe traffic using template
and data records; multiple records can occupy one export message. NetFlow v9
and IPFIX have different header/counting contracts. Their sequence information
can reveal discontinuities within its documented scope, but cannot prove that
all underlying traffic generated observations. UDP export inherits T1's
transport limits. IPFIX also specifies TCP and Stream Control Transmission
Protocol (SCTP), a message-oriented transport with configurable delivery behavior;
this is standards coverage, not demonstrated support by the Cisco candidate.
IPFIX specifies security mappings; none is assumed for Cisco's NetFlow Secure Event Logging (NSEL) mode.
**Raw-boundary pressure:** source/domain context and templates affect future
interpretability; preserving only selected data fields risks accidental
normalization. **Needs experiment:** template arrival/loss/restart, actual
versioned template layout, export batching, source counters, and failure gaps.
Cisco evidence establishes firewall NSEL support, not firewall-wide IPFIX
adoption. Generic IPFIX firewall applicability remains plausible here.
A NetFlow FlowSet or IPFIX Set is a length-delimited group of template or data
records; data-record layout depends on its template. IPFIX also has a total
message length (S8/S9). The **observation domain** scopes observations within an
exporter; it is not a globally unique device ID. Export time, sequence, domain,
and templates are source-provided metadata, while network peer and receipt time
are collection observations. UDP cannot push receiver pressure back through
this protocol; connection-based mappings can, but exporter waiting/dropping
policy remains separate. Local exporter errors/counters (V7) are not receiver
acknowledgments; source counter observation would require separate access.

**T5 — Source/vendor API retrieval (A6).**
An API can return stored firewall logs via
requests, rather than a firewall pushing syslog. PAN's documented retrieval
uses an asynchronous job: request work, then retrieve results. It exchanges
Hypertext Transfer Protocol over TLS (HTTPS) responses with Extensible Markup
Language (XML) content; response and entry boundaries replace syslog framing.
Service endpoint, request/job status, and returned source fields are possible
receipt evidence. Pacing queries is not backpressure on firewall observation.
**Authoritative documentation:** A6 exposes batch size, skip and order controls;
a completed retrieval deletes its job. **Inference/unknown:** repeated queries
can overlap; changing source data, retention and permissions constrain completeness. The cited API
page establishes retrieval support, not continuous lossless export or exact
wire equivalence with syslog. **Needs experiment/documentation follow-up:**
version-pinned result serialization, pagination under concurrent writes, retry behavior,
authentication, truncation and retrieval failure limits. Management APIs that
only change policy are excluded; this one returns relevant observations.

**T6 — Managed service streams and object delivery (A1–A5).**
AWS Network Firewall supports log destinations in CloudWatch Logs (managed log
service), Firehose (managed delivery stream), and Simple Storage Service (S3,
object storage). These are source-side delivery alternatives here, not chosen
platform infrastructure. S3 delivers compressed log files, whose outer object
boundary is distinct from inner JSON record boundaries. Retrieving an object
exposes HTTP status/body and object metadata; it does not expose the firewall's
network peer. Publication interval and end-to-end arrival latency differ (A3/A4).
**Inference/unknown:** delayed/interleaved records are not a total event order;
successful object retrieval does not prove upstream completeness. Reader pacing
cannot be assumed to slow the firewall. Storage encryption is distinct from
HTTPS transport protection. **Needs experiment:** exact decompressed record
separator and compression representation, duplicates/retries, permissions and
late arrivals, and behavior when delivery fails. CloudWatch/Firehose batch and
subscription contracts were not investigated; no guarantees are transferred
between the three destinations. The provider-internal first hop is opaque.

Not surveyed further: network protocols carrying user traffic, pure management
control, downstream platform envelopes, exhaustive vendor integrations, or
all possible telemetry mechanisms. RFC 3195 appears only as Fortinet's separate
legacy option (V1); it is not conflated with RFC 6587 and is not investigated as
a candidate. This taxonomy is sufficient for the named comparison, not exhaustive.

## Comparison criteria for later selection

These are **proposed decision criteria**, derived from E1/E3, OQ-0001–0003,
and the taxonomy. They have no numerical weights and imply no winner.

| Criterion | Why it changes the decision | Evidence sufficient to evaluate it |
| --- | --- | --- |
| C1 Customer/source relevance | A teachable integration might be irrelevant to the intended customer. | Dated customer inventory or conversation naming deployment, versions, export mode, constraints, and provenance. In its absence explicitly accept a teaching-only rationale. |
| C2 Mechanism representation | A fixture should expose a meaningful collection responsibility, not merely recognizable branding. | Applicable vendor support plus standards explaining the boundary and an explicit account of what the fixture omits. Support is not usage. |
| C3 Precise profile identity | A vendor name cannot determine reproducible bytes. | Product/deployment, build or bounded document version, payload family, logging/filter settings, transport/security/framing, header mode, customizations, and remaining ambiguity. |
| C4 Accessible primary evidence | Readers and agents need to inspect and challenge claims. | Stable public source links, sections, dates, version applicability, and a retrievable reference; redirects/access failures must be disclosed. |
| C5 Layer separation | Otherwise the first fixture silently chooses unrelated architecture. | Each layer in the model populated independently, with unknowns where the vendor does not specify it. |
| C6 Wire reconstructability | A log UI example can omit precisely the boundary under investigation, while the teaching contract may deliberately choose a bounded source-shaped representation. | Record separately exact vendor-observed bytes, exact vendor-documented behavior, responsibly reconstructed source-shaped content, explicit teaching simplifications, exact simulator bytes, and runtime behavior still requiring experiment. |
| C7 Synthetic reproducibility | Public teaching cannot depend on private traffic or credentials. | Independently authored synthetic values and provenance; exact encoding/settings; reproducible representation without claiming an appliance emitted it. |
| C8 First-edge pressure | Useful teaching needs observable distinctions around failure, delimitation, identity, or drift. | Documented failure modes plus questions a bounded later experiment could test; no experiment is designed here. |
| C9 Convenience and atypicality | Convenient JSON or a perfect one-line sample may conceal the real boundary. | Explicit account of omitted layers, intermediary transformations, templates, source filters, and unavailable validation; compare against other families. |
| C10 Teaching value | Precision must remain accessible without teaching all of network security. | A plain-language path, a concrete example, retained architectural qualifications, and reader-confusion notes. No prevalence claim needed. |
| C11 Version stability/drift | Reproducible evidence must survive mutable pages, optional fields, and software updates. | Pin source/build and configuration; inspect changelog/version scope; record fields or behaviors whose stability is unproven. |
| C12 Rights and provenance | Public accessibility does not automatically permit copying examples, manuals, or software. | Identify applicable terms and permissions before redistribution; original synthetic values, minimal attributed excerpts, and source links. A legal conclusion is not established by this research. |

C3 remains a profile-identity gate. C6 is satisfied for selection only when the
exact bytes of the simulator contract are specified and every departure from
vendor-documented behavior is named; vendor-observed bytes are not required if
the contract clearly labels reconstruction and teaching simplification. C7 is
satisfied only when those bytes can be reproduced from synthetic inputs without
claiming device observation. Runtime behavior remains an experiment claim even
after C6/C7 are satisfied.

## Primary-source trace

All sources below are **authoritative documentation within their stated domain**,
accessed **2026-09-10**. Access date is not publication date. Unless stated,
no revision date was visible/established. V/A sources establish vendor support
only in their stated version/deployment; S sources establish protocol/format
conventions, never vendor conformance. No source is verified local runtime
behavior. The taxonomy and candidate references state the applicable claim;
the limitations below constrain every reuse. Repeated references point to the
same claim, not independent corroboration.

### Protocol and format evidence

| ID | Primary source, title, version/date and relevant section | Directly established / applicability | Limits and verification still needed |
| --- | --- | --- | --- |
| S1 | [RFC 791, Internet Protocol](https://www.rfc-editor.org/rfc/rfc791.html), September 1981, §§2.3, 3.1–3.2; [RFC 8200, Internet Protocol, Version 6 Specification](https://www.rfc-editor.org/rfc/rfc8200.html), July 2017, §§3, 4.5 | Network header addresses and version-specific fragmentation. Explains why export IP packets and application messages are separate units. | No local path, address translation, packet capture, or socket metadata availability established. Actual fragmentation requires observation. |
| S2 | [RFC 768, User Datagram Protocol](https://www.rfc-editor.org/rfc/rfc768.html), August 1980, Format/Fields/User Interface | Datagram ports/length and minimal transaction-oriented delivery, without delivery or duplicate protection guarantees. | No vendor packing, retries, or OS receive API behavior; test those separately. |
| S3 | [RFC 9293, Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc9293.html), August 2022, §§2.2, 3.1, 3.8.6 | Reliable ordered byte stream, transport acknowledgment/flow control, and no correlation of segment boundaries with application read/write boundaries. | Does not promise application persistence or determine vendor queues/reconnects; no local TCP test. |
| S4 | [RFC 5424, The Syslog Protocol](https://www.rfc-editor.org/rfc/rfc5424), March 2009, §§4–6, 8; [RFC 3164, The BSD Syslog Protocol](https://www.rfc-editor.org/rfc/rfc3164.html), August 2001, §§4–5 | Layered originator/relay/collector model; 5424 header, priority, structured data and optional body. 3164 documents legacy header/message practice. | 3164 is Informational, not proof every vendor has an identical envelope. 5424 body is not universally UTF-8 text. Vendor modes and field availability require confirmation. |
| S5 | [RFC 5426, Transmission of Syslog Messages over UDP](https://www.rfc-editor.org/rfc/rfc5426), March 2009, §§3.1–3.4, 4–5 | One potentially truncated syslog message per datagram; message sizing/fragment risks; no loss repair or ordering; source address can be relay; weak authentication/no confidentiality. | Applies to this mapping, not arbitrary UDP output. Vendor and receive-buffer limits, truncation indicators, loss and diagnostics need experiments. |
| S6 | [RFC 6587, Transmission of Syslog Messages over TCP](https://www.rfc-editor.org/rfc/rfc6587), April 2012, §§3.2–3.5, 4–5 | Historical TCP syslog framing: byte-count prefix or non-transparent terminator framing, with embedded-delimiter ambiguity. | Historic RFC, not a vendor-specific framing guarantee or a security recommendation. Exact mode/terminator and reconnect behavior need vendor evidence/capture. |
| S7 | [RFC 5425, TLS Transport Mapping for Syslog](https://www.rfc-editor.org/rfc/rfc5425), March 2009, §§4.2–4.4, 5–6 | TLS peer authentication policies and octet-counted application data; TLS record and syslog message boundaries can differ; hop protection limits. | Old protocol requirements are not a current cipher policy. Product TLS support alone does not prove this mapping; test trust, framing, negotiation, and failures. |
| S8 | [RFC 3954, Cisco Systems NetFlow Services Export Version 9](https://www.rfc-editor.org/rfc/rfc3954.html), October 2004, §§2–7, 9–10 | Export header, template/data FlowSets, field layouts, source/observation-domain scope; sequence counts export messages. | Informational; not IPFIX and not an exact ASA firmware template. Source/domain continuity and templates require versioned appliance evidence. |
| S9 | [RFC 7011, Specification of the IPFIX Protocol for the Exchange of Flow Information](https://www.rfc-editor.org/rfc/rfc7011), September 2013, §§2–3, 8–11 | Template/data sets; IPFIX sequence counts data records, excluding templates; UDP/TCP/SCTP mappings, security considerations, pressure and loss limits. | No candidate's IPFIX implementation established. Sequence gaps do not count all unobserved traffic. Field interpretation and template recovery remain beyond raw receipt. |
| S10 | [ArcSight Common Event Format white paper](https://community.opentext.com/cfs-file/__key/telligent-evolution-components-attachments/00-224-01-00-00-15-93-98/CEF-White-Paper-071709.pdf), 2009 document, “Common Event Format” and “Character Encoding,” CEF version 0; [IBM LEEF event components](https://www.ibm.com/docs/en/qradar-on-cloud?topic=overview-leef-event-components), living QRadar on Cloud page, LEEF 1.0/2.0, Syslog header/LEEF header/Event attributes | CEF header/extension and escaping; LEEF optional syslog header, own header and tab/custom attribute delimiters. These are integration record formats. | Not interchangeable or transports. IBM receiver-specific allowances are not general RFC grammar. No vendor mapping or current CEF revision inferred; no sample redistribution permission inferred. |
| S11 | [RFC 4180, Common Format and MIME Type for CSV Files](https://www.rfc-editor.org/rfc/rfc4180.html), October 2005, §2 | Informational CSV quoting/separator example, including quoted embedded commas/newlines. | Explains why commas are not sufficient to split arbitrary records; does not establish PAN/Fortinet dialect or syslog framing. |
| S12 | [RFC 8259, The JSON Data Interchange Format](https://www.rfc-editor.org/rfc/rfc8259.html), December 2017, §§2–8 | JSON value/object/array/string grammar and interoperable text encoding. | Not a stream record separator, schema, or claim that source serialization is canonical. |

### Firewall export evidence

| ID | Primary source, title, version/date and section | Directly established / applicability | Limits and verification still needed |
| --- | --- | --- | --- |
| V1 | [FortiOS 7.4.8 CLI Reference: config log syslogd4 setting](https://docs.fortinet.com/document/fortigate/7.4.8/cli-reference/326975389/config-log-syslogd4-setting), mode/format/port/source-ip/enc-algorithm | Explicit UDP, RFC 6587 reliable TCP, distinct RFC 3195 legacy mode; default/CSV/CEF/RFC5424/JSON formats; transport/security controls for this target. | Search service returned primary page text; direct open returned 403. Does not identify RFC 6587 sub-framing or complete default bytes. Candidate uses the documented fourth syslog target to avoid silently transferring commands across targets. Re-fetch before contract freeze. |
| V2 | [FortiOS 7.4.8 Administration Guide: Log settings and targets](https://docs.fortinet.com/document/fortigate/7.4.8/administration-guide/250999/log-settings-and-targets), Remote logging/Log filters | Remote syslog and format choices; export filters can choose log types/severities, including forward traffic. | Primary text obtained through search; direct open 403. Configuration possibilities do not prove deployed filters, queue behavior, or exact serialization. |
| V3 | [FortiOS 7.4.8 Log Message Reference: Log message fields](https://docs.fortinet.com/document/fortigate/7.4.8/fortios-log-message-reference/357866), raw traffic example and field table; [Introduction](https://docs.fortinet.com/document/fortigate/7.4.8/fortios-log-message-reference/524940/introduction); [What's new](https://docs.fortinet.com/document/fortigate/7.4.8/fortios-log-message-reference/172065/whats-new), 7.4.8 | Key-value traffic/forward example; field meanings. Introduction says 7.4.8 or higher. Changelog lists removed event fields. | Example is downloaded raw log text dated 2017, not captured 7.4.8 wire bytes. Introduction/changelog available via primary search text, direct opens 403. Header, terminator, field optionality and exact build remain unverified. |
| V4 | [PAN-OS 11.1 Configure Syslog Monitoring](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring), steps 1–5; [Device > Server Profiles > Syslog](https://docs.paloaltonetworks.com/ngfw/help/11-1/device/device-server-profiles-syslog), Servers/Custom Log Format | UDP/TCP/SSL transport choices, SSL means TLS 1.2 here, BSD/IETF formats, custom CEF setup, logging/forwarding activation, optional client authentication. Help ties formats to RFC 3164/5424. | 11.1 family, not patch pin. No byte framing guarantee found. Actual certificates, egress, runtime TLS and buffering need validation. |
| V5 | [PAN-OS 11.1 Syslog Field Descriptions](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions), introduction and linked log types | Default fields are comma-separated; different payload families and custom escaping exist. | Must obtain exact patch-applicable field order and escaping before reconstructing bytes; CSV label is not RFC 4180 conformance. |
| V6 | [Traffic Log Fields, requested 11.1 URL](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/traffic-log-fields), redirects to [shared Traffic Log Fields](https://docs.paloaltonetworks.com/ngfw/administration/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/traffic-log-fields), field table | Shared page distinguishes management-plane receive time, generation time, serial, session source addresses, action, packet counts and log sequence. | Observed redirect loses a unique version pin. Useful semantic examples, not authoritative evidence of the entire 11.1 field list. No field count or exact byte fixture inferred. |
| V7 | [Cisco ASA Series General Operations CLI Configuration Guide 9.2: NetFlow Secure Event Logging](https://www.cisco.com/c/en/us/td/docs/security/asa/asa92/configuration/general/asa-general-cli/monitor-nsel.html), updated 2014-03-18, Information/Configuring collectors/Flow-export actions/Monitoring | ASA NSEL uses NetFlow v9 and exports flow-state observations. Destination config names interface, IPv4 target and UDP port; source counters and event filtering are documented. | Explicitly an older versioned comparison, not a claim about current deployment/support. Patch/model unpinned. No reproduction performed. Exact template must match the target build. |
| V8 | [Cisco Secure Firewall ASA NetFlow Implementation Guide](https://www.cisco.com/c/en/us/td/docs/security/asa/special/netflow/asa_netflow.html), updated 2022-05-31, About NSEL/Templates/Field Descriptions/Guidelines/Configure NSEL Collectors | Binary field definitions, per-device templates, time-based template refresh, UDP export, source-side flow lifecycle records. | Cross-version corroboration of the mechanism, not permission to graft 2022 templates onto ASA 9.2. Numeric field layouts need a compatible revision/capture. “Secure” in NSEL does not establish encrypted transport. |
| V9 | [Check Point R81.20 Security Management Guide: Log Exporter](https://sc1.checkpoint.com/documents/R81.20/WebAdminGuides/EN/CP_R81.20_SecurityManagement_AdminGuide/Content/Topics-LMG/Log-Exporter.htm), page date 2026-06-14, support list | Exporter is on Management/Log Server; TCP/UDP, CEF/LEEF/JSON/syslog, TLS 1.2 mutual authentication, security/audit logs, filtering. | Exporter is updated independently; article links support knowledge-base details not audited here. Earlier gateway-to-server hop is not specified by these external-export settings. |
| V10 | [R81.20 Configuring Log Exporter in SmartConsole](https://sc1.checkpoint.com/documents/R81.20/WebAdminGuides/EN/CP_R81.20_LoggingAndMonitoring_AdminGuide/Content/Topics-LMG/Log-Exporter-Configuration-in-SmartConsole.htm), dated 2026-07-07, Data Manipulation; [Advanced Configuration in CLI](https://sc1.checkpoint.com/documents/R81.20/WebAdminGuides/EN/CP_R81.20_LoggingAndMonitoring_AdminGuide/Content/Topics-LMG/Log-Exporter-Configuration-in-CLI-Advanced.htm), protocol/read-mode/reconnect-interval | Format selection, optional aggregation, raw versus semi-unified export, reconnect interval. | Semi-unified mode combines a record with earlier records sharing its ID. CLI extraction displays conflicting default read-mode text; do not infer default. No exact terminator/LEEF revision or field mapping established. Explicit settings and target configuration files/capture needed. |
| V11 | [FortiOS 7.4.8 Log Message Reference: 13 - LOG_ID_TRAFFIC_END_FORWARD](https://docs.fortinet.com/document/fortigate/7.4.8/fortios-log-message-reference/13/13-log-id-traffic-end-forward), Traffic category/field table, accessed 2026-09-10 | The named Traffic/forward, session-end log family and its documented fields/action meanings for the 7.4.8 reference. | Page was discoverable through Fortinet's primary index/search but direct retrieval returned 403. The field table does not specify which optional fields appear, their emitted order, a syslog envelope, character encoding, or a datagram/message boundary. |
| V12 | [PAN-OS Syslog Field Descriptions: Escape Sequences](https://docs.paloaltonetworks.com/ngfw/administration/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/escape-sequences), shared living page, accessed 2026-09-10 | A field containing a comma or double quote is enclosed in double quotes; an interior quote is doubled. | Primary source for the described escaping, but it is not a version-pinned 11.1 page. It does not supply a complete Traffic record, syslog envelope, stream boundary, character-set declaration, or an emitted sample. |
| V13 | [FortiOS 7.4.0 CLI Reference: config log syslogd4 filter](https://docs.fortinet.com/document/fortigate/7.4.0/cli-reference/419620), filter parameters, accessed 2026-09-10 | Primary documentation explicitly names the fourth syslog target's `forward-traffic` filter and its enable/disable behavior. | This corroborates the mechanism but is 7.4.0, not a 7.4.8 reference. V2 is the 7.4.8 administration source for the corresponding remote-syslog filter; a 7.4.8 target-specific filter page or device observation would close the minor version gap. |

### Service delivery and API evidence

| ID | Primary source, title, version/date and section | Directly established / applicability | Limits and verification still needed |
| --- | --- | --- | --- |
| A1 | [Logging network traffic from AWS Network Firewall](https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-logging.html), living Developer Guide, logging eligibility/types; [LogDestinationConfig](https://docs.aws.amazon.com/network-firewall/latest/APIReference/API_LogDestinationConfig.html), API reference, destination types | Logging applies to traffic sent to the stateful engine; flow/alert/TLS families have different triggers. S3, CloudWatch Logs, Firehose are supported destinations. | No fixed firewall software version. No universal full-traffic coverage or lossless guarantee established. No cloud runtime inspected. |
| A2 | [Contents of an AWS Network Firewall log](https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-logging-contents.html), fields/flow and alert examples | Service fields wrap engine JSON. Flow type `netflow` is unidirectional; engine output and TLS inspection attributes have specified distinctions. | Examples are JSON records, not a complete S3 byte stream. Do not substitute upstream Suricata version or assume Cisco NetFlow. Engine version and serialization need evidence. |
| A3 | [Sending AWS Network Firewall logs to S3](https://docs.aws.amazon.com/network-firewall/latest/developerguide/logging-s3.html), delivery/file layout/permissions/log access | Compressed files published in batches, five-minute publication interval, 75 MB size threshold, interleaved connection entries, named bucket path and storage-encryption options. | Not a five-minute receipt guarantee. Does not establish exact compressed bytes or inner separator here. No duplication/completeness guarantee inferred. |
| A4 | [Timing of AWS Network Firewall log delivery](https://docs.aws.amazon.com/network-firewall/latest/developerguide/firewall-logging-timing.html), timing/delays | Describes average S3 arrival of 8–12 minutes and possible longer delay; late logs use activity-period dates. | Provider-described averages, not measured results, upper bounds, or service-level guarantees. |
| A5 | [Amazon S3 API: GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html), Request Syntax/Response Syntax/Response Elements | Object retrieval returns an HTTP response body and metadata such as content length, last modified time and entity tag. | Retrieval metadata is about storage, not original firewall identity or pre-storage completeness. Concrete network/TLS session and exact bytes untested; no downstream storage design selected. |
| A6 | [PAN-OS XML API: Retrieve Logs](https://docs.paloaltonetworks.com/ngfw/api/pan-os-xml-api-request-types-and-actions/retrieve-logs), current shared API guide, log-type/action and traffic retrieval example | HTTPS job request and later result retrieval for firewall logs, including XML results. | Shared version selector, not immutable 11.1 patch evidence. Batch/skip/order controls and job deletion are documented; authentication, concurrent-pagination and retry correctness remain unverified; supported alternative, not an exact candidate contract. |

Access/rights boundary: links and short paraphrases are retained, not copies of
manuals or vendor sample datasets. RFCs carry IETF Trust notices; vendor pages
remain subject to their publishers' terms. Public readability is not a blanket
license for redistribution, example adaptation, firmware access, or trademarks.
No fixture, license grant, or legal clearance is claimed. Before publishing
reconstructed bytes, distinguish an original synthetic construction from quoted
vendor material and check applicable terms for anything actually reused.

## Neutral candidate comparison

The five candidates are **research comparison definitions**, not platform source
profiles. Before E5, none was selected, preferred, ranked, or ready for contract
freeze; E5 later selected F1 as the Article 2 source-shaped basis. They cover direct datagram export, encrypted stream export, binary
flow export, a vendor-managed intermediate hop, and managed cloud object
delivery. This is **inferred mechanism coverage** from cited support, not a
survey of customer or market prevalence. Clear TCP is additionally evidenced
by the syslog candidates' available modes; it needs no sixth vendor.

| Candidate definition | Network / transport / connection / security | Framing / envelope / encoding / payload family | Readiness and criterion implications |
| --- | --- | --- | --- |
| P1 Fortinet FortiGate appliance family, FortiOS 7.4.8; fourth remote syslog target, `mode udp`, `format default`, forward-traffic family (V1–V3) | Research configuration: IPv4 destination and UDP target port; connectionless, no TLS. Source address/interface are configuration-dependent. | Datagram-delimited family; exact one-message packing still to verify. Default syslog envelope bytes unresolved. Vendor key-value traffic/forward record; log ID 0000000013 example is a reference, not a selected fixture. | C2/C5: direct UDP and key-value comparison. C3/C4/C6/C11: model/build, header and exact emitted fields unresolved; reference includes older sample and “or higher” applicability. F1 is selected as the source-shaped basis, pending contract shaping. |
| P2 Palo Alto Networks firewall appliance family, PAN-OS 11.1; direct syslog server profile with SSL/TLS 1.2, IETF header mode, default Traffic format, session-end logging (V4–V6) | Research configuration: IPv4 syslog endpoint, TCP/TLS, explicit port; sender establishes secure connection. Client authentication is available but trust configuration is unresolved. | Intended IETF/syslog envelope plus comma-separated Traffic body. Stream framing unknown; RFC 5425 octet counting is a standard comparison, not proven PAN behavior. | C2/C5/C8: stream/security and positional format pressure. C3/C4/C6/C11: patch/model, escaping, framing and versioned field table gaps. Shared-page redirect prevents claiming exact 11.1 bytes. |
| P3 Cisco ASA appliance family, ASA 9.2 documentation profile; NSEL NetFlow v9 export, IPv4 collector, flow-state records under configured event filter (V7; V8 corroborates family only) | Explicit interface, destination and UDP port. Connectionless; no encryption established. “Secure Event Logging” is a name, not a TLS guarantee. | UDP export message containing NetFlow header and template/data FlowSets; binary records describe flow changes rather than syslog text. Exact template/field membership unresolved. | C2/C5/C8: tests the comparison against template-dependent records. C1/C3/C9/C11: old version is a serious relevance limitation; patch/model and compatible field reference needed. Not evidence that customers run 9.2. |
| P4 Check Point R81.20 gateway observations via R81.20 Management/Log Server Log Exporter; TCP, LEEF, security logs, explicit `read-mode raw` comparison (V9/V10) | External connection originates at server, not necessarily gateway. Research mode is clear TCP; TLS 1.2 is a documented alternative, not implicitly enabled. Exporter build, OS/deployment and source address unresolved. | TCP framing/terminator unknown. LEEF encoding is documented support; exact LEEF version, header template and field map unproven. Raw read-mode avoids combining updates with earlier records by definition; does not mean original gateway bytes. | C2/C5/C8/C9: exposes an earlier hop and exporter transformation. C3/C6/C11: pin gateway/server/exporter builds and configuration; conflicting rendered default text makes explicit mode essential. |
| P5 Amazon Web Services (AWS) Network Firewall managed service, FLOW logging to S3; Developer Guide/API documentation as accessed 2026-09-10 (A1–A5) | Provider-internal export network/security not exposed by these docs. External retrieval is object API/HTTPS, not firewall-to-socket syslog. No controllable firewall firmware pin. | Compressed object with inner service-wrapped JSON flow records; no syslog envelope. Exact compression and record separators unresolved. Source format is AWS flow JSON with engine `netflow` output. | C2/C5/C9: moves the visible collection boundary after managed delivery. C3/C6/C11: access-date scope is weaker than a firmware pin; account/region/export configuration and stable source snapshot needed. |

### P1 — What the comparison can and cannot reproduce

**Authoritative documentation:** V1–V3 establish supported configuration and a
source-format example. **Inference:** independently authored key-value values
can support a syntactic teaching illustration. They cannot establish the full
syslog envelope or pretend the historical example was captured from 7.4.8.
**Unknown:** exact model/build, character escaping, field omissions/order,
message length/truncation, timestamp precision and sender buffering. A
version-pinned appliance or appropriately licensed virtual environment plus
capture would be needed for vendor runtime verification; none was used.

**Risks:** pre-collection UDP loss, an address mistaken for device identity,
format drift, and sensitive host/policy values. Choosing only uncomplicated
basic Latin text values could conceal escaping pressure (C7–C10). Responsibly reconstructable
payload fragments are possible; a complete vendor-verified wire representation
is not yet established. No fixtures were produced. **Rights/access:** public
primary pages with intermittent 403; no permission to redistribute sample logs
or firmware established. Use original synthetic values and resolve V3's exact
applicability before any later contract.

### P2 — What the comparison can and cannot reproduce

**Authoritative documentation:** V4/V5 separate export settings from vendor
fields. **Observed documentation fact:** V6's requested versioned URL redirects
to a shared page. **Inference:** the family usefully exposes independently
variable security, framing and positional encoding. **Unknown:** complete
11.1 patch-specific bytes and connection framing; no standard fills that gap.
A target-build appliance or licensed virtual deployment would be needed for
runtime validation. No access was exercised or assumed.

**Risks:** TLS can conceal a framing error from packet inspection unless the
application boundary is examined; successful TLS does not establish prior
completeness. Header hostname, connection peer, and serial are different claims.
A record's “Receive Time” can refer to an internal management component (V6),
not our receipt. Commas, optional/trailing fields, and custom formats create
drift pressure (C6/C8/C11). **Reproducibility:** original synthetic CSV values
are feasible only after pinning fields/escaping; TLS ciphertext is not the
reproducible payload contract. **Rights/access:** public docs, mutable redirect;
no guide/sample redistribution or software license granted by access.

## Narrow direct-edge finalist evidence-gap pass — 2026-09-10

**Scope and status:** This pass applies E4 to P1 and P2 only. It changes neither
the five-candidate comparison nor platform authority. “Established” below means
the cited source directly supports the bounded item; “established with
qualification” means it does so with the stated version, access, or scope limit;
“unresolved” means the cited sources do not establish it; “contradicted” would
mean primary sources make the definition impossible. No item is contradicted and
neither profile is silently corrected or substituted.

### Finalist definitions

**F1 — FortiGate / FortiOS 7.4.8 direct UDP Traffic/forward session-end log.**
Fortinet FortiGate firewall product family, deployment form unpinned by the
sources, using the fourth remote-syslog target documented by V1. The bounded
configuration is `status enable`, a configured server/port, `mode udp`,
`format default`, and a syslog filter with `forward-traffic enable` (V1/V2).
The precisely named payload family is the Traffic-category, forward-traffic,
session-end record `LOG_ID_TRAFFIC_END_FORWARD` (V11). This names a direct
firewall-to-collector export, not FortiAnalyzer, FortiManager, FortiCloud, or a
historic simulator. FortiOS 7.4.8 is the documentation release, not a hardware
model, patch/build, or captured appliance revision.

**F2 — Palo Alto Networks PAN-OS 11.1 direct TLS Traffic session-end syslog.**
A Palo Alto Networks next-generation firewall running the PAN-OS 11.1 document
family, physical/virtual deployment and maintenance release unpinned. The
bounded configuration is a direct Syslog Server Profile with an address/FQDN,
explicit server port, `SSL (TLS)` transport, IETF header-format selection, and a
Log Forwarding Profile assigned to a security policy with `Log at Session End`
(V4). The precisely named payload family is the Traffic log, `end` subtype
(V6). The source directly says SSL supports TLS 1.2; client authentication is
optional and must be explicitly configured if used (V4). This excludes Panorama,
CEF custom formatting, API retrieval, and the historical simulator.

### Selection-threshold matrix

| Threshold item | F1 — FortiOS UDP Traffic/forward | F2 — PAN-OS TLS Traffic/end |
| --- | --- | --- |
| Vendor/product/deployment family | **Established with qualification.** FortiGate and 7.4.8 are named; appliance/VM model is not. V1/V2/V11. | **Established with qualification.** PAN-OS 11.1 NGFW is named; physical/virtual product and maintenance release are not. V4/V6. |
| Applicable version/documentation | **Established with qualification.** 7.4.8 reference pages; no appliance build/capture. V1–V3/V11. | **Established with qualification.** 11.1 document family; Traffic field URL redirects to a shared living page. V4–V6. |
| Official export configuration | **Established.** Target, UDP mode, default format, and forward-traffic filter are documented. V1/V2. | **Established.** Profile, TLS option, IETF format, policy forwarding, and session-end trigger are documented. V4. |
| Precisely named payload family | **Established.** `LOG_ID_TRAFFIC_END_FORWARD` Traffic/forward session-end. V11. | **Established.** Traffic, `end` subtype. V6. |
| Transport | **Established.** Vendor `mode udp`. V1. | **Established.** Vendor SSL/TLS option over a syslog server connection. V4. |
| Applicable transport security | **Established.** This bounded UDP mode has no TLS; UDP itself supplies neither confidentiality nor peer authentication. V1/S2. | **Established with qualification.** TLS 1.2 is vendor-documented; client authentication is optional and trust/certificate settings are profile facts. V4/S7. |
| Defensible message framing | **Unresolved.** UDP supplies datagram boundaries (S2), and RFC 5426 describes one syslog message per datagram only for that mapping (S5); Fortinet does not document default-format record packing or RFC 5426 conformance. | **Unresolved.** TCP is a stream (S3); TLS records do not define syslog boundaries (S7). PAN names IETF format and TLS but does not state octet counting, delimiter use, or RFC 5425 implementation. V4. |
| Documented message envelope | **Unresolved.** `format default` does not document the leading syslog/header bytes or their absence. V1–V3. | **Established with qualification.** IETF header mode, facility/PRI selection, and hostname-format choices are documented; complete emitted header values/bytes are not. V4/S4. |
| Payload encoding and field order/structure | **Established with qualification.** Versioned field reference and raw key-value example establish family structure, but optionality/order and quoted-value rules remain unproven. V3/V11. | **Established with qualification.** 11.1 field documentation says comma-delimited fields; Traffic ordering is published on a shared page and quote/comma escaping is published on a living page. Full 11.1-pinned list is not retrievable. V5/V6/V12. |
| Exact or responsibly reconstructable application bytes | **Unresolved.** No authoritative complete default syslog envelope, packing rule, or 7.4.8 emitted sample. | **Unresolved.** No vendor framing declaration, complete 11.1-pinned Traffic list, or emitted TLS/IETF sample. |
| Primary provenance for every vendor-specific element | **Unresolved.** Configuration/family claims trace to V1/V2/V11, but the missing envelope/packing bytes have no source. | **Unresolved.** Configuration and field claims trace to V4–V6/V12, but exact header/framing bytes do not. |
| Documentation/inference/simplification separation | **Established.** This record labels each layer and refuses a byte reconstruction. | **Established.** This record labels vendor claims separately from RFC mappings and refuses a byte reconstruction. |
| Public inspectability | **Established with qualification.** Primary Fortinet pages are public but direct requests returned 403; search-indexed text was reviewed. | **Established with qualification.** Public primary pages are readable, but the versioned Traffic field URL redirects to a shared page. |
| Publishable short synthetic fixture | **Unresolved.** Original fictional values are practically possible, but no complete wire format or permission/terms assessment supports publication as a derived fixture. V1–V3/V11. | **Unresolved.** Original fictional values are practically possible, but versioned bytes, framing and permissions are not established. V4–V6/V12. |

The matrix does not make a legal conclusion. It records that public
documentation alone has not established permission to reproduce vendor sample
logs. A later original synthetic fixture could cite field structure without
copying a vendor sample only after its exact contract and publication review are
settled.

### Transport and framing evidence chains

**F1, vendor claim → standard mapping → remaining gap.** V1 explicitly exposes
remote-syslog `mode udp`; V2 documents remote syslog targets and the relevant
filter. S2 establishes that UDP carries discrete datagrams and provides no
connection, delivery acknowledgment, ordering, or receiver-driven flow control.
S5 establishes the conventional syslog-over-UDP mapping—one potentially
truncated syslog message per datagram—*if* the sender implements that mapping.
Neither V1 nor V2 says the default format is RFC 5424, RFC 5426, one record per
datagram, newline-terminated, ASCII/UTF-8, or unfragmented. Therefore the only
documented boundary is a UDP datagram; assigning a record/message boundary
inside it needs device observation or a vendor statement. A UDP send attempt or
collector datagram receipt would still not establish source-side completeness.

**F2, vendor claim → standard mapping → remaining gap.** V4 explicitly configures
SSL/TLS 1.2, a server/port, IETF or BSD header mode, and optional client
authentication; it describes IETF/TLS association as traditional, not a framing
specification. S3 establishes an ordered, reliable byte stream with no
application-message boundary. S7 specifies octet-counting for the RFC 5425 TLS
syslog mapping and separately explains that TLS records need not equal syslog
messages. It applies only if PAN's feature implements that mapping; V4 does not
say so. Thus TLS can authenticate/protect the configured hop subject to its
certificate setup, but neither TCP nor TLS establishes a delimiter, a length
prefix, reconnect behavior, byte encoding, or receipt of every earlier log.
Only a version-pinned vendor statement or an authorized device observation can
close that gap.

### Proposed application-message representations

**No representation is proposed for either finalist.** This is a deliberate
evidence result, not an omission to be repaired by invented bytes.

- **F1:** V3/V11 support a named key-value Traffic/forward *record body* and
  S2 supports an outer UDP datagram. They do not support the full application
  message: its default syslog envelope, any byte-order marker, character
  encoding, trailing newline/other terminator, or whether exactly one record is
  placed in each datagram. Writing a synthetic line would be a teaching
  simplification with several unstated assumptions, so it is withheld.
- **F2:** V4 supports the TLS/IETF configuration and V5/V6/V12 support
  comma-delimited Traffic fields and escaping. They do not support the complete
  11.1-pinned field list, IETF header bytes/values, an octet-count prefix or
  delimiter, or the relationship among application messages, TCP segments and
  TLS records. A TLS ciphertext capture would also not itself reveal application
  framing. Writing a line or prefix would therefore be an unsupported
  reconstruction, so it is withheld.

For both, the missing representation is application-layer bytes only. It would
not be an IP packet, UDP datagram, TCP segment, or TLS record. No observed device
output, packet capture, or synthetic fixture was produced in this pass.

### Twelve-criterion comparison without scoring

| Criterion | F1 outcome and reason | F2 outcome and reason |
| --- | --- | --- |
| C1 customer/source relevance | **Established with qualification:** E4 permits teaching-led selection; customer inventory remains absent. | **Established with qualification:** same limitation. |
| C2 mechanism representation | **Established:** direct UDP export exposes datagram/loss boundary. | **Established:** direct TLS stream exposes security/framing separation. |
| C3 precise profile identity | **Established with qualification:** vendor/release/mode/family named; model/build and byte contract absent. | **Established with qualification:** vendor/11.1/mode/family named; deployment/patch and byte contract absent. |
| C4 accessible primary evidence | **Established with qualification:** public source, intermittent 403. | **Established with qualification:** public source, mutable field-page redirect. |
| C5 layer separation | **Established with qualification:** all layers named; vendor record packing unknown. | **Established with qualification:** all layers named; stream framing unknown. |
| C6 wire reconstructability | **Unresolved:** complete application bytes unavailable. | **Unresolved:** complete application bytes unavailable. |
| C7 synthetic reproducibility | **Established with qualification:** future original values possible, exact wire contract absent. | **Established with qualification:** future original values possible, exact wire contract absent. |
| C8 first-edge pressure | **Established:** receipt cannot prove UDP delivery/completeness. | **Established:** TLS security and stream framing remain distinct. |
| C9 convenience/atypicality | **Established:** absence of a convenient exact fixture is explicit. | **Established:** no TLS/RFC 5425 shortcut is allowed. |
| C10 teaching value | **Established:** distinguishes traffic payload from UDP export path. | **Established:** distinguishes TLS protection from message framing. |
| C11 version stability/drift | **Established with qualification:** 7.4.8 docs, optional fields and live access issue. | **Established with qualification:** 11.1 docs, mutable redirect and evolving fields. |
| C12 rights/provenance | **Unresolved:** publication permission not assessed. | **Unresolved:** publication permission not assessed. |

### Teaching implications and reader-confusion review

| Finalist | New concept and architectural pressure | Misconception corrected | Complexity before it is needed |
| --- | --- | --- | --- |
| F1 | A firewall can report a Traffic/forward session observation as a key-value record while using UDP only to export it. The raw boundary must distinguish one received datagram from source completeness. | `proto=6` or `action=accept` in the record describes the observed session, not the UDP export path or proof the record arrived. | Explain UDP datagrams and loss limits; do not teach policy configuration, IP fragmentation mechanics, or FortiGate field catalogs. |
| F2 | TLS protects a firewall-to-collector connection while TCP remains a byte stream that still needs application framing. The raw boundary must not treat one TCP read, TLS record, or management-plane timestamp as one source record/collector receipt. | “TLS syslog” does not prove RFC 5425 octet counting, a newline delimiter, end-to-end completeness, or durable firewall identity. | Explain IETF header versus CSV body, basic certificate purpose, and stream framing; defer TLS setup, ciphers, policy UI, and the full Traffic field list. |

Both can remain a first-edge teaching example without a configuration guide: show
the selected export choices, then follow what a collector may observe and what
remains unknown. The article must retain the qualifications in the reader-
confusion notes: traffic versus export traffic; packet/datagram/stream versus
message; envelope versus payload; immediate peer versus source identity; and
receipt versus no prior loss.

### Eligibility result and smallest remaining judgment

Under the corrected threshold, F1 is selected as the source-shaped basis even
though vendor-observed bytes and vendor-backed packing are unavailable. The
selection requires an exact simulator contract later, with every departure from
FortiOS behavior labeled. F2 remains rejected for now because its TLS stream
framing and exact contract are not yet justified. This is a sequencing decision,
not a claim that F2 is inferior architecture.

The smallest remaining judgment is how to shape F1's exact simulator contract:
which documented fields and syntax to retain, which exact bytes to choose as
teaching simplifications, and which runtime behavior must remain an experiment
claim.

## Selected source-shaped basis and corrected contract threshold — E5

The selected profile is **FortiGate / FortiOS 7.4.8, direct fourth remote
syslog target, `mode udp`, `format default`, forward-traffic filter enabled,
Traffic/forward session-end record `LOG_ID_TRAFFIC_END_FORWARD`** (V1, V2, V11,
V13). V2 establishes the 7.4.8 remote-syslog filter capability; V13 is the
target-specific `syslogd4` filter corroboration from 7.4.0, so that small
version-specific detail remains qualified. The product family is FortiGate/FortiOS; physical versus virtual form,
hardware model, exact maintenance build, VDOM/override settings, destination
address/port, source interface/address, facility, priority, rate limit, and
other logging filters remain unselected or configuration-dependent. FortiOS
7.4.8 is a documentation release, not a claim that a particular appliance
revision was observed.

### Evidence states and contract boundary

The earlier threshold treated vendor byte documentation as a prerequisite for
the simulator's exact bytes. That was too strong for a teaching contract. The
corrected threshold keeps provenance strict while separating six states:

1. **Exact vendor-observed bytes:** captured from a named FortiGate build and
   configuration. None exist here; this requires controlled device observation.
2. **Exact vendor-documented behavior:** configuration options, payload-family
   identity, field definitions, and documented syntax/examples in V1–V3/V11/V13.
   Documentation does not prove runtime emission.
3. **Responsibly reconstructed source-shaped content:** independently authored
   from documented field names, syntax, and semantics with synthetic values. It
   is reconstruction, never captured output.
4. **Teaching simplification:** an intentional bounded choice where sources leave
   behavior open, such as one teaching record per UDP datagram. It names what is
   omitted or altered and the consequence.
5. **Exact simulator contract:** the complete byte sequence and boundary rule
   chosen and versioned before implementation. It is exact for the simulator,
   but need not equal unknown FortiOS output.
6. **Runtime experiment behavior:** actual packing, truncation, buffering,
   retries, loss, ordering, receiver failure, and observed envelope/encoding.
   These remain experiment claims until measured.

Selection requires complete provenance for Fortinet claims, an exact simulator
contract, and explicit reconstruction/simplification labels. It does not require
pretending that the simulator contract is vendor-observed.

**Vendor-documented elements:** FortiGate/FortiOS 7.4.8 scope; fourth syslog
target; UDP mode; default format; forward-traffic filter; Traffic/forward
session-end family and log ID 13; documented field names, types, lengths and
action meanings (V1/V2/V11/V13).

**Standards-derived elements:** UDP has source/destination ports and a datagram
payload without delivery acknowledgment, ordering, confidentiality, or
receiver-driven flow control (S2). RFC 5426 describes a syslog-over-UDP mapping,
but Fortinet's `format default` is not claimed to implement RFC 5426 or RFC 5424
unless later evidence says so (S4/S5). IP packets, UDP datagrams, and
application messages remain separate units (S1/S2).

**Inferred elements:** a short body using documented FortiOS field names and
syntax can be authored with synthetic values; a collector can observe a datagram
boundary and immediate peer metadata; receipt cannot establish source
completeness. These inferences do not fill the missing envelope, encoding,
terminator, packing, or queue behavior.

**Choices still required for the contract:** minimal documented field subset;
field order; numeric spelling; quoting/escaping; character encoding; any header
or terminator; exact application bytes; and explicit treatment of optional
fields. Synthetic addresses, timestamps, hostnames, identifiers, and values are
teaching inputs, not device output.

**Simulator-owned behavior later:** deterministic fixture generation, synthetic
values, clock/sequence policy, run bounds, send-attempt accounting, and one
exact application message per datagram once authorized. These do not become
FortiOS claims or a platform event schema.

**Experiment-owned behavior later:** actual FortiOS envelope/encoding; whether
one or multiple records occupy a datagram; omissions/order across builds; byte
limits and truncation; source queue/backpressure, retry and receiver-failure
behavior; source counters versus collector receipts; and source address,
timestamp, hostname, or identifier behavior.

### One teaching record per UDP datagram

This is a proposed teaching simplification, not FortiOS behavior. It gives the
first experiment a stable boundary: one exact simulator message maps to one UDP
datagram, allowing source attempts, datagram receipt, and byte preservation to be
explained without first teaching vendor batching. It excludes real variation such
as multiple records per datagram, source batching, truncation, queue drops, and
vendor packing. It preserves questions about send versus receipt, prior loss,
peer versus device identity, and observed TCP traffic versus UDP export.

A FortiOS capture showing different packing, a documented queue/size rule, or a
controlled loss/backpressure result would invalidate or expand this simplification.

### Smallest safe synthetic payload strategy

Author a short source-shaped Traffic/forward session-end body from documented
field names and key-value syntax, using only synthetic TEST-NET addresses,
fictional identifiers, fixed timestamps, and small numeric values. Do not copy
the Fortinet example wholesale. Retain only fields needed to show a session-end
observation and the transport/framing distinction; record the complete chosen
order in the future contract. Every omitted field, invented header, quoting rule,
terminator, or one-record-per-datagram rule is labeled a teaching simplification.

This is technically practical from V3/V11, but it is not yet a byte fixture:
default envelope, field order/optionality, encoding, and packing remain open.
Public access includes intermittent 403 responses, and no license, quotation,
redistribution, trademark, or sample-reuse permission has been established.
Original synthetic values reduce sensitivity and provenance risk without making a
legal conclusion. Publication review must check terms for any quoted material.

### PAN-OS TLS alternative and reconsideration

F2 is rejected for now because it introduces persistent stream behavior, TLS
protection, and application framing before the first direct-edge contract can be
made exact. This is a sequencing judgment, not a claim that TLS or PAN-OS is
inferior architecture. Reconsideration could be triggered by operating pressure
requiring stream security, evidence that UDP limitations defeat the teaching
claim, or complete PAN-OS framing/field evidence for a versioned fixture.

If reconsidered, the lesson would need persistent ordered TCP byte streams,
application delimiters or length framing, TLS records versus syslog messages,
certificate/trust behavior, and connection success versus evidence completeness.
PAN documentation does not establish its application boundary; S3/S7 establish
only generic transport/TLS behavior.

### OQ impact, ADR boundary, and simulator scope

E5 resolves only the profile-selection portion of OQ-0002 and OQ-0003 for this
teaching experiment. OQ-0002 remains open for platform-wide transport, framing,
loss, backpressure, ordering, and operating requirements. OQ-0003 remains open
for the eventual platform payload model, other profiles, drift policy, and
customer evidence. No universal UDP contract or normalized event schema follows.

No new ADR is warranted. E5 is an accepted next-experiment choice in this
mutable workspace under ADR-0001; it changes no platform responsibility or root
architecture. If contract shaping changes a root boundary, reconciliation must
precede implementation.

Selection makes simulator scope concrete but does not resolve it. Likely
experiment claims are exact simulator bytes matching the later contract, one
teaching record per UDP datagram, separate attempt/receipt observations, opaque
byte preservation, and visible separation of documented versus simplified
fields. The smallest experiment and its scope remain to be judged after
contract shaping; no simulator design or implementation is authorized.

### P3 — What the comparison can and cannot reproduce

**Authoritative documentation:** V7 is explicitly versioned and old; V8 supplies
later implementation detail, not a backdated guarantee. **Inference:** binary
flow export is a materially different boundary because data records depend on
templates. A complete synthetic template-and-data representation could be
authored from compatible specifications, but combining arbitrary V8 fields
with V7 would be an unsupported reconstruction. **Unknown:** exact target
firmware template, exporter identity behavior across restart, and current
customer relevance. A compatible licensed ASA appliance/software environment
would be needed; no such runtime was used. Failed retrieval of attempted 9.20
references is not evidence of lack of product support; 9.2 is retained as an
explicit historical comparison, pending judgment on its suitability.

**Risks:** missed templates, UDP loss, sequence scope misinterpretation, template
drift, sensitive addresses/user fields, and treating vendor binary fields as
normalized platform events. C9 is particularly important: protocol diversity
does not by itself justify a historically anchored teaching target. **Rights/access:**
public guides, no redistribution permission for software/manual tables assumed;
original synthetic layouts still need precise field provenance. No bytes generated.

### P4 — What the comparison can and cannot reproduce

**Authoritative documentation:** V9/V10 identify server-based export and read
modes. **Inference:** “raw” at this exporter and “raw” at our receipt boundary
refer to different stages. Keeping received LEEF bytes cannot recover gateway
information already filtered or transformed upstream. **Unknown:** exact
exporter build, LEEF revision/header/mapping, termination, gateway path and
buffer/reconnect guarantees. Public mapping/configuration evidence or a
licensed lab with gateway plus Management/Log Server is needed before claiming
an exact wire representation. No runtime environment was inspected.

**Risks:** update records mistaken for duplicate complete events; intermediary
identity mistaken for firewall identity; upstream loss and transformation;
format drift and sensitive audit/security values. C7 can be met with wholly
invented gateway/user/session values only after mapping is established.
C9 exposes the cost of teaching more than one hop. **Rights/access:** public
R81.20 pages; linked support articles and redistribution terms were not audited.
Do not imply access to support-only evidence. Read-mode default is unresolved,
not silently selected from contradictory rendered text.

### P5 — What the comparison can and cannot reproduce

**Authoritative documentation:** A1–A5 establish service eligibility, JSON
content and managed delivery. **Inference:** the visible boundary is storage
retrieval, so this candidate may not meet the intended direct first-edge lesson.
That is a scope question, not a rejection or recommendation. **Unknown:** fixed
engine/software revision, exact object/record bytes, duplicates and completeness.
A configured cloud account/firewall/destination with synthetic traffic would
be needed to validate service behavior; availability, permissions and cost were
not assessed or assumed. No cloud resources were accessed or created.

**Risks:** missing eligible coverage mistaken for network loss; delayed arrival
mistaken for absent events; service identity confused with a firewall peer;
record wrapping mistaken for a proposed platform envelope; account, topology
and traffic details exposed. **Reproducibility:** synthetic JSON examples are
possible, but arbitrary key ordering, whitespace, separators and compression
cannot be called exact AWS wire bytes. C3/C6/C11 remain open even with a dated
document reference. **Rights/access:** public living documentation; no sample
redistribution permission inferred and no engine version invented.

## Reader-confusion notes for later editorial review

These are editorial inferences/teaching boundaries, not article prose. Retain
all qualifications marked **must retain** if the associated mechanism appears.

| Point along the path | Concrete confusion to prevent | Qualification and detail that can wait |
| --- | --- | --- |
| Traffic → observation | A browser's connection and an exported log connection are different traffic. `proto=6` in a traffic record describes observed TCP traffic, even if the log travels over UDP (V3/S2/S3). | **Must retain:** two paths. IP routing internals can wait. |
| Observation → record | A session-end summary is not a copy of every observed packet; a drop action concerns user traffic, not lost log delivery (V4/V6). | **Must retain:** configured logging and source perspective. Exact policy engine/action catalogs can wait. |
| Record → exporter | A management/log server may be the immediate exporter (V9), and source filters affect what becomes visible (V2/A1). | **Must retain:** hop location and blind spots. Appliance administration screens need not be taught. |
| Address fields → provenance | Network Address Translation (NAT) rewrites an address or port. V6 distinguishes original session endpoints from translated ones; V3's field named `transport` means a translated source port, not export transport. | **Must retain:** distinguish export peer, original traffic address and source-claimed ID. NAT configuration and device reconciliation are outside scope. |
| Network → transport | An IP packet, a UDP datagram, and a TCP byte stream are different units; one fragmented datagram can involve multiple IP packets (S1–S3). | **Must retain:** message boundaries do not follow packet counts. Fragment bit layouts can wait. |
| Transport → framing | Two messages may arrive in one TCP read, or one across several. TLS records do not repair this (S3/S7). | **Must retain:** explicit framing; byte count versus character count. TCP algorithm internals can wait. |
| Envelope → body | RFC 5424 metadata is not arbitrary vendor key-value text; CEF/LEEF add their own headers, not a new transport (S4/S10). | **Must retain:** layered wrapper/body distinction. Full facility/severity and extension catalogs can wait. |
| Vendor format → platform | “Raw” downloaded log, exporter raw mode, and received raw bytes refer to different boundaries (V3/V10/architecture). | **Must retain:** source representation is not future normalized event format. Parser design is excluded. |
| Flow → export | AWS JSON `netflow`, Cisco NetFlow v9, and IPFIX are different formats/contracts (A2/S8/S9). | **Must retain:** named protocol/version, and template dependency if used. Registry catalogs can wait. |
| Receipt → completeness | TCP acknowledgment, TLS authentication, API success and object existence answer different questions; none alone proves no earlier loss (S3/S7/A5/A6). | **Must retain:** where evidence stops. Do not replace missing evidence with a delivery guarantee. |
| Source time → receipt time | PAN internal receive time and AWS activity-period time need not be collector receipt time (V6/A4). | **Must retain:** who assigned a timestamp. Clock synchronization design can wait. |
| Security of traffic → security of export | TLS inspection means the firewall examines encrypted user traffic; a TLS-related log describes that work (A1/A2). TLS on the export connection protects a different path. A certificate is a credential checked against configured trust to authenticate a peer (S7). | **Must retain:** inspection is not export encryption. Inspection internals and certificate provisioning can wait. |
| Documentation → observation | A public example or standard establishes a documented convention, not that our source emitted those bytes. | **Must retain:** exact revision, configuration, provenance, and unexecuted checks. No convenience fixture promoted to measured evidence. |

## Implications, uncertainties, and future reconciliation

- **OQ-0001 remains open:** customer/source inventory is absent in R2. Evidence
  needed: real conversations/inventory with provenance. E4 permits a
  documentation-led teaching choice without a prevalence claim, but missing
  customer evidence remains a limitation and reconsideration condition.
- **OQ-0002 remains open:** taxonomy establishes distinct transport/framing
  families. Exact vendor framing, failure/pressure behavior, source counters,
  metadata availability and operational needs remain unverified. Standards do
  not select a mode or establish an implementation.
- **OQ-0003 remains open:** candidates now have bounded source definitions and
  documentation gaps. Need selected profile authorization later, precise build
  and format/settings, and synthetic-byte provenance before a wire contract.
- **Risk findings (not new accepted architecture):** template loss can make
  received data uninterpretable; delimiter ambiguity can corrupt apparent
  records; source filtering or managed aggregation can hide earlier omissions;
  updated documentation can undermine an apparent version pin. These sharpen
  RISK-0001/0003/0005/0006. Intermediary and timestamp confusion sharpen
  RISK-0002; samples expose RISK-0004. If any becomes part of the accepted
  experiment, reconcile the affected root risk/question then.
- **Shaping boundary, not platform architecture:** E4 makes direct
  firewall-to-collector-controlled export the Article 2 teaching boundary.
  Check Point, AWS, and historical ASA remain contrasts rather than finalists;
  this does not redefine RAW-COLLECTION or establish a production integration.
- **Unknown requiring evidence judgment:** which version-pinned vendor statement
  or authorized device observation is sufficient to close F1/F2 application
  framing and exact-byte gaps, or whether the teaching promise should explicitly
  permit a narrower, simplified representation.
- **Hypotheses only:** a template-based source may require more teaching context
  than a self-describing text record; a mediated source may obscure the loss
  boundary the article wants to expose. Test these through conversational
  review of the path explanations, not numerical scoring or assumed preference.

### Historical implementation evidence — 2026-09-10

This read-only inventory covers pre-series siblings; it is neither an accepted
Article 2 profile nor architecture reconciliation.

**Clarification accepted in E4:** these repositories are historical reference,
not implementation repositories for *Architecting With Evidence*. They are not
Article 2 experimental evidence, do not establish that UDP, RFC 5424, Kafka or
any other mechanism is accepted, and must not influence source-profile selection
because they are convenient. They will not be reconciled into this root. Any
future reuse would need a separately justified decision against an accepted
contract; future series repositories may instead begin when their responsibilities
are earned.

**Inventory starting state (R3):** root remained on `article-2` at
`2e97282e60c5bff59d838b6d9b2e614f20495f46`. Its already-present, uncommitted
research diff modified only `articles/ROADMAP.md`, this file, and `review.md`;
there were no untracked root files. That research work was preserved while this
inventory added implementation evidence. The implementation repositories below
were treated as separate read-only evidence sources.

| Repository | Exact state and documented authority |
| --- | --- |
| `/Users/gabbott/git/gabbottron/firewall-simulator` | `origin` is `git@github.com:gabbottron/firewall-simulator.git`; `main` at `7fae91e19aa18a9f702a7529b4dad8da5365115d` (2026-08-28, `initial implementation`), tracking `origin/main`; no tags; clean/no untracked files before and after. Its AGENTS/README define a synthetic UDP source for simulator → ingestor → Kafka, not canonical events. |
| `/Users/gabbott/git/gabbottron/firewall-ingestor` | `origin` is `git@github.com:gabbottron/firewall-ingestor.git`; `main` at `b7758318f28d9b037494297d1f3e8c4b0888dae5` (2026-08-28, `initial implementaton`), tracking `origin/main`; no tags; clean/no untracked files before and after. Its AGENTS claims UDP/TCP syslog/IPFIX intent, while inspected code is a UDP-only listener and Kafka handoff. |

All tracked files were inspected. Neither repository has vendor documentation,
fixture provenance, architecture document, release tag, or execution artifact.
Their histories contain only initial and implementation commits. No branch was
switched, pulled, committed, reset, cleaned, or modified.

**Verified implementation at those revisions (I1):** the simulator generates a
byte slice then writes it through a connected UDP socket. A successful local
write increments `packets_sent`; JSON startup/progress/finish logs hold an
out-of-band generated or supplied `simulation_run_id`. It has sustained/burst
pacing, count/duration bounds, signal cancellation, optional local address and
a 65,507-byte send ceiling. It has no TLS, TCP, retry, acknowledgment, or
receiver-failure protocol. A send attempt is a successful local UDP write, not
collector receipt or network delivery.

Its only generator is explicitly **synthetic RFC 5424-shaped**, with no named
firewall vendor/product/version: `<134>1`, UTC RFC3339Nano time, configurable
hostname/application (`synthetic-fw`/`firewall-simulator` defaults), `- - -`,
and an unstructured space-separated key-value body. Sequence-derived TEST-NET
addresses, destination port 443, literal body `protocol=tcp`, `action=allow`,
and `bytes_sent` are the whole payload family. There is no rule/device serial,
vendor/profile switch, CEF/LEEF, CSV, JSON, NetFlow/IPFIX, TLS, prefix, newline,
terminator, escaping variation, seed, injected clock, or fixture artifact.

For the generator unit-test inputs `test-fw`, `simulator`, sequence 42, and
`2026-08-27T12:00:00Z`, inspected code constructs these exact application bytes:

```text
<134>1 2026-08-27T12:00:00Z test-fw simulator - - - sequence=42 source_ip=192.0.2.43 source_port=1066 destination_ip=198.51.100.215 destination_port=443 protocol=tcp action=allow bytes_sent=170
```

No trailing newline or length prefix is appended. This is code-derived fixture
content, not observed output, a captured IP packet, TCP segment, or vendor
sample. On successful send it becomes one UDP datagram. The sender test proves
its adapter preserves an independent non-UTF-8 byte slice; it does not make the
generator emit non-UTF-8 or prove this syslog form against a vendor.

**Collector boundary at I1:** startup constructs only `NewUDP`, `:5514` by
default. One `ReadFromUDPAddrPort` call determines each boundary; it has no
syslog, line, octet-count, TCP-stream, TLS, CEF, LEEF, NetFlow, or IPFIX parser.
It preserves the datagram slice as Kafka record value and records immediate
source IP/port, listener port, UTC receipt time, literal `udp`, and configured
`INGESTOR_SIMULATION_RUN_ID` as headers. It never reads payload bytes for the
run ID, validation, parsing, normalization, enrichment, or classification.

Buffers are `max+1`, so oversized datagrams are counted/dropped rather than
forwarded as truncated prefixes; empty, pool-exhausted, and nonblocking
sink-rejected datagrams also have distinct drop metrics. Defaults are 65,507
bytes, 4 MiB socket read buffer, 1,024 in-flight leases, and a 1,024-entry
Kafka queue. Read errors use bounded exponential backoff; cancellation/close
or 16 consecutive errors stop listening. The Kafka sink has bounded producer
buffers, all-in-sync-replica acknowledgments, a 30-second delivery timeout and
shutdown flush/abort behavior. These are historical delivery choices, not
Article 1 accepted guarantees. Metrics distinguish UDP receipt/submission/drop
and Kafka queue/delivery/failure/abort, but no durable attempt-versus-receipt
comparison or end-to-end run report exists.

**Validation actually executed (I2):** `go test ./...` passed in both clean
repositories under local `go1.26.3 darwin/arm64`, with already cached
`franz-go v1.21.6`; no external service was contacted. Simulator tests cover
the prefix/absent run ID, opaque non-UTF-8 UDP transmission, burst limits and
cancellation. Collector tests cover opaque payload/metadata, release,
empty/oversize/pool/sink drops, configuration bounds and Kafka headers. They
use loopback/test sinks, not Kafka; they do not run simulator → collector →
Kafka, inspect packets, prove broker retention, exercise service startup, or
validate the ingestor AGENTS' TCP/IPFIX language.

**Relation to taxonomy/candidates:** this is a synthetic hybrid closest to T1
and P1 only at the UDP plus RFC 5424-shaped-envelope level. It is not FortiOS:
no FortiOS version/configuration/default payload or primary-source provenance.
It is not P2 (no TCP/TLS/PAN CSV), P3 (no binary NetFlow templates), P4 (no
Check Point intermediary/LEEF), or P5 (no managed delivery/object boundary).
Existing code supplies exact synthetic bytes and UDP boundary evidence missing
from some documentation, but cannot establish vendor wire behavior.

**Article 1 alignment and tension:** opaque send/receive plus receipt metadata
align with SYNTHETIC-SOURCE and RAW-COLLECTION. The collector nevertheless
preselects UDP, Kafka, producer policy, HTTP metrics, queueing and shutdown;
the simulator preselects a traffic-like body and source metadata. Historical
implementation therefore cannot become platform architecture by default.
Format-agnostic sender/pacer, opaque listener, byte-preservation tests and
metrics may be reusable only after separately authorized profile/experiment
scope. Kafka handoff, HTTP telemetry, run correlation and shutdown policy are
better deferred. Missing vendor sources/profile/version, the body `protocol=tcp`
over UDP, no framing variation, uncontrolled event clock, absent comparison
artifact, and documentation-code mismatch on TCP/IPFIX require further evidence.

The [simulator scope question](#simulator-scope-question) remains open. The
historical inventory authorized no reuse and did not motivate E5; the later
profile decision is separately recorded above.

## Teaching contract definition — 2026-09-10

**E6 — Accepted Article 2 teaching contract, 2026-09-10:** the exact versioned
teaching contract below is accepted for the Article 2 next experiment. This
contract owns the smallest synthetic FortiOS-shaped representation needed to
teach the raw collection boundary, not vendor-verified bytes or a universal
platform event schema.

### Contract identity and version

**Name:** `FortiOS-Shaped Traffic Observation v1`

**Shorthand:** `FSTO/1`

**Purpose:** Define exact, reproducible, synthetic firewall traffic-observation
bytes for teaching the first raw-collection boundary. This contract specifies
application-layer message content only; it does not specify IP packets, UDP
datagrams, network addresses, or socket behavior.

**Scope:** A minimal source-shaped text record representing one completed TCP
session (proto=6, dstport=443) observed and allowed by a firewall, exported for
external collection. The contract is based on documented FortiOS 7.4.8
Traffic/forward session-end field names and key-value syntax (V3, V11),
including source observation timestamp (eventtime). It is not captured, observed,
or vendor-verified FortiOS output. The teaching record crosses the first edge in
a UDP datagram, distinct from the TCP traffic it describes.

**Audience constraint:** Must be explainable to generally technical readers who
do not know firewalls, syslog protocols, or security operations, without
requiring firewall-specific complexity beyond the architectural lesson.

### Source profile basis

- **Vendor/product/version:** Fortinet FortiGate firewall family, FortiOS 7.4.8
  documentation release (V1–V3, V11, V13).
- **Export path:** Fourth remote-syslog target, `mode udp`, `format default`,
  forward-traffic filter enabled (V1, V2).
- **Payload family:** Traffic category, forward-traffic type, session-end log
  `LOG_ID_TRAFFIC_END_FORWARD` (V11).
- **Primary evidence:** V1 (CLI syslog configuration), V2 (administration guide
  filters), V3 (log message field reference), V11 (Traffic/forward field table),
  V13 (target-specific filter corroboration from 7.4.0).
- **Documented elements:** Configuration mode, filter selection, field names,
  field types, key-value syntax example.
- **Unresolved vendor behavior:** Complete default-format envelope, record
  packing (one vs. multiple per datagram), field order/optionality, exact
  quoting/escaping rules, character encoding, build-specific variations.

### Transport and delivery

- **Transport:** User Datagram Protocol (UDP), documented by V1 and described by
  S2.
- **Datagram boundary:** One application message per UDP datagram payload. This
  is a **teaching simplification** (see below).
- **No syslog envelope:** Fortinet's complete `format default` envelope bytes
  are unresolved by V1–V3/V11. The contract uses **bare key-value body only**
  (teaching simplification; see below).
- **No length prefix, no terminator:** The application message is the complete
  datagram payload with no added framing bytes (teaching simplification).
- **Simulator sends one UDP write per teaching record:** The boundary is the
  operating system UDP send operation (teaching simplification).

### Character encoding and representation

- **Encoding:** UTF-8, derived from V3's text example and field content. Not
  explicitly documented by V1–V3/V11; labeled as **responsibly reconstructed**.
- **No byte-order marker:** None added; teaching simplification.
- **Printable ASCII subset:** All field names, delimiters, and canonical values
  use only printable ASCII (U+0020 through U+007E) for teaching clarity;
  teaching simplification that excludes international characters, control codes,
  and non-ASCII field content.

### Field structure and delimiters

- **Field delimiter:** Single space character (U+0020), derived from V3's
  key-value example. Exact delimiter not explicitly documented; responsibly
  reconstructed from example.
- **Key-value format:** `key=value` with no surrounding whitespace, derived from
  V3 example; responsibly reconstructed.
- **Field order:** Fixed order specified below; teaching simplification. The
  eleven field names and applicable values are source-shaped from V3/V11; their
  exact sequence is FSTO/1's deterministic teaching-contract order, not a claim
  about FortiOS field ordering.
- **No quoting:** Values contain no spaces, equals signs, or special characters
  requiring quotes; teaching simplification that defers escaping complexity.
- **All fields present:** Every contract field appears in every canonical record;
  teaching simplification (V3/V11 describe optional fields).

### Selected fields and rationale

The contract includes exactly eleven fields, chosen as the minimum necessary to
represent a firewall traffic observation of a completed TCP session and support
provenance, attempt, and receipt reasoning.

| Field name | Plain-language purpose | Vendor documentation | Teaching rationale | Exclusion rationale for omitted fields |
| --- | --- | --- | --- | --- |
| `devid` | Identifies which firewall reported the observation | V11 documents `devid` as device ID | Teaches that source identity is a claim made by the source, distinct from network peer address | Device serial, hostname, VDOM omitted: identity complexity deferred |
| `eventtime` | Source-side observation timestamp | V11 documents `eventtime` as event time (epoch nanoseconds for Traffic logs) | Teaches source observation time is distinct from simulator attempt time and receiver receipt time | Formatted date/time fields omitted: numeric epoch nanoseconds sufficient; no precision loss |
| `logid` | Identifies the FortiOS log family | V11 documents `logid` as Log ID; value `0000000013` means Traffic/forward session-end | Teaches that payload family is explicitly typed, not inferred from presence | Other log types, subtypes, event IDs omitted: only Traffic/forward needed |
| `type` | High-level category | V11 documents `type` as log type; value `traffic` | Reinforces payload family | Threat, event, UTM types omitted: not needed for traffic example |
| `subtype` | Subcategory within type | V11 documents `subtype`; value `forward` means forwarded traffic | Identifies the documented traffic subtype | Local, multicast, sniffer subtypes omitted: forward is sufficient |
| `action` | Session-end status | V11 documents `action`; value `close` for session-end logs means normal session termination | Under applicable Traffic/forward session-end semantics, action=close describes an allowed session that ended normally | Deny, drop, reject omitted: those describe blocked traffic; close describes normal end of allowed session |
| `proto` | IP protocol number of observed session | V11 documents `proto` as protocol; value `6` means TCP | Establishes that the firewall observed TCP traffic, distinct from the UDP export transport | Other protocols (UDP=17, ICMP=1, etc.) omitted: TCP is sufficient |
| `srcip` | Source IP address of observed session | V11 documents `srcip` as source IP | Teaches observed traffic address vs. export peer address | NAT addresses, IPv6 omitted: one address pair sufficient |
| `dstip` | Destination IP address of observed session | V11 documents `dstip` as destination IP | Teaches observed traffic address vs. export peer address | NAT addresses, IPv6 omitted: one address pair sufficient |
| `dstport` | Destination TCP port of observed session | V11 documents `dstport` as destination port | Completes the TCP session identification (proto=6, dstport=443 suggests HTTPS) | Source port omitted: destination port sufficient to show session; ephemeral source port adds no teaching value |
| `sentbyte` | Bytes sent from source to destination | V11 documents `sentbyte` as bytes sent | Teaches that traffic summary counts are distinct from export message size | Received bytes, packets, duration omitted: one directional count sufficient |

**Omitted complexity deferred for teaching clarity:** No policy/rule ID (firewall
configuration complexity), no user/application (identity complexity), no
interface (topology complexity), no session ID (correlation complexity), no
received bytes/packets (one directional count sufficient), no duration (time
complexity), no threat/signature fields (alert complexity), no source port
(destination port sufficient for session), no formatted date/time (numeric epoch
sufficient), no NAT fields (address translation complexity).

### Canonical values and synthetic-value rules

Every contract value is **original synthetic**, never copied from customer data
or vendor samples. Values are chosen for teaching clarity and documentation safety.

| Field | Canonical value | Value rationale and classification |
| --- | --- | --- |
| `devid` | `FW-TEACHING-01` | **Teaching simplification.** Fictional device identifier with explicit TEACHING marker; no FortiOS device serial format claimed. |
| `eventtime` | `1672531200000000000` | **Vendor-documented with qualification.** V11 documents `eventtime` for Traffic logs as epoch time in nanoseconds (19-digit values in 7.4.8 sample logs). Fixed value: 1672531200000000000 (2023-01-01T00:00:00Z in nanoseconds). Generic field page shows 10-digit second example; specific Traffic log ID 13 definition and 7.4.8 samples establish nanoseconds. This is source observation time, distinct from simulator attempt time and receiver receipt time. |
| `logid` | `0000000013` | **Vendor-documented.** V11 establishes log ID 13 as Traffic/forward session-end (`LOG_ID_TRAFFIC_END_FORWARD`). |
| `type` | `traffic` | **Vendor-documented.** V11 establishes `traffic` as the log type value for Traffic category. |
| `subtype` | `forward` | **Vendor-documented.** V11 establishes `forward` as the subtype for forwarded traffic. |
| `action` | `close` | **Vendor-documented with qualification.** V11 documents `action=close` for Traffic session-end logs, indicating normal session termination. Under applicable Traffic/forward session-end semantics, action=close describes an allowed session that ended normally. subtype=forward identifies Fortinet's forwarded-traffic subtype; action=close provides session-end status. |
| `proto` | `6` | **Vendor-documented and standards-derived.** V11 documents `proto` field; value `6` is TCP per IANA protocol numbers. Establishes observed traffic was TCP, distinct from UDP export transport. |
| `srcip` | `192.0.2.10` | **Standards-derived synthetic.** RFC 5737 TEST-NET-1 address (S13); safe for documentation examples. |
| `dstip` | `198.51.100.20` | **Standards-derived synthetic.** RFC 5737 TEST-NET-2 address (S13); safe for documentation examples. |
| `dstport` | `443` | **Teaching simplification.** HTTPS port; recognizable, common service port. Completes TCP session identification with proto=6. |
| `sentbyte` | `4096` | **Teaching simplification.** Small power-of-two value for readability; no claim about typical session size. |

**S13 — Standards-derived synthetic addresses:** [RFC 5737, IPv4 Address Blocks
Reserved for Documentation](https://www.rfc-editor.org/rfc/rfc5737.html), January
2010, defines 192.0.2.0/24 (TEST-NET-1), 198.51.100.0/24 (TEST-NET-2), and
203.0.113.0/24 (TEST-NET-3) as safe for documentation examples.

**Deterministic generation:** The canonical record is fixed. A simulator may
generate variations by systematically varying synthetic addresses or counts
while preserving the contract structure. Any variation must be declared before
execution.

**Source observation time vs. attempt time vs. receipt time:**

- **Source observation time (`eventtime` in payload):** The firewall's timestamp
  for when the observed TCP session ended. V11 documents `eventtime` for Traffic
  logs as epoch time in nanoseconds; 7.4.8 sample logs contain 19-digit values.
  Fixed canonical value 1672531200000000000 (2023-01-01T00:00:00Z in nanoseconds).
  Generic field page shows 10-digit second example; specific Traffic log ID 13
  definition and samples establish nanoseconds. This is part of the source-shaped
  payload because it describes when the firewall observed the event, not when we
  generated or sent the teaching record.
- **Simulator attempt time (simulator metadata, outside payload):** When the
  simulator wrote the teaching record to the UDP socket. This is simulator-owned
  metadata, not a FortiOS field. Recorded separately from the payload for
  attempt/receipt comparison.
- **Receiver receipt time (receiver metadata, outside payload):** When the
  experimental receiver observed datagram arrival. This is a collection-boundary
  observation, not part of the source payload. Recorded for attempt/receipt
  comparison.

The teaching distinction: a firewall's observation timestamp (`eventtime`) tells
when it saw the TCP session end. The simulator's send attempt time tells when
the teaching record was submitted to the local UDP socket. The receiver's
receipt time tells when a datagram arrived. These three times answer different
questions and must not be conflated.

**No correlation metadata in payload:** Simulator run ID, attempt sequence, or
other experimental correlation metadata must remain **outside the source-shaped
payload** unless authoritative vendor evidence supports an appropriate FortiOS
field. Receipt observations (peer address, receipt time, datagram size) belong
to the collection boundary, not the payload.

### Teaching simplifications and excluded real-world variation

The contract deliberately simplifies FortiOS behavior to teach the raw boundary
without requiring complete vendor wire fidelity. Every simplification below is
explicit.

| Teaching simplification | What it excludes | Why it's acceptable | Reconsideration trigger |
| --- | --- | --- | --- |
| One teaching record per UDP datagram | Multiple records per datagram; source batching; queue packing | Gives stable message boundary for first experiment; attempt vs. receipt distinction survives | FortiOS capture showing different packing; documented batching rule |
| Bare key-value body, no syslog envelope | Priority, hostname, facility, structured-data, version header | Fortinet's default envelope is unresolved (V1–V3/V11); key-value body is documented | Complete default-format envelope documentation or device observation |
| No length prefix or terminator | Octet-count prefix; newline/NUL/other delimiter | UDP datagram boundary is the message boundary in this teaching contract | Evidence that default format uses specific terminator |
| Fixed field order | Optional fields; build-specific order; dynamic presence | Deterministic fixture generation; complexity deferred | FortiOS specification of field order or optionality rules |
| No quoting or escaping | Quoted values; escaped spaces/equals; international characters | Teaching values contain no special characters | Field values requiring quotes; escaping rules documentation |
| UTF-8 encoding assumed | Other encodings; encoding declaration | Text example (V3) is consistent with UTF-8; common default | Explicit FortiOS encoding documentation |
| All fields present | Optional field omission; sparse records | Deterministic fixture; presence complexity deferred | Optional field behavior documentation |
| Printable ASCII values only | Non-ASCII characters; control codes | Teaching clarity; avoids encoding edge cases | Requirement to demonstrate international address/hostname handling |
| Fixed numeric eventtime | Formatted date/time fields; timezone representation; sub-nanosecond precision | Numeric epoch nanoseconds (19 digits) is sufficient to teach source observation time vs. attempt/receipt time distinction | Formatted timestamp requirement for experiment claims |
| Eleven fields only | 50+ documented FortiOS fields (V3, V11) | Minimum sufficient to teach TCP session observation, traffic vs. export transport distinction, and source/attempt/receipt time separation | Additional fields needed for specific architectural lesson |

**What the contract preserves:** Documented FortiOS field names; key-value
syntax; Traffic/forward session-end family; vendor-specific log ID and action
values; TCP protocol identification (proto=6) distinct from UDP export transport;
source-observation timestamp distinct from attempt/receipt times; traffic
addresses distinct from export peer; observation summary distinct from raw
packets; source identity as a claim.

**What controlled experiments must still establish:** Actual FortiOS envelope;
actual packing/batching; field order/optionality; quoting/escaping; character
encoding; truncation/size limits; source queueing; backpressure behavior;
address/identity/timestamp handling; loss/reordering under network failures;
relationship between source eventtime, attempt time, and receipt time in real
deployments.

### Conformance and change rules

**Conformance checks required:**

1. **Golden-byte equality:** Generated bytes exactly match the canonical byte
   sequence specified below, including all whitespace, encoding, and field order.
2. **Deterministic generation:** The canonical record is produced from declared
   inputs with no randomness.
3. **One application message per UDP write:** Each teaching record becomes
   exactly one UDP send operation, creating exactly one datagram payload.
4. **UTF-8 encoding:** All bytes decode as valid UTF-8.
5. **Field structure:** Eleven fields in documented order, space-delimited,
   key=value format, no quotes.
6. **Declared destination:** Simulator configuration explicitly names the target
   address and port.
7. **No undeclared bytes:** No byte-order marker, length prefix, syslog header,
   or trailing newline/NUL unless explicitly added to a future contract version.
8. **Bounded run completion:** Simulator finishes with an explainable result,
   not an unexplained hang or crash.
9. **Attempt accounting separate from payload:** Source send attempts are
   recorded outside the UDP payload, not as an embedded sequence field. Attempt
   time is simulator metadata, distinct from eventtime in the payload.
10. **Contract version visible in execution evidence:** Experiment records must
    identify `FSTO/1` without embedding the version string in the source-shaped
    payload.

**Compatible fixture variation:** Systematic changes to synthetic values
(different TEST-NET addresses, different `sentbyte` counts, different fictional
`devid`) while preserving all field names, order, structure, and encoding.
Compatible variations must be declared before execution.

**New fixture:** Adding fields, changing field order, adding envelope/terminator,
or changing delimiters. Requires contract version increment.

**New contract version:** Any change to field structure, encoding, delimiter
rules, envelope treatment, or conformance checks. Increment version: `FSTO/2`.

**Vendor-profile revision:** Evidence that FortiOS 7.4.8 behavior differs from
this contract's reconstruction, or a decision to base the contract on a different
FortiOS version. Requires new contract with updated version and provenance.

**Reconsideration triggers:** Complete default-format envelope documentation;
device-observed packing/batching behavior; explicit field-order/optionality
specification; quoting/escaping rules; encoding declaration; evidence that
teaching simplifications prevent a necessary architectural claim.

### Canonical record — exact bytes

**Human-readable representation:**

```
devid=FW-TEACHING-01 eventtime=1672531200000000000 logid=0000000013 type=traffic subtype=forward action=close proto=6 srcip=192.0.2.10 dstip=198.51.100.20 dstport=443 sentbyte=4096
```

**Exact application-byte sequence:**

```
devid=FW-TEACHING-01 eventtime=1672531200000000000 logid=0000000013 type=traffic subtype=forward action=close proto=6 srcip=192.0.2.10 dstip=198.51.100.20 dstport=443 sentbyte=4096
```

**Hexadecimal representation (grouped by 16 bytes, then by 8):**

```
00000000: 64 65 76 69 64 3d 46 57  2d 54 45 41 43 48 49 4e  |devid=FW-TEACHIN|
00000010: 47 2d 30 31 20 65 76 65  6e 74 74 69 6d 65 3d 31  |G-01 eventtime=1|
00000020: 36 37 32 35 33 31 32 30  30 30 30 30 30 30 30 30  |6725312000000000|
00000030: 30 30 20 6c 6f 67 69 64  3d 30 30 30 30 30 30 30  |00 logid=0000000|
00000040: 30 31 33 20 74 79 70 65  3d 74 72 61 66 66 69 63  |013 type=traffic|
00000050: 20 73 75 62 74 79 70 65  3d 66 6f 72 77 61 72 64  | subtype=forward|
00000060: 20 61 63 74 69 6f 6e 3d  63 6c 6f 73 65 20 70 72  | action=close pr|
00000070: 6f 74 6f 3d 36 20 73 72  63 69 70 3d 31 39 32 2e  |oto=6 srcip=192.|
00000080: 30 2e 32 2e 31 30 20 64  73 74 69 70 3d 31 39 38  |0.2.10 dstip=198|
00000090: 2e 35 31 2e 31 30 30 2e  32 30 20 64 73 74 70 6f  |.51.100.20 dstpo|
000000a0: 72 74 3d 34 34 33 20 73  65 6e 74 62 79 74 65 3d  |rt=443 sentbyte=|
000000b0: 34 30 39 36                                       |4096            |
```

**Total byte length:** 180 bytes

**Byte-level verification:**

- First byte: `0x64` ('d' in UTF-8)
- Last byte: `0x36` ('6' in UTF-8)
- Byte count: 180 (verified by hex representation and independent count)
- No newline (0x0A), no NUL (0x00), no carriage return (0x0D)
- No byte-order marker (no 0xEF 0xBB 0xBF UTF-8 BOM)
- All bytes are printable ASCII (0x20–0x7E) including spaces (0x20) used as delimiters
- Valid UTF-8: all bytes in 0x20–0x7E are single-byte UTF-8 characters

**Component classification:**

| Byte range | Content | Classification |
| --- | --- | --- |
| 0x00–0x14 | `devid=FW-TEACHING-01 ` | Field name vendor-documented (V11); value teaching simplification; delimiter responsibly reconstructed |
| 0x15–0x37 | `eventtime=1672531200000000000 ` | Field name vendor-documented (V11); value vendor-documented with qualification (epoch nanoseconds per Traffic log ID 13 definition and 7.4.8 samples; generic field page conflict noted); delimiter responsibly reconstructed |
| 0x38–0x48 | `logid=0000000013 ` | Field name and value vendor-documented (V11); delimiter responsibly reconstructed |
| 0x49–0x55 | `type=traffic ` | Field name and value vendor-documented (V11); delimiter responsibly reconstructed |
| 0x56–0x65 | `subtype=forward ` | Field name and value vendor-documented (V11); delimiter responsibly reconstructed |
| 0x66–0x73 | `action=close ` | Field name and value vendor-documented (V11); action=close documents normal session-end for allowed traffic; delimiter responsibly reconstructed |
| 0x74–0x7B | `proto=6 ` | Field name vendor-documented (V11); value standards-derived (IANA protocol 6=TCP); delimiter responsibly reconstructed |
| 0x7C–0x8C | `srcip=192.0.2.10 ` | Field name vendor-documented (V11); value standards-derived synthetic (S13 TEST-NET-1); delimiter responsibly reconstructed |
| 0x8D–0xA0 | `dstip=198.51.100.20 ` | Field name vendor-documented (V11); value standards-derived synthetic (S13 TEST-NET-2); delimiter responsibly reconstructed |
| 0xA1–0xAC | `dstport=443 ` | Field name vendor-documented (V11); value teaching simplification (HTTPS port); delimiter responsibly reconstructed |
| 0xAD–0xB3 | `sentbyte=4096` | Field name vendor-documented (V11); value teaching simplification; no trailing delimiter (end of record) |

**Framing and boundary:**

- **Application message:** The 180-byte sequence above.
- **UDP datagram payload:** The complete 180-byte application message, with no
  added prefix, header, or terminator.
- **Datagram boundary:** Operating system UDP send writes the 180-byte message
  as the complete datagram payload. This is the simulator's responsibility.
- **No syslog envelope:** The application message contains only the FortiOS
  key-value body. Fortinet's `format default` envelope is unresolved (V1–V3/V11);
  this contract teaches the traffic observation without requiring unknown envelope
  bytes. This is a teaching simplification.
- **No length prefix:** UDP datagram length is in the UDP header; no application
  length prefix is added. Teaching simplification.
- **No terminator:** The message ends after the last field value. No newline,
  NUL, or other terminator byte. Teaching simplification consistent with datagram
  boundary.

**Not vendor-verified:** These bytes are the canonical representation of the
`FSTO/1` teaching contract. They are not captured from a FortiGate appliance,
not observed in network traffic, and not claimed to be identical to FortiOS
7.4.8 output. They are based on documented field names and syntax (V3, V11),
combined with explicit teaching simplifications for envelope, packing, encoding,
value representation, and TCP session identification (proto=6, dstport=443).

### Simulator scope resolution — E7

**Editorial decision, 2026-09-10:** Article 2 will include design, implementation,
and execution of the smallest reproducible simulator experiment necessary to
demonstrate the `FSTO/1` teaching contract and the attempt-versus-receipt
distinction at the first raw edge.

**Experiment must establish:**

1. **Deterministic generation of canonical bytes:** Simulator generates exactly
   the 180-byte `FSTO/1` canonical record with 19-digit nanosecond eventtime.
2. **Golden-byte verification:** Generated bytes match the hex representation
   above, byte-for-byte.
3. **One teaching record per UDP send:** Each `FSTO/1` record becomes one UDP
   socket write operation.
4. **Source-attempt observation:** Simulator records each send attempt with
   attempt timestamp (simulator metadata, distinct from eventtime nanoseconds in
   payload), declared destination, and attempt sequence, separate from the payload.
5. **Receiver-receipt observation:** Experimental receiver records datagram
   arrival with peer address, peer port, receipt timestamp (receiver metadata,
   distinct from eventtime nanoseconds in payload), and payload size.
6. **Byte preservation:** Receiver preserves the complete received application
   bytes without parsing, normalizing, or interpreting them.
7. **Bounded, explainable result:** Simulator completes a declared run (fixed
   count or duration) and reports attempts, successful sends, and any failures.

**Required experiment scenarios (exactly two):**

1. **clean_success:** One send attempt that results in one observed receiver
   receipt with preserved bytes matching the canonical record exactly. Demonstrates
   successful generation, transmission, receipt, and byte preservation in the
   controlled local experiment.

2. **receiver_unavailable:** One successful local UDP write operation (send
   system call succeeds) with no corresponding receiver receipt observed during
   the declared run. Demonstrates that a successful local send attempt does not
   guarantee receiver receipt, establishing the attempt-versus-receipt gap in a
   controlled local experiment. This does NOT identify where a production network
   lost data or establish real FortiOS queue/retry/backpressure behavior.

Do not expand into multiple loss scenarios, retry behavior, loss-rate testing,
or multi-failure reliability coverage; those belong in later installments.

**Experiment does NOT claim:**

- That these bytes were emitted by FortiOS
- That FortiOS packing/batching matches the one-per-datagram rule
- That UDP delivery succeeded across an actual network path
- That the experiment proves real firewall identity, queueing, or pressure behavior
- That preserved bytes are interpretable without a parser (bytes are opaque at
  raw boundary)
- That one controlled scenario covers all failure modes

**Experimental receiver boundary:**

- **Responsibility:** Observe receipt and preserve bytes for the first-edge
  teaching experiment only.
- **Not a production collector:** No parsing, no normalized schema, no
  classification, no enrichment, no alerting.
- **Not durable handoff:** No broker, no persistence guarantees, no replay, no
  fan-out. The receiver is a minimal test harness, not a platform service.
- **Placement:** Co-located with simulator in the same experimental repository
  as a test harness, or in a separate minimal receiver harness if a clean
  boundary justifies it. Decision deferred to implementation authorization.

**Evidence artifacts required:**

1. Canonical `FSTO/1` record as a versioned contract file.
2. Generated bytes from simulator execution, hex-dumped or byte-compared.
3. Attempt log from simulator (timestamp, destination, sequence).
4. Receipt log from receiver (peer address/port, timestamp, size, preserved bytes).
5. Run completion report (attempts, sends, receipts, discrepancies).
6. Validation proof that generated bytes match the canonical record exactly.

### Implementation authorization packet — E8

**New repository responsibility:**

- **Repository name (proposed):** `firewall-traffic-simulator` (or similar;
  final name deferred to implementation task).
- **Authority:** Owns the `FSTO/1` contract implementation, deterministic
  generation, bounded-run execution, and attempt accounting. It is authoritative
  for its own implementation and tests.
- **Explicit non-authority:** Does not own the platform event model, raw
  collection service, durable handoff, parser, normalized schema, or broker
  selection. Does not define production deployment, tenancy, or operating model.
  Does not represent FortiOS behavior beyond the explicit teaching contract.

**Experimental receiver placement:**

- **Option A (preferred for first experiment):** Include minimal receiver test
  harness in the same simulator repository. This keeps the controlled experiment
  self-contained and avoids premature collector service abstraction.
- **Option B (if justified):** Separate minimal receiver harness repository if
  the boundary is clean enough to test independently. Must still be labeled as
  experimental harness, not production collector.
- **Decision:** Deferred to implementation task; default to Option A unless a
  compelling boundary justifies separation.

**Minimum implementation acceptance criteria:**

1. **Contract file:** `FSTO/1` canonical record and spec versioned in repository.
2. **Deterministic generation:** Code generates exactly the 180-byte canonical
   record from contract definition, with all eleven fields in documented order,
   including 19-digit nanosecond eventtime and action=close.
3. **UDP send:** Code writes generated bytes through one UDP socket send per
   record.
4. **Attempt accounting:** Code logs each send attempt with attempt timestamp
   (simulator metadata, distinct from eventtime nanoseconds in payload),
   destination, and sequence, separate from payload.
5. **Receiver harness:** Code receives UDP datagrams, logs peer address/port,
   receipt timestamp (receiver metadata, distinct from eventtime nanoseconds in
   payload), payload size, and preserves complete application bytes.
6. **Golden-byte test:** Automated test verifies generated bytes match canonical
   hex exactly (180 bytes, all eleven fields, eventtime=1672531200000000000,
   action=close).
7. **Two-scenario validation:** Execution demonstrates both required scenarios:
   clean_success (one send → one receipt with exact byte match) and
   receiver_unavailable (one local send succeeds with no receiver receipt).
8. **Run bounds:** Simulator accepts declared count or duration and completes
   with explainable result.
9. **No parsing:** Receiver treats payload as opaque bytes; no key-value
   extraction, no field validation, no interpretation of eventtime/action/proto/port.
10. **Execution artifacts:** Logs/reports from both scenarios showing attempt
    time, receipt time (when applicable), eventtime nanoseconds from payload (via
    hex dump, not parsing), and byte preservation, suitable for Article 2 evidence.

**Required validation commands:**

- Unit tests covering 180-byte canonical record generation (eleven fields,
  19-digit nanosecond eventtime, action=close), UDP send preparation, attempt
  logging with attempt timestamp, and receiver byte preservation with receipt
  timestamp.
- Integration test or manual execution demonstrating both scenarios: clean_success
  (simulator → receiver → preserved bytes match) and receiver_unavailable
  (send succeeds, no receipt).
- Golden-byte comparison test proving generated bytes match `FSTO/1` canonical
  exactly (180 bytes, exact hex match, eventtime=1672531200000000000,
  action=close).
- Three-way time distinction test: verify eventtime nanoseconds in payload
  (1672531200000000000), attempt timestamp (simulator metadata), and receipt
  timestamp (receiver metadata) are recorded separately.
- No vendor appliance, cloud service, broker, or parser integration required.

**Historical repository consultation:**

- **May consult for patterns only:** firewall-simulator and firewall-ingestor
  code may be read to understand general patterns for UDP send/receive, opaque
  byte handling, bounded runs, and test harness structure (I1, I2).
- **Must NOT adopt without justification:** Do not copy Kafka handoff, HTTP
  metrics, specific run-ID embedding, RFC 5424 envelope, or any mechanism not
  explicitly required by `FSTO/1` or the experiment claims. Historical code is
  not series implementation authority.
- **Must implement independently:** The `FSTO/1` contract, golden-byte generation,
  and experiment-specific evidence artifacts must be original implementation
  against the accepted contract, not historical code reuse.

**Prohibited until separately justified:**

- Language, library, or framework selection (defer to implementation unless
  already constrained)
- Metrics systems (HTTP, Prometheus, StatsD, etc.)
- Message broker (Kafka, NATS, RabbitMQ, etc.)
- Storage system (databases, object storage, filesystems beyond minimal logging)
- Deployment infrastructure (containers, orchestration, cloud resources)
- Repository structure (monorepo, directory layout, build system)
- Logging framework (beyond standard output or simple file writes)
- Configuration formats (YAML, TOML, JSON, environment variables)
- Durable handoff implementation
- Normalized event schema
- Parser or field extraction
- Detection, enrichment, or classification

These remain implementation choices for the next task or are excluded from this
experiment entirely.

**Implementation task input:**

- `FSTO/1` contract definition and canonical bytes from this workspace.
- Experiment claims and evidence requirements from simulator scope resolution.
- Minimum acceptance criteria and validation commands from this authorization.
- Prohibited scope from exclusions above.
- Authority: this root for platform boundaries; implementation repository for
  its own code and tests.

### Platform reconciliation

**No platform architecture changes at this checkpoint.** E6 (teaching contract),
E7 (simulator scope), and E8 (implementation authorization) are Article 2
workspace decisions under the existing ADR-0001 provisional boundary. They
establish the next experiment, not a production platform commitment.

**Existing architecture, questions, and risks remain authoritative:**

- ARCHITECTURE.md: SYNTHETIC-SOURCE and RAW-COLLECTION boundaries remain as
  stated; no transport, vendor, or parser selected for the platform.
- OQ-0001: Customer inventory remains absent; teaching selection is explicitly
  limited.
- OQ-0002: Platform-wide transport and framing remain open; `FSTO/1` UDP is an
  experiment choice.
- OQ-0003: Platform payload model remains open; `FSTO/1` is a teaching fixture,
  not the universal schema.
- OQ-0004–0008: Identity, handoff, operating boundaries, parsing, and scale
  remain unknown.
- RISK-0001–0006: Attempt/receipt gaps, address/identity confusion, format
  drift, raw exposure, lost recoverability, and documentation drift remain
  recognized risks.

**Platform changes proposed after implementation validation:**

After the `FSTO/1` experiment is implemented and executed, reconcile findings
into platform documents:

1. **ARCHITECTURE.md update (proposed):** Add `FSTO/1` as the first accepted
   teaching fixture under SYNTHETIC-SOURCE, with explicit non-universality and
   teaching-simplification disclosure. State that it does not select platform
   transport, framing, or event model.
2. **OQ-0002 partial resolution (proposed):** Note that UDP datagram boundaries
   and opaque byte preservation are validated for the teaching experiment;
   platform-wide transport and framing remain open pending real customer needs.
3. **OQ-0003 partial resolution (proposed):** Note that FortiOS-shaped traffic
   observations are validated as a teaching source; platform payload model
   remains open for other vendors, formats, and operational needs.
4. **RISK-0001 validation (proposed):** Note that attempt-vs.-receipt gaps are
   experimentally demonstrated in controlled conditions; production treatment
   remains unimplemented.

**Rationale for deferring reconciliation:** AGENTS.md requires reconciling
findings into affected platform documents before drafting claims. The findings
are not complete until implementation executes and produces validated evidence
artifacts. Reconciliation should occur during the Reconciling lifecycle stage,
not before implementation. Recording the proposed changes now establishes intent
without prematurely accepting unvalidated claims.

**No ADR required:** E6–E8 are next-experiment decisions under the existing
ADR-0001 authority. If the contract or experiment changes a platform boundary
after validation, reconciliation will determine whether a new ADR is warranted.

### Lifecycle advancement

**Researching complete:** Sources traced (S1–S13, V1–V13, A1–A6), applicability
assessed (five candidates, F1/F2 finalists, F1 selected), assumptions separated
from documented behavior (vendor-documented vs. teaching simplifications vs.
unknowns), and exact teaching contract defined with complete provenance.

**Cannot advance to Experimenting:** No implementation or execution has occurred.
Generated bytes, attempt/receipt observations, and validation results do not
exist.

**Lifecycle state remains Researching** because AGENTS.md requires experimental
execution before advancing to Experimenting, and there is no intermediate state
for "contract defined, implementation not started." The roadmap will record the
exact next action as implementation authorization and execution.

### Review and validation record

Research handoff: [review.md](review.md). Base revision is R1; changed files are
this record, the roadmap, and the research handoff. `draft.md` remains unwritten
and unchanged. No Article 2 checkpoint revision is proposed.

Root validation is documentation-only: inspect the complete working diff; run
`git diff --check`; verify evidence IDs/primary URLs and local Markdown links;
check terminology and qualifications across taxonomy, candidates and trace;
verify untouched architecture/transcript/draft/tag, no unapproved platform
architecture, no invented customer fact, and the open simulator question. Separately, the
historical implementation unit tests recorded in I2 passed; no appliance, cloud,
broker, parser, or end-to-end transport runtime was run. Those tests establish
only their bounded local assertions.

### Implementation completion record — E9

**Date:** 2026-09-12
**Repository:** `tds-firewall-traffic-simulator` at commit `a62ac98`
**Contract:** FSTO/1 (FortiOS-Shaped Traffic Observation v1)
**Canonical SHA-256:** `62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653`

**Implemented components:**

1. **CONTRACT.md:** Complete FSTO/1 specification with canonical 180-byte record,
   hex representation, SHA-256 digest, field-by-field provenance, and documented
   simplifications.
2. **fsto1/generator.py:** Deterministic canonical record generation (fixed
   eventtime=1672531200000000000, action=close, proto=6, dstport=443).
3. **fsto1/simulator.py:** UDP send with independent attempt metadata (attempt
   time distinct from source eventtime).
4. **fsto1/receiver.py:** Experimental UDP receiver harness with receipt metadata
   (receipt time, peer address/port, payload length). Preserves bytes opaquely,
   no parsing.
5. **tests/test_contract.py:** Golden-byte verification (11 tests), SHA-256 match,
   hex match, deterministic generation, UTF-8 encoding, field structure.
6. **tests/test_integration.py:** Four integration scenarios including both
   required scenarios from E8.
7. **run_experiment.py:** Experiment runner with evidence capture (experiment
   schema, git revision, scenario results, limitations, findings).

**Test results:** All 15 pytest tests pass (11 contract tests, 4 integration tests).

**Validated scenarios:**

1. **clean_success:** One send → one receipt with byte preservation verified.
   Payload SHA-256 matches canonical. Attempt time < receipt time.
2. **receiver_unavailable:** Send succeeds (bytes_sent=180) but no receipt
   observed at different port. Demonstrates: successful UDP send ≠ guaranteed
   receipt.

**Evidence artifacts:**
- `evidence/experiment-20260913-145108.json` (commit `f828949`): **VALIDATED**
  evidence from clean implementation revision a62ac98. Complete experiment run
  with metadata, scenario results, verification, limitations, and findings.
  Git dirty=false, all scenarios successful.
- `evidence/experiment-20260912-005655-SUPERSEDED.json`: **SUPERSEDED** by
  clean-run evidence. Original run captured from dirty working tree
  (git_dirty=true, untracked implementation files). Preserved for historical
  record but not cited for Article 2 claims. See evidence/EVIDENCE.md for
  supersession rationale.

**Platform reconciliation completed:**
- ARCHITECTURE.md: Added "Teaching fixtures" section documenting FSTO/1,
  validated scenarios, limitations, and language selection rationale.
- OPEN-QUESTIONS.md: Updated OQ-0002 (transport/framing) and OQ-0003
  (payload/source profile) with bounded FSTO/1 findings.
- RISKS.md: Updated RISK-0001 with validated attempt/receipt gap finding.
- articles/ROADMAP.md: Updated Article 2 lifecycle to Drafting, updated next
  concrete action.

**Lifecycle state:** Researching → Drafting. Implementation and execution
complete. Platform findings reconciled. Ready for manuscript drafting.

**Explicit limitations preserved:**
- Local loopback only (127.0.0.1), not real network
- Synthetic teaching record, not real FortiOS output
- Experimental receiver harness, not production collector
- No claim about production firewall behavior or real network loss rates
- Python accepted for experimental tooling only; NOT selected for future
  RAW-COLLECTION service (provisional preference: Go)

**Language selection judgment (Geoffrey, 2026-09-12):**
Python accepted for firewall-traffic-simulator and minimal receiver harness
(bounded experimental tooling, readability, reproducible evidence). This does
NOT select Python for future RAW-COLLECTION service. When production collector
is architecturally earned, provisional preference is Go (continuous network
intake, bursts, controlled concurrency/allocation, graceful shutdown). This is
a provisional future direction, not a platform-wide mandate, and must be
revisited against workload evidence. Do not expand Python receiver into
production collector.
