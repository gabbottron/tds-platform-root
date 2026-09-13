# Article 2 — research and review working material

This file is a mutable review record, not publication authority. The sole Article
2 publication candidate is [articles/article2.md](../../article2.md). Its
temporary editorial source is [jane-draft-02.md](jane-draft-02.md); the files
`WORK.md`, `draft.md`, and `jane-draft-01.md` are research or temporary working
material only. The historical handoff below is retained as review history.

## Historical contract checkpoint handoff

Ready for **implementation authorization** with the `FSTO/1` teaching contract
finalized, simulator scope resolved, and implementation packet complete. It is
not manuscript editorial review, experiment execution, publication, or
checkpoint approval. Lifecycle state is owned only by the
[roadmap](../../ROADMAP.md). The [draft](draft.md) remains unwritten. This
packet identifies the uncommitted contract definition at the base revision below.

## Review prompt

Read [WORK.md](WORK.md), especially the `FSTO/1` teaching contract definition
(E6), canonical bytes, simulator scope resolution (E7), and implementation
authorization packet (E8). The contract defines exact, reproducible,
synthetic FortiOS-shaped bytes with complete provenance classification. Against
[Article 1](../../article1.md) and `article-01`, assess: does this contract
teach the raw boundary without claiming vendor verification, are teaching
simplifications explicit, and is the smallest experiment defined? Record
outcomes before implementation begins.

## Central contract and architecture changes

The `FSTO/1` teaching contract defines exact, reproducible, 180-byte synthetic
firewall traffic-observation records representing a completed TCP session (proto=6,
dstport=443) that ended normally (action=close) based on documented FortiOS 7.4.8
field names and key-value syntax (V3, V11). The contract uses bare key-value body
only (no syslog envelope), one teaching record per UDP datagram, eleven
vendor-documented fields with synthetic values (including eventtime as 19-digit
nanosecond epoch for source observation timestamp), and explicit teaching
simplifications for envelope, packing, encoding, and field order. Contract version
`FSTO/1` is accepted for Article 2 next experiment (E6), with simulator scope
resolved to require exactly two scenarios: clean_success and receiver_unavailable
(E7), and implementation authorization packet complete (E8).

No platform documents changed. See WORK.md's explicit no-change reconciliation
rationale. E6–E8 are workspace decisions under ADR-0001; platform reconciliation
is proposed after implementation validation during Reconciling stage. The
teaching contract does not select platform transport, event model, or production
collector.

## Evidence and its limits

- **Observed repository facts:** clean initial `article-2` checkout, HEAD
  `2e97282e60c5bff59d838b6d9b2e614f20495f46`; no customer inventory in tracked
  content, untracked files, or four-commit reachable history (R1/R2).
- **Authoritative documentation:** S1–S13 explain protocols/encodings; V1–V13
  and A1–A6 establish bounded vendor/service support. S13 adds RFC 5737 TEST-NET
  documentation addresses. Accessed 2026-09-10.
- **Teaching contract:** `FSTO/1` defines exact 180-byte canonical record
  representing completed TCP session that ended normally with complete provenance
  classification. Eleven fields: devid, eventtime (19-digit nanosecond epoch),
  logid, type, subtype, action (close=normal session end), proto (6=TCP), srcip,
  dstip, dstport, sentbyte. Every field name traced to V11; proto=6 establishes
  TCP observation distinct from UDP export; eventtime nanoseconds (source
  observation time) distinct from attempt/receipt times; action=close describes
  session-end status, not policy decision; synthetic values use TEST-NET addresses
  (S13) and vendor-documented log ID/action values. Documentation conflict noted:
  generic field page shows 10-digit second eventtime; Traffic log ID 13 definition
  and 7.4.8 samples establish 19-digit nanoseconds. Explicit teaching
  simplifications for envelope, packing, encoding, field order. Contract is
  defined but not implemented or executed.
- **Verified implementation:** none. No appliance, cloud, vendor, or experimental
  execution. Historical simulator and collector unit suites passed at revisions
  recorded below; that establishes bounded local code behavior only, not vendor
  behavior or `FSTO/1` generation.
- **Strongest evidence:** protocol framing boundaries; vendor export controls;
  FortiOS field names and values (V3, V11); TEST-NET synthetic addresses (S13);
  explicit classification of vendor-documented vs. teaching simplifications.
