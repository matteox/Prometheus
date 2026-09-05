# The State Machine That Wasn't Needed

*Post 6 of a new series on the tools that shape AI work. [Post 1](./the-relational-cage.md) — databases. [Post 2](./the-inbox-that-eats-everything.md) — email. [Post 3](./the-sprint-that-wasnt-needed.md) — sprints. [Post 4](./the-code-review-ceremony.md) — code review. [Post 5](./the-knowledge-graph-we-already-had.md) — documentation. This post tackles the work tracking system that defines how modern software organizations see themselves: the ticket.*

---

Jira exists for managers.

That's not an insult — it's a description. The board view, the state machine, the velocity chart, the sprint report — these are all artifacts for humans who need to see what's happening without doing the work themselves. They aren't tools for the people doing the work, and they certainly aren't tools for AI doing the work.

The post argues that ticketing systems — Jira, Linear, Asana, Trello, Monday, every Kanban-shaped tool — are optimized for a specific human need: *visible progress*. The state machine, the board view, the velocity metric all exist to make progress visible to managers. When the worker is AI, none of that visibility infrastructure does useful work. AI doesn't transition through states. AI doesn't have a velocity. AI doesn't need a board.

## What ticketing systems were actually designed for

The modern ticketing system is a hybrid of two older ideas: bug tracking (Bugzilla, 1998) and Kanban (Toyota production system, 1950s, applied to software in the early 2000s). Atlassian's Jira (2002) was the first major commercial combination of the two. Linear, Asana, Trello, and the rest are variations on the same theme.

The design choices optimized for specific values:

**Tracking work units.** A ticket represents a unit of work with a title, description, assignee, and priority. This survives in any system — work has to be tracked somewhere.

**Showing progress through state transitions.** The ticket moves from To Do to In Progress to In Review to Done (or some variant). The transitions tell managers how the work is going. A ticket stuck "In Review" for three days is a signal; a ticket that just transitioned to "Done" is a signal.

**Visualizing the queue through board views.** The Kanban board — columns representing states, cards representing tickets, cards moving across columns as work progresses — is the iconic visualization. Managers glance at the board and see what's happening.

**Measuring throughput through velocity.** Velocity is the sum of story points completed per sprint. It tells managers how much work the team can finish in a given period, used for planning.

**Holding people accountable.** Who is working on what? Who's blocked? Who hasn't moved a ticket in a week? The system records and surfaces all of this.

These are real values for human teams, especially for managers who aren't doing the work themselves. The question is whether the values survive when the worker is AI.

## What survives, what doesn't

Carrying the data-model-vs-workflow distinction:

**What survives in ticketing systems:**

- The work item as atomic unit. A ticket with title, description, priority, dependencies, due date — this is a useful record regardless of who does the work.
- Tracking who is responsible. AI agents have identities too; assigning work to an agent is meaningful.
- Priority and severity metadata. Some work is more urgent than other work; the system can route accordingly.
- Dependencies between items. AI work has dependencies just like human work; representing them is useful.

**What doesn't survive:**

- The state machine. AI doesn't transition through To Do → In Progress → In Review → Done. AI works continuously on an item until it's done. There is no "in progress" state — the work is either happening or it isn't. The transition that exists is binary: not done, done.
- The board view (Kanban columns). The board exists because humans need a glance-able visualization of state across many tickets. AI doesn't glance; it queries. A human-facing dashboard can render the queue however makes sense, but the *primary* representation isn't a board.
- Velocity. Velocity measures story points per sprint for human teams. AI produces work at a different rate than humans — usually much faster, in bursts, with no fixed capacity. Velocity is unmeasurable for AI systems, both because story points don't apply and because the rate of completion isn't a fixed capacity number.
- Sprint reports and burndown charts. Same reasoning as Post 3 — sprints are human-cognition artifacts.
- The assignee-as-accountability mechanism. AI agents don't have careers at stake when a ticket sits "In Progress" too long. The accountability framing doesn't apply.

The pattern is similar to Post 1 (databases): the *data* of the ticket (title, description, priority, dependencies) survives, transformed. The *workflow* around the ticket (state machine, board view, velocity) doesn't.

## What AI actually needs for "work tracking"

Strip away the state machine and the board view. What's left?

