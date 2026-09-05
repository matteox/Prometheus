# The Ceremony That Outlived Its Audience

*Post 7 of a new series on the tools that shape AI work. [Post 1](./the-relational-cage.md) — databases. [Post 2](./the-inbox-that-eats-everything.md) — email. [Post 3](./the-sprint-that-wasnt-needed.md) — sprints. [Post 4](./the-code-review-ceremony.md) — code review. [Post 5](./the-knowledge-graph-we-already-had.md) — documentation. [Post 6](./the-state-machine-that-wasnt-needed.md) — Jira. This post covers the cleanest case in the series: Git.*

---

Of every tool in this series, Git is the cleanest case.

Its data model is genuinely abstract — it doesn't assume a human author any more than TCP assumes a human sender. Snapshots of file trees, identified by SHA-1 hashes of their content, arranged in a DAG with branches as cheap pointers. None of this needs a human-shaped ceremony to function. Git is, structurally, almost the only tool in this series that doesn't need to be redesigned for AI.

The problem isn't Git. The problem is the ceremony humans built around Git, which outlived its audience the moment AI started writing code. The data model survives. The culture dies.

## What Git was actually designed for

Git was created in 2005 by Linus Torvalds to manage the Linux kernel — a project with thousands of contributors, distributed across the world, with code changing at a rate that broke every previous version control system. Git's design is one of the great works of practical computer science: content-addressed snapshots forming a DAG, branches as cheap pointers, distributed by default, cryptographically auditable.

The design choices optimized for specific values:

**Snapshots, not diffs.** Every commit is a full snapshot of the file tree. This makes most operations efficient — checking out a commit, comparing two states, finding when a change was introduced.

**Content-addressed storage.** Files and commits are identified by the SHA-1 hash of their content. This is what makes Git distributed — two clones of the same repository can verify they have the same content without trusting each other.

**Branches as pointers.** A branch is just a name pointing to a commit. Cheap to create, cheap to delete. The whole branching model is built on this primitive.

**Distributed.** No central server required. Every clone is a full repository. (Most teams use a central server for coordination, but the data model supports pure distribution.)

**Cryptographic integrity.** The hash chain makes tampering detectable. Every commit is signed by its parent's hash; the chain of hashes back to the initial commit is verifiable.

These are abstract properties. They don't assume a human author. They don't assume a human reviewer. They don't assume a human workflow. The data model is, in the truest sense, post-human.

## What survives, what doesn't

Carrying the data-model-vs-workflow distinction:

**What survives in Git:**

- The data model. Snapshots, content addressing, branching as pointers, distributed by default. All of this works for AI-authored code the same as human-authored code.
- The protocol layer. Push, pull, fetch — these are abstract operations on the data model. They work for AI.
- The collaboration model. Central repository plus clones. AI agents can clone, commit, push, pull, the same as humans.

**What doesn't survive:**

- Commit message conventions. "Fix login bug" or "Add validation" are written for humans skimming history. AI commits are denser and more frequent; messages become low-value metadata or generated noise.
- Branching strategies. Git Flow, GitHub Flow, trunk-based development — all are human coordination patterns. AI doesn't coordinate via branches the way humans do. The model works sequentially; the branching is for review and release, not for parallel development.
- Pull request ceremonies. Covered in Post 4 — human-shaped review workflow that doesn't translate to AI.
- Merge vs rebase debates. Humans care about linear history for readability. AI doesn't read history; it queries state.
- Conflict resolution patterns. Humans create conflicts by editing the same file at the same time. AI doesn't have that pattern; when conflicts do arise (model producing code, then producing different code on the next attempt), they're different conflicts.
- Conventional commits, semantic versioning. Commit types (feat, fix, chore) and version bumps are coordination mechanisms for human release management. AI doesn't release; the system deploys.
- Signed commits, GPG keys. Cryptographic identity verification of the author. AI identity is fungible; the signature proves "this was committed by *some* authorized agent," not by a specific human.

