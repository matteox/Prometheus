# The Data Shapes

*Post 3 of Prometheus. [Post 1](./the-ones-that-survive.md) covered tools whose data model was abstract enough to survive; [Post 2](./the-rituals.md) covered rituals with nothing underneath but a human need.*

---

This is the hardest case: tools where the *data model itself* is shaped by how humans browse, read, and file. There is no ceremony to peel away and leave a clean core. The shape goes all the way down.

Four of them: the relational database, email, documentation, and the layer beneath all three that nobody thinks of as a tool at all.

## The relational database

Postgres is the wrong shape for AI. So is MySQL, Oracle, SQL Server, SQLite, and every relational database since Edgar Codd's [1970 paper](https://dl.acm.org/doi/10.1145/362384.362685) introduced tables, rows, columns, and keys — with a mathematical foundation, relational algebra, that made query optimizers possible. The model is one of the great achievements of computing. It is also a particular answer to "how should information be organized for retrieval," and the answer was tuned for who was asking.

Tables are for human browsing; an agent doesn't browse. Schemas are declared before data exists because humans plan first and write second; an agent infers structure from examples and revises. Joins let humans compose questions across related tables; an agent asks "tell me about this customer" and traverses the relationships as part of the answer. B-tree indexes find the row where column X equals Y; an agent's question is "everything semantically related to this concept," which no key-based index answers.

One thing worth being careful about: ACID transactions are *not* a human-cognition artifact. Atomicity and isolation exist so that two concurrent writers can't double-book a seat or double-spend a balance, and that problem is exactly as real when the writers are agents. What changes is the default. Most of what an agent reads and reasons over tolerates eventual consistency, provided it carries provenance — which version of which fact came from where, and when. Strict transactions remain the right tool for the narrow cases that need them.

What the agent needs: persistent state across invocations, retrieval by meaning, relationships as traversable primitives rather than query-time constructions, and provenance. The closest existing things are vector stores (retrieval by similarity, no schema, no relationships), knowledge graphs (explicit typed relationships, older tooling, worse ergonomics), and the RAG pipelines that sit on top of either. Postgres with pgvector is a perfectly reasonable implementation today.

What's obsolete is not SQL. It's the assumption that a new project starts relational and only deviates for special needs. AI-first systems should start from structured-knowledge primitives and reach for SQL when the problem demands it. Almost no team builds that way yet, and the reason is inertia, not technology.

*Faster horse? A faster ledger.*

## Email

Email was designed in 1971, when Ray Tomlinson needed two people on different ARPANET nodes to leave each other notes — sender, recipient, subject, body, and the `@host` syntax he introduced. SMTP formalized it in 1982; the inbox metaphor arrived with Lotus Notes (1989) and Outlook (1997); by the time Gmail launched in 2004, the inbox was so dominant that Gmail's signature innovation was *search* — a workaround for a queue that had stopped scaling.

Every part of that shape is a human-cognition workaround. The subject line exists for triage. Threading exists so a person can follow a back-and-forth. Reply-as-new-message exists because people read top to bottom, and each reply quotes the last, doubling the artifact count. Follow-up flags exist because humans forget. And the inbox itself is a queue, because a human processes things in order.

An agent reads the whole conversation at once, doesn't forget, doesn't triage, and doesn't process from a queue — it processes from a knowledge state. The data model underneath, a message with attribution and a timestamp, survives as a useful primitive. Everything around it is the original hand-off architecture: partition, and lossy serialization at every boundary. The first series argued that role-shaped AI systems inherit this pattern. Email is where the pattern came from, and every multi-agent system doing "message passing between agents" is reinventing the inbox with its bottlenecks intact.

What the agent needs looks like event sourcing: a shared, queryable log of state changes with author, timestamp, and diff. No inbox, no reply, no thread — a history you query, edited in place when new information supersedes old. Slack, Linear, and Notion comments are partial attempts at this for human teams, and every one of them still ships with an inbox-shaped default, because the alternative requires admitting the inbox was the wrong shape to begin with.

*Faster horse? The original one. Every communication tool since has inherited its shape.*

