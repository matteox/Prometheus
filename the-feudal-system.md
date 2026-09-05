# The Feudal System

*Post 9 of a new series on the tools that shape AI work. [Post 1](./the-relational-cage.md) through [Post 8](./the-languages-wont-change.md) covered the technical stack. This post tackles the most institutionally entrenched layer: the engineering ladder, hiring, performance review, and promotion.*

---

Every software engineer knows the ladder. Junior, mid, senior, staff, principal, distinguished. Manager, director, VP, CTO. The ladder is so foundational to engineering culture that questioning it feels like questioning the profession itself.

The post argues that the engineering ladder is human-cognition-shaped — like every other tool in this series — and that the institutions built around it are feudal in ways we don't usually say out loud. Unlike the earlier posts, where the fix was "redesign the workflow" or "build better tooling," the fix here is structural: dismantle the institutions that depend on the ladder.

This is the most uncomfortable post in the series. It's also, by some distance, the most important.

## What the ladder was actually designed for

The engineering ladder serves real functions in human organizations:

- **Distinguishing levels of experience and authority.** Senior engineers know more than juniors. The ladder makes that distinction formal.
- **Providing career progression.** Engineers want to grow. The ladder is how they grow.
- **Gating access to high-stakes decisions.** Architecture review, design approval, hiring — these are gated to senior engineers.
- **Justifying compensation differences.** Engineers at higher levels earn more. The ladder justifies the difference.
- **Creating professional identity.** "I'm a Staff Engineer" is a meaningful statement about who you are and what you've accomplished.

These functions are real, and they have worked for human engineering organizations for decades. The argument isn't that the ladder is broken for humans. The argument is that the ladder is shaped for humans in ways that don't translate to AI-augmented teams, and that the institutions built around it become harmful when AI is doing the work.

## The feudal structure

The engineering ladder has a structure that mirrors feudalism in ways we don't usually acknowledge:

- The ladder **gates access to tools** — architecture review, design approval, hiring decisions, promotion votes.
- The tools **justify the ladder** — to be a senior engineer, you need to be doing senior things. Senior things are defined as "things senior engineers do."
- This is a **mutual justification loop**: the ladder needs the tools to have content, and the tools need the ladder to have access.

In feudal terms: lords controlled access to land. Access to land gave lords status. Status enabled lords to control access to land. The same loop.

The institutions of engineering management reproduce this loop:

**Architecture review boards** — senior engineers approve designs. Senior engineers are defined by their approval authority. Approval authority gives senior engineers the standing to approve designs.

**Promotion processes** — senior engineers decide who advances. Promotion validates seniority. Seniority enables promotion decisions.

**Hiring loops** — senior engineers interview and select. Hiring authority validates seniority. Seniority enables hiring authority.

**Performance reviews** — managers rate reports. Rating authority validates management. Management enables rating authority.

Each institution reinforces the others. Each is justified by reference to the others. The system is closed.

## Hiring, performance review, promotion

Three institutions that explicitly carry the feudal structure:

**Hiring** — interview processes test human cognition: communication, personality, whiteboard performance, system design discussion. None of these are skills AI-augmented teams need from new hires in the same shape. The unspoken claim: *most engineering interviews don't predict job performance even for human candidates. They predict how well candidates perform in interviews. The whole apparatus is a sorting ritual dressed up as evaluation.*

For AI-augmented teams, hiring should be:
- **Portfolio-based** — what has the person built?
- **Work-sample based** — give them the actual work and see what they produce
- **Model-assisted** — use AI to assess portfolios and work samples systematically

**Performance reviews** — periodic rituals where managers rate reports. The functions are: documentation for HR decisions, perceived accountability, perceived fairness. The reality is: reviews feel like accountability; they function as documentation for later HR decisions. They are organizational anxiety management, not actual feedback mechanisms.

For AI-augmented teams, performance should be:
- **Continuous** — output quality signals collected as work happens
- **Multi-dimensional** — collaboration, technical quality, impact, learning
- **Visible to the person** — no surprises in a quarterly ritual

**Promotion** — senior engineers decide who advances through the ladder. The promotion process is the explicit gate. The unspoken claim: *promotion processes are how senior engineers preserve the value of being senior. The gate exists to be a gate, not to ensure quality.*

For AI-augmented teams, advancement should be:
- **Project-based** — what did you build, what impact did it have?
- **Function-based** — what role are you playing, what skills are you demonstrating?
- **No gate** — recognition comes from contribution, not from gatekeeping

## What AI-augmented teams actually need

Strip away the ladder. What's left?

**Function-based roles, not levels.** Architect, implementer, reviewer, mentor — these are functions people play. They aren't a ladder. Someone can be an architect for one project and an implementer for another. The function follows the work, not the other way around.

**Continuous signals, not periodic reviews.** Performance data should be collected as work happens — output quality, collaboration signals, learning trajectory, impact. The data is continuous; the ritual is not needed.

**Portfolio evaluation, not whiteboard interviews.** Show me what you've built. Talk me through the decisions. Let me see how you think about hard problems. Don't perform algorithm inversion under time pressure.

**Project-based recognition, not promotion.** Recognition comes from what you've shipped, what you've taught, what you've made possible. Not from a gate that senior engineers open once a year.

The closest existing things:

- **Open-source communities** have function-based recognition (maintainer, contributor). The recognition follows the contribution.
- **Some companies** have moved to continuous feedback (Microsoft's "Connects," Adobe's "Check-ins"). Most still have periodic reviews layered on top.
- **A few organizations** have removed levels entirely (Valve's flat structure, Gore's lattice). These are early experiments.

A real AI-first organization would have all of these. We're maybe 5-10 years from this being a credible default for AI-augmented teams. The challenge isn't technical — it's institutional. The institutions that benefit from the ladder will resist dismantling it.

## The unspoken claim

The engineering ladder is a feudal system. The "senior" title gives the holder access to tools (architecture review, design approval, hiring, promotion) that justify the title. Take away the tools, the title has no content.

The title is not the source of authority. The tools are. The tools give the title its meaning. The title gives access to the tools. The mutual justification is what makes the system stable.

This is the post in the series with the deepest institutional implications. The earlier posts argued that specific tools need redesign or replacement. This post argues that the institution itself needs to change. The engineering ladder, the interview process, the performance review, the promotion gate — these are not separate tools; they're parts of a single institutional structure that has shaped engineering culture for decades.

## The deeper pattern

The first eight posts in this series made the case for tools that are human-cognition-shaped. This post extends to the institution itself.

The pattern is consistent: each layer of the stack optimizes for a human constraint, and each layer is defended by institutional interests. The engineering ladder optimizes for human career progression, identity, and gatekept authority. The institutions built around it (hiring, review, promotion) optimize for the same things.

The fix in this case is more radical than in earlier posts. We're not redesigning a workflow or upgrading a toolchain. We're dismantling institutions. The architecture review board, the promotion committee, the interview loop — these are mutual justification systems that benefit the people inside them at the cost of the people outside them.

The post that follows this one tackles the most invisible layer: the abstractions nobody questions because they're so foundational we don't see them as tools. The file system. The network stack. The authentication system. The document.

---

*Next: [Post 10 — The Ones Nobody Questions](./the-invisible-abstractions.md) — the file system, the network protocol, the auth stack, the document. The abstractions so foundational we don't notice them. Each one is human-shaped in ways that limit what AI can do — and each one is invisible enough that nobody is allowed to question it.*
