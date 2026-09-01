# Architecting With Evidence

This repository is the platform-level companion to Geoffrey Abbott's
*Architecting With Evidence* series. It records the evolving architecture of a
threat-detection system as decisions become knowable through implementation,
measurement, and customer conversation.

## Current checkpoint

This is the `article-01` checkpoint: **Before the First Service**. It contains
architecture, decisions, risks, open questions, and the initial agent contract.
It contains no runtime service implementation and is not a production reference
architecture or universal recommendation.

Read [Article 1](articles/article1.md) first, then read [AGENTS.md](AGENTS.md),
[ARCHITECTURE.md](ARCHITECTURE.md), [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md),
[RISKS.md](RISKS.md), and [ADR-0001](decisions/0001-establish-the-platform-root.md).

The immutable Article 1 checkpoint is identified by the annotated tag
`article-01`. Future tags should identify the immutable root and service
revisions that each installment has actually earned.

The root owns platform-level intent and cross-service coherence. Future service
repositories will own their implementation, tests, and build instructions. The
architecture will grow when evidence creates a responsibility; we are not
completing the final system in advance.
