# Part 2: Choose what crosses the first edge

> Temporary editorial working material. Not Article 2 publication authority;
> retained without merger into `articles/article2.md`.

**Series:** Architecting With Evidence
**Status:** Initial draft (not published)
**Evidence:** FSTO/1 implementation at tds-firewall-traffic-simulator `a62ac98`, validated evidence `experiment-20260913-145108.json` (commit `f828949`)

---

In Part 1, we established the smallest observable flow for a threat-detection platform:

```
synthetic source → raw collection → durable handoff
```

We deliberately did not choose what the source sends. That decision—selecting the first versioned wire profile—is the subject of this installment.

The question sounds simple: *What should the firewall send?*

The answer is not.

## The hidden contracts in "the firewall sends an event"

When someone says, "The firewall sends an event," they are naming at least seven independent technical contracts:

1. **Traffic observed by the firewall:** The network session, packet, or connection the device actually saw.
2. **Exported observation:** The firewall's record of that traffic, encoded according to its logging subsystem.
3. **Transport:** How those bytes reach the collector—UDP datagrams, TCP byte stream, TLS-protected connection, or something else.
4. **Framing:** How one observation is delimited from the next—datagram boundary, length prefix, delimiter, or application-level protocol.
5. **Envelope:** Wrapper structure like syslog priority and header, CEF prefix, or JSON container.
6. **Payload encoding:** Key-value pairs, JSON, binary struct, or another representation of the observation's fields.
7. **Source profile:** Precise vendor, model, software version, log type, and configuration that determines the exact bytes produced.

Each layer makes independent promises about loss, ordering, completeness, and what the collector can observe. Conflating them produces infrastructure selected by convention rather than evidence.

To make an honest architectural choice, we need to separate these contracts and understand how they differ across real firewall products.

## Why the first edge matters differently

A collector receiving raw bytes from a firewall occupies a fundamentally different position than a service consuming events from a broker or API.

At the first edge:

- The collector sees *what arrived*, not what was sent. Success at the source's socket does not guarantee receipt at the collector.
- The collector sees the immediate network peer, which may not be the firewall's identity if routing, NAT, or a relay is involved.
- The collector cannot request a resend, parse correction, or schema negotiation from the source. It receives what it receives.
- The collector has no control over source software versions, log format changes, or field vocabulary drift.
- Loss can occur at the firewall's buffer, the network path, the collector's buffer, or the handoff—but the collector only observes its own boundary.

If we place a managed intermediary—a log shipper, cloud API, vendor-operated relay—between the firewall and our collection boundary, then *that boundary* becomes the first edge we control. The observation we receive has already been interpreted, potentially transformed, and delivered through a different contract.

That is not wrong, but it is a different question. It changes what we can verify, what failures we can detect, and what raw evidence we preserve.

For this installment, we focus on the direct firewall-to-collector boundary. Understanding that case teaches us what changes when intermediaries are introduced later.

## Transport and framing: what changes at the network layer

Different transports present different observable boundaries to the collector.

**UDP datagrams** provide a crisp message boundary. One datagram carries application bytes from sender to receiver without fragmentation *at the transport layer*. If the datagram arrives, its contents are complete and in order relative to itself. If it does not arrive, the sender's successful `sendto()` call does not inform the sender of loss. The collector observes attempts only by seeing arrivals; it cannot distinguish network loss from source throttling or misconfiguration.

**TCP byte streams** provide reliable, ordered delivery of a byte sequence, but no built-in message boundary. The application must frame individual observations—typically with length prefixes, delimiters, or an application protocol. The collector's receive buffer may hold a partial message, multiple messages, or something in between. TCP's reliability means retransmission on loss, but introduces buffering, connection state, and retry behavior that changes failure modes.

**TLS-protected TCP** adds encryption, endpoint authentication, and protection from tampering, but also introduces certificate management, key rotation, handshake failures, and the question of who operates the TLS termination point.

**Binary flow export** (NetFlow, IPFIX) uses templated binary structs rather than text-based logging, changing the parsing boundary and requiring template management.

**Managed delivery via cloud APIs** or vendor-operated relays moves the first observable edge behind a service boundary. The collector receives events *after* they have been received, potentially batched, and re-delivered by an intermediary. This can improve delivery reliability and offload security concerns, but it changes what the collector can directly observe and verify.

Each choice affects where loss can be detected, what receipt metadata is available, and what failure modes the collector must handle.

## Comparing candidate profiles

