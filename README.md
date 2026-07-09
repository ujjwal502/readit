# ReadIt 📖

Turn any codebase into a **reference-grade book** — with chapters, runtime flows, and
Mermaid diagrams — so people actually understand what's happening instead of vibe
coding.

ReadIt is a [Claude Code](https://docs.claude.com/en/docs/claude-code) plugin. Point
it at a repo, run `/book`, and it produces a `book/` folder you can read front to
back or publish as docs.

**The bar:** after reading the book, a competent engineer should be able to answer
virtually any question about the system — design rationale, architecture, data flow,
contracts, edge cases, failure modes, and implementation details — *without going back
to search the source.* If a plausible question can't be answered from the book, the
book isn't done.

## What it answers

Every chapter is written against eight knowledge categories:

1. **Purpose & domain** — what it does, for whom, and the core concepts.
2. **Architecture** — components, boundaries, and dependency direction.
3. **Design rationale** — why it's built this way, trade-offs, invariants.
4. **Control & data flow** — exact step-by-step paths, with real traced examples.
5. **Interfaces & contracts** — signatures, validation, pre/postconditions, errors.
6. **State & data** — the data model, lifecycles, and state transitions.
7. **Failure, edge cases & security** — "what happens if…", concurrency, retries, auth.
8. **Operations & change** — how to run, configure, test, and extend it.

## What it generates

- **Start Here** — a 10-minute onboarding: what it is, how to run it, the mental model.
- **The Big Picture** — an architecture overview with a top-level diagram.
- **Core Concepts** — the domain vocabulary and mental model.
- **One deep chapter per subsystem** — Auth, Data Layer, Request Lifecycle, etc.
- **Data Model** — entities and relationships (`erDiagram`).
- **Key Flows** — end-to-end walkthroughs as Mermaid sequence diagrams.
- **Cross-Cutting Concerns** — config, errors, logging, auth, concurrency, caching.
- **Failure Modes & Edge Cases** — what breaks and how it's handled.
- **How Do I…?** — a cookbook of common change recipes, grounded in real files.
- **Glossary** — domain terms as used *in this codebase*.
- **Quizzes** — a grounded active-recall quiz after every chapter, plus an interleaved
  final exam, so reading actually sticks. Answers cite real `file:line` and double as
  review. (On by default; `--no-quiz` to skip.)

Every claim is grounded in real files — ReadIt reads the code before it writes about
it, labels inferences as inferences, and says so honestly when something is unclear.

## See it in action

Here's a **complete book ReadIt generated automatically** from the
[`expressjs/express`](https://github.com/expressjs/express) source (v5.2.1), with no
hand edits — 15 chapters, ~26,800 words, 400+ `file:line` citations:

**👉 [ujjwal502/readit-example-express](https://github.com/ujjwal502/readit-example-express)**

Good chapters to skim first: the
[Table of Contents](https://github.com/ujjwal502/readit-example-express/blob/main/README.md),
[The Big Picture](https://github.com/ujjwal502/readit-example-express/blob/main/01-the-big-picture.md),
and [Key Flows](https://github.com/ujjwal502/readit-example-express/blob/main/10-key-flows.md).

## Install

From inside Claude Code:

```
/plugin marketplace add ujjwal502/readit
/plugin install readit@readit-marketplace
```

> To develop locally instead, load it for a single session without installing:
> `claude --plugin-dir /path/to/readit`.

## Usage

In any repository you want documented:

```
/book                # writes to ./book
/book docs/handbook  # writes to a custom directory
```

ReadIt will detect the language/framework, propose a Table of Contents, explore the
code (in parallel via a read-only explorer agent), then write the chapters.

## How it works

| Piece | Role |
| --- | --- |
| `commands/book.md` | The `/book` slash command entry point. |
| `skills/codebase-book/SKILL.md` | The methodology: survey → coverage map → outline → deep write → audit → assemble. |
| `agents/codebase-explorer.md` | Read-only sub-agent that deeply maps codebase slices (contracts, flows, edge cases) and returns grounded findings. |
| `agents/book-auditor.md` | Read-only sub-agent that fires hard design + implementation questions at the finished book and reports gaps to fill. |
| `agents/quiz-master.md` | Read-only sub-agent that writes grounded, self-checked active-recall quizzes (per chapter + a final exam) with answers that cite real `file:line`. |

The `book-auditor` is the key to the "answer anything" bar: after a full draft, it
stress-tests the book with the questions a reader would actually ask, and any gap it
finds gets filled by reading more code — the loop repeats until no material gaps remain.

## Design principles

1. **Grounded, not hallucinated** — read code before describing it; cite file paths.
2. **Reference-grade, not overview** — contracts, edge cases, and rationale, not summaries.
3. **Narrative over inventory** — explain *why*, follow the flow, don't list files.
4. **Complete, not sampled** — a coverage map ensures nothing load-bearing is skipped.
5. **Map-reduce for scale, audit for completeness** — explore in slices, then verify.

## Depth modes

- `--depth deep` (default): full contract + question-driven audit loop. For the "answer
  anything" goal.
- `--depth quick`: fast overview pass, skips the exhaustive audit.

## Roadmap

- Refresh mode: regenerate only chapters whose files changed, then re-audit them.
- mdBook / Docusaurus export.
- "Danger zones" chapter for fragile / high-churn areas.
- Persisted coverage map so re-runs are incremental.

## License

MIT
