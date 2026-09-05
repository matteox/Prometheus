# The Invisible Abstractions

*Post 10 of a new series on the tools that shape AI work. The previous posts covered the visible stack — databases, email, docs, tickets, Git, languages, the engineering ladder. This post covers the abstractions so foundational that we don't see them as tools: the file system, the network protocol, authentication, the document.*

---

There are abstractions so foundational to computing that we don't see them as tools. We see them as the medium computing happens in. The file system. The network protocol. The authentication system. The document.

We don't see them as tools because we see them as the medium tools exist in. The same way fish don't see water. The post argues that these invisible abstractions are the most AI-hostile layer of the computing stack, precisely because nobody is allowed to question them. Every AI system has to convert its reasoning into files, send HTTP requests, authenticate as a user, and produce documents because that's where the world expects AI to live.

The post covers four abstractions. Each is invisible. Each is human-shaped. Each is hostile to AI work.

## The file system

The file system is a hierarchy of named files in named directories, accessed by path. It was designed in 1970s Unix and has been the substrate of every computing system since.

The design choices optimized for:

- **Hierarchy.** Directories inside directories. Mirrors how humans organize paper files in folders in cabinets.
- **Named files.** Every file has a name. The name is human-meaningful: "letter.txt," "report.pdf," "data.csv."
- **Paths as addresses.** To find a file, you give its path: /home/user/projects/work/report.pdf. The path is a human-readable location.
- **Extensions as types.** The part after the dot tells humans (and some software) what kind of file it is. Humans learn these; software uses them as hints.

AI doesn't need any of this:

- **AI retrieves by meaning, not by path.** When the model wants the file about X, it doesn't traverse a directory tree. It queries.
- **AI doesn't need named files.** Chunks of content with semantic metadata work better than "file with a name."
- **AI doesn't need paths.** Semantic addressing ("the document about Q3 financials") works better than "/home/user/finance/q3.pdf."
- **AI doesn't need extensions.** Type can be inferred from content. The .pdf extension is a hint for software, not for AI.

The AI-first equivalent is object storage with semantic metadata. S3 buckets with descriptive tags. Vector databases where each entry is a chunk of content with embedding metadata. Content-addressed storage where the address is a hash of the content, not a human-chosen name. Files-as-blobs in a flat namespace is closer to what AI wants than hierarchy.

The unspoken claim: *the file system is the most pervasive AI-hostile abstraction in computing. Every AI system has to convert its reasoning into files because that's where the data lives.*

## The network protocol

The network stack (TCP/IP, HTTP, REST, gRPC) is built around request/response between identified endpoints. A client makes a request; a server responds. The model is stateless, transactional, and client-initiated.

The design choices optimized for:

- **Request/response.** A clear initiator and responder. Maps to how humans think about asking for and receiving information.
- **Stateless interactions.** Each request is independent. The server doesn't remember prior requests.
- **Client-initiated.** The client decides when to communicate. The server waits.
- **Identified endpoints.** Each service has an address. URLs are human-readable.

AI doesn't need most of this:

- **AI often wants persistent connections.** When an AI agent is doing ongoing work, it doesn't want to negotiate a new connection for every operation.
- **AI needs state.** The model has internal state; the network should carry it.
- **AI wants push semantics.** When new information arrives, the agent should be notified — not have to poll.
- **AI wants shared state.** Agents often want to share state with each other, not request from a server.

The AI-first equivalent is gRPC streaming (persistent bidirectional connections), pub/sub systems (Kafka, NATS), event-driven architectures, and shared state protocols. Each is closer to what AI agents actually need than request/response.

The unspoken claim: *the entire network stack is shaped around discrete client/server transactions, which is how humans imagine interacting with services. AI agents want persistent shared presence.*

## Authentication

The authentication stack (SSO, OAuth, SAML, login forms) is built around identifying individual humans and granting them access based on their identity.

The design choices optimized for:

- **Human login.** Username and password, biometric, MFA. The human proves who they are.
- **Session management.** Once logged in, the session tracks the human across requests.
- **Role-based access control.** Different humans have different permissions based on their role.
- **Audit trails.** Every action is attributed to a specific human identity.

AI doesn't need most of this:

