# The Code Review Ceremony

*Post 4 of a new series on the tools that shape AI work. [Post 1](./the-relational-cage.md) — databases. [Post 2](./the-inbox-that-eats-everything.md) — email. [Post 3](./the-sprint-that-wasnt-needed.md) — sprints. This post tackles the most engineering-culture-shaped ceremony: code review.*

---

Code review is one of the most-loved ceremonies in software engineering. The mythology says: experienced engineers mentor junior ones through review; bugs get caught before they ship; knowledge spreads across the team so no one person owns a piece of code; quality is maintained because humans hold each other to a standard.

The mythology was designed for a particular kind of work — human-authored code reviewed by other humans. Most code being written today is AI-generated. The ceremony still happens, but the people in it are different from the people it was designed for. The post argues that when the author is AI, code review becomes a ritual without a clear function.

We're keeping it because we don't know what to replace it with. Not because it's working.

## What code review was actually designed for

The pull request workflow as we know it came with GitHub in 2008. Before that, code review was informal: patch files emailed around, code reviewed over a shoulder, "hey can you look at this" in the hallway. The PR workflow formalized it — branch, commit, push, open PR, assign reviewer, comment, approve, merge — and the design choices optimized for specific values:

**Catching errors the author missed.** Humans have blind spots in their own work. Code reviewed by another person surfaces what the author doesn't see. This is the load-bearing value of code review for human teams.

**Spreading knowledge across the team.** When only one person knows a piece of code, that person becomes a bottleneck for changes. Review spreads context so multiple people can maintain each piece.

**Maintaining quality standards.** Senior engineers use review to enforce style, architecture, and patterns that the team has agreed on. The review is the operationalization of team norms.

**Creating an audit trail.** Who approved this? When? Why? The PR is a record. Compliance frameworks, postmortems, and code archaeology all depend on this trail.

These are real values. They were real for the human-authored, human-reviewed workflow that produced them. The question is whether they survive when one side of the equation becomes AI.

## What survives, what doesn't

Carrying the data-model-vs-workflow distinction:

**What survives in the PR workflow:**

- The data model: a diff with comments, an approval record, a merge timestamp. This is a useful structured artifact regardless of who wrote or reviewed the code. Compliance, postmortems, archaeology — all still benefit.
- The notion of staged changes with quality gates between stages. Work moves from "in progress" to "in review" to "approved" to "merged." This pattern is generic enough to survive.
- The audit trail. What changed, who signed off, when it shipped. AI teams need this as much as human teams.

**What doesn't survive:**

- The reviewer as teacher. The traditional argument was that authors learn from review feedback — they update their mental model of how to write code, what patterns to follow, what mistakes to avoid. When the author is an AI, no learning happens during review. The model's weights don't update from a PR comment. The teaching function is gone.
- The reviewer as second pair of eyes. The argument was that the reviewer brings a different perspective than the author — different mental model, different assumptions, different blind spots. When the reviewer and the author are both working from the same specification, and the AI author has more context than the human reviewer (because the AI read the entire codebase), the second-perspective advantage erodes.
- Knowledge spreading. When the author is human, the review spreads the knowledge of "how this code works" to the reviewer. When the author is AI, the knowledge of "how this code works" lives in the AI's training data and in the codebase itself, not in any human's head. The review doesn't spread knowledge because the knowledge was never localized.
- The duration of the review. Human code reviews take hours to days. The same review applied to AI-generated code often takes longer, because the reviewer is checking work they didn't write, in unfamiliar patterns, with less context than the author.

The interesting case is the **second pair of eyes**. When the AI author has read the entire codebase and the human reviewer has read only their own changes, the AI author has more relevant context than the reviewer. The reviewer's value is in catching what the AI missed — but the AI doesn't typically miss things the reviewer would catch. It misses different things. The reviewer doesn't have the right shape of context to catch them.

## What AI actually needs for "review"

Strip away the ceremony. What's left?

**Self-review during generation.** Workspace reasoning — covered in the first series — already does this. The model generates code with interleaved self-critique. The "review" happened during generation, not after. A separate review pass is redundant for the same-model case.

**Testing as the primary quality gate.** Tests are mechanical, repeatable, and don't have human cognitive limits. AI-generated code should be tested more thoroughly than reviewed. Tests scale; reviews don't.

**Cross-model review for high-stakes code.** For critical paths — security boundaries, payment flows, data integrity — a different model reviewing the code is worth the cost. The cost is real (10-100x the generation cost), but the value is also real: a different training distribution catches different errors.

**Audit trail for compliance.** The PR record still has value: what changed, who approved, when. This survives, transformed. Compliance frameworks should accept AI-audited code the same way they accept human-audited code.

**Architectural review by humans, when the architecture is novel.** The architectural decisions — what to build, what shape to build it in, what trade-offs to accept — those are still human-shaping decisions in most AI-augmented teams. Review of architecture survives. Review of code generated from architecture doesn't.

The pattern: the *gatekeeping* layer of code review (does this change ship?) survives, because someone still needs to make that decision. The *quality* layer (is this code correct?) gets replaced by testing and cross-model review. The *teaching* layer (does this author learn from this?) doesn't apply to AI authors.

## The unspoken claim

Code review is human-in-the-loop theater when the code is AI-generated. We're keeping the ceremony because we don't know what to replace it with — not because it's working.

The institutional reasons for keeping the ceremony:

- **Compliance frameworks require "human review."** Regulators in many industries (finance, healthcare, aviation) require a human to sign off on code changes. The frameworks predate AI code generation. They're being applied to AI-generated code because there's no other option, not because the application makes sense.
- **Engineering culture is built around the ceremony.** Code review is where senior engineers demonstrate judgment, where junior engineers learn, where team norms get enforced. Without it, what do senior engineers do? The ceremony is part of the job.
- **The alternative requires admitting uncertainty.** Nobody knows how to review AI-generated code well. Reviewing it badly is worse than admitting we don't know. So we keep doing what we were doing.

None of these reasons are about the code being better. They're about institutional inertia.

The honest position: most code review of AI-generated code is a ritual performed for the comfort of the humans involved, not for the quality of the code being reviewed. The reviewer's confidence in catching errors is lower than the data justifies. The author's learning is zero (the AI doesn't learn from review). The team's knowledge of the code is unchanged (the AI knew it all along).

We're keeping the ceremony because admitting we don't know how to review AI code is harder than performing the ceremony badly. That's an institutional fact, not an engineering one.

## The deeper pattern

The first three posts in this series made the case for tools (databases, email) and rituals (sprints) that are human-cognition-shaped. This post extends the case to the most engineering-culture-shaped ceremony: code review.

Each layer of the stack optimizes for a human constraint. When the constraint is removed, the optimization is wasteful. Code review optimizes for human authors and human reviewers. When the author is AI and the reviewer doesn't have the same context, the optimization is doing less than we think — and the time spent on it is time not spent on testing, cross-model review, or actual architectural decisions.

The fix in each case is the same: identify the human constraint being optimized for, separate it from the underlying need, and redesign for the underlying need alone. The underlying need for code quality survives (testing, cross-model review, architectural review by humans). The ceremony of two humans reviewing a diff doesn't.

The post that follows this one tackles the artifact most engineering teams believe is the most AI-friendly: documentation. The case there is that hierarchical prose — Confluence, Notion, wikis — was designed for human browsing, and AI doesn't browse.

---

*Next: [Post 5 — Confluence and the Hierarchical Prose Trap](./the-knowledge-graph-we-already-had.md) — every Confluence space is a knowledge graph someone flattened into prose for human browsing. The flattening lost information. AI doesn't browse, so the flattening serves no one.*
