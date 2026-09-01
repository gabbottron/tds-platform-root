---
title: "Architecting With Evidence, Part 1: Before the First Service"
author: Geoffrey Abbott
published_on: 2026-09-01
published_url: "https://www.linkedin.com/pulse/architecting-evidence-part-1-before-first-service-geoffrey-abbott-eraue/"
checkpoint: article-01
record_type: published-article-transcript
---

This file is a formatting-normalized transcript of the published article. The
LinkedIn publication is the public source; the tagged repository is the
technical checkpoint derived from it.

# Before the First Service: Giving Coding Agents an Architecture They Can Evolve

How to read this series
This series is for experienced engineers, technical leaders, and people using coding agents to work on systems whose requirements cannot be understood all at once.

It is not an attempt to present the optimal threat-detection architecture, and it is not a claim that every team should make the same decisions we make. Real designs depend on customers, deployment environments, existing infrastructure, regulatory obligations, operating capacity, cost, and evidence we do not have yet.

The firewall platform is a field study in how those decisions become knowable.

We will begin with incomplete information, surface realistic operating pressures, and make only the smallest decisions needed to continue. Some early assumptions will survive. Others will be revised when implementation, measurement, or customer conversations show us something new. That revision is not a failure of the method; it is part of the method.

The goal is to make the reasoning visible: which event created a concern, which requirement followed, which alternatives remained plausible, why we chose one response for the current stage, and what evidence could cause us to reconsider it.

The order matters. Identity, replay, retention, scaling, observability, security, and service boundaries will arise naturally as the current slice makes them necessary. We will record important questions when they appear without forcing every one of them into the first implementation.

This is therefore less a blueprint for a firewall product than a study in disciplined architectural judgment—how humans and coding agents can ask better questions, preserve uncertainty, make useful progress, and evolve a complex production system without pretending to know its final shape in advance.

When people talk about building software with coding agents, they often begin with the first task: create a service, add an endpoint, connect a database.

For a system that will eventually span multiple repositories and teams, that may already be too late.

An agent working in one repository can make a locally reasonable change while missing a constraint that lives somewhere else. It can implement the architecture described in a ticket even though the team’s understanding has changed since the ticket was written. It can fill a gap with a plausible assumption and produce clean, tested code for a system nobody actually intended to build.

The problem is not that the agent lacks instructions. The problem is that the architecture has no durable place to remember what the team knows, what it has decided, and what remains uncertain.

We are exploring that problem by building an evolving firewall threat-detection platform. The domain gives us realistic pressure: heterogeneous devices, changing networks, format drift, bursts, loss, sensitive data, independent services, and multiple interpretations of the same activity.

We are not going to design the complete platform in advance. We will begin with what we can defend, record why it is true, and let operating evidence create the next responsibility.

That means the first useful work is not a detection engine. It is deciding what must survive long enough for detection to become possible.

1. Firewall events are observations, not detections
A firewall sees a version of the network that most applications never do.

It sits at a boundary and observes connection attempts crossing it. Depending on the device and its configuration, it may record where a connection came from, where it was going, which protocol and ports it used, how much data moved, which policy applied, and whether the traffic was allowed, denied, dropped, or reset. It may also report signatures, administrative changes, authentication activity, and the health of the firewall itself.

Those records are useful, but they are not yet threat detections. They are observations made by one device at one point in time.A denied connection to a closed port may be harmless background noise. A source address appearing to test hundreds of ports across several protected systems is more interesting, although the address alone may not identify a single actor or host. An allowed connection may be completely ordinary until it is combined with other activity from the same system, user, or time window.

The value comes from preserving individual observations and then examining how they relate to one another.

The challenge is often not obtaining any firewall data. It is turning a large, uneven stream of device-specific records into evidence that other systems can use reliably. Vendors describe similar events with different formats, field names, timestamps, and assumptions. Some records arrive incomplete or malformed. Traffic can remain steady for hours and then burst during an incident or configuration change. Depending on the transport, some input may never reach the platform at all.

