# Prometheus — Talking Points (revised)

*A reference document for the companion series to "The Comfortable Cage."*

If the first series argued that humans project organizational shapes onto AI reasoning (org charts, roles, SDLC), this follow-up argues the parallel case for tools: humans project human-tool shapes onto AI development workflows, with the same limiting effect.

The framing is **"the emperor has no clothes"** — naming the unsayable about tools everyone uses and nobody is allowed to question, because their job depends on them.

---

## The analytical framework

For any tool, the full analytical move has four steps:

1. **State the need.** What problem does this tool serve? (Not the tool's stated purpose — the underlying need.)
2. **Question the need.** Does this need exist for AI at all? Or is the need itself a human-cognition frame for something more fundamental?
3. **If the need survives, separate data model from workflow.**
   - **Data model:** what information does the tool actually represent? (Abstract data models survive.)
   - **Workflow:** what human-shaped process does it enforce? (Almost never survives.)
4. **Articulate the AI equivalent.** What does AI actually need, given its cognition, to satisfy whatever need survives?

If the need doesn't survive, the tool is obsolete. If the need survives but only the workflow is in the way, the tool is redesignable. If both the need and the data model survive but the workflow is wrong, we keep the core and redesign the ceremony.

The strongest taboo claims come from step 2 — the need doesn't survive. The constructive redesign claims come from step 3 — the workflow is the problem. A complete analysis runs both moves.

---

## The template

Each per-tool post fills in this template:

- **Need:** What problem does the tool serve?
- **Survives for AI?** Yes / no / transforms-into-X.
- **If yes — data model:** What information does the tool actually represent?
- **If yes — workflow:** What human-shaped process does it enforce?
- **AI equivalent:** What does AI actually need, given its cognition?
- **Unspoken claim:** The line that lands the punchline without stating it.

---

## Worked example: email

- **Need:** Durable, asynchronous, ordered communication between individuals, with audit trail.
- **Survives?** Partially. "Communication between individuals" assumes discrete actors sending discrete messages — that's the human frame. The underlying need (durable ordered state with attribution) does survive. So the tool is obsolete, replaced by something that satisfies the underlying need directly.
- **Data model:** Messages with sender/recipient/timestamp/body.
- **Workflow:** Inbox, threading, triage, CC chains, follow-up flags.
- **AI equivalent:** A shared mutable log of state changes, with semantic query. Every "email" is a state-change with author/timestamp/diff, not a message. No inbox, no threading, no reply. Just a queryable history.
- **Unspoken claim:** email is the original hand-off architecture, and we keep using it because nobody is allowed to say it's the bottleneck.

---

## The taboo inventory — everything a software company has installed

Organized by category, ranked by how uncomfortable it is to question. The entries with the most institutional defenders come first.

### Tier 1: The untouchables

**Relational databases (Postgres, MySQL, etc.)**
- *Need:* Durable structured storage of related data with consistency guarantees.
- *Survives?* Transforms. AI doesn't need ACID guarantees (eventual consistency is fine for reasoning), but does need structured storage of related data. The consistency model changes; the relational model mostly survives.
- *Data model:* Tables, schemas, relationships, queries (SQL).
- *Workflow:* Schema design, migration management, query optimization, ORM mapping.
- *AI equivalent:* Structured knowledge with semantic relationships — vector stores, knowledge graphs, RAG pipelines. The relational model is a human-cognition optimization (organizing info in tables for human browsing); AI reasons semantically.
- *Unspoken claim:* the relational model is a human-cognition optimization, not a fundamental truth. We've been building software around how humans organize information, not how AI retrieves it.

**Email**
- *Need:* Durable, asynchronous, ordered communication between individuals, with audit trail.
- *Survives?* Partially. The underlying need (durable ordered state with attribution) survives. The framing of "communication between individuals" does not.
- *Data model:* Messages with sender/recipient/timestamp/body.
- *Workflow:* Inbox, threading, triage, CC chains, follow-up flags.
- *AI equivalent:* A shared mutable log of state changes, with semantic query. No inbox, no threading, no reply.
- *Unspoken claim:* email is the original hand-off architecture, and we keep using it because nobody is allowed to say it's the bottleneck.

**The sprint / agile process**
- *Need:* Coordinate work across a team with bounded cycles and progress visibility.
- *Survives?* No. Bounded cycles assume humans need recovery periods; progress visibility assumes humans can't see everything at once. AI has neither constraint.
- *AI equivalent:* Continuous work with quality-gated termination. No sprints, no story points, no velocity. Just "is the work done to the threshold?"
- *Unspoken claim:* agile ceremonies exist to manage human anxiety about progress. AI doesn't have that anxiety, so the ceremonies solve no problem for it.

### Tier 2: The workflows people are defensive about

**Code review / pull requests**
- *Need:* Catch errors in code and spread knowledge across the team.
- *Survives?* Transforms. AI-generated code still benefits from review (errors exist), but the human-in-the-loop pattern doesn't translate.
- *Data model:* Code diff with comments.
- *Workflow:* PR creation, reviewer assignment, approval/rejection, merge.
- *AI equivalent:* Same-model self-review during generation (workspace reasoning); cross-model review for high-stakes code. PR as a human-readable artifact, not the primary mechanism.
- *Unspoken claim:* code review is human-in-the-loop theater when the code is AI-generated. We're keeping the ceremony because we don't know what to replace it with, not because it's working.

**Architecture review boards**
- *Need:* Ensure proposed designs are sound before implementation.
- *Survives?* Transforms. Designs should be sound, but board meetings assume humans need synchronous deliberation.
- *AI equivalent:* Model generates multiple design candidates, self-critiques, presents alternatives for human review. No meeting required.
- *Unspoken claim:* ARBs exist to give senior engineers status. The status is real; whether it improves the resulting architecture is an open question.

**Standups, retros, planning meetings**
- *Need:* Synchronize distributed humans.
- *Survives?* No. AI doesn't need synchronization — it has shared state by default.
- *AI equivalent:* None. Skip these entirely.
- *Unspoken claim:* standups are coordination overhead disguised as communication. AI doesn't need them and never did.

**On-call rotations / PagerDuty**
- *Need:* Have someone available to handle incidents.
- *Survives?* Transforms. Incidents still happen, but "having someone available" assumes humans have other commitments and need sleep.
- *AI equivalent:* Always-on monitoring with continuous reasoning. AI is always available.
- *Unspoken claim:* on-call exists because humans need sleep and have lives. AI has neither constraint, so the rotation is a workaround for a problem AI doesn't have.

### Tier 3: The artifacts that get more scrutiny

**RFCs / design docs**
- *Need:* Propose designs for approval.
- *Survives?* Transforms. Designs still need to be proposed, but the approval-gating pattern is human-shaped.
- *Data model:* Documents with sections, revision history.
- *Workflow:* Draft, review, comment, approve.
- *AI equivalent:* Model generates designs, self-evaluates against criteria, presents options. Humans approve or push back asynchronously.
- *Unspoken claim:* most RFCs are written to be approved, not to be right. The format optimizes for committee agreement.

**Jira / Linear / Asana**
- *Need:* Track work units and their progress.
- *Survives?* Transforms. Work units are useful; the state machine is human-shaped.
- *Data model:* Tickets with metadata (assignee, priority, dependencies).
- *Workflow:* State transitions, board views, velocity dashboards.
- *AI equivalent:* Work queue with confidence/progress per item, no state machine. Query interface for humans.
- *Unspoken claim:* the board view is for human managers; AI doesn't need it. Velocity is unmeasurable for a system that produces a day's work in seconds.

**Confluence / Notion / docs**
- *Need:* Durable organizational knowledge.
- *Survives?* Transforms. Knowledge is necessary; prose-in-hierarchy is the human frame.
- *Data model:* Prose documents with hierarchy.
- *Workflow:* Editing, reviewing, version history.
- *AI equivalent:* Structured knowledge with semantic relationships (typed entities, RAG indices). Prose generated for humans on demand.
- *Unspoken claim:* every Confluence space is a knowledge graph someone flattened into prose for human browsing. The flattening lost information.

**Git workflows**
- *Need:* Versioned shared state of files.
- *Survives?* Yes. The need clearly applies to AI; only the workflow layer is in the way.
- *Data model:* Snapshots of files with parent pointers. Abstract — doesn't assume human author.
- *Workflow:* Commits, branching, merging, conflict resolution, code review ceremonies.
- *AI equivalent:* Same data model; leaner workflow — denser commits, less branching, fewer merge ceremonies, automated squash on integration.
- *Unspoken claim:* Git's data model survives because it's abstract. Git's culture dies because it's human-shaped. The ceremony outlived its audience.

**Spreadsheets as databases**
- *Need:* Human-browsable structured data with formulas.
- *Survives?* No, for the "human-browsable" part. AI doesn't browse; it queries.
- *AI equivalent:* An actual database (structured, queryable) or vector store plus structured metadata. The spreadsheet-as-database pattern should just be a database.
- *Unspoken claim:* every critical spreadsheet in your company is a database that nobody has the authority to migrate, and your most important business logic lives there.

**CI/CD pipelines**
- *Need:* Catch errors and ensure deployable state.
- *Survives?* Transforms. Catching errors is good; the assumption that AI can't self-test is wrong.
- *Data model:* Build artifacts, test results, deployment records.
- *Workflow:* Trigger, build, test, deploy, notify.
- *AI equivalent:* Model tests as it generates (workspace reasoning); self-deploys when ready. CI/CD becomes a backup, not the primary correctness mechanism.
- *Unspoken claim:* most CI/CD pipelines exist to catch problems AI could prevent at generation time. We're paying for the test instead of fixing the generate.

**Logging / monitoring / observability**
- *Need:* Understand system state and detect problems.
- *Survives?* Transforms. State needs to be inspectable; human-watched dashboards are the wrong shape.
- *Data model:* Time-series metrics, log entries, traces.
- *Workflow:* Dashboards, alerts, manual investigation.
- *AI equivalent:* Continuous self-monitoring, semantic alerts. AI watches itself; humans query when they need to.
- *Unspoken claim:* observability tooling is a human attention substitute. AI doesn't need to "see" its state; it is its state.

**Ticketing systems for support / customer issues**
- *Need:* Track and resolve customer issues.
- *Survives?* Transforms. Issues exist; queue-based human agents are the wrong frame.
- *Data model:* Tickets with customer info, status, resolution.
- *Workflow:* Triage, assignment, SLA tracking, escalation.
- *AI equivalent:* AI agents resolve directly with structured records. Humans review edge cases.
- *Unspoken claim:* support ticket systems are queue-management for human agents; AI doesn't need a queue.

### Tier 4: The deeply structural stuff

**The job title hierarchy (Junior / Senior / Staff / Principal / etc.)**
- *Need:* Distinguish levels of experience and authority.
- *Survives?* No. Levels assume discrete skill/authority gradations humans need for career progression. AI doesn't have a career.
- *AI equivalent:* Project-based roles or function-based roles (architect, implementer, reviewer) — not a ladder. The ladder exists to gate access; the gates exist to justify the ladder.
- *Unspoken claim:* the engineering ladder is a feudal system. The "senior" title gives the holder access to tools (architecture review, design approval) that justify the title. Take away the tools, the title has no content.

**The interview / hiring process**
- *Need:* Evaluate candidates.
- *Survives?* No. Interviews test human cognition (communication, personality, whiteboard performance) — none of which translate to AI-augmented teams.
- *AI equivalent:* Portfolio-based evaluation, work-sample testing, model-assisted assessment.
- *Unspoken claim:* most engineering interviews don't predict job performance even for human candidates. They predict how well candidates perform in interviews. The whole apparatus is a sorting ritual dressed up as evaluation.

**The performance review**
- *Need:* Document performance and inform decisions.
- *Survives?* No. Reviews are organizational anxiety management. AI-augmented teams need continuous evaluation, not periodic ones.
- *AI equivalent:* Continuous performance signals from output quality and collaboration. Reviews obsolete.
- *Unspoken claim:* performance reviews feel like accountability; they function as documentation for later HR decisions.

**The promotion process**
- *Need:* Advance people through levels.
- *Survives?* No. Promotion is a feudal gatekeeping mechanism.
- *AI equivalent:* Project-based recognition. No gates.
- *Unspoken claim:* promotion processes are how senior engineers preserve the value of being senior. The gate exists to be a gate, not to ensure quality.

### Tier 5: The ones nobody questions because nobody notices

**The file system**
- *Need:* Durable storage of named files.
- *Survives?* Transforms. Storage is necessary; hierarchy is the human frame.
- *Data model:* Named blobs in a tree.
- *Workflow:* Create, edit, move, delete, search by path.
- *AI equivalent:* Object storage with semantic metadata. Vector stores with associated blobs. Files-as-blobs in a flat namespace is closer to what AI wants than hierarchy.
- *Unspoken claim:* the file system is the most pervasive AI-hostile abstraction in computing. Every AI system has to convert its reasoning into files because that's where the data lives.

**The network protocol (TCP/IP, HTTP, REST, gRPC)**
- *Need:* Communicate between processes.
- *Survives?* Transforms. Communication is necessary; request/response is the human frame.
- *Data model:* Packets, requests, responses, streams.
- *Workflow:* Client initiates, server responds. Stateless interactions.
- *AI equivalent:* Persistent connections, push semantics, shared state protocols. gRPC streaming is closer than REST; pub/sub is closer than request/response.
- *Unspoken claim:* the entire network stack is shaped around discrete client/server transactions, which is how humans imagine interacting with services. AI agents want persistent shared presence.

**Authentication / SSO / OAuth**
- *Need:* Identify actors and authorize access.
- *Survives?* Transforms. Identity exists; humans-distinguishing identity is the human frame.
- *Data model:* User records with roles and permissions.
- *Workflow:* Login, session management, access checks.
- *AI equivalent:* Capability-based access, model-attributed actions, semantic authorization. Identity is fungible for AI.
- *Unspoken claim:* enterprise identity management is mostly about distinguishing humans from each other for audit and access control. AI doesn't need that distinction, so 90% of the auth stack is overhead.

**Office productivity software (Google Workspace, M365, Slack)**
- *Need:* Create and share documents and messages.
- *Survives?* Transforms. Communication and documentation are necessary; document-as-stable-visual-artifact is the human frame.
- *Data model:* Documents, spreadsheets, slides, chat messages.
- *Workflow:* Create, edit, share, comment, version.
- *AI equivalent:* Model generates and consumes directly. Documents are ephemeral renderings, not stored artifacts.
- *Unspoken claim:* documents are a human-cognition artifact. They're stable visual artifacts humans can hold in their head. AI doesn't hold documents in its head; it generates and consumes them on the fly.

---

## The meta-argument

Every entry on this list shares a common structure: a tool designed around human cognition, defended by institutional interests, projected onto AI work where it limits rather than enables.

The first series identified the org chart as the artifact of liability allocation. This follow-up identifies the tool stack as the artifact of human accountability. Together they form a complete description of the cage:

- **Org charts** tell you who is responsible for what
- **Tool stacks** tell you how that responsibility gets exercised
- **Together** they form the institutional machinery that humans use to defend AI's outputs to themselves

The fix is the same in both cases: separate the parts that serve human cognition from the parts that serve the work. Redesign the latter. Keep humans in the loop where they actually add value. Trust AI cognition where it's better than human cognition.

---

## The position paper

**Every tool in a software company was designed for how humans think. In an AI-first world, those tools either get redesigned to serve AI cognition (with humans as a secondary audience) or they become the bottleneck. The choice is not whether to question them — the choice is whether to admit we're questioning them. The emperor has no clothes. We're just not saying it yet.**

---

## Series structure proposal

**Post 0 — Position paper.** *The emperor has no clothes.* Lays out the meta-argument before the per-tool dives. Establishes the frame.

**Post 1 — Databases.** The deepest sacred cow. The relational model as a human-cognition optimization. What an AI-first data store might look like.

**Post 2 — Email.** The original hand-off architecture. The most entrenched communication tool in human history, and the most obviously wrong shape for AI.

**Post 3 — The sprint / agile process.** Pure human-cognition artifact. Ceremonies that exist to manage human anxiety about progress.

**Post 4 — Code review and PR workflow.** Human-in-the-loop theater when the code is AI-generated.

**Post 5 — Confluence / docs.** Hierarchical prose as the wrong shape for AI knowledge.

**Post 6 — Jira and ticket systems.** State machines and board views that don't translate to AI work.

**Post 7 — Git workflows.** Data model survives, ceremony doesn't.

**Post 8 — Programming languages and tooling.** The languages probably won't change; the tooling will.

**Post 9 — The deeply structural stuff.** Engineering ladder, hiring, performance review, promotion. The feudal system.

**Post 10 — The ones nobody questions.** File system, network stack, auth, documents. The invisible AI-hostile abstractions.

**Post 11 — What "AI-first" actually means.** The design principle: serve the work, not the worker's cognition. The close that ties this series back to "The Comfortable Cage."

---

## Title options

- **"Faster Horses, Slower Tools"** — keeps the Ford frame, makes the parallel to the first series explicit. Strongest choice.
- **"The Tool Trap"** — short, direct.
- **"Human-Readable, AI-First"** — names the tension in the subtitle.
- **"The Comfortable Toolchain"** — echoes "The Comfortable Cage."

---

## What we know going into the new series

**Strong carryover from the first series:**
- The faster-horses frame
- The data-model-vs-workflow distinction
- The "institutional pressure" argument (humans defend AI by mapping onto existing structures)
- The epistemic discipline (tractable / frontier / moonshot framing for any proposed alternative)

**Hard problems we'll hit:**
- Less concrete alternative than the first series had. Workspace reasoning is a clear substitute for role-shaped agents; AI-first databases are less clear (vector DBs are early).
- More institutional resistance per claim. The first series argued with AI practitioners; this one argues with everyone whose job depends on the existing tool stack.
- More taboo claims (databases, email). Each post will be a small fight.

**Editorial discipline to maintain from the first series:**
- Acknowledge what works (the data model survives for most tools)
- Distinguish what fails (the workflow layer)
- Distinguish epistemic status (tractable / frontier / moonshot for any proposed alternative)
- Don't overclaim — these are arguments, not findings
- Make the joke land without stating the punchline

---

## Running joke to consider carrying over

The first series opens with the apocryphal "faster horses" Ford quote and never states the punchline about AI hallucination. The reader figures it out.

The new series could carry a similar frame. One option: the Ford quote returns at the top of the position paper (Post 0), but this time the apocryphal-acknowledgment is paired with a different observation — that the *tools* we use to build software are themselves the result of similar projection: each tool was someone asking "what do users want?" and getting answers about faster horses. The reader carries the joke forward into the per-tool posts, where every claim about a tool is implicitly a claim about faster horses.

The deeper joke: AI hallucination isn't unique to AI. It's the same pattern as apocryphal attributions, as the success of agile theater, as the institutional defense of tools that don't work. The whole industry is hallucinating faster horses, and the tools are how it institutionalizes the hallucination.
