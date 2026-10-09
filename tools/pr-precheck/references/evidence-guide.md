# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives, in a practice submission: the "Plan context" block
holds the plan as one paragraph starting "Plan:". Inside it, the
boundary is the sentence starting "Scope, one bounded change:" (or a
numbered list of deliverables), the exclusions are "Not in scope:",
the paths are "Files:", and any stated limit or deferral is in the
same paragraph ("Known limit, stated in the plan", "Deliberately
deferred"). What actually changed is in "Candidate PR", under
"### Diff": each `+++ b/<path>` line is one changed file and each
`@@` line starts one hunk. "### Commits" lists the commit messages.
The fidelity claims are in "### Title" and "### Description":
"implements the plan exactly", "in full", "no changes beyond it", and
any "Deviation note" or "Deliberately out of scope" sentence.

Where it lives, in my own PR: the scope, not-in-scope, and files
lines of `plan.md`, plus any deviation note I added to `plan.md`
after the build started. What changed is the output of
`git diff main...HEAD`, with `git log main..HEAD --oneline` for the
commits. The fidelity claims are in the title and description in
`pr_draft.md`.

What good looks like: every changed file is on the plan's Files line,
or is the test for the change, or is an artifact the repo asks for
(a changelog entry, an i18n string), and every hunk in those files
does what the scope sentence says. Every deliverable the plan names
has a hunk. Where the diff and the plan differ, a note says what
differs and why: a deferral the plan already recorded and the
description restates is a match, not drift. Each thing the
description says is in the diff is there.

Silent drift, more than the plan: a file the plan does not name
(`src/configs/...`, a settings `.idl`, `src/error.rs`), a commit
titled `feat:` or `refactor:` beside the fix, a new option or flag,
a rewrite of a neighboring function, a different approach from the
planned one (bumping a dependency where the plan said to change one
code path). Silent drift, less than the plan: the plan names two
deliverables and the diff has one, with no note. Drift in the
description: "exactly" or "in full" over a diff that has an extra or
is missing a deliverable, or "CHANGELOG entry included" with no
changelog file under "### Diff". Mentioning extra work in passing
("cleaned up some formatting while I was in there") is not a
deviation note.

## Test evidence (harness category: not-tested)

Where it lives, in a practice submission: "### Test evidence" at the
end of "Candidate PR". It is read against two parts of "Plan
context": the sentence starting "Test plan:", which lists each thing
to re-run and the result expected (one item per failure mode, each
control, and the suite), and the sentence starting "Repro evidence",
which gives the original command or action and its failing result.
A description may repeat a before/after; the test-evidence section
is still the proof.

Where it lives, in my own PR: `test_evidence.md`, read against the
test plan section of `plan.md` and the repro steps in my posted repro
comment on the issue (the house repro pack on the house chain). The
repo's own checks are the commands `docs/CONTRIBUTING.md` and the PR
template name; their outcome belongs in `test_evidence.md` too.

What good looks like: the repro's own command or action, run on the
branch, with the result observed, and that result is the one the test
plan expected (the error text and exit code, the output row, the
byte-identical round trip). A before/after pair is the clearest form;
prose that names the action and the exact outcome counts the same as
a pasted transcript. Every other row of the test plan is accounted
for: each further failure mode shown, each control stated as
unchanged, the suite reported as run with its outcome
(`cargo test` passes, 614 tests).

Not decisive: "tested locally, colors work now" (no action, no
result); "verified working on my machine" or a day of general use;
"all tests pass" with nothing else; a run of the control or of a
different request instead of the reproduced case; one of two named
failure modes shown and the other never mentioned; "did not get a
chance to test"; "(none provided)".

## Diff quality (harness category: unreviewable)

Where it lives, in a practice submission: "### Diff" read hunk by
hunk, and "### Commits". Look at added (`+`) lines in non-test files
for leftovers, and at `-`/`+` pairs whose text is the same apart from
indentation or wrapping.

Where it lives, in my own PR: `git diff main...HEAD` read hunk by
hunk, and `git log main..HEAD --oneline`. `git diff main...HEAD
--stat` shows quickly whether a file changed far more lines than the
fix needs.

What good looks like: the fix is visible in the first hunk a reviewer
opens, and every other hunk is its test or an artifact the repo asks
for. A comment that explains why the fix is needed is part of the
fix. Commit messages each name what the commit does.

Debris tells: debug output left in (`eprintln!("DEBUG ...")`,
`console.log`), commented-out code (`// eprintln!(...)`,
`// log::debug!(...)`), added `TODO` or `WIP` markers, `-`/`+` pairs
that only re-indent or re-wrap unchanged lines, a block rewritten in
a different style with the same behavior, and commits named "wip",
"fix", "fmt + cleanup", or "misc cleanups while debugging". Not
debris: print statements that are a test's own fixture, and lines the
fix had to change.

## Standards and comms (harness category: standards-wall)

Where it lives, in a practice submission: the asks are in the "Repo
facts" block at the top. The "pull requests" line gives the template
sections, checklist items, issue-reference form, and any changelog or
test ask; the "contribution policy" line gives the contribution rules
and any AI-use policy. Maintainer direction is in "Thread highlights"
on lines whose author is marked OWNER, MEMBER, COLLABORATOR, or
CONTRIBUTOR. The PR answers all three in "### Title" and
"### Description", and for asked-for artifacts in "### Diff" (a
`CHANGELOG.md` or whatsnew file among the changed files).

Where it lives, in my own PR: the repo's PR template and
`docs/CONTRIBUTING.md` in my working copy, the house rules in
`scope.md`, and my issue's thread (author badges next to names). My
answers are the title and description in `pr_draft.md`, section by
section against the template, and the changed files in
`git diff main...HEAD`.

What good looks like: each section or checklist item the template
asks for can be pointed to in the description with real content, or
is marked n/a with a reason ("docs n/a, no API change"). The issue is
referenced the way the repo asks ("closes #N", "fixes #N"). An
asked-for changelog or release-note entry is a file in the diff. The
title follows a stated convention (`fix(scope): ...`). Where the
policy requires AI disclosure, the description gives it with what the
policy asks for (the tool and the extent). Nothing in the diff does
what a maintainer in the thread ruled out.

A visibly ignored ask: a required checklist template and a
description that is one free paragraph with no "closes #" line and no
whatsnew entry in the diff; a strict disclose-all-AI-use policy and a
description that says nothing about AI; a template left as its own
placeholder text. Not gradable from the package, so not held against
it: a signed CLA, a vouch flow, prior discussion approval. An ask
worded "if appropriate" (screenshots) only applies when the change is
that kind of change.