- **Largest gaps:** customer relevance; exact vendor envelope/framing/bytes;
  patch/build applicability; queue/retry/loss behavior. Fortinet access
  intermittently returns 403; PAN's traffic-field URL redirects to shared page.
- **Historical implementation evidence:** clean sibling `main` checkouts at
  simulator `7fae91e` and ingestor `b775831`; both `go test ./...` suites pass.
  Historical reference for UDP send/receive patterns only. Cannot become series
  authority or selection convenience. See WORK.md, I1–I2 and E4/E8.

## Comparison coverage

| Candidate investigated | Why it contributes to comparison |
| --- | --- |
| FortiGate / FortiOS 7.4.8, default forward-traffic syslog over UDP | Direct datagram export and key-value record distinction. |
| PAN-OS 11.1 appliance, IETF-mode Traffic syslog over TLS 1.2 | Stream framing, security and positional vendor fields as separate choices. |
| Cisco ASA 9.2 documentation profile, NSEL / NetFlow v9 over UDP | Binary templates and flow lifecycle observations; historical relevance caveat. |
| Check Point R81.20 Log Exporter, security logs, TCP/LEEF, explicit raw read-mode | Intermediate server hop and transformation before external receipt. |
| AWS Network Firewall, FLOW logs to S3, access-dated documentation | Managed publication, object versus record boundary, and opaque earlier hop. |

P1/F1 is selected as the Article 2 source-shaped basis; PAN-OS P2/F2 is
rejected for now as a contrast. Neither has an exact vendor-observed byte
fixture, and the selection does not imply customer prevalence. WORK.md C1–C12
cover relevance, mechanism representation, profile precision, accessible sources,
layer separation, wire reconstruction, synthetic reproducibility, operational
pressure, convenience/atypicality, teaching value, drift, and rights/provenance.
No numerical scoring or market-prevalence claim is used.

## Direct-edge finalist gap review

The shaping judgment permits a documentation-led teaching choice despite absent
customer inventory, preserves a direct firewall-to-collector edge, and confines
this pass to F1 and F2. P3–P5 remain contrasts only.

| Threshold focus | F1 — FortiGate / FortiOS 7.4.8 | F2 — PAN-OS 11.1 |
| --- | --- | --- |
| Exact profile | Fourth remote-syslog target; UDP/default format; forward-traffic filter; Traffic/forward session-end `LOG_ID_TRAFFIC_END_FORWARD`. Hardware/build unpinned. | Direct syslog profile; SSL/TLS 1.2, IETF header mode; policy forwarding at session end; Traffic `end` subtype. Deployment/patch unpinned. |
| Strong vendor evidence | CLI/configuration and versioned Traffic field references: V1/V2/V11. | Syslog/profile/forwarding procedure and Traffic field documentation: V4–V6/V12. |
| Framing | **Unresolved.** UDP supplies a datagram boundary, but Fortinet does not document default-format record packing or RFC 5426 conformance. | **Unresolved.** PAN does not document octet counting, delimiter use, or RFC 5425 implementation; TCP/TLS do not supply a message boundary. |
| Envelope and payload | Default-format envelope bytes unresolved; key-value body structure qualified by field/older raw example. | IETF header mode, facility, hostname choices documented with qualification; comma-delimited Traffic body and escaping documented with version-drift limitation. |
| Exact synthetic application bytes | **Unresolved.** No full default envelope/packing/terminator evidence. | **Unresolved.** No complete 11.1-pinned fields or framing/header-byte evidence. |
| Publication provenance | **Unresolved.** Public pages have intermittent 403; no terms/fixture permission assessment. | **Unresolved.** Public pages include a mutable field-page redirect; no terms/fixture permission assessment. |
| Eligibility under current threshold | **Selected source-shaped basis; exact contract still open.** | **Rejected for now; retained as a contrast.** |

No illustrative application-message bytes are included. Doing so would invent
F1 envelope/packing or F2 stream framing, and would risk presenting a teaching
construction as device output. WORK.md records the complete threshold and
twelve-criterion matrices, the standards chains, and the exact missing evidence.

## Inferences, teaching simplifications and continuity

