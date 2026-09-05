# Prometheus

A six-post series on the tools that shape AI work — and the human-cognition-shaped projections that limit them.

## The thesis

If the first series — [The Comfortable Cage](https://github.com/matteox/Comfortable-Cage) — argued that humans project organizational shapes onto AI reasoning (org charts, roles, SDLC), this series argues the parallel case for tools: humans project human-tool shapes onto AI development workflows, with the same limiting effect.

The framing is **the emperor has no clothes** — naming what everyone using these tools already knows and is not allowed to say, because their job depends on the tool. The name draws on the classical allusion: Prometheus gave humanity fire, the most important tool. This series asks which of the tools we've built since are still doing useful work in an AI-first world, and which have become faster horses.

## Reading order

The series is organized by verdict, not by tool. Post 0 sets the frame and the method; Posts 1–3 each cover one verdict; Post 4 states the principle the verdicts share; Post 5 draws the architecture that follows from it and closes both series.

| # | Post | Verdict | Tools covered |
|---|---|---|---|
| 0 | [The Emperor Has No Clothes](./the-emperor-has-no-clothes.md) | The frame, the method, and the lens: *is this just a faster horse?* | — |
| 1 | [The Ones That Survive](./the-ones-that-survive.md) | The data model is abstract; only the ceremony dies | Git, programming languages |
| 2 | [The Rituals](./the-rituals.md) | The tool exists for human anxiety, visibility, or status | Sprints, code review, ticket state machines, the engineering ladder |
| 3 | [The Data Shapes](./the-data-shapes.md) | The data model itself is human-shaped; make structure the source and the familiar form a view | Databases, email, documentation, the file system, the network, auth, documents |
| 4 | [What "AI-First" Actually Means](./what-ai-first-actually-means.md) | Serve the work, not the worker's cognition | — plus a worked example of the translation cost |
| 5 | [Leaving the Cage](./leaving-the-cage.md) | The building. An AI-first architecture assembled from both series — five layers, a worked example, and how to start | Closes both series |

## The argument in one paragraph

Every tool in a software company was designed for how humans think. In an AI-first world those tools either get redesigned to serve the work — with humans as one audience among two — or they become the bottleneck. The first series made the case at the reasoning layer: org charts are the artifact of liability allocation, and we keep reaching for role-shaped AI systems because role shapes are what institutions can defend. This series makes the case at the tool layer: tool stacks are the artifact of human accountability, and we keep reaching for human-shaped tools because that's what humans know how to defend. Where the data model is abstract (Git, languages) it survives and the ceremony goes. Where the tool is ritual (sprints, review, boards, ladders) the coordination survives and the ceremony goes. Where the data model itself is human-shaped (databases, email, docs, the invisible layer) structure becomes the source and the familiar form becomes a view. Two series, one trap, and — in the closing post — one building.

## What this series is not

Not a how-to guide. Not a research survey. Not a balanced both-sides treatment — every claim is an argument, not a finding, and the series is intended to start discussion rather than close it. Not anti-human: humans are still in the loop for architecture, accountability, and judgement. The argument is about which human contributions add value and which are vestigial.
