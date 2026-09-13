# Article 2 — durable evidence record

This is the durable evidence companion to the unpublished Article 2 publication
candidate, [What Crosses the First Edge?](../article2.md). It replaces the
temporary Article 2 workspace before merge. It is not platform architecture;
accepted platform implications remain in the root architecture, questions, and
risks.

## Artifact identities

- **Candidate:** `articles/article2.md`, unpublished and not approved for publication.
- **Implementation:** `tds-firewall-traffic-simulator` commit
  `a62ac9897e0086ff256f996249ed56154717773c` (`a62ac98`).
- **Validated execution evidence:** `tds-firewall-traffic-simulator` commit
  `f828949`, `evidence/experiment-20260913-145108.json`.
- **Superseded execution evidence:** `experiment-20260912-005655-SUPERSEDED.json`;
  it was captured from a dirty working tree and is not evidence for Article 2
  claims.

## What is supported

- FSTO/1 generates a deterministic 180-byte synthetic teaching record with
  SHA-256 `62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653`.
- The validated clean-loopback run recorded one 180-byte receipt at a listening
  receiver and byte equality with the canonical record.
- The simulator records source eventtime in the payload separately from local
  send-attempt and receiver-receipt metadata.

## Experiment limitation requiring careful wording

The `receiver_unavailable` run sent to UDP port 25140 and waited on a different
port, 35140. It established local `sendto()` success and no cross-delivery to
35140; it did **not** observe port 25140 and therefore does not establish
destination non-receipt or locate loss. A successful UDP send is attempt
evidence, not application-receipt evidence.

## Source-profile boundaries

FSTO/1 is synthetic, not captured or vendor-verified FortiOS output. The
FortiOS 7.4.8 documentation supports the selected Traffic/forward vocabulary and
the remote-syslog target's UDP/default-format settings, but does not establish
FSTO/1's exact envelope, packing, field order, quoting, terminator, or encoding.
Those are declared teaching simplifications.

- Fortinet, [FortiOS 7.4.8 syslogd4 settings](https://docs.fortinet.com/document/fortigate/7.4.8/cli-reference/326975389/config-log-syslogd4-setting).
- Fortinet, [FortiOS 7.4.8 Log Reference](https://fortinetweb.s3.amazonaws.com/docs.fortinet.com/v2/attachments/a4de4e5a-2f60-11f0-a9d0-d2b0d2e22f7d/FortiOS_7.4.8_Log_Reference.pdf).
- IETF, [RFC 5426](https://www.rfc-editor.org/rfc/rfc5426) for UDP syslog transport limitations.
- IETF, [RFC 5737](https://www.rfc-editor.org/rfc/rfc5737) for TEST-NET addresses.

## Article 3 handoff

Article 3 must begin with a new workspace and its own question. It may use the
Article 2 candidate and this durable record as historical evidence, but must not
restore or treat the removed Article 2 working files as authority.
