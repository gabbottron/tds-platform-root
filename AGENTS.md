# Agent contract

Begin every task by reading README.md, ARCHITECTURE.md, and the open questions,
risks, and decisions applicable to the work. Read the associated article when
historical narrative or checkpoint context is relevant. Inspect the relevant
working trees before editing. This root is the durable source for
platform-level intent; service repositories, when they exist, remain authorities
for their own implementation and tests.

Keep evidence, inference, accepted decision, proposal, risk, and unknown
separate. Never invent missing facts or silently turn a later-stage idea into
accepted architecture. If an unresolved fact could change correctness or scope,
stop and ask, or record it as an open question with a provisional boundary and
evidence needed to resolve it.

Keep this root small. Do not create services, schemas, contracts, automation,
skills, or infrastructure until a real responsibility and its boundary have
been earned. When a platform boundary changes, reconcile this root with the
affected implementation before calling the work complete. Preserve history and
show the owner the material changes.

## Article work and resumption

For installment work, read [the roadmap](articles/ROADMAP.md), the active
workspace when one exists, and the previous published article. Read the previous tagged root
checkpoint, including its architecture, decisions, questions, and risks, using
`git show <checkpoint>:<path>`; inspect its tag and changes since it with
`git diff <checkpoint> --`. Inspect untracked files and relevant service working
trees separately. Do not switch or modify historical tags to read them.

Identify the exact promised next question. Inventory inherited decisions,
deferrals, exclusions, questions, and risks in the installment's WORK.md with
source references. Preserve discrepancies between the published promise and
newer instructions as explicit shaping questions with a provisional boundary
and evidence needed to resolve them.

The roadmap owns installment lifecycle state and next concrete action. Do not
duplicate that state in workspace files. Before handing back work, update the
roadmap and relevant working notes so a fresh session can resume without chat
history. Record blockers and the evidence or decision needed to continue.

`articles/work/` is temporary, branch-local scratch space and must not merge to
`main`. Create `articles/work/article-NN/` only for an active installment. Before
cleanup, reconcile durable findings into platform documents, place any durable
article evidence outside `work/`, and identify the publication candidate outside
`work/`. A future installment begins with a new empty workspace; do not copy a
prior installment's working files forward.

## Editorial material and evidence

The roadmap and workspaces record editorial intent, evidence references, and
working prose. They are not platform authority. Accepted platform intent remains
in the architecture, decisions, questions, and risks; service repositories own
implementation and tests. Execution records support only what was observed.
Published transcripts preserve what was published, not the latest architecture.

Classify material explicitly where claims are recorded:

- **Authoritative evidence:** a primary source within its stated domain and
  version; documentation alone does not prove local runtime behavior.
- **Verified implementation:** inspected implementation with relevant validation
  at an identified revision; state exactly what the checks establish.
- **Inference:** a conclusion drawn from cited evidence, with its limits.
- **Hypothesis:** an untested explanation or prediction and how to test it.
- **Teaching simplification:** a deliberate departure from source behavior,
  with its purpose and consequences.
- **Decision:** an accepted choice with its authority and rationale; proposed
  choices remain proposals until accepted and reconciled where applicable.
- **Risk:** a plausible harmful outcome and its trigger.
- **Unknown:** missing information, its consequence, and how to resolve it.

Do not introduce technology by convention or invent customer facts, measurements,
or vendor behavior. Trace external claims to primary documentation with URL,
version/date, section, applicability, and limitations. Preserve synthetic fixture
provenance and distinguish documented bytes from deliberate simplifications.

Implement and validate before claiming experimental results. Record repository
revisions and relevant dirty changes, environment, declared inputs, commands,
actual results, and durable evidence paths. Planned or unexecuted checks are
unverified. Keep service internals and bulky artifacts in their owning repository;
retain concise findings and exact references here.

Reconcile findings into affected platform documents before drafting claims from
them. Record changes or an explicit no-change rationale in the active WORK.md. Unresolved
findings remain qualified; they cannot silently become accepted architecture.

## Per-article workspace

Use a three-file `articles/work/article-NN/` layout for each active installment;
reset content rather than copying prior conclusions. The directory is temporary
and removed before its branch merges to `main`.

- **WORK.md:** brief (question, promise, scope, exclusions, evidence needs),
  inherited context, evidence/source trace, implementation/experiment findings,
  and platform reconciliation. Keep shaping questions and their resolutions here.
- **draft.md:** manuscript prose, separate from notes; trace material claims to
  WORK.md evidence entries. Mark an unwritten draft explicitly.
- **review.md:** editorial handoff packet linking the draft and evidence. Include
  central claim, architecture changes, verified evidence, inferences and teaching
  simplifications, source support, deviations from the brief, unresolved editorial
  choices, continuity/overlap concerns, and relevant repository state.

## Lifecycle

Shaping → Researching → Experimenting → Reconciling → Drafting →
Editorial Review → Publication Ready → Published.

Advance only when the current stage's substance is recorded:

| Stage | Required before advancing |
| --- | --- |
| Shaping | Question, inherited promise, starting scope, exclusions, and evidence needs established. |
| Researching | Sources traced, applicability assessed, and assumptions separated from documented behavior. |
| Experimenting | Needed implementation and experiments executed and validated; results and limits recorded. |
| Reconciling | Findings reconciled with platform architecture, decisions, questions, and risks, or no-change rationale recorded. |
| Drafting | Manuscript claims trace to the reconciled evidence record. |
| Editorial Review | Review packet and current draft reviewed conversationally; outcomes recorded back in the workspace. |
| Publication Ready | Material choices resolved and exact publication material and proposed checkpoint revision set ready for Geoffrey's approval. |
| Published | Actual publication and approved checkpoint references recorded; transcript preserved separately from draft. |

A stage may be not applicable only with a recorded reason; this never implies
experimental validation. Return to earlier work when evidence or scope changes,
recording why in the active WORK.md. Lifecycle progress does not grant architectural acceptance.

## Conversational review and publication

The review packet must identify the draft being reviewed, root and affected
service revisions, relevant uncommitted changes, checks and limitations, and
proposed checkpoint references clearly distinguished from existing tags. Refresh
this snapshot when handing it off; do not treat an old snapshot as current.
Provide a short review prompt and the files needed for a conversation without
CLI history. Record Geoffrey's editorial outcomes back into the workspace.
Changes affecting architecture return through reconciliation; material changes
after review require a refreshed packet and renewed review.

Stop before publication or checkpoint tag creation/movement without Geoffrey's
explicit approval of the concrete material and revisions. Publication Ready is
not publication approval. Preserve existing published transcripts and immutable
checkpoint tags; future transcripts remain separate from working drafts.
