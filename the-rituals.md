# The Rituals

*Post 2 of Prometheus. The frame is in [Post 0](./the-emperor-has-no-clothes.md); the constructive cases are in [Post 1](./the-ones-that-survive.md).*

---

Every two weeks, in thousands of software organizations, the same thing happens. A team pulls work into a sprint. The work is estimated in story points. A daily standup tracks a burndown chart. Work that didn't get done rolls to the next sprint. Everyone feels like progress was made.

None of it is engineering. All of it is ritual — and the ritual exists because humans can't sustain focus indefinitely, can't estimate accurately, can't see what's in progress without a chart, and need rhythm to feel in control. AI has none of those constraints.

This post covers four rituals built on that same foundation: the bounded cycle, code review, the ticket state machine, and the engineering ladder. Each is defended as process. Each is, on inspection, anxiety management for humans — sometimes the workers', more often the managers'.

## Sprints

The Agile Manifesto, signed in 2001 by seventeen practitioners, was a reaction against waterfall: years of planning and testing, software that shipped late if at all. Scrum codified the answer as bounded cycles of two to four weeks, and every ceremony in the sprint answers a specific human limitation:

- Story points, because humans can't estimate time; relative complexity is easier than absolute hours.
- Velocity, because humans need a capacity number they can plan against.
- Burndown charts, because humans can't feel progress without seeing it shrink.
- Daily standups, because humans lose alignment between sync points and can't share state directly.
- Retrospectives, because humans need a periodic ritual to consolidate learning.

Remove the human and every one of these solves nothing. An agent has no recovery period to bound. It has no fixed capacity to measure. It can see the queue without a chart, share state without a meeting, and correct course immediately without waiting for a fortnightly retro.

What survives is the coordination underneath: a prioritized backlog, a definition of done, iterative delivery with feedback. What the agent actually needs is a work queue with quality-gated termination — items enter, get worked, and leave when they meet the bar. Done means done, not "sprint over." Continuous integration already runs on this logic at the level of commits; the sprint is the part of agile that never caught up with it.

The specimen already exists. There is a product on a major cloud marketplace that automates Scrum end-to-end with a team of specialized agents — a Standup Agent that captures updates, a Backlog Agent that prioritizes, a Planning Agent that balances workloads, a Retrospective Agent that extracts insights — and reports back into Jira and Teams. Every ceremony preserved. Every ceremony now performed by a system that has none of the limitations the ceremony was invented to manage. It is hard to imagine a purer example of the pattern this series is about, and it is for sale.

*Faster horse? A faster horse with a burndown chart — now with the horse automated too.*

## Code review

Pull requests as we know them arrived with GitHub in 2008. Before that, review was patches on a mailing list and someone looking over your shoulder. The PR formalized it — branch, push, assign reviewer, comment, approve, merge — and it earned its place by delivering four real things for human-authored code: catching what the author couldn't see, spreading knowledge across the team, enforcing agreed standards, and leaving an audit trail.

Most code being written today is generated. The ceremony still happens; the participants have changed. Take the four values one at a time:

- **The reviewer as teacher.** The author was supposed to learn. A model's weights don't update from a PR comment. The teaching function is gone.
- **The reviewer as second pair of eyes.** This assumed the reviewer brought a different perspective. When the agent has read the whole codebase and the reviewer has read the diff, the reviewer has *less* context, not a different one — and the agent's mistakes are not the kind a person with less context tends to catch.
- **Knowledge spreading.** The knowledge was never localized in a human head to begin with; it lives in the codebase and the model.
- **The audit trail.** This one survives. What changed, who signed off, when it shipped — compliance, postmortems, and archaeology still need it.

Picture the reviewer who spends three hours on a 900-line PR the agent produced in ninety seconds, approves it because nothing looked wrong, and discovers two weeks later that the bug was in a file the PR didn't touch. The review took longer than a review of human code would have, caught less, and made everyone feel better.