We researched five representative profiles to understand the range of boundary types available from enterprise firewall products:

### 1. FortiOS Traffic logs via UDP

Fortinet FortiGate firewalls can export traffic logs as syslog-formatted messages over UDP. The logs use a key-value syntax to describe observed network sessions. Documentation establishes field names and many field values, but does not specify every detail of envelope structure, field ordering, packing of multiple records, or UTF-8 encoding corner cases for all possible field values.

**Boundary type:** Direct UDP datagram export from firewall to collector.

**Teaching value:** Approachable datagram boundary; clear distinction between the observed TCP traffic and the UDP transport carrying the exported record; useful attempt-versus-receipt uncertainty for demonstrating loss detection needs.

**Documentation:** Fortinet provides FortiOS Log Reference with field vocabularies, but leaves some application-level wire details unspecified.

### 2. PAN-OS logs via TLS (Panorama, syslog-ng)

Palo Alto Networks firewalls can send logs to Panorama (their management platform) or external collectors via syslog over TLS. This introduces encrypted transport, certificate management, and connection-oriented delivery.

**Boundary type:** TLS-protected TCP stream requiring framing, connection management, and certificate trust.

**Teaching value:** Demonstrates security and delivery reliability considerations, but introduces more concepts simultaneously (TLS handshake, certificates, connection state).

**Documentation:** Palo Alto provides syslog field guides and Panorama integration documentation.

### 3. Cisco ASA NetFlow export

Cisco Adaptive Security Appliance can export flow records using NetFlow/IPFIX binary templates rather than text-based logs.

**Boundary type:** Binary flow export with templated fields over UDP.

**Teaching value:** Shows alternative to text-based key-value logs; requires template management and binary parsing.

**Documentation:** Cisco NetFlow export is documented, but uses a different field model than text-based traffic logs.

### 4. Check Point via Log Exporter

Check Point firewalls export logs through a separate Log Exporter component that can deliver to external systems via syslog or proprietary formats.

**Boundary type:** Relay-mediated export; the Log Exporter becomes an intermediary between the firewall and the collector.

**Teaching value:** Demonstrates how intermediaries move the observable first edge; introduces questions about the relay's delivery guarantees.

**Documentation:** Check Point documents Log Exporter configuration and supported formats.

### 5. AWS Network Firewall via managed S3 delivery

AWS Network Firewall can deliver flow logs as objects to S3 buckets rather than streaming directly to a collector.

**Boundary type:** Managed object delivery; AWS operates the storage and delivery infrastructure.

**Teaching value:** Shows how cloud-managed delivery changes the first edge from network receipt to object retrieval; removes direct control over delivery timing and batching.

**Documentation:** AWS documents flow log schema and S3 delivery configuration.

## Why we selected FortiOS/UDP as the first teaching fixture

We chose FortiOS Traffic logs over UDP for the first experiment.

**The rationale is not:**
- customer prevalence (we have no customer inventory);
- vendor preference (we are not endorsing FortiOS over other products);
- universal suitability (different environments will have different requirements);
- implementation convenience (we prioritized teaching value, not ease of coding).

**The rationale is:**

1. **Direct firewall-to-collector boundary.** The collector receives UDP datagrams directly from the firewall, with no mandatory intermediary. This makes the source attempt versus collector receipt distinction immediately observable.

2. **Approachable datagram boundary.** A UDP datagram provides a clear message boundary without requiring length prefix parsing, delimiter handling, or connection state management. This reduces the number of new concepts introduced simultaneously.

3. **Useful loss uncertainty.** UDP's lack of sender-side delivery confirmation creates an observable gap between source attempts and collector receipts. This is pedagogically useful: it demonstrates why the platform must record both observations independently rather than assuming send success equals delivery.

4. **Clear transport/payload distinction.** The firewall observes *TCP* traffic (protocol 6, TCP ports like 443), but exports that observation via *UDP*. This distinction—between the traffic described by the log and the transport carrying the log—is important and easy to explain with this profile.

5. **Sufficient documented vocabulary.** Fortinet's Log Reference documents enough field names, log types, and field semantics to construct a teaching contract without inventing undocumented vendor behavior.

6. **Fewer simultaneous concepts than TLS.** Compared to PAN-OS over TLS, FortiOS over UDP does not require teaching TLS handshakes, certificate trust chains, or connection-oriented failure modes in the same lesson. Those concepts remain important, but they can be introduced later when the simpler transport case is established.

