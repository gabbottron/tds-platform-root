# Architecting With Evidence

This repository is the platform-level companion to Geoffrey Abbott's
*Architecting With Evidence* series. It records the evolving architecture of a
threat-detection system as decisions become knowable through implementation,
measurement, and customer conversation.

## Latest published checkpoint

The latest published checkpoint is `article-02`: [**What Crosses the First
Edge?**](articles/article2.md). It identifies the published Article 2 material
and its supporting simulator/evidence revisions; it is not a production
reference architecture or universal recommendation. Its durable evidence record
is [articles/evidence/article-02.md](articles/evidence/article-02.md).

The immutable `article-01` checkpoint remains the previously published
**Before the First Service** installment. It contains the initial architecture,
decisions, risks, open questions, and agent contract.

Read [Article 2](articles/article2.md) first, then read [AGENTS.md](AGENTS.md),
[ARCHITECTURE.md](ARCHITECTURE.md), [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md),
[RISKS.md](RISKS.md), and [ADR-0001](decisions/0001-establish-the-platform-root.md).

The annotated [`article-02`](https://github.com/gabbottron/tds-platform-root/tree/article-02)
tag identifies this latest immutable installment. Future tags should identify the
immutable root and service revisions that each installment has actually earned.

## Series checkpoints

`main` is the accumulated current platform state. Each annotated article tag is
an immutable reader entry point for the state earned by that installment.

| Installment | Checkpoint | Repository state |
| --- | --- | --- |
| Part 1 — *Before the First Service* | [`article-01`](https://github.com/gabbottron/tds-platform-root/tree/article-01) | The initial platform root, architecture, decisions, risks, questions, and agent contract. |
| Part 2 — *What Crosses the First Edge?* | [`article-02`](https://github.com/gabbottron/tds-platform-root/tree/article-02) | The Article 2 publication material, FSTO/1 reconciliation, and durable evidence record. |

Future installments belong in this index when their immutable checkpoints are
earned. The latest `main` branch remains the place to see everything the series
has established so far.

The root owns platform-level intent and cross-service coherence. Future service
repositories will own their implementation, tests, and build instructions. The
architecture will grow when evidence creates a responsibility; we are not
completing the final system in advance.

## Continuing article work

The working branch may advance beyond the published checkpoint. Start with the
[series roadmap](articles/ROADMAP.md) for the current installment and next action,
then follow the installment workflow in [AGENTS.md](AGENTS.md).
The roadmap and article workspaces hold mutable editorial intent and evidence
references; they do not establish platform architecture. Published transcripts
remain separate from working drafts, and tagged checkpoints preserve history.

`articles/work/` is branch-local scratch space. It is intentionally absent from
`main` between active installments: create a fresh `article-NN` workspace only
on the branch doing that installment, then reconcile durable evidence, review
outcomes, and the publication candidate outside `work/` before merge.