What the work actually needs: self-critique during generation (the first series' workspace argument), testing as the primary quality gate because tests scale and reviews don't, cross-model review for security boundaries and payment paths where a different training distribution is worth its cost, and human review of *architecture* — what to build and in what shape — which remains a human decision in most teams. The gatekeeping question, "does this ship?", still needs someone to answer it. The quality question, "is this correct?", is better answered by tests. The teaching question no longer applies.

We keep the ceremony because compliance frameworks written before code generation demand a human signature, because engineering culture is built around it, and because admitting we don't know how to review generated code well is harder than reviewing it badly. None of those reasons is about the code.

*Faster horse? Human-in-the-loop theater. A well-attended faster horse.*

## The ticket state machine

Jira exists for managers.

That isn't an insult; it's a description. The board view, the state transitions, the velocity chart, the sprint report — all of it exists so that someone not doing the work can see what's happening. Jira (2002) fused bug tracking (Bugzilla, 1998) with Kanban (Toyota's production system, adapted to software in the 2000s); Linear, Asana, Trello, and the rest are variations on the theme, and the theme is *visible progress*.

Watch what an agent does with a ticket: it picks it up and works until it's finished. There is no "In Progress" state that lasts long enough to be a signal. There is no "In Review" that a manager needs to notice has been stuck for three days. The transitions that make a board readable are the transitions an agent doesn't pause at.

What survives: the work item itself — title, description, priority, dependencies, acceptance criteria — is a useful record regardless of who does the work. What replaces the state machine: a few continuous signals per item (not-started, in-progress, blocked, done, failed, plus a confidence and a progress estimate), a queue processed continuously rather than in fortnightly batches, throughput instead of velocity, and a *query* interface for the humans who want visibility. The board can still exist — as a rendering, not as the system's source of truth. The mistake is letting the manager-facing view become the shape the work is done in.

*Faster horse? A faster status board.*

## The engineering ladder

This is the uncomfortable one, because it isn't a tool. Junior, mid, senior, staff, principal; manager, director, VP. The ladder is so foundational that questioning it feels like questioning the profession.

The ladder does real work for human organizations: it distinguishes experience, provides progression, gates high-stakes decisions, justifies pay, and gives people an identity. The uncomfortable observation is how the pieces hold each other up. The ladder gates access to certain tools — architecture review, design approval, hiring loops, promotion committees. Doing those things is what defines seniority. Seniority grants access to those things. Lords controlled land; land conferred status; status controlled land. It's a closed loop, and every institution inside it — the review board, the promotion process, the interview panel, the performance cycle — is justified by reference to the others.

Fairness requires the counter-argument. Maybe the loop is fine for what it was built for: predictable careers, defensible pay, authority paired with responsibility, something to work toward. And organizations without formal ladders do ship competitive software — Valve's handbook tells employees the org chart is "none of your business"; W.L. Gore has run on a lattice for decades; Linux, Python, and Kubernetes recognize maintainers by function, not level. These are existence proofs, not a controlled study.

So the claim here is narrow. For a purely human team whose goals include human career development, the ladder may be net positive. For a team whose output is AI-augmented, the priority flips: the ladder's functions still exist, but they are no longer the primary goal, and where they get in the way of the work they should give way. Concretely: function-based roles rather than levels (architect on one project, implementer on the next), continuous performance signals rather than a quarterly ritual, portfolio and work-sample hiring rather than whiteboard performance, and recognition that follows contribution rather than passing through a gate that senior engineers open once a year.

Of all the rituals in this post, this is the one most fiercely defended by the people it benefits. That isn't evidence it's wrong. It's evidence of where the discussion will be hardest.

*Faster horse? A faster feudal system.*

## The verdict

Every ritual here optimizes for a human constraint — recovery, estimation, visibility, status — and each one is preserved after the constraint is gone. The coordination underneath survives: a backlog, a definition of done, an audit trail, someone accountable for what ships. The ceremony doesn't. Removing it costs nothing technically. It costs the comfort of people whose role is defined by it, which is exactly why it persists.

---

*Previous: [Post 1 — The Ones That Survive](./the-ones-that-survive.md) · Next: [Post 3 — The Data Shapes](./the-data-shapes.md)*