The data model survives entirely. The ceremony around it doesn't.

## What AI actually needs from version control

Strip away the ceremony. What's left is what AI needs:

**The data model, unchanged.** Snapshots, content addressing, branching. All of this works exactly the same for AI-generated code.

**A leaner workflow.** Specifically:

- **Auto-commits.** Every N seconds of generation, or every completed logical unit, the AI commits. The commit is automatic; no human writes it.
- **Minimal or generated commit messages.** Metadata-only ("commit abc123 by agent at time T") or auto-generated from the diff. No human-meaningful narrative.
- **Branching as routing, not coordination.** Branches created by routing rules — "work for ticket X goes on branch X" — rather than by humans deciding to coordinate.
- **No PR ceremony.** Direct integration or automated review. No human approval gate for routine code; high-stakes code gets cross-model review (from Post 4).
- **Squash on integration.** When work lands on the main branch, it lands as a single commit. No merge commits to clean up; no "preserve history" debate.
- **Conflict resolution by re-generation.** When conflicts arise (model A produced code that conflicts with model B's later work), the right answer is usually to re-generate from a shared base, not to manually reconcile.

This is much leaner than the typical human Git workflow. It assumes the author is AI, the reviewer is automated, and the consumer is automated.

The interesting case: what survives when humans are involved? Some ceremony persists when humans are in the loop — humans want to read commit messages, humans want release notes, humans want merge commits to trace history. For AI-human teams, the workflow should support both modes:
- AI-to-AI work uses the lean workflow above
- AI-to-human work preserves the human-facing ceremony
- The transition between modes is at integration (humans see release notes, not the underlying commit graph)

## The unspoken claim

Git's data model survives because it's abstract. Git's culture dies because it's human-shaped. The ceremony outlived its audience.

The institutional reasons the culture persists:

- **Engineers have strong opinions about Git workflows.** git rebase vs merge, trunk-based vs GitFlow, conventional commits vs freeform. These are tribal markers. Engineers identify with their Git workflow preferences.
- **The ceremony provides visible process.** A clean PR with good commit messages and a linear history is *evidence of engineering hygiene*. Removing it removes the evidence.
- **The tools around Git enforce the ceremony.** GitHub's UI, Bitbucket's UI, GitLab's UI — all reinforce the PR-ceremony, code-review, human-approval workflow. Changing the workflow requires changing the tools.

The honest position: Git itself is fine. The culture around Git is built for human authors and reviewers, and the culture is now limiting what AI can do with the data model. We're keeping the culture because admitting it's the bottleneck means admitting that years of accumulated Git workflow conventions are slowing AI down.

## The deeper pattern

The first six posts in this series made the case for tools (databases, email, docs, ticket systems) and rituals (sprints, code review) that are human-cognition-shaped. This post is the cleanest case — the data model survives, only the ceremony dies.

The pattern is consistent across the series: every layer of the stack has both data and ceremony. Some layers have data that doesn't survive (most of what we've covered). Git is unusual in having a data model so abstract that it survives entirely. The ceremony is still human-shaped — branches, PRs, commit messages, merge conflicts — but the underlying representation of state is post-human.

The fix in this case is more conservative than for previous posts. We're not redesigning Git; we're thinning the workflow around it. For AI-only work, the workflow can be radically lean. For AI-human teams, the workflow needs to support both modes — but the AI mode shouldn't be held back by the human mode.

The post that follows this one tackles the most fundamental tool in software: programming languages. Like Git, the languages themselves mostly survive — Python, JavaScript, TypeScript work for AI. But the tooling around them is human-shaped.

---

*Next: [Post 8 — The Languages Won't Change, But the Tooling Will](./the-languages-wont-change.md) — the second cleanest case in the series. Programming language semantics survive. The IDE, the build system, the package manager, the linter — all human-shaped around human cognition.*