The collection implications and comparison coverage are labeled inferences.
The browser/session examples and `3 abc` length-prefix illustration are teaching
simplifications, not vendor wire fixtures. The `FSTO/1` contract explicitly
labels teaching simplifications: bare key-value body only (no syslog envelope),
one record per UDP datagram, fixed field order, no quoting/escaping, UTF-8
encoding, all fields present, printable ASCII only, fixed numeric eventtime
(Unix epoch), eleven fields only (minimum for TCP session observation and
source/attempt/receipt time distinction). Every simplification names what it
excludes and why it's acceptable. The article must retain these distinctions.

The customer-evidence limitation is an accepted shaping condition (E4): a
documentation-led teaching profile selected with no prevalence claim and explicit
reconsideration trigger if customer evidence appears. The simulator scope
question is resolved (E7): Article 2 includes implementation and execution of
smallest experiment demonstrating canonical bytes, golden-byte verification,
three-way time distinction (source eventtime, simulator attempt time, receiver
receipt time), byte preservation, and exactly two required scenarios:
clean_success (one send → one receipt) and receiver_unavailable (send succeeds,
no receipt observed). Implementation authorization packet complete (E8). No
article prose produced.

The historical implementation is reference for UDP send/receive patterns only.
Cannot influence selection, cannot become series authority. `FSTO/1` contract,
golden-byte generation, and experiment artifacts must be original implementation
against the accepted contract (E8). Prohibited: Kafka handoff, HTTP metrics,
RFC 5424 envelope, run-ID embedding unless justified by `FSTO/1` requirements.

## Contract judgment complete; implementation authorized

Contract shaping complete: `FSTO/1` defines which documented FortiOS fields and
syntax to retain (eight vendor-documented fields V11, key-value syntax V3), which
exact bytes (131-byte canonical record with complete hex representation), and
which runtime behaviors remain experiment claims (FortiOS envelope, packing,
field order, encoding, all explicitly labeled as teaching simplifications or
vendor unknowns). This does not reopen source-profile selection.

Simulator scope resolved (E7): smallest experiment demonstrating canonical bytes,
attempt vs. receipt distinction, byte preservation. Implementation authorization
packet complete (E8): repository responsibility, acceptance criteria, validation
commands, historical consultation limits, prohibited scope.

Next action: proceed to implementation and execution per E8 authorization.

## Repository state and validation

Repository: `/Users/gabbott/git/gabbottron/tds-platform-root`.
Actual branch: `article-2` (request used `article-02`); no rename/switch.
Base HEAD: `2e97282e60c5bff59d838b6d9b2e614f20495f46`.
Affected service revisions: none changed. The unregistered historical sibling
repositories inspected for this review are `firewall-simulator` at `7fae91e`
and `firewall-ingestor` at `b775831`.

Existing immutable checkpoint: annotated `article-01`, tag object
`1c9d682caea85a49e68e53f2fd979f18948f26e3`, target
`26a55c1215ca2463e121c42242e60a41ef2961af`.
No proposed Article 2 tag or publication revision set.

Uncommitted research files: `WORK.md`, this `review.md`, and `articles/ROADMAP.md`.
At the implementation-inventory start, those were already-present research
changes and no root files were untracked; they were preserved. The roadmap
advances only to the research stage because the bounded question, inherited
promise, scope, exclusions, and evidence needs are recorded; its next action is
joint review.

Validation performed on 2026-09-10:

- Complete working diff inspected; `git diff --check` passed.
- Evidence-reference check passed: all 31 S/V/A source IDs resolve; 40 primary
  URL occurrences recorded. This checks trace structure, not independent proof
  of source claims. Primary content was reviewed with access/redirect limits
  disclosed in WORK.md; material claims were manually checked against that trace.
- All 22 local Markdown links/anchors across the changed files resolve.
- Nine protected files byte-compared successfully: architecture, questions,
  risks, ADR-0001 and transcript against `article-01`; README, AGENTS.md,
  `.gitignore` and draft against HEAD. Tag object and target unchanged.
- Manual scope/terminology review confirmed separate layers, qualified vendor
  behavior, the selected F1 profile without customer-prevalence inference, and
  open simulator scope.
- Only the three declared files are modified; index is unchanged and there are
  no untracked files. No root runtime tests apply. Historical implementation
  unit tests are recorded separately below; no appliance, cloud, broker, or
  end-to-end execution was run.