- **AI doesn't log in.** AI agents establish identity differently — typically through model attestation, deployment credentials, or service-to-service authentication.
- **AI doesn't have sessions in the human sense.** An AI agent either has its context or it doesn't; the session concept is human-shaped.
- **AI doesn't need roles.** Capability-based access (does this agent have permission to do X?) is more appropriate than role-based access (does this human have the right title?).
- **Audit is still important, but attribution is different.** Every action should be attributable, but to which AI agent and which deployment is more useful than to which human.

The AI-first equivalent is capability-based access, model-attributed actions, semantic authorization ("can this model perform this operation?"), and identity as fungible. The 90% of the auth stack that exists to distinguish humans from each other becomes overhead when the actor is AI.

The unspoken claim: *enterprise identity management is mostly about distinguishing humans from each other for audit and access control. AI doesn't need that distinction, so 90% of the auth stack is overhead.*

## The document

Documents — Word files, PDFs, Google Docs, spreadsheets, slides — are stable visual artifacts that humans can read, share, and edit.

The design choices optimized for:

- **Stable visual artifacts.** A document looks the same to everyone who opens it. The visual stability is the point.
- **Hierarchical organization.** Sections, subsections, headers, footers. Mirrors how humans write papers.
- **Version history.** Track changes over time. Useful for humans who want to see the evolution.
- **Sharing and collaboration.** Send the document; the recipient sees what you saw.

AI doesn't need this shape:

- **AI doesn't need stable visual artifacts.** AI generates content; the visual rendering is for human consumption.
- **AI doesn't need hierarchical organization.** Semantic relationships work better than nested sections.
- **AI doesn't need version history of the document.** Re-generate from current state.
- **AI doesn't need document sharing.** Output goes directly to the consumer (human or another system).

The AI-first equivalent is direct generation and consumption: the model produces content, the consumer (human or system) renders it appropriately. Documents become ephemeral renderings of structured knowledge, not stored artifacts.

The unspoken claim: *documents are a human-cognition artifact. They're stable visual artifacts humans can hold in their head. AI doesn't hold documents in its head; it generates and consumes them on the fly.*

## The pattern across the four

Each of these abstractions shares a structure:

- **Designed for human cognition.** Hierarchy, request/response, identity, visual artifacts — all match how humans think about storage, communication, access, and information.
- **Invisible because foundational.** We don't notice them because we've been using them for decades.
- **Hostile to AI work.** AI doesn't need hierarchy, request/response, identity distinction, or visual artifacts.
- **Defended by institutional interests.** Operating systems, network infrastructure, identity providers, document vendors — all have business models built on the current shape.

The deepest pattern: every AI system has to do translation work to fit into these abstractions. The model produces structured reasoning, but stores it as files. The model wants persistent state, but communicates via HTTP. The model has its own identity, but authenticates as a user. The model generates content, but outputs as documents.

The translation work is invisible too. It's the per-inference overhead that every AI system pays to exist in a world built for humans. We don't measure it. We don't optimize it. We accept it as the cost of doing business.

## The unspoken claim across the four

These four abstractions are the most pervasive AI-hostile abstractions in computing, precisely because we don't notice them.

Most of the friction in AI deployment isn't in the model. It's in the world the model has to live in. The model can produce excellent reasoning, but the reasoning has to be stored in a file system designed for human browsing. The model can coordinate with other agents, but the communication has to go over HTTP designed for client/server. The model can act on a user's behalf, but the action has to be authenticated as if a human did it. The model can generate content, but the content has to be a document a human can read.

Every translation is lossy. Every translation is slow. Every translation is invisible.

## The deeper pattern

This is the tenth post in a series that has covered the visible stack (databases, email, docs, tickets, Git, languages), the cultural layer (sprints, code review), the institutional layer (the engineering ladder), and now the invisible layer (file system, network, auth, documents).

What we have is a complete catalog of human-cognition-shaped abstractions projected onto AI work. Each post has made the same basic argument: identify the human constraint being optimized for, separate it from the underlying need, and redesign for the underlying need.

The closing post of the series will articulate the design principle that ties this catalog together: *serve the work, not the worker's cognition*. The work is the same. The worker's cognition is no longer reliably human.

---

*Next: [Post 11 — What "AI-First" Actually Means](./what-ai-first-actually-means.md) — the closing post. The design principle that ties the series together: serve the work, not the worker's cognition. The work is the same. The worker is not.*