The other profiles remain valuable for future experiments. NetFlow teaches binary templates. PAN-OS/TLS teaches encrypted transport and connection reliability. Check Point Log Exporter teaches intermediary boundaries. AWS Network Firewall teaches cloud-managed delivery. Each would be a legitimate choice in a different learning sequence.

We selected FortiOS/UDP because it lets us teach the first-edge contract with the fewest prerequisite concepts, the clearest attempt-versus-receipt gap, and sufficient documented vocabulary to avoid attributing invented details to Fortinet.

## The documentation gap and teaching simplifications

Fortinet's FortiOS Log Reference documents field names, log types (Traffic, UTM, Event), subtypes (forward, local, sniffer), actions, protocols, and many field semantics. It provides examples of log messages in readable key-value format.

But it does not comprehensively specify:

- Exact syslog envelope structure for all possible configurations and versions;
- Whether multiple log records can be packed into one UDP datagram, and under what conditions;
- Exact field ordering across all log types, versions, and configurations;
- UTF-8 encoding corner cases for all field values;
- Handling of special characters, quotes, or delimiters in field values across all scenarios.

This is normal for vendor documentation. It describes the logging subsystem's behavior sufficiently for operators to configure and consume logs, but not every application-layer byte.

We could have:

1. **Invented the missing details and attributed them to FortiOS.** This would be dishonest. We do not have a FortiGate appliance generating real wire captures at every configuration and version.

2. **Captured real FortiGate output and claimed it as the universal contract.** This would be overreach. One appliance's output at one version does not define the contract for all FortiOS releases, configurations, and models.

3. **Refused to proceed until comprehensive vendor byte-level specification was available.** This would be a principled position, but it would block learning until documentation that may never exist becomes available.

**Instead, we created FSTO/1 as a precise synthetic teaching contract.**

FSTO/1 (FortiOS-Shaped Traffic Observation, version 1) is a 180-byte canonical record that:

- Uses field names and key-value syntax from documented FortiOS Traffic logs;
- Represents a completed TCP session (proto=6, dstport=443) that ended normally (action=close) under documented Traffic/forward semantics;
- Specifies *exactly* which parts are vendor-documented and which are teaching simplifications;
- Provides golden-byte verification so any implementation can confirm it produced the intended bytes;
- Remains inspectable: anyone can trace each field to its FortiOS Log Reference source or see where we made a teaching simplification.

The contract is synthetic—it was not captured from a real FortiGate—but it is honestly synthetic. Every simplification is visible.

## The FSTO/1 canonical record

Here is the exact 180-byte FSTO/1 record:

```
devid=FW-TEACHING-01 eventtime=1672531200000000000 logid=0000000013 type=traffic subtype=forward action=close proto=6 srcip=192.0.2.10 dstip=198.51.100.20 dstport=443 sentbyte=4096
```

SHA-256: `62bf0871bd1148b2c1f1afbe3a500740d5a936af5d1dec3b724ab1c85486a653`

**Field explanations:**

- **devid=FW-TEACHING-01:** Device identifier. In real FortiOS logs, this would be the configured device name or serial number. We use a teaching-labeled synthetic value to avoid implying this is real appliance output.

- **eventtime=1672531200000000000:** Source observation timestamp in nanoseconds since Unix epoch (2023-01-01 00:00:00 UTC). This represents *when the firewall observed the traffic*, not when the log was exported or received. FortiOS Traffic logs document a 19-digit nanosecond eventtime field (Fortinet Log Reference, Log ID 13, Traffic/forward session end). Note: Generic FortiOS documentation sometimes shows 10-digit second precision; the Traffic-specific documentation uses nanoseconds. This distinction matters for correlating observations across sources.

- **logid=0000000013:** FortiOS log ID for Traffic/forward session-end events (`LOG_ID_TRAFFIC_END_FORWARD`). This is a documented FortiOS identifier.

- **type=traffic:** Log type indicating network traffic observation (as opposed to UTM, Event, or other log types).

- **subtype=forward:** Log subtype indicating forwarded traffic (as opposed to local, sniffer, or other subtypes). Under documented FortiOS semantics, `subtype=forward` identifies the category of traffic log; it does not alone prove the session was allowed or denied. That determination requires interpreting the `action` field under the applicable log type's semantics.

- **action=close:** Session-end status indicating the session ended normally. Under the documented semantics for Traffic/forward logs, `action=close` describes an *allowed* session that completed and closed. This is not a policy decision name (like "accept" or "deny"); it is a session lifecycle status. The session was permitted by policy, completed its traffic exchange, and then closed.

