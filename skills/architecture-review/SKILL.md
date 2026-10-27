---
name: architecture-review
description: Use for a periodic (for example monthly) health check of a codebase's structure, or when the team asks "where should we refactor?" or "why is this area so hard to change?". Looks for shallow modules and tangled dependencies in the parts of the code that change most, and returns a short, ranked list of refactoring candidates.
disable-model-invocation: true
---

# Architecture review

Refactoring effort should go where change actually happens. A messy module nobody touches is cheaper to leave alone than a mildly messy one changed every week.

## 1. Find the hotspots

- Churn: files changed most often in the last 3–6 months, for example `git log --since="6 months ago" --name-only --pretty=format: | sort | uniq -c | sort -rn | head -30`.
- Pain signals: files that appear together in bug-fix commits, files with the most review comments or reverts.
- Size and complexity of those files (lines, nesting, number of dependencies).

Work on the intersection: high churn **and** high complexity.

## 2. Look for these shapes

- **Shallow modules:** interfaces almost as complex as their implementation; callers must know internals to use them. Candidates to merge or deepen.
- **Shotgun surgery:** one conceptual change needs edits in many files.
- **Divergent change:** one file changes for many unrelated reasons.
- **Leaky layers:** domain code importing web, persistence or framework types.
- **Cycles:** packages or modules depending on each other.
- **Parallel conditionals:** the same `switch`/`when` on a type repeated in several places.

## 3. Rank and propose

For each candidate:

```
Area: <path(s)>
Evidence: <churn, bugs, size — numbers>
Problem: <shape from the list>
Proposed direction: <one paragraph, the smallest refactor that removes the problem>
Effort: S / M / L · Risk: low / med / high
Payoff: <what gets easier>
```

Return at most five candidates, ranked by payoff per effort. Large changes become an ADR proposal (see `writing-adrs`), not a surprise refactor.

## Rules

- This is a report. Do not refactor during the review.
- Prefer deleting complexity over adding abstractions.
- Respect existing ADRs; if a candidate contradicts one, say so and propose superseding it explicitly.

## Known failures

_Record real mistakes here with a date and the rule that prevents them._
