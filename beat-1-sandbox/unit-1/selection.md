# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

"https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36"

**Verdict output**

{
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "5/5 recent commits authored by Aburke225 (COLLABORATOR) within 90 days; same user replied to issue #52 within 6 days of it opening"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived:false, pushed_at 2026-09-16 (today)"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Body: 'There's no test that exercises the review endpoint when a profile exists but has no associated ingested content. Add a test...' — single bounded test to add in tests/unit/test_review_routes.py"},
      {"name": "Nobody already on it", "grade": "pass", "evidence": "assignees: [], comments: 0, no linked PRs in timeline"},
      {"name": "AI-contribution policy", "grade": "pass", "evidence": "CONTRIBUTING.md, AGENTS.md, AI_POLICY.md all 404 (silence passes)"},
      {"name": "Issue age and attempt history", "grade": "pass", "evidence": "Opened 2026-09-10, no prior linked/mentioned PR attempts"},
      {"name": "Good-first-issue label", "grade": "pass", "evidence": "labels include 'good first issue'"},
      {"name": "Adoption scale", "grade": "fail", "evidence": "stargazers_count: 2"}
    ],
    "verdict": "accept"
  },

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--limit 3` smoke run: 2/3 (issue-01 disagreed: gold `accept`, rubric `reject` on
   "Maintainer alive" — the 30-day first-response threshold was too strict for a busy
   repo where only 1 of 5 sampled issues ever got a maintainer reply, at 32.9 days).
2. `--only issue-01` after loosening the first-response threshold to "any latency":
   0/1 (issue-01 now failed "Scope fits a newcomer" instead — the multi-file docs
   checklist read as an umbrella issue to the grader).
3. `--only issue-01,issue-05,issue-10` after clarifying "one PR-sized goal with a
   checklist of files is not automatically an umbrella": 3/3.
4. Full 20-issue run: 16/20, below the 18/20 bar. Four disagreements: issue-14
   (false reject, "Maintainer alive"), issue-15 and issue-20 (false accepts,
   "Scope fits a newcomer"), issue-19 (false reject, "Scope fits a newcomer").
5. `--only issue-14,issue-15,issue-19,issue-20` after reworking "Maintainer alive"
   to treat a thin first-response sample as non-gating, and reworking "Scope fits a
   newcomer" to catch abandoned-attempt history and undecided product content:
   3/4 (issue-19, a maintainer-diagnosed bug with suggested fixes, was now
   misread as having an "unresolved decision").
6. `--only issue-19,issue-20,issue-15` after narrowing the new scope clause to
   exclude maintainer-suggested implementation techniques for an already-diagnosed
   bug: 3/3.
7. Full 20-issue run (confirming): 18/20, bar: PASS.
8. Full 20-issue run with `--save-run`: **19/20, bar: PASS** — matches the
   agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-01` (source `conda/conda#16475`). Gold label: `accept`
("docs task with a stated home and scope; active repo, unclaimed"). My rubric's
final verdict: `accept`, agreeing with gold. Reasoning: the repo passed "Maintainer
alive" (5/5 commits within a day of capture) and "Repo in use" easily; the issue
itself lists five related doc-page edits (add one new page, update four pages that
pointed at the old workflow) under one single goal — moving PyPI-install guidance
to a permanent home. My "Scope fits a newcomer" check only fails a checklist-shaped
issue when it is *self-declared* as work to be split into separate issues, or the
listed items serve unrelated goals; here every item serves the one stated goal, so
it passed as one bounded task rather than being misread as a tracking issue.

**Check rationale**

`Scope fits a newcomer`, as currently written in `rubric.md`:

> Issue describes one PR-sized change serving a single stated goal — a checklist of
> files or sections to touch toward that one goal (e.g. "add this new doc page,
> then update the older pages that pointed at the old guidance to point here
> instead") is still one bounded task, not a scope failure. Only fail scope when
> the issue itself says the listed items should become separate issues/PRs, or the
> items serve unrelated goals with no single change tying them together. Also fail
> scope if: it is a pure usage/support question; a maintainer has stated the fix
> touches core internals or the design is still unsettled; the thread shows years
> of unresolved design debate or multiple abandoned (closed, unmerged) prior PR
> attempts, which means the work is harder than it looks even without a maintainer
> saying so; or the body itself leaves an unresolved product/content decision
> needed before implementation can start — what to ship is undecided, not just how
> (e.g. an unspecified asset/format marked TBD, no acceptance criteria for a new
> feature) — this fails even when no maintainer has commented at all. This does not
> cover a maintainer-diagnosed bug that lists multiple implementation techniques or
> optimizations as suggestions for fixing it: a newcomer can pick any one of those
> and still resolve the diagnosed cause, so that is not an unresolved decision.

It grew this long because the first version ("one bounded piece of work, not an
umbrella") was too blunt in both directions: it read a legitimate multi-file docs
task (issue-01) as an umbrella, and separately it missed two real scope failures
that had no maintainer comment to lean on — a years-old feature request with two
abandoned PRs behind it (issue-15) and a one-line feature wish with a literal "TBD"
asset hiding a product decision (issue-20). The rule now separates "many files, one
goal" (fine) from "the issue itself proves the work isn't actually decided or
actually bounded" (fail), using thread history and the body's own hedges as
evidence instead of requiring a maintainer to say so out loud.

**Trade-offs**

The canary re-run at step 5 above (`--only issue-14,issue-15,issue-19,issue-20`)
shows what this check gives up: the same clause that correctly rejects issue-20's
undecided "logo asset TBD" also flipped issue-19 to a false reject, because
issue-19's maintainer-filed bug lists "additional suggestions" (multi-processing,
skip collapsed categories, separate thread) that look structurally like an
unresolved choice. I had to add an explicit carve-out — a maintainer diagnosing a
bug and suggesting several valid implementation techniques is not the same as an
unresolved product decision — to get issue-19 back to `accept` (step 6) without
losing issue-20's `reject`. The trade-off I'm accepting going forward: this check
now leans on the grader's judgment to tell "alternative techniques for one diagnosed
fix" apart from "a genuinely undecided requirement," which is a real edge it could
still miss on an issue that mixes the two more ambiguously than these two examples
did.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** I'm comfortable in Python and want to
   get better specifically at reading unfamiliar codebases and writing tests for
   existing code — issue #36 (`POST /reviews` has no test for a profile with no
   ingested documents) is directly a test-writing task, estimated at 2-3 hours,
   which matches a first-issue time budget without needing any build tooling I
   haven't touched.

2. **What the verdict got right vs. what I weighed myself.** The rubric correctly
   confirmed the mechanical facts: unclaimed (no assignee, no comments, no linked
   PR), the repo is alive (commits same day, a collaborator closing other issues
   within days), and no AI-contribution ban exists. What the rubric couldn't weigh
   is fit: of the three accepted candidates (#36, #72, #71), #36 is the one that
   actually exercises "write a test for existing code" rather than "remove an
   `xfail` marker from a test that already exists" (#71, #72) — the rubric's
   preferred checks rank on labels and stars, not on which skill I'm trying to
   practice, so I made that call myself.

3. **Anticipated difficulty in claiming it.** Low-to-moderate. The repo is small
   and single-collaborator, so I'll need to read the ingestion/profile models to
   find how "no ingested documents" is represented before I can assert the right
   error path, but there's no ambiguity in what the test should check (the issue
   states the expected behavior: return an appropriate error, don't crash), and
   the course's Path Review house rule means even if a classmate has also claimed
   it, that costs me nothing.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
