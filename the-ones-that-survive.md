# The Ones That Survive

*Post 1 of Prometheus. The frame and the method are in [Post 0](./the-emperor-has-no-clothes.md).*

---

Start with the good news, because the rest of the series has less of it.

Two of the most foundational tools in software need no redesign for AI at all. Their data models are abstract enough that they never assumed a human on either end. What needs to change is only the ceremony people built around them — and ceremony is cheap to change.

## Git

Git was created in 2005 by Linus Torvalds to manage the Linux kernel: thousands of contributors, distributed worldwide, changing code at a rate that had broken every previous version-control system. Its design is one of the great works of practical computer science. Snapshots of file trees, identified by the hash of their content, arranged in a graph, with branches as cheap pointers, distributed by default, cryptographically auditable.

Notice what that design does not assume. It does not assume a human author any more than TCP assumes a human sender. It does not assume a reviewer, a reader, or a workflow. An AI agent can clone, commit, push, and pull exactly as a person does, and the data model does not care.

The problem is everything people built on top:

- **Commit message conventions.** "Fix login bug" is written for a human skimming history. An agent committing every completed logical unit produces messages that are either generated noise or low-value metadata.
- **Branching strategies.** Git Flow, GitHub Flow, trunk-based development — coordination patterns for humans working in parallel. An agent works sequentially; its branches are routing ("work for ticket X goes on branch X"), not coordination.
- **The merge-versus-rebase debate.** Humans want linear history because humans read history. Agents query state.
- **Conventional commits and semantic versioning.** Release-management coordination for human release managers. The agent does not release; the system deploys.
- **Signed commits.** Cryptographic proof that a specific human authored this. Agent identity is fungible; the signature can prove "an authorized agent did this," which is a different and less useful claim.
- **The pull request.** Covered in [Post 2](./the-rituals.md), where it belongs.

Picture a repository where an agent has been working for an afternoon. Four hundred commits. Every message auto-generated from its diff. Squashed to one commit on integration. No merge commits, no history debate, conflicts resolved by regenerating from a shared base rather than hand-reconciling. The repository is perfectly healthy. The only thing missing is the story a human would have told about it — and the human who wants that story can have it rendered as release notes at the boundary, without the agent paying for it on every commit.

What survives: the whole data model, the protocol, the collaboration model. What goes: the culture. Git's ceremony outlived its audience the moment the author stopped being a person.

*Faster horse? No. The data model was never a horse.*

## Programming languages

The same verdict, one notch less clean.

A programming language is a specification of computation: values, operations, control flow, abstraction, composition. Its semantics do not assume a human author or reader. A correct Python program is correct regardless of who wrote it. Python optimizes for human readability, C for hardware access, Rust for memory safety without garbage collection, TypeScript for type safety over a dynamic ecosystem — and every one of those choices works for AI-authored code exactly as it works for human-authored code.

The frontier is not new languages. Attempts to displace Python have struggled; switching languages is expensive, disruptive, and rarely worth it. The frontier is the toolchain around the languages, which is human-shaped top to bottom:

- **The IDE.** File tree, outline view, autocomplete dropdown — instruments for a human navigating code by walking through it. An agent does not navigate; it queries. "Show all callers of Y." "Why is this structured this way?" The IDE becomes a query surface, not a file viewer. Today's AI-aware editors are early steps toward that.
- **Build systems.** Makefiles and Gradle DSLs written to be human-readable. The agent wants dependency declarations as structured data, reproducible builds, and configuration that humans can read as a feature rather than the goal.
- **Linters and formatters.** Most rules exist for human readability, not correctness. An agent generates consistent code without being told to; what it needs from static analysis is type checking, null safety, and dead-code and unreachable-branch detection.
- **Test frameworks.** Hand-written tests in a parallel file structure. The agent is better served by property-based testing (Hypothesis, fast-check), mutation testing, and tests generated alongside the code — tools human developers already have and mostly underuse.
- **Debuggers.** Step-through execution and breakpoints are a human's way of understanding a program by walking it. An agent wants a trace: full execution state, causal analysis ("what caused this?"), counterfactuals ("what if this input were different?").

This is constructive news. For an AI-augmented team the win is not switching languages; it is upgrading the toolchain, which is incremental, often reversible, and compounds.

Why the toolchain doesn't change on its own: engineers identify with their tools (Vim versus Emacs, pytest versus unittest — tribal markers), job descriptions and résumés list specific tools, and vendors have business models built on the current shape. None of that is about the code.

*Faster horse? The language isn't. The IDE is a faster horse with a file tree.*

## The verdict

Where the data model is genuinely abstract, the fix is conservative: leave the model alone, thin the ceremony. Git and languages are the two clearest cases in the stack, and they set the pattern for the harder ones. The next post covers tools where nothing underneath is abstract — where the whole thing exists to serve a human need that AI does not have.

---

*Previous: [Post 0 — The Emperor Has No Clothes](./the-emperor-has-no-clothes.md) · Next: [Post 2 — The Rituals](./the-rituals.md)*
