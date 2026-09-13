---
title: "Architecting With Evidence, Part 2: What Crosses the First Edge?"
record_type: unpublished-publication-candidate
publication_status: editorial-review-not-approved
publication_authority: articles/article2.md
evidence_record: articles/evidence/article-02.md
---

What Crosses the First Edge?

In Part 1, we gave the architecture somewhere to remember what we know, what we have decided, and what remains uncertain.

We also established the smallest useful flow for the threat-detection platform:

reproducible firewall-shaped source → raw collection → durable handoff

That flow was deliberately transport-neutral. We had not selected a firewall, a payload family, or even whether the first bytes would arrive in UDP datagrams or a TCP stream.

Now the diagram has to meet the wire.

The next question appears simple: What should cross the first source-to-collector edge?

The tempting answer is “a firewall event.” But that phrase hides nearly every decision that matters.

In this installment

We will separate the contracts hidden inside that phrase, compare five materially different firewall-export profiles, and explain why we selected one for the first experiment. Then we will define an exact 180-byte synthetic teaching record and test what a UDP sender and receiver can actually establish.

The result is not a production transport decision or a universal firewall format. It is the smallest defensible contract we can use to make the first edge observable.

A firewall event is not one thing

Imagine a user opening a website through a firewall. The application traffic may use TCP. The firewall observes the session and later creates a traffic record describing it. Its logging subsystem encodes that record, may wrap it in a syslog envelope, and transports the resulting bytes to another system.

Those are related events, but they are not the same event.

“The firewall sends a log” can conceal at least seven separate contracts:

Observed traffic: the packet, connection, or session the firewall saw.

Exported observation: the record the firewall created about that traffic.

Transport: how the exported bytes move, such as UDP, TCP, or TLS over TCP.

Framing: how the receiver knows where one record ends and another begins.

Envelope: any wrapper around the vendor payload, such as a syslog header.

Encoding: the representation of the payload—key-value text, CSV-like text, JSON, or binary fields.

Source profile: the vendor, software version, log family, and configuration that produced it.

Optional refresher: records, messages, datagrams, and streams

If these distinctions are already familiar, continue to “The available surface is wider than syslog over UDP.”

A record is the firewall’s description of something it observed. A message is the application-level representation sent to another system. A UDP datagram provides one transport boundary for those bytes, while one or more IP packets carry the datagram across the network.

TCP instead gives the receiver a continuous stream of bytes. The application must determine where one message ends and another begins. A single read may contain part of a message, exactly one message, or several messages together.

In our experiment, one FSTO/1 record becomes one application message carried in one UDP datagram. That is a rule of our teaching contract, not a claim about every firewall.

The distinctions are not academic. UDP preserves a datagram boundary but does not confirm application receipt. TCP provides an ordered byte stream but does not identify message boundaries. TLS protects application data on a configured hop, but a TLS record is not automatically one firewall record. A vendor relay or cloud delivery service may retry and batch data, but then the first boundary we observe is the intermediary rather than the firewall.

A collector cannot repair these distinctions after the fact. If it records only a parsed interpretation, we may lose the original evidence. If it treats a network peer address as device identity, a relay or address change can corrupt attribution. If it assumes that one socket read equals one source record, a TCP implementation may silently split or combine messages.

Before choosing infrastructure, we therefore needed a source contract precise enough to test.

The available surface is wider than syslog over UDP

Firewall telemetry does not have one universal delivery shape. We compared five profiles because each moves the observable boundary in a materially different way.

FortiOS Traffic logs can provide direct key-value export over UDP. This exposes datagram boundaries and the uncertainty between a local send and application receipt.

PAN-OS Traffic logs can travel over TLS-protected TCP. This introduces stream framing, connection state, certificates, and the question of where the encrypted connection ends and the bytes are decrypted.

Cisco ASA flow export uses binary NetFlow records. Some NetFlow versions send templates that tell the receiver how later bytes should be interpreted, so receiving a data record may not be enough if the required template is missing or stale.

Check Point Log Exporter places a vendor log server between the firewall and our system. The intermediary—not necessarily the original firewall—becomes the immediate source we can observe.

AWS Network Firewall can use managed stream or object delivery. The first boundary we control then occurs after AWS has received, potentially batched, and delivered the records according to a service contract.

Optional refresher: syslog is not one transport

If syslog transport and framing are already familiar, continue to the profile selection below.

Syslog is a family of conventions for packaging and transporting operational messages. Saying that a firewall “uses syslog” does not tell us whether its messages travel in UDP datagrams, across a TCP connection, or through a connection protected by TLS.

It also does not necessarily tell us how messages are separated. A datagram can provide a boundary. A TCP stream still needs an application rule such as a delimiter or length prefix. TLS protects bytes while they travel, but a TLS record is not automatically one firewall message.

The envelope is the wrapper around a vendor’s payload. Framing tells the receiver where the message ends. Encoding determines how the payload itself is represented. These are separate contracts even when people casually call the whole combination “syslog.”