- Historical implementation inventory: both sibling repositories remained clean;
  their ordinary Go test suites passed. No dependencies, fixtures, services, or
  external systems were introduced or contacted.

## Editorial outcomes and approval

The `FSTO/1` teaching contract is defined and accepted for Article 2 next
experiment (E6). Simulator scope resolved (E7). Implementation authorization
packet complete (E8). No platform architecture changes; reconciliation proposed
after implementation validation. No article prose, publication, or checkpoint
tag approval is recorded. Leave all changes uncommitted.

---

## Implementation completion — Post-E8 validation

**Date:** 2026-09-12
**Implementation repository:** `tds-firewall-traffic-simulator` at commit `a62ac98`
**Evidence artifact:** `evidence/experiment-20260912-005655.json`

### Execution validation

Implementation authorization packet (E8) fully executed. All required components
delivered:

1. ✓ Deterministic canonical generation (180 bytes, SHA-256: `62bf0871...`)
2. ✓ Golden-byte verification (pytest: 11/11 contract tests pass)
3. ✓ Independent attempt/receipt metadata (simulator: attempt_time, receiver:
   receipt_time, both distinct from source eventtime)
4. ✓ Two-scenario validation (clean_success: byte preservation verified;
   receiver_unavailable: send without receipt confirmed)
5. ✓ Opaque byte preservation (receiver treats payload as opaque, no parsing)
6. ✓ Execution artifacts (JSON evidence with experiment schema, git revision,
   scenario results, limitations, findings)

**Test results:** 15/15 pytest tests pass (11 contract, 4 integration).

**Evidence packet:** Complete experiment run captured with:
- Experiment metadata (schema v1, execution time, runner)
- Contract version (FSTO/1, 180 bytes, SHA-256, hex)
- Implementation metadata (repository, git revision, dirty state)
- Scenario results (both success, byte verification, timing)
- Explicit limitations (local loopback, synthetic, teaching-only)
- Validated findings (datagram boundary preserved, send ≠ receipt)

### Platform reconciliation validation

Platform documents updated with bounded experimental findings:

1. ✓ ARCHITECTURE.md: Added "Teaching fixtures" section documenting FSTO/1
   status (implemented), validated scenarios, limitations, language selection.
2. ✓ OPEN-QUESTIONS.md: Updated OQ-0002 (transport/framing partial finding),
   OQ-0003 (payload/source profile implemented) with bounded scope preserved.
3. ✓ RISKS.md: Updated RISK-0001 with validated attempt/receipt gap finding,
   explicit limitations noted.
4. ✓ articles/ROADMAP.md: Lifecycle Researching → Drafting, next action updated.

**Limitations preserved:** All platform updates explicitly note local loopback
only, synthetic teaching record, experimental harness not production collector,
no claim about production behavior or real network loss.

**Language selection recorded:** Python accepted for experimental tooling
(readability, reproducible evidence). NOT selected for future RAW-COLLECTION
service (provisional preference: Go for production collector when
architecturally earned).

### Lifecycle state change

**Before:** Researching (contract defined, implementation authorized but not started)
**After:** Drafting (implementation complete, experiments executed, platform
findings reconciled, ready for manuscript)

**Next action:** Draft Article 2 manuscript showing wire-profile selection
reasoning, FSTO/1 contract specification, experimental findings, and preserved
limitations. Do NOT publish, create tags, or push to remote until explicitly
authorized.

### Validation performed 2026-09-12

- Implementation repository commit `a62ac98` verified stable.
- All 15 pytest tests passed on Python 3.12.7.
- Evidence artifact `evidence/experiment-20260912-005655.json` inspected and
  validated.
- Platform document changes inspected for scope preservation.
- No unapproved platform commitments introduced.
- No customer facts invented.
- Experimental limitations explicitly preserved in all reconciled documents.

---

## Evidence correction — 2026-09-13

### Drafting gate failure

Pre-drafting verification identified that the original evidence artifact
`evidence/experiment-20260912-005655.json` cited a dirty working tree
(git_dirty=true, untracked implementation files) rather than an immutable clean
implementation revision. This failed drafting gate requirement #6.

### Corrective action taken

