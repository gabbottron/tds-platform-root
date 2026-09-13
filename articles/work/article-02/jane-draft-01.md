What Crosses the First Edge?

In Part 1, we gave the architecture somewhere to remember what we know, what we have decided, and what remains uncertain.

We also established the smallest useful flow for the threat-detection platform:

reproducible firewall-shaped source → raw collection → durable handoff

That flow was deliberately transport-neutral. We had not selected a firewall, a payload family, or even whether the first bytes would arrive in UDP datagrams or a TCP stream.

Now the diagram has to meet the wire.

The next question appears simple: What should cross the first source-to-collector edge?

The tempting answer is “a firewall event.” But that phrase hides nearly every decision that matters.

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

The distinctions are not academic. UDP preserves a datagram boundary but does not confirm application receipt. TCP provides an ordered byte stream but does not identify message boundaries. TLS protects a connection, but a TLS record is not automatically one firewall record. A vendor relay or cloud delivery service may retry and batch data, but then the first boundary we observe is the intermediary rather than the firewall.

A collector cannot repair these distinctions after the fact. If it records only a parsed interpretation, we may lose the original evidence. If it treats a network peer address as device identity, a relay or address change can corrupt attribution. If it assumes that one socket read equals one source record, a TCP implementation may silently split or combine messages.

Before choosing infrastructure, we therefore needed a source contract precise enough to test.

The available surface is wider than syslog over UDP

Firewall telemetry does not have one universal delivery shape. We compared five profiles because each moves the observable boundary in a materially different way.

Candidate

Delivery shape

Architectural pressure it exposes

FortiOS Traffic logs

Direct key-value export over UDP

Datagram boundaries and send-versus-receipt uncertainty

PAN-OS Traffic logs

Direct syslog over TLS/TCP

Stream framing, connection state, certificates, and TLS termination

Cisco ASA flow export

Binary NetFlow records over UDP

Templates, binary decoding, and schema state

Check Point Log Exporter

Firewall records delivered through a vendor log server

An intermediary becomes the immediate source

AWS Network Firewall logs

Managed stream or object delivery

Batching, service guarantees, and a cloud-controlled boundary

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

Its canonical representation is exactly 180 bytes:

devid=FW-TEACHING-01 eventtime=1672531200000000000 logid=0000000013 type=traffic subtype=forward action=close proto=6 srcip=192.0.2.10 dstip=198.51.100.20 dstport=443 sentbyte=4096

Its SHA-256 digest is:

62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653

The record describes a TCP session to destination port 443. Protocol number 6 means TCP in the IANA protocol registry. The two addresses come from the documentation-only ranges reserved by RFC 5737. sentbyte=4096 describes bytes sent during the observed session; it does not describe the 180-byte export record.

Several fields carry particularly important lessons:

Field

What it tells us

What it does not tell us

devid=FW-TEACHING-01

The source-shaped record makes an identity claim

That the network peer is the same device, or that identity remains stable

eventtime=1672531200000000000

When the source says the session ended

When the simulator attempted export or when a receiver observed arrival

action=close

Under the applicable session-end semantics, the allowed session closed normally

That close is a general synonym for “allow”

proto=6 and dstport=443

The observed traffic was a TCP session to port 443

That the log itself travelled over TCP

sentbyte=4096

A source-reported session volume

The size of the exported record

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

In clean_success, the receiver listened on 127.0.0.1:15140. The simulator attempted one send, the receiver observed one 180-byte datagram, and the received bytes matched the canonical digest. This demonstrated deterministic generation and byte preservation on that run.

In receiver_unavailable, the simulator sent the same 180 bytes to 127.0.0.1:25140, where no application was listening. The local sendto() call succeeded, but no receiver receipt existed for the run.

That second result is modest but important: a successful local UDP write is evidence of an attempt, not evidence of application receipt.

It does not tell us where data would be lost in production. It does not measure network-loss rates, FortiGate buffering, retry behavior, middleboxes, or load. The experiment took place on local loopback. Its purpose was to make the epistemic boundary observable, not to simulate the internet.

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

A locally successful UDP send does not prove that an application received anything.

What we decided

FSTO/1 is the first versioned teaching fixture.

This experiment uses a FortiOS-shaped Traffic/forward session-end record, UDP, and one record per datagram.

The exact 180-byte representation and its declared simplifications are the experiment contract.

The Python receiver remains a bounded harness, not the production collection service.

What remains open

We still have no customer source inventory. We have not verified real FortiGate wire output, production loss behavior, transport security requirements, device-identity continuity, collector throughput, or the durable-handoff contract. TCP framing, TLS, binary flow export, relays, and managed delivery remain legitimate future profiles rather than rejected designs.

Those omissions are not cleanup work hidden behind the experiment. They define its boundary.

The platform now has exact bytes it can generate, an explicit record of where those bytes came from, and evidence that attempt and receipt cannot be treated as the same observation.

The next responsibility must be derived from that evidence. We should not build a collector, choose a broker, or invent a platform envelope merely because those components are conventional. We should ask what must survive after receipt, what failure would otherwise destroy it, and which smallest boundary can make that claim testable.

That is the next edge the architecture has to earn.

Evidence checkpoint: FSTO/1 implementation a62ac98; validated experiment experiment-20260913-145108.json at f828949.

Publication status: Editorial review; not yet approved for publication.
