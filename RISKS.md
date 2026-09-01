# Risks — Article 1 checkpoint

These are plausible harmful outcomes, not completed mitigations. Escalate when
the stated evidence or event occurs.

## RISK-0001 — Input is lost before collection

- **Harmful outcome:** the platform presents incomplete evidence as complete.
- **Condition:** source or transport drops input before the collection boundary.
- **Concern:** observability and evidence integrity.
- **Current architectural treatment:** The first experiment must record source
  attempts and collector receipts independently; no implementation exists yet.
- **Escalate before:** treating an attempt/receipt gap as understood without a
  controlled experiment.

## RISK-0002 — Source address is mistaken for device identity

- **Harmful outcome:** one device history splits, or unrelated devices merge.
- **Condition:** a network change reuses or changes an address.
- **Concern:** identity, provenance, and investigation accuracy.
- **Current architectural treatment:** The first experiment must preserve
  observed address without asserting continuity; no identity implementation
  exists yet.
- **Escalate before:** address changes or conflicts are silently reconciled.

## RISK-0003 — Format drift is silently misinterpreted

- **Harmful outcome:** normalized data looks valid while fields mean something
  else.
- **Condition:** firmware, model, export format, or optional fields change.
- **Concern:** interpretation and recoverability.
- **Current architectural treatment:** The first experiment must separate raw
  collection from later interpretation; no parser exists yet.
- **Escalate before:** representative input no longer matches its declared
  profile and the mismatch is treated as valid data.

## RISK-0004 — Raw security evidence is exposed

- **Harmful outcome:** sensitive topology, addresses, identities, or policy data
  is disclosed.
- **Condition:** raw records or telemetry are broadly accessible or retained
  without defined obligations.
- **Concern:** security, privacy, access, and retention.
- **Current architectural treatment:** The checkpoint is limited to synthetic
  data while security questions remain open.
- **Escalate before:** introducing any non-synthetic data, external ingress,
  shared environment, or access beyond the named development participants.

## RISK-0005 — Partial normalization destroys recoverability

- **Harmful outcome:** later parser improvements cannot reconstruct the source
  observation.
- **Condition:** collection replaces raw bytes with an interpretation too early.
- **Concern:** raw boundary and future replay.
- **Current architectural treatment:** The first boundary must preserve before
  interpreting; no runtime implementation exists yet.
- **Escalate before:** any proposed boundary discards source-shaped input.

## RISK-0006 — Platform documentation drifts from implementation

- **Harmful outcome:** agents make locally valid changes that violate the
  platform's actual boundaries.
- **Condition:** service work changes a cross-service assumption without root
  reconciliation.
- **Concern:** multi-repository coherence and agent safety.
- **Current architectural treatment:** The checkpoint requires a root-start
  workflow and explicit authority split; no synchronization automation exists.
- **Escalate before:** implementation and root state disagree without a recorded
  reconciliation decision.