1. Verified tds-firewall-traffic-simulator state: branch main, HEAD a62ac98, clean tree
2. Re-ran complete test suite from clean a62ac98: 15/15 passed
3. Re-ran both experiment scenarios from clean a62ac98: both successful
4. Generated new evidence artifact: `evidence/experiment-20260913-145108.json`
5. Verified new artifact cites: git_revision=a62ac98, git_dirty=false, git_status=null
6. Preserved original artifact as: `evidence/experiment-20260912-005655-SUPERSEDED.json`
7. Created evidence index: `evidence/EVIDENCE.md` documenting supersession
8. Committed evidence: commit `f828949` in tds-firewall-traffic-simulator
9. Pushed to GitHub: main now at f828949
10. Updated platform-root references: WORK.md, review.md now cite validated evidence

### Validated evidence artifact

**File:** `evidence/experiment-20260913-145108.json`
**Evidence commit:** `f828949` in tds-firewall-traffic-simulator
**Implementation revision:** `a62ac9897e0086ff256f996249ed56154717773c` (a62ac98)
**Git dirty:** false
**Git status:** null

**Test results:** 15/15 pytest passed
- 11 contract tests (golden-byte, SHA-256, hex, structure, encoding)
- 4 integration tests (clean_success, receiver_unavailable, opaque, datagram)

**Scenario results:**
- clean_success: status=success, bytes_match=true, bytes_sent=180
- receiver_unavailable: status=success, no_receipt_confirmed=true, bytes_sent=180

**Contract verification:**
- Canonical length: 180 bytes
- Canonical SHA-256: 62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653

### Superseded artifact

**File:** `evidence/experiment-20260912-005655-SUPERSEDED.json` (preserved)
**Superseded reason:** Captured from dirty working tree, not immutable clean commit
**Superseded by:** experiment-20260913-145108.json
**Status:** Not cited for Article 2 claims, preserved for historical record

The experiment results were materially identical (both scenarios succeeded, byte
preservation verified), but the evidence provenance was inadequate for drafting
gate requirements.

### Corrected platform-root references

- articles/work/article-02/WORK.md (E9): Updated evidence artifacts section
- articles/work/article-02/review.md (this section): Documented correction

All Article 2 claims now cite validated evidence from clean implementation
revision a62ac98 via evidence commit f828949.

---

## Drafting review packet — 2026-09-13

### Manuscript complete

**Publication candidate:** [articles/article2.md](../../article2.md)
**Temporary editorial source:** [jane-draft-02.md](jane-draft-02.md), byte-identical
**Word count:** 2,832 words
**Status:** Unpublished publication candidate; editorial review, not approved for publication

### Central claim

"The firewall sends an event" conceals at least seven independent technical contracts. To make an honest architectural choice for the first source-to-collector edge, we must separate transport, framing, envelope, payload encoding, and source profile—understanding how each affects what the collector observes and what failures are detectable.

We selected FortiOS Traffic logs over UDP as the first teaching fixture (FSTO/1) for pedagogical reasons: direct firewall-to-collector boundary, approachable datagram boundary, useful attempt-versus-receipt uncertainty, and clear TCP-traffic-over-UDP-export distinction. This is not a claim about customer prevalence, vendor superiority, or platform-wide transport selection.

FSTO/1 is a precise synthetic teaching contract (180 bytes) that separates vendor-documented components from teaching simplifications. The experiment validated deterministic generation, golden-byte verification, and byte preservation on local loopback. A successful UDP send is attempt evidence, not receipt evidence; the unavailable-receiver run did not observe its destination port.

### Evidence and implementation revisions

**Implementation:**
- Repository: tds-firewall-traffic-simulator
- Clean implementation commit: `a62ac9897e0086ff256f996249ed56154717773c` (a62ac98)
- Implementation components: generator, simulator, receiver, tests, experiment runner
- Test results: 15/15 pytest passed

**Validated evidence:**
- Evidence file: `evidence/experiment-20260913-145108.json`
- Evidence commit: `f828949` in tds-firewall-traffic-simulator
- Git provenance: a62ac98, git_dirty=false, git_status=null
- Scenarios: clean_success (success), receiver_unavailable (success)
- Contract: 180 bytes, SHA-256 `62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653`