None is the universal firewall contract. More importantly, our repository contains no customer inventory that could justify a prevalence claim. Article 1’s customer scenarios were deliberately hypothetical. We cannot turn them into evidence merely because one product is familiar or common in our experience.

For the first teaching experiment, we selected a narrow FortiOS profile: FortiGate/FortiOS 7.4.8, its fourth remote-syslog target, UDP mode, default format, and the Traffic/forward session-end family identified as LOG_ID_TRAFFIC_END_FORWARD.

This was a sequencing decision, not a vendor endorsement or a production transport decision.

It gave us four useful teaching properties at once:

a direct firewall-to-collector shape, without a mandatory relay;

a visible datagram boundary that does not require us to teach stream framing first;

uncertainty between a successful local send and receiver observation;

and a clean distinction between the TCP session described by the record and the UDP transport carrying the record.

PAN-OS over TLS remains a useful later contrast. It will force questions about trust, encryption, connection recovery, and message framing. Starting there, however, would make several independent problems arrive in the same experiment. We chose the smaller lesson first.

Documentation did not give us an exact wire contract

Selecting the profile exposed the next problem.

Fortinet’s FortiOS 7.4.8 log documentation identifies the Traffic/forward session-end family and documents its fields. Other Fortinet references establish the remote-syslog configuration and key-value examples.

They do not, however, establish every byte we would need to claim one universal FortiOS wire representation. We could not prove a complete default-format syslog envelope, universal field order, character encoding, terminator behavior, or whether every record maps one-to-one to a UDP datagram. Hardware, build, VDOM, filters, and other configuration details can also affect what is emitted.

That left three bad options: invent the missing behavior, present one example as universal, or stop until a specification appeared that may never exist.

Instead, we made the uncertainty part of the contract.

FSTO/1: exact, synthetic, and inspectable

FSTO/1—FortiOS-Shaped Traffic Observation, version 1—is not captured FortiGate output. It is a synthetic teaching record based on documented FortiOS field names and semantics, with every reconstruction and simplification declared.

Optional refresher: canonical bytes and fingerprints

If canonical fixtures and content hashes are familiar, continue to the record itself.

Canonical means we have designated one exact byte sequence as the authoritative teaching example. This is stricter than saying two records look the same on screen: an invisible newline, a different character encoding, or a changed field order would produce different bytes.

The SHA-256 value below acts as a fingerprint. Two implementations can hash what they generated and compare the result with the expected value. Matching fingerprints give us strong evidence that the byte sequences are identical without requiring a person to compare all 180 bytes manually.

Its canonical representation is exactly 180 bytes:

devid=FW-TEACHING-01 eventtime=1672531200000000000 logid=0000000013 type=traffic subtype=forward action=close proto=6 srcip=192.0.2.10 dstip=198.51.100.20 dstport=443 sentbyte=4096

Its SHA-256 digest is:

62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653

The record describes a TCP session to destination port 443. Protocol number 6 means TCP in the IANA protocol registry. The two addresses come from the documentation-only ranges reserved by RFC 5737. sentbyte=4096 describes bytes sent during the observed session; it does not describe the 180-byte export record.

Several fields carry particularly important lessons:

devid=FW-TEACHING-01 is an identity claim made by the source-shaped record. It does not prove that the network peer is the same device or that the identity remains stable over time.

eventtime=1672531200000000000 says when the source claims the session ended. It is not the time when the simulator attempted export or when the receiver observed arrival.

action=close is an allowed-session end status in the applicable FortiOS Traffic/forward log family. It is not a general synonym for “allow” across every log family, and this teaching record does not infer a particular TCP shutdown sequence from it.

proto=6 and dstport=443 describe a TCP session to the port conventionally used for HTTPS. They do not mean that the exported log travelled over TCP.

sentbyte=4096 is a source-reported session volume. It is not the size of the 180-byte export record.

The eventtime field also revealed why versioned, source-specific evidence matters. A generic Fortinet field example shows a ten-digit epoch value, while the applicable Traffic log definition and FortiOS 7.4.8 samples use a 19-digit nanosecond representation. We preserved that conflict and followed the more specific evidence. Silently choosing whichever example looked convenient would make the record precise without making it defensible.

FSTO/1 then makes five explicit teaching simplifications:

it contains the bare key-value body, with no claimed syslog envelope;

it uses printable UTF-8-compatible ASCII bytes;

it has no newline, carriage return, null byte, or other terminator;

it fixes an eleven-field teaching order;

and it sends exactly one teaching record per UDP datagram.

These are FSTO/1 rules, not claims about all FortiOS devices. A future appliance capture or stronger vendor documentation may replace or expand them.

That distinction is the point. “Synthetic” does not have to mean vague. The fixture is exact enough to reproduce while remaining honest about what it represents.

The experiment records three different times

We implemented the contract in a small Python simulator and receiver harness. Python was appropriate for deterministic, readable experimental tooling; the receiver is not a production collector and its performance is not collector evidence.

The implementation records three timestamps from three observation points:

Source observation time is the fixed eventtime inside the payload. It describes when the synthetic firewall-shaped source says the TCP session ended.

Simulator attempt time records when the simulator submitted the datagram to its local UDP socket.