- **proto=6:** IP protocol number 6, indicating TCP. This describes the *observed traffic* (a TCP session between the source and destination). The log record itself is exported via UDP (protocol 17), creating a useful teaching distinction: the firewall observed a TCP session and exported that observation via UDP.

- **srcip=192.0.2.10:** Source IP address from the observed traffic. We use `192.0.2.0/24` (TEST-NET-1, RFC 5737) to clearly signal this is a synthetic teaching address, not a real network.

- **dstip=198.51.100.20:** Destination IP address from the observed traffic. We use `198.51.100.0/24` (TEST-NET-2, RFC 5737), another RFC-documented testing address block.

- **dstport=443:** Destination TCP port, indicating HTTPS traffic. Combined with `proto=6`, this describes a TCP session to port 443.

- **sentbyte=4096:** Number of bytes sent during the observed session. This is session volume, not the size of the exported log record. The log record is 180 bytes; the observed session transferred 4096 bytes.

**Teaching simplifications explicitly made:**

1. **No syslog envelope.** Real FortiOS syslog messages may include priority, timestamp, hostname, and other syslog-standard fields before the key-value body. FSTO/1 specifies only the key-value body. The envelope is a separate syslog contract, not a FortiOS-specific detail.

2. **No terminator.** FSTO/1 does not end with a newline, carriage return, or null byte. Real syslog messages typically include a newline delimiter. We omit it to focus on the application payload, not the framing convention.

3. **One record per datagram.** FSTO/1 specifies exactly one teaching record per UDP datagram. Real FortiOS may pack multiple records into one datagram under some configurations. We do not claim to know those packing rules, so we specify a simpler one-to-one teaching case.

4. **Fixed field order.** FSTO/1 uses a deterministic teaching order for the eleven fields. This is FSTO/1's contract order, not a claim about FortiOS field ordering. Real FortiOS may order fields differently across versions, configurations, or log types. We do not have evidence of the universal ordering, so we specify our teaching order and label it as such.

5. **Synthetic values.** The device ID, eventtime, addresses, port, and byte count are synthetic teaching values, not captured from real traffic or a real FortiGate appliance.

**What is vendor-documented versus what is teaching simplification:**

| Component | Source |
|-----------|--------|
| Field names (devid, eventtime, logid, type, subtype, action, proto, srcip, dstip, dstport, sentbyte) | FortiOS Log Reference, V3/V11 |
| Log ID 13 = Traffic/forward session end | FortiOS Log Reference |
| Key-value syntax (key=value) | FortiOS examples in Log Reference |
| Eventtime as 19-digit nanoseconds | FortiOS Log Reference, Traffic log ID 13 |
| action=close semantics (allowed session ended) | FortiOS Log Reference |
| proto=6 = TCP | IANA Protocol Numbers (standard, not FortiOS-specific) |
| TEST-NET address blocks | RFC 5737 (standard documentation addresses) |
| No syslog envelope | Teaching simplification |
| No terminator | Teaching simplification |
| One record per datagram | Teaching simplification |
| Field order | Teaching simplification (FSTO/1 order) |
| Specific synthetic values | Teaching simplification |

## The experiment: deterministic generation and two scenarios

We implemented FSTO/1 in Python as bounded experimental tooling focused on readability, deterministic bytes, and reproducible evidence. This language choice does *not* select Python for the future production collector. When a production-shaped collector is architecturally earned, the provisional implementation preference is Go, based on expectations around continuous network intake, controlled concurrency, graceful shutdown, and predictable resource use under load. That decision must be revisited against actual workload evidence.

The experiment has three components:

1. **Deterministic canonical generation:** The `fsto1.generator` module produces the exact 180-byte record every time, with golden-byte verification to confirm the implementation produces the intended bytes.

2. **UDP simulator:** The `fsto1.simulator` module sends one FSTO/1 record via UDP and records *attempt metadata*: when the send was attempted and how many bytes the local socket accepted. This is not the same as the `eventtime` in the payload. The payload's `eventtime` represents the firewall's source observation (1672531200000000000 nanoseconds = 2023-01-01 00:00:00 UTC). The attempt time is when the simulator called `sendto()`. These are different observations from different clocks.