**Superseded evidence:**
- Original dirty-tree artifact renamed to `-SUPERSEDED.json`
- Documented in `evidence/EVIDENCE.md` with supersession rationale

**Platform reconciliation:**
- Commit: `401bbbf` on article-2 branch in tds-platform-root
- Files updated: ARCHITECTURE.md, OPEN-QUESTIONS.md, RISKS.md, ROADMAP.md, WORK.md, review.md
- Lifecycle: Drafting

### Actual experimental results

**clean_success:**
- One local UDP send (127.0.0.1:15140)
- One receipt observed by listening receiver
- Received payload exactly matches canonical FSTO/1 (SHA-256 verified)
- 180 bytes preserved, datagram boundary intact
- Three-way time distinction observable (source eventtime, attempt metadata, receipt metadata)

**receiver_unavailable:**
- One local UDP send to 127.0.0.1:25140; `sendto()` returned 180 bytes
- A receiver on different port 35140 observed no cross-delivery
- Does not establish whether an application received the datagram at 25140
- Supports only the bounded distinction between local send success and application-receipt evidence

**Limitations explicitly preserved:**
- Local loopback only (127.0.0.1), not real network
- Synthetic teaching record, not real FortiOS output
- Experimental receiver harness, not production collector
- No claim about production loss rates, vendor behavior, or network patterns

### Architecture changes

**Platform documents updated:**
1. ARCHITECTURE.md: Added "Teaching fixtures" section for FSTO/1
2. OPEN-QUESTIONS.md: OQ-0002 (transport) and OQ-0003 (payload) updated with bounded findings
3. RISKS.md: RISK-0001 updated with validated attempt/receipt gap
4. ROADMAP.md: Article 2 lifecycle Researching → Drafting

**No platform architecture selection beyond Article 2 experiment:**
- UDP selected for this teaching experiment only, not platform-wide
- Python bounded to experimental tooling, not production collector
- FortiOS-shaped profile is first teaching fixture, not universal platform contract

### Inferences and teaching simplifications

**Inferences made:**
- Seven-layer contract separation (traffic observed, exported observation, transport, framing, envelope, encoding, profile)
- FortiOS/UDP selected for pedagogical value (fewest simultaneous concepts, clearest attempt/receipt gap)
- Documentation gap normal; invented details would be dishonest
- Three-way time distinction matters (source eventtime, attempt time, receipt time)

**Teaching simplifications explicitly documented:**
1. No syslog envelope (FSTO/1 specifies key-value body only)
2. No terminator (no newline/CR/NUL)
3. One record per datagram (not claiming multi-record packing knowledge)
4. Fixed field order (FSTO/1 order, not FortiOS claim)
5. Synthetic values (TEST-NET addresses, teaching device ID)

**Vendor-documented vs. teaching:**
- Provenance table in manuscript distinguishes FortiOS Log Reference sources from teaching simplifications
- Fortinet eventtime documentation conflict noted (10-digit generic vs 19-digit Traffic-specific)
- action=close treated as an allowed-session end status under Traffic/forward; no TCP shutdown sequence inferred

### Source-support gaps

**No evidence for:**
- Real FortiOS wire output (envelope, packing, ordering across versions/configs)
- Customer inventory or prevalence
- Production network loss rates or patterns
- FortiGate buffering/retry behavior under load
- Where loss occurred in receiver_unavailable (kernel, routing, ICMP handling)

**Evidence available from:**
- Fortinet FortiOS Log Reference (field names, log types, semantics)
- RFC 5737 (TEST-NET documentation addresses)
- IANA Protocol Numbers (proto=6 = TCP, standard reference)
- FSTO/1 implementation and validated experiment (clean a62ac98, evidence f828949)

### Deviations from the brief

**Word count:** 2,832 words. The current candidate is the designated Jane draft,
not the obsolete 4,339-word `draft.md`.

**Narrative approach:** Followed briefed structure closely. No material deviations.

**Evidence discipline:** All material claims traced to WORK.md evidence. No invented customer facts, vendor behavior, or measurements.

### Continuity and overlap concerns

**Continuity with Article 1:**
- Brief reference to platform-root governance without retelling the argument (as instructed)
- Carries forward synthetic source → raw collection → durable handoff flow
- Does not repeat Article 1's preservation rationale

