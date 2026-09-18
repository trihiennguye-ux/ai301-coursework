# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo facts: last 5 default-branch commits (dates + authors) and maintainer first-response sample | At least 1 of the last 5 commits was authored or merged by a human within the last 90 days (measured from capture date in eval mode, from today in live mode). The first-response sample is corroborating evidence, not a gate: a sample with zero maintainer replies does not fail this check on its own when commit activity already shows the repo is live, since a small or unlucky sample (e.g. 1-2 issues) is not enough on its own to call a maintainer dead | required |
| Repo in use | Repo facts: latest release date, last push to any branch, archived flag, star count | Repo is not archived, AND (last push within 90 days OR latest release within 180 days) | required |
| Scope fits a newcomer | Issue body and comment thread | Issue describes one PR-sized change serving a single stated goal — a checklist of files or sections to touch toward that one goal (e.g. "add this new doc page, then update the older pages that pointed at the old guidance to point here instead") is still one bounded task, not a scope failure. Only fail scope when the issue itself says the listed items should become separate issues/PRs, or the items serve unrelated goals with no single change tying them together. Also fail scope if: it is a pure usage/support question; a maintainer has stated the fix touches core internals or the design is still unsettled; the thread shows years of unresolved design debate or multiple abandoned (closed, unmerged) prior PR attempts, which means the work is harder than it looks even without a maintainer saying so; or the body itself leaves an unresolved product/content decision needed before implementation can start — what to ship is undecided, not just how (e.g. an unspecified asset/format marked TBD, no acceptance criteria for a new feature) — this fails even when no maintainer has commented at all. This does not cover a maintainer-diagnosed bug that lists multiple implementation techniques or optimizations as suggestions for fixing it: a newcomer can pick any one of those and still resolve the diagnosed cause, so that is not an unresolved decision | required |
| Nobody already on it | Repo facts: assignees, linked PRs, comments thread, label event timestamps | No assignee is set, no linked PR is open, and no unanswered claim comment from a maintainer confirms someone else is actively working it (per the Path Review house rule in scope.md, other students' claim comments do not count) | required |
| AI-contribution policy | CONTRIBUTING.md / AI_POLICY.md / PR-issue templates, or the "contribution policy" line in Repo facts | No outright ban on AI-generated or AI-assisted contributions; disclosure/testing/human-review conditions are fine and silence passes | required |
| Issue age and attempt history | Issue open date and history of prior linked/mentioned PRs | Prefer issues with no history of abandoned (closed, unmerged) prior attempts, and not open for multiple years without progress | preferred |
| Good-first-issue label | Issue labels | Prefer issues explicitly labeled `good first issue` or equivalent | preferred |
| Adoption scale | Repo facts: star count / "Used by" counter | Prefer repos with meaningfully higher star counts, as a proxy for real-world use | preferred |

## Verdict rule

Accept the issue only if every `required` check passes. If any `required`
check fails, or comes back `unclear`, the issue is rejected — `unclear`
is treated the same as fail, since a first-time contributor can't afford
to gamble on an ambiguous signal in any of these families. `preferred`
checks never flip the verdict; they only rank issues that already passed
every required check, with more preferred checks passed placing an issue
higher, weighted toward matching the fit profile in scope.md.