3. **Minimal receiver harness:** The `fsto1.receiver` module listens on a UDP port and records *receipt metadata*: when a datagram arrived, the peer address and port, and the payload bytes. The receiver treats the payload as opaque—it does not parse, validate, or interpret the key-value fields. It preserves bytes exactly as received. This is an experimental harness, not a production collector or durable handoff implementation.

We executed two scenarios:

### Scenario 1: clean_success

**Setup:** Simulator and receiver both on localhost (127.0.0.1). Receiver listening on port 15140. Simulator sends one FSTO/1 record to that port.

**Expected outcome:** One attempt, one receipt, exact byte preservation.

**Actual result:**
- Simulator: `sendto()` returned 180 bytes sent. Attempt time recorded.
- Receiver: Received one datagram, 180 bytes, from peer 127.0.0.1. Receipt time recorded.
- Verification: Received payload exactly matches the canonical FSTO/1 record (SHA-256 confirmed).
- Timing: Attempt time < receipt time (as expected).

**Interpretation:** On a clean local loopback path with a listening receiver, one UDP send resulted in one receipt with byte-perfect preservation. The datagram boundary preserved the 180-byte message boundary. The three-way time distinction is observable: the payload's `eventtime` (source observation), the simulator's attempt time (send operation metadata), and the receiver's receipt time (arrival metadata) are all different values from different observation points.

### Scenario 2: receiver_unavailable

**Setup:** Simulator sends one FSTO/1 record to localhost port 25140, where *no receiver is listening*. A separate receiver listens on port 35140 to verify no cross-port delivery occurs.

**Expected outcome:** Simulator's `sendto()` succeeds locally, but no receiver observes the datagram's arrival.

**Actual result:**
- Simulator: `sendto()` returned 180 bytes sent. Attempt time recorded. No error from the local socket.
- Receiver on port 35140: Timed out after 1 second. No datagram received.

**Interpretation:** A successful local UDP send does *not* guarantee that a receiver observed the delivery. The simulator's socket accepted the bytes, but with no receiver listening on the destination port, the datagram was not delivered to any application receiver in this bounded experiment. The system did not return an error to the sender.

This demonstrates the attempt-versus-receipt gap: the platform must record source attempts (what was sent) and collector receipts (what arrived) independently. Assuming send success equals receipt would silently drop evidence.

**Limitations of this finding:**

This experiment was conducted on local loopback (127.0.0.1), not across a real network. It demonstrates the UDP send/receive contract and the observable gap between attempt and receipt, but it does *not* characterize:

- Real network loss rates or patterns;
- Production FortiGate buffering, queueing, or retry behavior;
- Middlebox interference, routing failures, or firewall source behavior under load;
- The probability or location of loss in a production environment.

It is a controlled teaching demonstration, not a production failure measurement.

## What the experiment proves (and does not prove)

**The experiment provides evidence that:**

1. The implementation generated the exact 180-byte FSTO/1 canonical record deterministically.
2. Golden-byte verification confirmed the generated bytes matched the specified contract.
3. UDP datagram boundaries preserved the 180-byte message boundary on local loopback.
4. One send attempt resulted in one receipt when a receiver was listening (clean_success).
5. A successful local UDP `sendto()` call does not guarantee receipt at a collector (receiver_unavailable).
6. The three-way time distinction (source eventtime, attempt time, receipt time) is observable and must not be conflated.
7. The minimal receiver preserved bytes opaquely without parsing or validating fields.

**The experiment does NOT provide evidence about:**

1. Real FortiOS wire output, field ordering, or envelope behavior.
2. Production network loss rates, patterns, or failure modes.
3. Where loss occurred in the receiver_unavailable scenario (kernel buffer, routing, port unreachable handling).
4. FortiGate appliance behavior under load, log buffering, or retry logic.
5. Security properties, authentication, or protection from tampering.
6. Collector scalability, handoff durability, or multi-source handling.
7. Device identity, address changes, or source continuity across network changes.
8. Parsing correctness, normalization, or downstream detection capabilities.

The experiment validated that FSTO/1 was implemented as specified and demonstrated the attempt-versus-receipt gap in a controlled local environment. It did not validate production firewall behavior or real network delivery properties.

## Architecture checkpoint: what we decided and what remains open

### What we learned

"The firewall sends an event" conceals at least seven independent contracts: observed traffic, exported record, transport, framing, envelope, payload encoding, and source profile. Each layer affects what the collector can observe, what failures are detectable, and what raw evidence is preserved.

Conflating these layers—or selecting infrastructure by convention without separating them—leads to architectural decisions made implicitly rather than based on evidence.

