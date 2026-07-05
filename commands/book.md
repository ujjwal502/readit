---
description: Generate a complete, reference-grade "book" of the current codebase — deep enough to answer any design or implementation question without re-reading the source.
argument-hint: "[output-dir] (default: ./book)  [--depth quick|deep] (default: deep)"
---

# /book — Generate a reference-grade codebase book

Generate a **book** that lets a reader answer virtually **any** question about this
codebase — internal design, architecture, data flow, contracts, edge cases, failure
modes, and implementation details — **without needing to search the source again.**

Follow the **codebase-book** skill as your complete methodology. Do not shortcut it.

## Arguments

- `$1` (optional): output directory. Default: `./book`.
- `--depth deep` (default): full contract + question-driven audit loop. Use this for the
  "answer anything" goal.
- `--depth quick`: fast overview pass (skips the exhaustive audit). Only if the user
  explicitly asks for a quick version.

## What to do

1. **Load the method.** Read and follow the `codebase-book` skill end to end, including
   the eight knowledge categories and the definition of done.
2. **Survey & build a coverage map.** Use the `codebase-explorer` agent — **in parallel**
   across subsystems — to map entry points, mechanisms, contracts, data/state, errors,
   concurrency, security, and design rationale. Nothing load-bearing may be skipped
   silently.
3. **Confirm scope.** Briefly report the detected language(s)/framework(s), the proposed
   Table of Contents, the coverage map, and the output dir. Get a quick thumbs-up, then
   proceed (don't stall if the user already gave direction).
4. **Write deeply.** Produce one Markdown file per chapter with Mermaid diagrams, real
   (trimmed, annotated) code excerpts, contracts, and at least one concrete traced
   example per load-bearing flow. Ground every non-trivial claim in `path:line`.
5. **Audit for completeness (deep mode).** Run the `book-auditor` agent to fire hard
   design + implementation questions at the book, then **fill every gap it finds** by
   reading more code and expanding chapters. Repeat until it reports no material gaps.
   Close gaps by improving the book, never by weakening the questions.
6. **Assemble.** Write `README.md` as the Table of Contents (with a coverage summary and
   per-chapter one-liners), cross-link chapters, and verify all references and diagrams.
7. **Report** the ToC, coverage summary, and any residual open questions.

## Non-negotiables

- **Grounded, not hallucinated.** Read the code before describing it. Label inferences as
  inferences; state honest "unclear from the code" rather than inventing.
- **Depth, not just coverage.** Explain the *why*, walk the mechanism step by step, and
  document contracts, edge cases, and failure modes — that's what makes the book able to
  answer anything.
- **Finish the audit loop.** In deep mode, the book is not done until the auditor is
  satisfied and the coverage map is fully accounted for.
