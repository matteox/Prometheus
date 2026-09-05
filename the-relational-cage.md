# The Relational Cage

*Post 1 of a new series. The first series — [The Comfortable Cage](https://github.com/matteox/Comfortable-Cage) — argued that humans project organizational shapes onto AI reasoning, limiting it. This follow-up argues the parallel case for tools: humans project human-tool shapes onto AI development workflows, with the same limiting effect.*

---

Postgres is the wrong shape for AI.

So is MySQL. So is Oracle, SQL Server, MariaDB, SQLite, and every relational database that has shipped since Codd published his 1970 paper. They were designed for a problem AI doesn't have, optimized for a workflow AI doesn't perform, and built around an assumption AI doesn't share.

This post argues the case. It's the deepest sacred cow in software — and the one most worth poking at, because the relational model is so foundational that questioning it feels like questioning arithmetic. It isn't arithmetic. It's a particular answer to the question of how to organize information, and the answer was shaped by who was asking.

## What databases were actually designed for

Edgar Codd's 1970 paper, "A Relational Model of Data for Large Shared Data Banks," introduced the world to the idea that data could be organized into tables — relations — with rows and columns, and that the relationships between tables could be expressed through shared keys. The genius of the model was its mathematical foundation: relational algebra gave queries a formal shape, and the formal shape made query optimizers possible.

But notice what the model optimizes *for*:

**Tables are for human browsing.** Humans can scan a tabular dump, see the rows, make sense of the columns. The relational layout puts information in a form a person can read. AI does not browse.

**Schemas are for human planning.** Schemas require humans to declare the structure of data before data exists. You design the tables, then you write to them. AI plans at inference time. It can infer structure from examples, propose a schema, write to it, and revise. The "design first, write second" pattern is a human workflow.

**Joins are for human-query composition.** Joins let humans ask complex questions about related data — "find customers who ordered product X but not Y in the last 90 days." AI reasons semantically across related data without composing joins. It asks "tell me about this customer" and traverses relationships as part of the answer, not as part of the query.

**Indexes are for key-based lookup.** B-tree indexes optimize for "find the row where column X = Y." AI retrieves by meaning, not by key. The fastest index in the world doesn't help if your query is "find me everything semantically related to this concept."

**ACID guarantees are for human-observed consistency.** Atomicity, consistency, isolation, durability — these properties protect humans from seeing partial updates and stale reads. AI doesn't observe consistency the way humans do. It can reason about staleness at inference time and decide whether the version of the data it has is good enough.

The relational model is a particular answer to "how should data be organized for retrieval?" The answer was tuned for human cognition. It works extremely well for that case. It is not the only possible answer, and it is not the right answer for AI.

## What AI actually needs from a data store

If we strip away the human-cognition optimizations, what survives?

**Persistent structured state.** AI agents need to remember things across invocations. They need a place where state lives between runs, where facts accumulate, where history is preserved. This is what databases provide, in some form or another.

**Semantic retrieval.** When an AI agent asks "what do we know about customer X?", the right answer isn't a SQL query with three joins. The right answer is "retrieve everything semantically related to customer X" — which might include notes, conversations, structured records, inferred relationships, and recent interactions, none of which fit cleanly into a relational schema with foreign keys.

**Reasoning across structure, not against it.** AI agents reason *across* data, not *against* it. They don't execute a query; they think about what's in the data. The data store should make relationships explicit and traversable as primitives, not require the agent to construct them at query time.

**Eventual consistency with provenance.** AI agents don't have a human in the loop who gets confused by a temporary inconsistency. They can reason about consistency at inference time. What they need is provenance — knowing which version of a fact came from where, when, and why — not the strict atomicity that ACID provides.

These are different requirements than the relational model was built for. They are not impossible to satisfy with a relational database — Postgres with `pgvector` is a perfectly reasonable implementation choice today, and many AI-first systems use exactly that. But the *default* — the assumption that you start a new project with a relational database and only deviate if you have specific non-relational needs — that default is doing a lot of work, and most of it is human-shaped.

## What an AI-first data store might look like

The closest existing things, ranked by how AI-native they are:

**Vector databases** — Pinecone, Weaviate, Chroma, Qdrant. Designed for semantic retrieval. Optimize for "find me the k most similar things to this embedding." They don't have schemas, don't enforce relationships between records, and don't provide ACID guarantees. The single primitive is similarity. They're the most AI-shaped data store we have today, and they're also the most limited — they handle retrieval but not reasoning across relationships.

**Knowledge graphs** — Neo4j, Amazon Neptune, RDF stores. Designed for explicit relationships. Represent data as nodes and edges with typed properties. Reasoning across the graph is the natural primitive, not table joins. They have schemas of a kind (the node types and relationship types), but those schemas emerge from the data rather than being declared up front. Closer to what AI wants than relational, but the tooling is older and the developer ergonomics are worse.

**RAG pipelines** — which sit on top of any of the above. The dominant AI pattern for retrieval: embed a query, retrieve relevant context from a vector store, augment the prompt with it, generate. The retrieval step is what an AI-first data store has to do well, and RAG is the current operationalization of that requirement.

These are early. None of them is "the answer." But all of them are evidence that the relational model isn't the only shape, and that the shape AI actually wants is closer to "structured knowledge with semantic relationships" than to "tables with foreign keys."

A real AI-first data store would probably combine:

- Vector retrieval for semantic similarity
- Graph traversal for relationship reasoning
- Persistent state with versioning and provenance
- Streaming updates so live reasoning has fresh data
- Schemas inferred at write time, not declared up front
- Eventual consistency with provenance, not ACID atomicity

None of this exists yet in production form. Each piece exists; the assembly is a tractable engineering problem rather than a research one. We're maybe three to five years from a system that has all of this and is credible enough to be a default.

## What survives, what doesn't

The data-model-vs-workflow distinction (a carryover from the first series) is useful here.

**What survives in the relational model:**

- The idea of structured data with explicit relationships
- The idea of querying rather than scanning
- Transactions as a unit of consistency (for the cases where AI does need strict consistency)
- SQL as a declarative query language, even if the underlying engine changes

**What doesn't survive:**

- Schemas as planning artifacts (AI plans at inference time, can infer structure)
- Tables as human-browsable representations (the model reasons across data, not against tabular dumps)
- Joins as the primary composition mechanism (semantic retrieval and graph traversal replace this)
- ACID as the default consistency model (eventual consistency with provenance is what AI needs)
- Indexes as the primary retrieval mechanism (embedding-based retrieval is what AI uses)
- ORMs as the impedance-mismatch layer (because there shouldn't be impedance if the model is right)

The data model mostly survives, transformed. The workflow doesn't.

This is a similar pattern to the first series' verdict on Git: the data model was abstract enough to survive, but the ceremony around it didn't. The same is true of relational databases — the formal model is good enough; the workflow conventions around it are wrong-shaped for AI.

## The unspoken claim

The relational model is a human-cognition optimization, not a fundamental truth about how information must be organized. We've been building software around how humans organize information — in tables, with explicit schemas, with joins for cross-table queries — not around how AI retrieves information, which is by meaning and by relationship traversal.

Postgres isn't obsolete. SQL isn't obsolete. They're an excellent implementation of a particular model, and that model is well-suited to a particular class of problems (the ones where humans are the primary readers of the data). What's obsolete is the assumption that the relational model is the right *default* — that you start every project with a relational database and only deviate when you have specific non-relational needs.

That inversion is the part most software teams haven't made. AI-first systems should start from structured-knowledge primitives and reach for SQL only when the problem demands it. Today, almost no teams build this way. The inertia is institutional, not technical — and the institutional pressure is the same one the first series identified around org charts and roles.

## The deeper pattern

The org chart of roles (covered in the first series) and the relational model of data are the same trap at different layers. Both are projections of human cognition onto AI work. Both are defended by institutional interests — humans whose careers depend on the existing structures. Both are doing real work (the relational model is good at what it was designed for) that gets misapplied when projected onto a different kind of work.

The fix in both cases is the same: separate the parts that serve human cognition from the parts that serve the work. Redesign the latter. Keep the parts that work.

The relational data model survives, transformed. The relational workflow doesn't. The post that follows this one makes the same case for a different tool.

---

*Next: [Post 2 — Email](#) — the original hand-off architecture. The most entrenched communication tool in human history, and the most obviously wrong shape for AI.*