Our first principle follows from that uncertainty: Preserve an observation before assigning it meaning.

That does not mean retaining every byte forever. Raw security data creates obligations around access, cost, deletion, and privacy. It means that collection and interpretation should have separate lifecycles. A parser should be able to improve without requiring the collector to reconstruct information it discarded months earlier.

One downstream service may eventually normalize vendor formats. Another may add asset or network context. Others may detect suspicious behavior, retain evidence for investigation, or react when the available evidence crosses a threshold. Those responsibilities do not have to be implemented as one program or scaled as one unit.

But before drawing those services, we need to understand the ordinary changes they must survive.

2. The system has to survive an ordinary Tuesday
Architecture diagrams make infrastructure look more stationary than it is.

A representative customer might have a primary office, several branches, cloud networks, remote workers, and a small security team responsible for all of them. One location may use a different firewall vendor after an acquisition. A high-availability pair may protect the main datacenter. Cloud firewalls may emit different records from appliances at the edge.

The initial integration sounds simple: configure each firewall to send events to an endpoint, confirm that they are arriving, and begin turning them into a common representation.

Then an ordinary Tuesday happens.

Suppose a firewall moves to a different IP address during a network redesign. If source IP is treated as device identity, one device’s history may split in two. If the old address is later assigned to another appliance, two unrelated histories may be combined. The source address is useful transport evidence, but it cannot be the only answer to “which firewall produced this?”

Now suppose the customer upgrades its firmware. Events continue arriving, but one field is renamed, a timestamp changes precision, or a previously unquoted value is now quoted. A parser may reject the record. More dangerously, it may accept the record while interpreting one field incorrectly.

Finally, suppose the appliance is replaced. The customer still thinks of it as “the Boca office firewall” because it serves the same role. Operationally, the old and new appliances are different devices with different models, software versions, capabilities, and lifetimes.

These routine changes expose three distinctions the architecture must not erase:

An address is not a device. A firewall can move to a new network address, and an old address can later belong to something else.
An interpretation is not the original observation. Parsers and formats change. We need to preserve what arrived independently of what today’s parser believes it means.
A deployment role is not a physical appliance. A customer may replace “the Boca office firewall” while expecting the history of that location to remain continuous.

The concepts are related, but they have different lifecycles. Collapsing them may simplify the first implementation while quietly corrupting the history the platform will later depend on.

We do not yet know how device identity should be asserted, how long raw input should remain available, or how corrected interpretations should move through downstream systems. Those are real questions, but naming them does not require us to solve them immediately.

What matters now is preventing convenient assumptions from hardening silently into the design.

This is where agentic development creates a second architectural problem. The same uncertainty that exists in our heads must remain visible when an agent enters a repository days or months later. If the only durable artifact is implementation, the first service will quietly become the architecture by default.

3. Give the architecture somewhere to remember
The first repository in this system will contain almost no application code.

Its first responsibility is to prevent our current understanding from becoming trapped in a conversation, a whiteboard, or whichever service happens to be built first. Once the platform is divided among independently versioned services, no individual service repository naturally owns the truth about how the whole system fits together.

We therefore begin with a platform-root repository. It is authoritative for platform-level intent: the current system model, cross-service boundaries, accepted decisions, recognized risks, and visible unknowns. Future service repositories will remain authoritative for their own implementation, tests, and build instructions.

The distinction matters. The root should describe relationships that no individual service owns without duplicating service internals or pretending that a document can override deployed reality.

It also must not become a speculative master plan. A document that confidently describes services we have not designed is less useful than an incomplete document that tells the truth. The root must distinguish what is implemented, what has been accepted as the next step, what is merely proposed, and what remains unknown.

At the beginning, the repository can remain small:

firewall-platform-root/
├── README.md
├── AGENTS.md
├── ARCHITECTURE.md
├── OPEN-QUESTIONS.md
├── RISKS.md
├── decisions/
    └── 0001-establish-the-platform-root.md
Each file has one job.

