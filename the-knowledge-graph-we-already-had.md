# The Knowledge Graph We Already Had

*Post 5 of a new series on the tools that shape AI work. [Post 1](./the-relational-cage.md) — databases. [Post 2](./the-inbox-that-eats-everything.md) — email. [Post 3](./the-sprint-that-wasnt-needed.md) — sprints. [Post 4](./the-code-review-ceremony.md) — code review. This post tackles the artifact most people believe is AI-friendly: documentation.*

---

Documentation is the tool most people believe is AI-friendly. AI assistants read docs. AI tools generate docs. Docs are how the org stores what it knows.

The belief is wrong. Docs are how the org *forgets* what it knows, by storing it in a shape that loses the structure of the original information.

Every Confluence space is a knowledge graph someone flattened into prose. The flattening lost information. AI doesn't browse, so the flattening serves no one. The post argues that the right shape for organizational knowledge is structured — typed entities, semantic relationships, queryable — and that prose should be a generated view of that structured source, not the source itself.

## What documentation was actually designed for

Documentation serves real needs:

**Capturing organizational knowledge.** Decisions, processes, context — the things a company learns and shouldn't have to relearn. Docs are how the org stops forgetting.

**Sharing information across teams and time.** A person who joined last week can read what a person who left three years ago learned. Docs are how the org persists across personnel changes.

**Onboarding new employees.** New hires go to the wiki. The wiki is how they learn what the company knows.

**Reference material.** "How do I deploy to production?" "What's our policy on X?" The doc answers the question. Without it, the question gets asked repeatedly.

**Institutional memory.** Sometimes the doc exists primarily because someone cared enough to write it down. The doc carries the memory forward even when the original author leaves.

These are real values. The question is the *shape* the knowledge takes in the doc.

## The flattening problem

Here's what happens in a typical company. Someone learns something. They open Confluence and write a page. The page is prose, organized into sections and subsections, with maybe a diagram or two. Other people find the page, edit it, add their own knowledge. The page grows. After a few years, the page is several thousand words of nested prose covering what was originally a structured set of facts.

The original information had structure:

- **Entities** — projects, people, systems, decisions, policies
- **Relationships** — X is part of Y; P owns S; D happened before E
- **Properties** — D was made on date Z; P has role R
- **Hierarchies** — X is a kind of Y; A is a sub-thing of B

The prose page flattened all of that. The page says "P owns S" but doesn't represent the ownership relationship as a queryable fact. It says "D happened on date Z" but the date isn't a field you can sort by. It says "X is a kind of Y" but the taxonomy isn't enforced.

The flattening happened for human-browsing reasons. Prose is what humans read. Hierarchies (spaces, pages, sub-pages) match how humans organize their thoughts. So the org flattened its knowledge into a shape optimized for human reading.

AI doesn't read prose the way humans do. AI ingests — it takes the whole document, all at once, into context. The hierarchy doesn't help. The prose doesn't help. The lost structure hurts, because AI would benefit from the original structure more than humans benefit from the prose.

> **Every Confluence space is a knowledge graph someone flattened into prose for human browsing. The flattening lost information.**

The original information had structure. The org threw the structure away because humans read prose. Now the org has prose that humans occasionally browse and AI occasionally ingests, and neither the structure nor the prose is doing its job well.

## What survives, what doesn't

Carrying the data-model-vs-workflow distinction:

**What survives in documentation:**

- The underlying need: durable organizational knowledge. This is real and applies to AI-augmented teams.
- The notion of a knowledge base — somewhere the org's accumulated knowledge lives. Survives, transformed.
- Search and retrieval — though the shape of what's searched changes.

**What doesn't survive:**

- Prose as the source of truth. Prose can be a *rendering* of structured knowledge, but it shouldn't be the source. The source needs structure.
- Hierarchy as the primary organization. Hierarchies of spaces and sub-spaces mirror how humans organize thoughts, not how knowledge is actually structured. Real organizational knowledge has many overlapping structures — by team, by project, by date, by topic, by relationship to other knowledge.
- Pages as the atomic unit. Real knowledge has finer-grained structure than pages. Entities, relationships, properties — these are the atoms, not pages.
- Edit-and-review workflows designed for prose. The workflow optimized for prose-writing humans doesn't translate to structured knowledge management.