## Documentation

Documentation is the tool most people believe is AI-friendly. Assistants read docs; tools generate docs. The belief is wrong. Docs are how an organization *forgets* what it knows, by storing it in a shape that loses the structure of the original information.

Someone learns something. They open Confluence and write a page — prose, sections, a diagram. Others edit it. Years later the page is several thousand words of nested prose describing what was originally a structured set of facts: entities (projects, people, systems, decisions), relationships (P owns S; D happened before E), properties (D was decided on date Z), hierarchies (X is a kind of Y). The page says "P owns S," but ownership is not a queryable fact. It says "decided on date Z," but Z is not a field you can sort by. The flattening happened for good human reasons — prose is what people read, and page hierarchies match how people organize their thoughts. It threw away exactly the structure an agent would use.

Every Confluence space is a knowledge graph someone flattened into prose for human browsing.

What survives is the need — durable organizational knowledge, and something that can be searched. What doesn't survive is prose as the source of truth. The source should be typed entities with properties, explicit relationships, provenance (who, when, why, on what evidence), and semantic retrieval over all of it. Prose becomes a *generated view* — rendered for a human who wants to read, while the agent queries the structure directly. Notion's database feature, Airtable, and knowledge graphs all point in this direction; none is the finished thing.

*Faster horse? A faster reference card.*

## The invisible layer

Beneath all three sit abstractions so foundational we don't see them as tools. Fish don't see water.

**The file system** — hierarchy, named files, paths as addresses, extensions as types — is a 1970s Unix design mirroring folders in cabinets. An agent retrieves by meaning, not path; "the document about Q3 financials" is a better address than `/home/user/finance/q3.pdf`. The AI-shaped view is content-addressed blobs with semantic metadata in a flat namespace.

**The network stack** — TCP/IP, HTTP, REST — is built around request/response between identified endpoints: stateless, transactional, client-initiated. An agent doing ongoing work wants persistent connections, push rather than poll, and state shared between agents rather than requested from a server. Streaming gRPC, pub/sub, and event-driven architectures are closer to what it needs.

**Authentication** — SSO, OAuth, SAML, login forms — exists to distinguish individual humans from each other and grant access by identity. An agent doesn't log in and doesn't have a session; it has a deployment and a set of capabilities. Capability-based access ("can this agent perform this operation?") replaces role-based access ("what is this person's title?"). Audit survives, but attribution shifts from *which human* to *which agent, which deployment*.

**The document** — Word, PDF, slides — is a stable visual artifact that looks the same to everyone who opens it. An agent doesn't need the stability; it generates and consumes. Documents become ephemeral renderings of structured knowledge rather than stored artifacts — the same move as documentation, one layer down.

Now the obvious objection. Replacing the file system is multi-decade work. Replacing HTTP is generational. Replacing the auth stack runs through compliance cycles. Replacing the document is a war against every productivity vendor on earth. If these can't realistically be replaced, what is this section for?

The answer is adapters, not replacement. A semantic, content-addressed view layered over whatever file system is there. HTTP kept for crossing boundaries — to humans, to external systems — and bypassed for internal agent-to-agent traffic. A capability layer on top of existing SSO. Documents rendered from structured sources rather than stored as sources. In each case the agent sees an AI-native view through a translation layer, and the human-shaped substrate stays where it's appropriate. The translation cost gets paid once, at the layer, instead of on every operation — which is the whole point, because right now it's paid invisibly on every operation and nobody measures it.

*Faster horse? Four of them, ridden so long we call them the ground.*

## The verdict

When the data model itself is human-shaped, the fix is deeper than thinning ceremony and shallower than replacement: the source of truth becomes structured knowledge with semantic retrieval and provenance, and the familiar human-shaped forms — tables, inboxes, pages, files — become views rendered from it. The inversion is the hard part. Not because it's technically difficult, but because it means admitting the default was chosen for the reader we used to have.

---

*Previous: [Post 2 — The Rituals](./the-rituals.md) · Next: [Post 4 — What "AI-First" Actually Means](./what-ai-first-actually-means.md)*