Receiver receipt time records when the experimental receiver observed a datagram.

Collapsing these into a single “event time” would erase useful evidence. The source can describe an old event, the simulator can attempt delivery later, and the collector can only timestamp what reaches its own boundary.

We ran two bounded loopback scenarios.

Evidence boundary: what loopback establishes

Loopback means that the sender and receiver ran on the same computer. The operating system moved the datagram internally; it did not cross a physical network.

This lets us examine the local UDP send-and-receive contract under controlled conditions. It cannot measure production network loss, routing failures, middlebox behavior, or FortiGate buffering and retry behavior. Those remain outside the evidence produced here.

In clean_success, the receiver listened on 127.0.0.1:15140. The simulator attempted one send, the receiver observed one 180-byte datagram, and the received bytes matched the canonical digest. This demonstrated deterministic generation and byte preservation on that run.

In receiver_unavailable, the simulator sent the same 180 bytes to 127.0.0.1:25140 and the local sendto() call succeeded. The harness then waited on a different port, 35140, and observed no cross-delivery there. It did not observe the destination port, so this run does not establish whether an application received the datagram at 25140.

That result is modest but important: a successful local UDP write is evidence of an attempt, not evidence of application receipt. UDP's lack of application acknowledgement is a transport property; this particular run does not measure or locate loss.

Its purpose was to make that epistemic boundary observable, not to simulate the internet.

The implementation passed fifteen tests covering the canonical bytes, deterministic generation, transport behavior, and evidence structure. The validated experiment artifact identifies the clean implementation revision a62ac98; the evidence itself is preserved at revision f828949. That provenance matters because our first run occurred before the implementation was committed. Its results looked correct, but its evidence pointed to a dirty working tree. We reran the experiment from the immutable revision rather than asking readers to trust that the uncommitted files were equivalent.

Evidence-first work includes the provenance of the experiment, not only its output.

What changed in the architecture

Article 1 established a transport-neutral first flow and one governing principle: preserve an observation before assigning it meaning.

This experiment did not turn UDP or FortiOS into platform-wide commitments. It produced something smaller and more useful: the first versioned input contract against which the collection boundary can eventually be tested.

What we learned

“Firewall event” hides independent contracts for observation, export, transport, framing, envelope, encoding, and source profile.

A precise synthetic fixture can be more honest than an unexplained “realistic” sample.

The traffic described in a record and the transport carrying that record are separate protocols.

Source observation, send attempt, and receiver receipt are different facts.

A locally successful UDP send is not application-receipt evidence.

What we decided

FSTO/1 is the first versioned teaching fixture. It gives the collection boundary exact, reproducible bytes. It does not define a universal firewall format or normalized platform event.

The experiment uses UDP with one teaching record per datagram. This is sufficient to expose framing and attempt-versus-receipt uncertainty. It does not select the production transport.

The fixture uses a FortiOS-shaped Traffic/forward session-end record. This gives us documented vocabulary for an approachable first lesson. It does not establish customer prevalence or vendor superiority.

The simulator and receiver harness are implemented in Python. This keeps the bounded experiment readable and reproducible. It does not choose the language or design of the production collection service.

What remains open

We still have no customer source inventory. We have not verified real FortiGate wire output, production loss behavior, transport security requirements, device-identity continuity, collector throughput, or the durable-handoff contract. TCP framing, TLS, binary flow export, relays, and managed delivery remain legitimate future profiles rather than rejected designs.

Those omissions are not cleanup work hidden behind the experiment. They define its boundary.

What the method changed

We did not begin by choosing UDP or copying a plausible firewall log into a fixture. We separated the contracts hidden inside the first edge, compared representative alternatives, identified where vendor documentation stopped, and made the missing details explicit.

The resulting fixture is synthetic, but it is not arbitrary. Its exact bytes, provenance, simplifications, and experimental limits are inspectable. That is the difference between using an example to illustrate an architecture and using evidence to let an architecture evolve.

Next: define what receipt must preserve

The platform now has exact bytes it can generate and an explicit record of where those bytes came from. Source attempts and receiver receipts remain distinct observations; a future collector experiment must measure each at its own boundary.

In Part 3, we will turn to the raw collection boundary. Starting with FSTO/1, we will determine which received bytes and transport observations must survive, distinguish the immediate network peer from the identity claimed inside a record, and define what the collector can honestly say about accepted, rejected, or missing input.

Only then can we decide what durability contract the next boundary must provide. We should not choose a broker or invent a platform envelope merely because those components are conventional.

Reference repositories for this article: tds-platform-root and tds-firewall-traffic-simulator

Evidence checkpoint: FSTO/1 implementation a62ac98; validated experiment experiment-20260913-145108.json at f828949.

About this series: Architecting With Evidence follows the design of an evolving threat-detection platform to explore how humans and coding agents can make disciplined architectural decisions under uncertainty.

Geoffrey Abbott is a senior platform engineer and founder of Northwatch Systems. He writes about production architecture, agentic development, and building systems whose behavior can be explained.

Publication status: Editorial review; not yet approved for publication.