**A work queue.** Items enter the queue. Each item has a description, priority, dependencies, and acceptance criteria. The queue is the source of truth.

**A single progress field per item, not a state machine.** For each work item: how far along is it? Is it blocked? Has it failed? Has it succeeded? A small set of progress signals — not a multi-state workflow. Something like:

```
status: not-started | in-progress | blocked | done | failed
confidence: 0.0 - 1.0  (how confident the system is that this will complete successfully)
progress: 0.0 - 1.0    (how much of the work has been done, if measurable)
```

No transitions. The status is set; the model updates confidence and progress as it works. When status becomes "done" or "failed," the item leaves the queue.

**Continuous processing, not sprint-batched.** The work items flow through the queue continuously. No two-week batches. The system processes what's next whenever it has capacity. (For AI, "whenever it has capacity" is roughly "all the time.")

**Query interface for humans, not a board.** Humans want to see what's happening. The right interface is a query interface: "show me all items in progress," "show me items that failed in the last 24 hours," "show me high-priority items that haven't been touched." These can be rendered as lists, tables, or Kanban-style boards — the board is a *view*, not the underlying representation.

**Throughput metrics, not velocity.** Throughput is items completed per unit time. Velocity is story points per sprint. For AI work, throughput is meaningful; velocity is not. A team processing 100 items per hour is observably different from a team processing 10 per hour. The number is real, even if the underlying unit (story points) is human-shaped.

The closest existing things:

- **Linear** with the state machine collapsed. Linear's data model is reasonably good; the state machine is the main friction.
- **Simple FIFO or priority queues.** Most programming languages have queue primitives; work tracking is mostly queue management with metadata.
- **Notion databases** (the structured side, not the page side). Approximate this.
- **Specialized agentic-workflow tools** (LangChain, LlamaIndex task systems, etc.). Often have work-queue patterns baked in.

A real AI-first work tracking system would have:

- Work items as structured records (not stateful tickets)
- Progress signals (continuous fields, not discrete states)
- Query interfaces for humans (the board is a render, not the source)
- Throughput metrics (not velocity)

This exists in fragments. We're maybe two to three years from a credible packaged solution that replaces the Kanban-shaped tools for AI-augmented teams.

## The unspoken claim

The board view is for human managers; AI doesn't need it. Velocity is unmeasurable for a system that produces a day's work in seconds.

The institutional reasons the Kanban shape persists:

- **Managers want visibility.** That's a real need. But the right solution is a query interface that managers can render however they want, not a fixed Kanban board that's the system's primary representation.
- **The board is a cultural artifact.** Software organizations have internalized "we have a Kanban board" as part of their identity. Removing it feels like removing a vital organ.
- **The board provides accountability.** A ticket stuck "In Progress" for a week is a conversation starter. Removing the board removes the conversation.

The honest position: the Kanban board exists for managers, not workers. It does its job for managers. It does nothing for AI workers. The mistake is letting the manager-facing artifact become the system's primary representation, which then shapes how the work is done.

We're keeping the Kanban shape because admitting it's a manager-facing artifact, not a worker-facing one, is uncomfortable. It implies the work is being tracked for the benefit of someone other than the worker. That implication is true. Most organizations don't want to admit it.

## The deeper pattern

The first five posts in this series made the case for tools (databases, email, docs) and rituals (sprints, code review) that are human-cognition-shaped. This post extends to the system that organizes the work itself: the ticket.

The pattern is consistent: each layer of the stack optimizes for a human constraint, often a *manager's* constraint rather than a *worker's* constraint. Ticketing systems optimize for manager visibility. AI doesn't need manager visibility; AI needs to do the work.

The fix in each case is the same: identify the human constraint being optimized for (often visibility, often accountability, often emotional reassurance), separate it from the underlying need (track work, coordinate across workers, measure throughput), and redesign for the underlying need. The underlying need for tracking survives. The state machine doesn't. The board view doesn't. Velocity doesn't.

The post that follows this one tackles the system that holds the work itself: version control. Git's data model is the cleanest case in the series — the data model survives, but the workflow conventions around it don't.

---

*Next: [Post 7 — Git and the Ceremony That Outlived Its Audience](./the-ceremony-that-outlived-its-audience.md) — the cleanest case in the series. The data model survives. The workflow — branches, merges, conflicts, commit messages — is the part that doesn't translate to AI work.*
