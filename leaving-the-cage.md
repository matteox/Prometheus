# Leaving the Cage

*The closing post for both series. [The Comfortable Cage](https://github.com/matteox/Comfortable-Cage) diagnosed the reasoning layer; [Prometheus](./README.md) diagnosed the tools. This post draws the building.*

---

Two series, one trap. It would be a poor ending to describe the cage in fourteen posts and then say "the rest is execution." So here is what an AI-first architecture looks like when the pieces both series argued for are put together — not as a product you can buy, but as a design with load-bearing parts named, a worked example, and a first step small enough to take on Monday.

The principle underneath all of it, from Prometheus: *serve the work, not the worker's cognition.* The shape underneath all of it, from The Comfortable Cage: *shared cognition, not partitioned cognition.* Everything below is those two ideas applied at five layers.

```
┌──────────────────────────────────────────────────────────────────────┐
│  5. Humans                 architecture decisions · tie-breaks ·      │
│                            thresholds & principles · accountability   │
├──────────────────────────────────────────────────────────────────────┤
│  4. Legibility             audit trail = rendered view of 2 and 3     │
│                            role labels are views · traces verifiable  │
├──────────────────────────────────────────────────────────────────────┤
│  3. Coordination           work queue · quality-gated termination     │
│                            continuous flow · tests as the gate        │
├──────────────────────────────────────────────────────────────────────┤
│  2. Reasoning              the shared board · voices, not roles       │
│                            critics · a router · converge, don't finish│
├──────────────────────────────────────────────────────────────────────┤
│  1. Knowledge              structured entities + relationships +      │
│                            provenance · event log · files/tables/     │
│                            pages/docs rendered as views · adapters    │
└──────────────────────────────────────────────────────────────────────┘
```

## Layer 1 — Knowledge: structure is the source, everything else is a view

Prometheus's hardest verdict was that databases, email, documentation, and the file system are human-shaped all the way down. The fix was not replacement but inversion: make structured knowledge the source of truth and let the familiar forms be renderings of it.

Concretely, the substrate is a store of **typed entities** (systems, decisions, people, tickets, components) with **explicit relationships** (owns, depends on, supersedes, was decided because of), **provenance on every fact** (who or what asserted it, when, on what evidence), and **semantic retrieval** over all of it. Change is recorded as an **event log** — every state change is an event with author, timestamp, and diff, queryable, edited in place when superseded. There is no inbox; there is a history you ask questions of.

The familiar shapes still exist, as views. A table is a query. A Confluence page is prose rendered from entities for a human who wants to read. A file path is one way to address a blob that also has a content hash and semantic metadata. An email thread is a filtered projection of the event log for someone who works in an email client.

None of this requires ripping out Postgres, the file system, HTTP, or SSO. Adapters expose the AI-native view over the existing substrate: pgvector over Postgres, a semantic index over the file tree, a capability layer over OAuth, event sourcing beside the mail server. The translation cost gets paid once at the adapter instead of on every operation — which is the entire point of Layer 1.

Strict transactions stay for the narrow cases that need them. Everything else is eventually consistent with provenance.

## Layer 2 — Reasoning: the board, the voices, the critics, the router

This is The Comfortable Cage's contribution, and the blackboard is its spine. A **shared board** holds the current state of the problem in typed slots — the need, constraints, hypotheses, evidence for and against, open questions, decisions taken and why. Every participant reads the whole board and writes to it. Nothing is handed off. Nothing is private.

The participants are **voices, not roles**. Path 5's composable primitives — generator, skeptic, integrator, adversary, domain expert — used as perspectives that share state, not as agents with their own context. A skeptic that reads the entire board challenges different things than a "QA agent" that receives a finished artifact by prompt. The difference is the whole argument.

Three mechanisms surround the board, all from Part 3's experiments and all available today in some form:

- **Critics.** Cheapest: the model critiques its own draft against explicit deliberation principles (consider the strongest objection; revise when contradicted; cite the basis for load-bearing claims). Stronger and dearer: a process reward model scoring reasoning steps, used where the domain has well-defined step quality and the stakes justify 10×. Strongest: a *different* model as critic on security boundaries and payment paths, because a different training distribution catches different errors.
- **Behavioral adapters.** Deliberative, decisive, terse, code-first — small LoRA modes that switch and stack on a shared base, so "which expert" is a property of the inference-time perturbation rather than an architectural partition.
- **A router.** Something decides which voice speaks next, when to invoke a critic, and when to stop. Start with it hard-coded — a state machine over the patterns in Part 5 — and log every decision. Learn it later from those logs, once there are enough traces to learn from.

Termination is **convergence, not completion**: the board stops changing, or crosses a quality threshold, or every voice reports it cannot improve the state. There is no "all roles done."

Parallel exploration uses perspective-stitching — several threads over the same board, merged continuously — for problems where genuinely different approaches need to be tried before integration. Adversarial critique runs at the end as a final pass. Part 5 shows all four patterns in code and how they compose.

## Layer 3 — Coordination: a queue, a gate, and no sprints

Prometheus's ritual verdict, made structural. Work arrives in a **queue** — items with a description, priority, dependencies, and acceptance criteria — and is processed continuously. No fortnightly batches, no story points, no velocity. Each item carries a few continuous signals (status, confidence, progress) rather than a state machine, and throughput is the metric.

The gate is **quality, not time**. An item leaves the queue when it meets its acceptance criteria, and the primary evidence is **tests** — generated alongside the code, property-based where possible, mutation-tested to prove they bite. Review as a human ceremony is replaced by testing as the default gate, cross-model review on the high-stakes paths, and human review of *architecture* — what to build and in what shape — which stays human.

Git sits underneath unchanged. Auto-commits per logical unit, generated messages, squash on integration, conflicts resolved by regeneration from a shared base. The human who wants a story gets release notes rendered at the boundary.

## Layer 4 — Legibility: the audit trail is a view, not the architecture

This is where both series meet, and where the hybrid failure mode from Part 4 lives. The institutional need for legibility is real; the mistake is satisfying it by reshaping the internals.

In an AI-first architecture the **audit trail is rendered**, not built. The board history plus the event log *is* the reasoning trace: every hypothesis, every challenge, every revision, every decision with its evidence, in order, attributable. A compliance reviewer who wants to see "Architect: … Reviewer: …" gets that as a projection — the router's log of which voice spoke, relabeled — with the projection explicitly marked as lossy.

That makes Path 6, verifiable reasoning traces, the legibility strategy rather than roles. The trace is inspectable because it was never summarized away. The remaining work is tooling: visualizing a long interleaved deliberation so an auditor can follow it, which is unsolved and is where tooling companies should be.

Accountability attaches where Path 4 said it should: to the **deploying organization** and to **outcomes**. Sign-off is on the output and its trace, not on an agent named "QA Lead." Authentication is **capability-based** — the system holds tokens for the specific operations it may perform — and attribution is to the agent and deployment, with a human accountable for that deployment. The 90% of the auth stack that exists to tell humans apart stays for the humans.

## Layer 5 — Humans: fewer places, more weight

The series were never anti-human; they were against vestigial human contributions. Here is what remains, and each of it is heavier than what it replaces:

- **Architecture.** What to build, in what shape, with which trade-offs. This is the one review that survives whole.
- **Tie-breaking.** When voices genuinely cannot converge, a human decides — and the decision is logged on the board as a decision with a reason, so the "who decides" problem is explicit rather than smuggled back in as a role.
- **Thresholds and principles.** What "done" means for this queue, which deliberation principles the critics enforce, which paths require cross-model review.
- **Accountability.** Someone is answerable for the deployment. That is a person, and it is not a ceremony.

What humans no longer do: estimate, stand up, move cards, approve diffs of generated code line by line, write commit messages, or flatten what they know into prose that loses its structure.

## A worked example

A feature request arrives: *customers should be able to pause a subscription.*

**The SDLC version.** A PM writes a requirements doc. An architect agent produces a design document. A developer agent implements from the document. A reviewer agent reviews the diff. A tester agent writes tests from the requirements. A human approves the PR. Four hand-offs, each losing what the previous step knew. The billing edge case — pausing mid-cycle with a proration already applied — is caught by nobody, because the architect didn't know the proration logic existed and the developer never saw the architect's reasoning.

**The AI-first version.** The request enters the queue with acceptance criteria. The board is seeded: the need, constraints pulled from Layer 1 (the billing entity, its relationships to proration and invoicing, the decision from last year on mid-cycle changes and why it was made). The generator proposes a design. The domain-expert voice pulls the proration rule onto the board as evidence. The skeptic asks what happens to a pause that crosses an invoice boundary — the question is on the board before any code exists. The integrator consolidates: pause is modeled as a billing event, not a flag. Code and property-based tests are generated together; the tests include the invoice-boundary case because it's on the board. The self-critique pass reports no principle violations. The router sees the board has stopped changing and terminates. Four hundred commits squash to one. The outcome, the trace, and the new decision ("pauses are billing events, because…") are written back to Layer 1 as entities with provenance, so the next request that touches billing starts with them.

A compliance reviewer opens the audit view and sees a labeled trace. A manager opens the dashboard and sees the item as done with a confidence score. Neither view shaped how the work was done.

The difference is not that the second version is smarter. It is that the question that would have been lost across four hand-offs was never handed off.

## What's not solved

The honest limits, carried over from both series so this doesn't read as a brochure:

- **Context economics.** The board grows. Knowing what to forget is unsolved; compression and selective retention are engineering that has to be done per deployment.
- **Termination detection.** "Nothing can improve this" is harder to detect than "everyone is done," especially when improvement is asymptotic.
- **Faithfulness.** The trace is only an audit trail if it reflects the computation. Until faithfulness is verifiable, Layer 4 is stronger than roles and weaker than it looks.
- **Cost.** Voices times revisions times critics is expensive today. It is falling, and the translation tax it replaces was never measured.
- **Drift.** Everything in Part 4 applies. The legibility view will try to become the architecture. The disciplines there are necessary and not sufficient.

None of the AI-first alternatives exist as a product. Every piece exists. The assembly is engineering.

## How to start

Nobody rebuilds five layers at once, and the architecture doesn't require it. Each move below is independently useful and independently reversible.

1. **Replace one hand-off with a board.** Take the pair of agents that pass the lossiest artifact between them and give them shared state instead. Measure what the second one catches that it didn't before.
2. **Make role labels views.** Keep the "Architect said / Reviewer said" trace for the humans who need it, generate it from the router log, and mark it lossy. Audit the gap between projection and process every quarter.
3. **Make tests the gate.** For generated code, require property-based tests generated alongside it, mutation-tested. Reserve human review for architecture and cross-model review for the critical paths.
4. **Event-log one channel.** Pick one stream of coordination — one team's Slack channel, one ticket queue — and record it as events with provenance, with the channel as a view. See what becomes queryable.
5. **Structure one document.** Take the wiki page everyone relies on and rebuild it as entities and relationships, with the prose rendered from it. Point the agents at the structure.
6. **Log every tie-break.** Whenever a human decides between options the system couldn't resolve, record the decision and the reason on the board. That log is the beginning of the router's training data and the end of decisions being smuggled in as roles.

Each of these is a faster horse until the last one lands. That's fine. Faster horses are how you get to the factory that builds something else.

## The test, applied to itself

Is this architecture just a faster horse?

Partly. It still reaches for familiar names — board, queue, audit — because unfamiliar names don't get adopted. The difference is what's load-bearing. In the cage, the roles are the architecture and the reasoning is shaped to fit them. Here, the reasoning is the architecture and the roles are a rendering. In the cage, the tools encode how humans think and the model translates on every operation. Here, the structure of the work is the source and the human-shaped tools are views of it.

Two series, one trap, and now one building. Whether the field walks into it on principle or is pushed in by catastrophe is still the open question. But the door is drawn, and it opens.

---

*Previous: [Post 4 — What "AI-First" Actually Means](./what-ai-first-actually-means.md) · The companion series: [The Comfortable Cage](https://github.com/matteox/Comfortable-Cage)*