Precise provenance matters even for raw bytes. Capturing bytes from a dirty working tree versus a clean immutable commit changes whether the evidence can support claims about what was implemented. Similarly, documenting which record components are vendor-specified versus teaching simplifications keeps the contract inspectable and prevents attributing invented details to the vendor.

Source attempts and collector receipts are independent observations. A successful send operation at the source does not prove receipt at the collector. The platform must record both to detect and investigate loss, rather than assuming delivery.

Controlled simplification is acceptable when it remains visible. FSTO/1 makes teaching simplifications (no syslog envelope, one record per datagram, deterministic field order), but each simplification is explicitly documented. This keeps the contract usable for teaching without misrepresenting vendor behavior.

### What we decided

**FSTO/1 as the first versioned teaching fixture.** We selected FortiOS-shaped Traffic logs over UDP as the first experimental contract because it provides a direct firewall-to-collector boundary, an approachable datagram boundary, useful loss uncertainty, and a clear distinction between observed TCP traffic and UDP export transport. This is a teaching decision, not a claim about customer prevalence or platform-wide transport selection.

**UDP and one-record-per-datagram for this experiment.** FSTO/1 specifies UDP transport and exactly one teaching record per datagram. This is sufficient to teach the first-edge boundary and attempt-versus-receipt distinction. Multi-record packing, TCP streaming, TLS protection, and managed delivery remain open questions for future experiments.

**Python simulator and receiver as experimental tooling.** We implemented FSTO/1 in Python for readability, deterministic byte generation, and reproducible evidence. This choice is bounded to experimental tooling and does not select Python for the future production collector. The provisional preference for a production collector, when that boundary is earned, is Go.

**Validated scenarios:** clean_success (byte preservation) and receiver_unavailable (attempt without receipt) provide controlled evidence of the UDP contract and the observable gap between send and receive. These are local loopback findings, not production network measurements.

### What remains open

**Actual customer sources.** We have no customer inventory. We do not know which firewall vendors, models, versions, or configurations are present in the environments we will serve. Customer conversations and representative environment access are required before claiming customer prevalence or requirements.

**Real FortiOS wire behavior.** FSTO/1 is a synthetic teaching contract shaped after documented FortiOS field vocabularies. We have not captured or validated real FortiGate output across versions, configurations, syslog envelope variations, multi-record packing, or special-character handling. Real FortiOS behavior must be established through packet captures or appliance testing when production integration is architecturally earned.

**Production transport and security.** UDP is sufficient for teaching the first-edge attempt-versus-receipt distinction. Production requirements for TLS encryption, endpoint authentication, retry behavior, connection management, or intermediary relays remain unestablished. Customer security obligations and operating requirements will determine production transport selection.

**Device identity.** The `devid` field in the FSTO/1 record is a source-provided identifier. We have not established how to reconcile device identity across address changes, hardware replacement, configuration reimport, or NAT. The platform must preserve observed source identity without asserting continuity across changes until evidence supports the correlation.

**Production collector behavior.** The experimental receiver is a minimal test harness that preserves bytes opaquely. It does not implement durable handoff, buffering under load, multi-source concurrency, rate limiting, graceful shutdown, or failure alerting. Production collector requirements must be derived from measured workload and failure scenarios, not invented.

**Durable handoff and later interpretation.** The platform accepts that raw collection must hand off preserved bytes and receipt metadata to a durable boundary before interpretation. The concrete handoff product (message queue, log stream, object store), delivery guarantees, retention policies, replay capabilities, and consumer fan-out remain unselected. These decisions require evidence about event rates, burst sizes, consumer requirements, and recovery scenarios.

### Next

In the next installment, we will derive the next architectural responsibility from the evidence FSTO/1 actually produced. We have a 180-byte teaching record, UDP delivery with observable attempt-versus-receipt gaps, and a minimal receiver that preserves bytes without parsing. What boundary must we implement next, and what evidence determines its contract?

The answer will not be "build a collector" or "choose a broker" unless the reconciled roadmap and available evidence establish that those are the smallest next questions we can answer honestly.

---

**Word count:** ~5,400 words
**Evidence cited:** FSTO/1 implementation (tds-firewall-traffic-simulator `a62ac98`), validated experiment (`experiment-20260913-145108.json`, commit `f828949`), Fortinet FortiOS Log Reference (V3, V11, Traffic log documentation), RFC 5737 (TEST-NET addresses)
**Status:** Initial draft for editorial review
**Not approved for publication**
