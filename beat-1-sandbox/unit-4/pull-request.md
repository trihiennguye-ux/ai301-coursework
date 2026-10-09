# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/118

**Branch**

`fix/36-review-no-ingested-content`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One run, a full run of all 20 scored packages with `--save-run eval-run.txt`:

1. `agreement: 20/20 scored items  (bar: 18/20: PASS)`

Its categories line was `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4
standards-wall 2/2  unreviewable 3/3`. No row was marked NO, so I did no `--only` re-runs
and revised nothing after it. That line is the agreement line in the committed
`eval-run.txt`.

**Package analysis**

`pkg-20` (category `standards-wall`). My rubric decided `reject`; the gold label is
`reject`.

The check that carries the category is "AI-policy compliance". The package's repo facts
say: "strict AI rules. All AI usage in any form must be disclosed, stating the tool used
and the extent of the assistance". The description says nothing about AI use, and my
rubric treats every package as AI-assisted work, so the grader's evidence line was: "The
policy requires disclosing the tool used and the extent of assistance; the description
says nothing about AI use."

Everything about the code itself passed: "Diff stays inside the plan" ("Only
src/config/Config.zig changed (guard plus tests), matching the plan's Files line"),
"Plan delivered or shortfall disclosed", "Description matches the diff", and "Diff is
reviewable". That is why this package is a standards wall and not drift: the work is
fine and the repo's stated ask is ignored.

One more check failed, which I did not expect. "Evidence shows the fix" failed because
the plan's test plan reads "the conditional-pair control unchanged" and the test evidence
only says "the conditional-pair control still returns `?997;1n`" under a line that
begins "Expected-after per the plan". The grader read that as an expectation restated,
not a control that was run: "never shown as run". The verdict would have been `reject`
without it.

**Check rationale**

The check, as it reads in `tools/pr-precheck/rubric.md`:

> | AI-policy compliance | The repo-facts "contribution policy" line (live: `docs/CONTRIBUTING.md` and the PR template's disclosure section) read against the description text (Standards and comms family); every package is treated as AI-assisted work | An outright ban on AI-assisted contributions fails. A policy that requires AI use to be disclosed passes only if the description discloses it with what the policy asks for (the tool, the extent, wherever it says). A policy that only asks for understanding, responsibility, or human-written words, or a repo with no stated AI policy, passes with or without a disclosure. A template disclosure section passes only when it is filled in | required |

It reads this way because I rejected the simpler version, "the description discloses AI
use", before the run. Six of the seven accept-labelled packages (`pkg-02`, `pkg-05`,
`pkg-08`, `pkg-13`, `pkg-16`, `pkg-19`) have a contribution policy line ending "no
stated AI policy", and none of them mentions AI anywhere else. `pkg-11`'s policy says
"AI-assisted coding is welcome with a human in the loop who understands the work", which
asks for understanding, not a disclosure. A flat disclosure check would have failed all
seven. So the condition is tied to what the repo's own policy asks: a ban fails, a
disclosure requirement needs the disclosure with "the tool, the extent", and anything
softer passes. I did not revise the check after the run, because the run had no
disagreement to revise for.

**Trade-offs**

Nothing changed elsewhere, and here is how I know: the check can only fail a package
whose policy bans AI or requires disclosure, and the run shows `clear-accept 7/7`, so it
failed none of the packages that should be accepted. The two it was written for both
agree: `standards-wall 2/2`.

What it gives up: "every package is treated as AI-assisted work" means a repo with an
outright ban fails every PR, including one written with no AI at all, because the
package cannot show that. I accept that miss. My own PR is AI-assisted, so in live mode
the assumption is true, and a false reject there costs me a second look while a false
accept costs a maintainer's trust.

A second thing I saw and did not act on: in `pkg-20` "Evidence shows the fix" failed on
a control that was stated but not shown as run. That strictness changed no verdict in
this run (`not-tested 4/4`, `clear-accept 7/7`), and I did not re-run it with `--only`,
so I only know it is safe on these 20 packages.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