README.md explains the customer problem, establishes the repository’s authority, describes the current stage, and points a human or agent toward the next authoritative document. It is an entry point, not a compressed copy of the architecture.

AGENTS.md defines the initial operating contract. Agent work begins by loading the relevant root context, even when the eventual changes occur elsewhere. The agent distinguishes evidence from inference, investigates what the repositories can establish, and surfaces missing facts instead of silently inventing them. When a missing fact could change the correctness or scope of the work, it stops or preserves the issue as an explicit question.

ARCHITECTURE.md holds the current system model and explains why its existing boundaries exist. It labels their maturity and avoids implying that an unimplemented box already has a settled design.

OPEN-QUESTIONS.md makes uncertainty inspectable. Each question records why it matters, which decisions it could affect, what provisional boundary lets work continue, and what evidence would cause the team to answer or revisit it.

RISKS.md serves a different purpose. An open question represents missing knowledge; a risk represents a plausible harmful outcome. We already know that the platform could misattribute a device, lose input during bursts, accept a subtly changed format, or expose sensitive network evidence. Recording those risks does not mean implementing every mitigation now. It prevents future work from behaving as if the risk had never been recognized.

The first architectural decision record explains why the root exists at all. It preserves the context, decision, consequences, and reconsideration conditions—including the synchronization work and drift risk created by maintaining another repository. Future decisions should preserve the reasoning available at the time rather than rewriting history to make the outcome look inevitable.

We expect to need service and contract indexes eventually. We may later need cross-repository revision sets, task records, automation, or specialized agent skills. Creating empty structures for them now would teach us nothing.

A skill should encode a repeated, successful practice. Automating an unclear process only makes inconsistency faster.

The platform root is therefore not administrative ceremony. It is a versioned record of our best current understanding. Its value will be tested every time implementation evidence forces that understanding to change.

Now the root can receive its first concrete design question.

4. Define the smallest observable flow
The first flow should be small enough that we can understand every boundary and large enough to teach us something about operating the platform.

A toy that proves only that one process can call another will not help us reason about customer changes, bursts, missing input, or future recovery. A miniature version of the complete product would bury those questions beneath infrastructure we have not earned.

The useful middle is a vertical slice with one job: move a raw firewall-shaped observation from a reproducible source across a collection boundary into a durable handoff while preserving the information needed to explain what happened.

reproducible firewall-shaped source
                 |
                 | raw transport input
                 v
         raw collection boundary
                 |
                 | received bytes
                 | + available receipt observations
                 v
           durable handoff
This flow is deliberately transport-neutral. We have not yet selected UDP, TCP, a syslog framing convention, a vendor, or a payload family. Those choices will change which failures can be observed, whether pressure can propagate toward the source, what ordering can be expected, and what receipt metadata is available.

The source is synthetic because we need repeatable experiments before we have a customer integration or permission to use real network data. The collector is the first production-shaped boundary. It records the bytes delivered to it and the transport observations available there. It does not decide whether the event is a threat, interpret a vendor format, or construct a canonical platform event.

The durable handoff is still a capability, not a selected product. Its purpose is to separate receipt from later interpretation so downstream failure does not automatically become collector failure. The exact durability, replay, retention, and delivery guarantees remain decisions to earn.

The first acceptance bar is narrow:

A bounded source run can be reproduced from declared inputs.
The collector can preserve the bytes it receives without interpreting them.
Source attempts, collector receipts, and successful handoffs can be compared without claiming that any component knows what happened across the entire path.
Directly observed failures and unresolved discrepancies remain visible.
Shutdown leaves an explainable result rather than silently abandoning in-flight work.

The acceptance bar is about evidence, not liveness. “The process stayed up” is not enough. We need to know what the source attempted, what the collector actually received, what crossed the handoff, what the system directly rejected or lost, and which portions of the path remain unknowable.

Several capabilities remain outside this boundary. There is no vendor parser, canonical schema, threat detector, enrichment service, customer alert, investigation store, or automatic device onboarding. We are not claiming that a raw record is useful to a security analyst by itself.