The data model — text + structure + hierarchy — is too generic to evaluate as a whole. What's specific to docs is the prose-first orientation, and that's what doesn't survive.

## What AI actually needs for "knowledge management"

Strip away the prose. What's left?

**Typed entities with properties.** Not pages; entities. A person, a project, a decision, a system — each with structured fields. The same data model a database would use, but with semantic types that capture what each thing *is*.

**Explicit relationships.** Not narrative statements of relationship; structured relationship records. "P owns S" becomes a row in an ownership table, queryable and traversable. "D is a kind of D'" becomes a type-hierarchy record.

**Semantic retrieval as the primary access pattern.** When someone asks "what do we know about X?", the right answer is "retrieve everything semantically related to X" — entities, relationships, properties, recent changes. This is what RAG pipelines over knowledge graphs do.

**Provenance.** Every piece of knowledge has metadata: who created it, when, why, what evidence supports it. Prose can encode provenance in narrative ("according to the meeting on date Z..."), but structured knowledge should encode it as fields.

**Prose as a generated view, not the source.** When a human wants to read the documentation, render the structured knowledge into prose for them. When an AI wants to use the knowledge, query the structure directly. The same source, different consumers.

The closest existing things:

- **Knowledge graphs** (Neo4j, Amazon Neptune, RDF stores). Designed for exactly this. Rare in practice because the dev ergonomics are bad.
- **Notion databases** (the database feature, not the page feature). Approximate this for human-friendly teams. Limited but pointed in the right direction.
- **Airtable.** Spreadsheet-shaped structured knowledge. Closer to right than prose; still spreadsheet-shaped, which is its own limitation.
- **RAG pipelines over vector stores.** The dominant AI pattern. Retrieval over embeddings is one form of semantic access; it's missing the typed-entities-and-relationships part of the picture.

A real AI-first knowledge system would combine:

- Typed entity model (the schema is inferred or specified)
- Explicit relationships (queryable, traversable)
- Semantic retrieval (vector-based for "find related things")
- Provenance metadata (who, when, why, evidence)
- Generated prose views for human readers

This doesn't exist as a packaged product. The pieces exist and are well-understood. We're maybe three to five years from a credible open-source implementation, and the category is currently a research area for several teams.

## Why the flattening persists

The institutional reasons the flattening persists:

- **Prose is what humans are used to writing.** Engineers and managers don't think in entities and relationships; they think in paragraphs. Changing the tool doesn't change the writing habit.
- **The current tools optimize for prose.** Confluence, Notion, Google Docs — these are all prose-first. The structured features exist but are secondary.
- **Nobody is allowed to say the docs are the bottleneck.** Pointing out that the knowledge management system is losing information is uncomfortable because everyone has spent years writing into it.

The honest position: most organizational knowledge is stored in a shape that loses information on the way in. The information was structured when it lived in someone's head. By the time it reached Confluence, it was prose. AI can't recover the lost structure from the prose — it can only ingest the flattened version.

We're keeping the prose shape because admitting it's wrong requires rebuilding years of organizational knowledge. That's an institutional fact, not an engineering one.

## The deeper pattern

The first four posts in this series made the case for tools (databases, email) and rituals (sprints, code review) that are human-cognition-shaped. This post extends to the artifact most believed to be AI-friendly: documentation.

The pattern is consistent: each layer of the stack optimizes for a human constraint, and the optimization is preserved even when the constraint is removed or doesn't apply. Documentation optimizes for human reading — prose, hierarchy, page-as-unit. When the consumer is AI, those optimizations are working against the goal.

The fix in each case is the same: identify the human constraint being optimized for, separate it from the underlying need, and redesign for the underlying need alone. The underlying need for organizational knowledge survives. The prose-first shape doesn't. Prose can be a generated view; the source of truth should be structured.

The post that follows this one tackles the tool that organizes the work itself: the ticket system. Tickets are flattened work items, with state machines that don't translate to AI work.

---

*Next: [Post 6 — Jira and the State Machine That Wasn't Needed](./the-state-machine-that-wasnt-needed.md) — work items flattened into tickets, with state transitions that exist for human cognitive needs. AI doesn't transition through states. AI works continuously on items until they're done.*
