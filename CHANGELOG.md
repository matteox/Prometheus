# Consolidation Changelog — Prometheus

The series was restructured from 13 files (README, position paper, 11 per-tool
posts, quantitative companion) to 6 (README plus five posts), organized by
verdict rather than by tool. Word count went from 21,835 to 5,626 across the
posts — a 74% cut — with no tool dropped and no factual claim removed except
where it was wrong (see "Corrections").

## Why

Measured across the 11 original per-tool posts, the same section scaffolding
recurred in nearly every one:

| Boilerplate | Occurrences |
|---|---|
| "The deeper pattern" closing section | 10 of 11 |
| "The unspoken claim" closing section | 10 of 11 |
| "What survives, what doesn't" | 9 of 11 |
| "Strip away X. What's left?" | 9 of 11 |
| "What AI actually needs for..." | 8 of 11 |
| "Carrying the data-model-vs-workflow distinction" | 7 of 11 |
| "The closest existing things:" | 7 of 11 |
| "We're maybe N years from a credible..." | 7 of 11 |

Structurally it was one post with the tool name swapped out eleven times.
Consolidating by verdict removes the repetition at the source instead of
trimming it eleven times.

## Old → new mapping

| Old file | Now lives in |
|---|---|
| `README.md` | `README.md` (rewritten) |
| `the-emperor-has-no-clothes.md` | `the-emperor-has-no-clothes.md` (rewritten, same filename) |
| `the-ceremony-that-outlived-its-audience.md` (Git) | `the-ones-that-survive.md` |
| `the-languages-wont-change.md` | `the-ones-that-survive.md` |
| `the-sprint-that-wasnt-needed.md` | `the-rituals.md` |
| `the-code-review-ceremony.md` | `the-rituals.md` |
| `the-state-machine-that-wasnt-needed.md` (Jira) | `the-rituals.md` |
| `the-feudal-system.md` (engineering ladder) | `the-rituals.md` |
| `the-relational-cage.md` (databases) | `the-data-shapes.md` |
| `the-inbox-that-eats-everything.md` (email) | `the-data-shapes.md` |
| `the-knowledge-graph-we-already-had.md` (docs) | `the-data-shapes.md` |
| `the-invisible-abstractions.md` | `the-data-shapes.md` |
| `what-ai-first-actually-means.md` | `what-ai-first-actually-means.md` (rewritten, same filename) |
| `the-translation-tax.md` | one worked example kept as a sidebar in `what-ai-first-actually-means.md`; the rest dropped |

Two filenames were deliberately preserved (Post 0 and the closing post) so
that external links to the frame and the close keep working. The other eleven
files are superseded and should be deleted from the repo, or left in place with
a one-line pointer to their new home if inbound links matter.

## Structural changes

- **Organized by verdict, not by tool.** Post 0 sets frame and method; Posts
  1–3 each cover one verdict (data model survives / tool is ritual / data model
  itself is human-shaped); Post 4 closes. The verdict table in Post 0 doubles
  as the table of contents.
- **Reordered for trust.** The constructive verdict (Git, languages — "the
  model is fine, only the ceremony changes") now comes first, so the reader
  agrees with the series before it asks them to accept the contested claims.
  The original opened with databases, its hardest sell and its shakiest
  technical argument.
- **The method is explained once** (Post 0) and then applied without
  narration. The original re-introduced the framework in every post.
- **One closing beat per section** instead of the three-section closing triad
  ("The unspoken claim" / "The honest position" / "The deeper pattern") that
  ended nearly every original post by restating its thesis three times.
- **The "we're maybe N years from" prediction** — seven interchangeable,
  unfalsifiable timelines in the original — is now one sentence in Post 0
  ("timelines are guesses, and this series won't pretend otherwise").
- **Breadcrumb intros** listing every prior post inline (Post 8's opener listed
  seven) are replaced by a one-line prev/next nav, as in the first series.
- **The faster-horse lens actually runs.** Post 0 invited the reader to hold
  the question throughout, and the original then barely used it. Each section
  now closes on a single one-line answer (*"A faster horse with a burndown
  chart."* / *"No. The data model was never a horse."*) — about ten across the
  series, which is the recurring beat without the tic.
- **One concrete scenario per major section** — the agent's 400-commit
  afternoon, the three-hour review of a 90-second PR — so the argument has at
  least one thing the reader can picture. The original was assertion
  throughout.

## Corrections

- **ACID (databases).** The original claimed ACID guarantees exist "for
  human-observed consistency" and that AI "doesn't need the strict atomicity
  ACID provides." That's wrong — atomicity and isolation prevent concurrent
  writers from double-booking or double-spending, and the problem is identical
  when the writers are agents. Rewritten to say the *default* shifts toward
  eventual consistency with provenance while strict transactions remain right
  for the cases that need them.
- **The Ford opening (Post 0).** The original used the flat
  `— Henry Ford (attributed, almost certainly apocryphal)`, which gives away the
  reveal in the attribution line. Replaced with the version developed for the
  first series: the quote stands clean, the reveal arrives a beat later in
  prose, closing on "Sounds good, though." — then bridges directly into the
  series' subject ("this series is about tools that sound right").
- **Leaked editorial notes removed** — three passages that read as drafting or
  reviewer comments left in the published text: "developed and refined through
  the talking-points work" (Post 0); "and this is the one to land without
  stating:" immediately followed by stating it, twice (docs post); "a critique
  correctly observed that the per-tool tables are where the overreach lives"
  (translation tax).
- **Broken links fixed** — the relational-cage post's "next" link was a bare
  `(#)`; the state-machine post linked to a typo'd
  `the-inbox-that-eats-eats-everything.md`. All links in the new set were
  verified to resolve.
- **Translation tax reduced to its defensible core.** The original's per-tool
  token tables, ranked annual-cost table ($660K–2.06M), enterprise scaling
  ($6.6M–20.6M), and savings projections were stacked estimates presented with
  dollar-figure precision. Kept only the single compounded-task worked example
  (the ~2.6× multiplier), explicitly labeled illustrative, as a sidebar.

## Not changed

- No tool was dropped. All fourteen subjects from the original (Git,
  languages, sprints, code review, tickets, the ladder, databases, email,
  documentation, file system, network, auth, documents, plus the translation
  cost) are covered.
- All verified historical facts carried over unchanged: Codd 1970, Tomlinson
  1971, SMTP 1982, Lotus Notes 1989, Outlook 1997, Gmail 2004, Agile Manifesto
  2001, Bugzilla 1998, Jira 2002, Git 2005, GitHub pull requests 2008, the
  Valve / W.L. Gore / open-source examples.
- The author's voice — short declarative openers ("Jira exists for managers."
  / "Postgres is the wrong shape for AI.") — was kept and used more
  consistently, not softened.
- The series' explicit self-description as argument rather than finding is
  preserved in Post 0 and the README.

## Addition — Post 5, Leaving the Cage

Both series ended on diagnosis ("the rest is execution"). A capstone post
now closes them constructively: an AI-first architecture assembled from the
pieces both series argued for, in five layers (knowledge, reasoning,
coordination, legibility, humans), with an ASCII layer diagram, a worked
example contrasting the SDLC pipeline and the AI-first flow on the same
feature request, an explicit "what's not solved" list carried over from
both series, six independently reversible first moves, and a closing that
applies the faster-horse test to the architecture itself.

Wired in: Prometheus README (now six posts), Post 0's method section, Post
4's closing line and nav; Comfortable Cage README and Part 5's end-of-series
line both point to it. No claims in the capstone are new — every component
is one already argued for in a prior post; the capstone only assembles them.