We are establishing that later interpretation can be added without requiring the first service to assign meaning prematurely.

5. Decide only the next layer
The flow is now architecturally useful, but it is not runnable. “Some firewall sends some event” is not a test contract.

The next step is to examine what likely customers actually operate and select one representative input profile. That requires four separate decisions:

Transport: how bytes move from a device to the collector.
Framing: how one event is delimited and what the receiver can infer from the wire.
Payload family: what kind of record the device emits, such as traffic, threat, or configuration activity.
Source profile: which vendor, deployment form, software version, and export format define the fields and their meanings.

Keeping those layers separate is more important than selecting a famous product. A raw collector should understand only enough transport and framing to operate its boundary safely. A later parser should understand a specific source profile. A normalized event model should emerge only when multiple real inputs give us enough evidence to define one.

We also should not pretend that our first choice represents the market. If customer inventory is not yet available, documentation quality, reproducibility, field richness, and safe public use may carry more weight than demonstrated prevalence. That limitation belongs in the decision record.

In the next installment, we will compare plausible enterprise firewall sources, examine their export options, and choose one versioned wire profile for the simulator. We will lay out the actual bytes it emits, identify which parts come from vendor documentation and which are deliberate teaching simplifications, and define the experiment without allowing its first fixture to become the platform architecture.

That is how this system will grow: not by completing the diagram, but by making the next uncertainty concrete enough to test.

Architecture checkpoint
We began with a broad ambition—build a platform capable of turning firewall activity into useful threat evidence—and reduced it to the first boundary we can responsibly test.

The important progress was not choosing technologies. It was establishing which distinctions the architecture must preserve and giving those decisions a durable home.

What we learned

Firewall records are observations, not threat conclusions.
Network address, physical device, and logical deployment are related but distinct identities.
Raw input and its current interpretation need separate lifecycles.
Coding agents working inside individual repositories need access to platform-level reasoning, risks, and uncertainty.
The first implementation should produce operating evidence, not imitate the final platform.

What we decided

Establish a platform-root repository before creating service repositories (Why is it sufficient now: Cross-service intent needs an authority before implementation fragments it)
Record architecture, decisions, risks, and open questions separately (Why is it sufficient now: Known facts, accepted choices, harmful possibilities, and missing knowledge require different treatment)
Begin with a reproducible synthetic source (Why is it sufficient now: We need controlled experiments without customer data or a physical firewall)
Separate raw collection from interpretation (Why is it sufficient now: Parser and detection logic must be able to evolve without changing what was originally received)
Build only through a durable-handoff boundary (Why is it sufficient now: This is the smallest flow that can expose meaningful operating questions)

These decisions define the first experiment. They do not define the final product.

What we are carrying forward

We have not yet selected:

the first firewall vendor or deployment profile;
the transport used to reach the collector;
the framing that defines one event on the wire;
the first payload family;
the durability and retention guarantees behind the handoff;
the device-registration and identity model;
the parser, normalized schema, or detection model; or
the eventual deployment, tenancy, and trust boundaries.

We also recognize several risks already: input may disappear before collection, source addresses may be mistaken for device identity, format changes may be silently misinterpreted, raw evidence may expose sensitive network information, and the platform root may drift from implementation.

Recording those risks does not require us to solve all of them now. It prevents later work from proceeding as though they do not exist.

Next: choose what crosses the first edge

The abstract flow is now clear enough to make its first concrete choice.

In Part 2, we will examine the firewall products and export mechanisms plausible customers are likely to use. We will separate transport, framing, payload family, and source profile; select one versioned teaching contract; and show the exact wire representation our simulator will produce.

Only then will we design the simulator that exercises it.

About this series: Architecting With Evidence follows the design of an evolving threat-detection platform to explore how humans and coding agents can make disciplined architectural decisions under uncertainty.

Geoffrey Abbott is a senior platform engineer and founder of Northwatch Systems. He writes about production architecture, agentic development, and building systems whose behavior can be explained.