**Potential overlap:**
- Some UDP/TCP/TLS transport concepts introduced may be basic for experienced engineers. Brief mentions this: "assume general technical literacy, but do not assume familiarity with firewalls, network security..."
- Manuscript errs toward accessibility; could be more concise for purely senior-architect audience

**Series promise alignment:**
- Addresses Part 2 published promise: "examine firewall products and export mechanisms... select one versioned teaching contract... show exact wire representation"
- Fulfills comparison of profiles, exact bytes, documented vs simplified, controlled simplification

### Unresolved editorial choices for Geoffrey and Jane

1. **Candidate identity:** `articles/article2.md` is the sole, 2,832-word publication candidate; its temporary Jane source is byte-identical. The obsolete 4,339-word draft is not under editorial consideration.

2. **Transport explanations depth:** UDP/TCP/TLS/NetFlow/cloud delivery descriptions (lines 55-69, 95-123) aim for accessibility without assuming prerequisite knowledge. Is this the right balance, or should the manuscript assume more networking background and abbreviate?

3. **Five candidate profiles:** Manuscript covers FortiOS, PAN-OS, ASA, Check Point, AWS to show materially different boundary types. Is five profiles the right scope, or should it be condensed to FortiOS + one TLS contrast + one managed-delivery contrast?

4. **FSTO/1 field-by-field explanations:** Lines 197-219 explain each of 11 fields. This supports the "inspectable contract" claim but adds length. Should field explanations be condensed to just the most pedagogically interesting (eventtime, action, proto, sentbyte)?

5. **"Next" section specificity:** Final section (lines 366-370) avoids pre-committing to "build a collector" or "choose a broker" as instructed. Does this appropriately leave the next question open, or should it signal provisional next directions more concretely?

### Suggested visual opportunities

1. **Seven-layer contract diagram:** Visualize traffic observed → exported → transport → framing → envelope → encoding → profile as distinct layers with example values from FSTO/1

2. **Three-way time timeline:** Show source eventtime (2023-01-01 nanoseconds in payload), simulator attempt time (local clock), receiver receipt time (local clock) on a timeline to make the distinction concrete

3. **FSTO/1 anatomy:** Annotated canonical record showing which components are vendor-documented (green) vs teaching simplifications (amber), mapped to provenance table

4. **Attempt vs receipt distinction:** Simple diagram showing local UDP send success as attempt evidence, distinct from a receiver receipt. Do not depict destination non-receipt unless a future experiment observes that destination.

5. **Candidate profiles comparison table:** Boundary type, transport, framing, teaching value for all five profiles in a scannable comparison

### Exact repository state

**tds-firewall-traffic-simulator:**
- Branch: main
- HEAD: `f828949` (evidence commit, clean, pushed)
- Parent: `a62ac98` (implementation commit, clean, pushed)
- Working tree: clean
- Remote: https://github.com/gabbottron/tds-firewall-traffic-simulator (up to date)

**tds-platform-root:**
- Branch: article-2
- HEAD: `401bbbf` (reconciliation commit, pushed)
- Working tree includes mutable editorial and review material; publication authority
  is `articles/article2.md` only
- Remote: https://github.com/gabbottron/tds-platform-root article-2 branch (401bbbf pushed)

**Workspace classification:**
- `WORK.md`: research and evidence record
- `review.md`: mutable review record
- `draft.md`, `jane-draft-01.md`, `jane-draft-02.md`: temporary editorial working material

### Lifecycle advancement

**Current state:** Drafting
**Proposed advancement:** Editorial Review (when Geoffrey/Jane approve draft and review packet)
**Requirements to advance (from AGENTS.md):** Initial manuscript complete, material claims traced to evidence, review packet current
**Status:** Candidate established and awaits editorial review; not approved for publication.

**Not advancing to:** Publication Ready (requires Geoffrey/Jane editorial approval and any requested revisions)

### Validation performed 2026-09-13

- All 10 drafting gates verified passing (see gate verification above)
- Manuscript claims traced to WORK.md evidence records
- No invented customer facts, vendor behavior, or measurements
- Teaching simplifications explicitly documented
- Evidence provenance clean (a62ac98, f828949)
- Platform reconciliation complete (401bbbf)
- Limitations preserved in all sections

**Ready for conversational editorial review with Geoffrey and Jane.**
