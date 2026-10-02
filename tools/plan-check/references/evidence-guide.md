# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the candidate plan's "Cause:" or
"Diagnosis" lines, read against the "Repro evidence" block: its
numbered steps, artifacts, any "Control" runs, and the closing
"Actual:" line. In live mode, the cause stated in the draft `plan.md`,
read against the student's posted repro comment on the issue (or the
house repro pack as quoted in the drafts).

What good looks like: the stated cause explains every step and every
control run the repro evidence shows. Control runs are where a wrong
cause gets caught: if a control shows the blamed component working, or
shows the symptom with the blamed component out of the loop, the cause
is ruled out no matter how confident or detailed the plan reads, and
no matter who in the thread suggested it. A cause that merely goes one
inference past the evidence without contradicting any of it is still
grounded.

## Scope

Where it lives: the plan's "Scope", "In scope / Not in scope", or
"Change:" lines and its numbered list of changes, read against the
issue body's stated current and expected behavior. Same places in a
live draft.

What good looks like: every listed change is needed to make the
reported behavior correct, or is the test or doc for that one fix. A
"not in scope" line that names what is being left alone is the mark of
a bounded plan, including when the plan fixes a narrower case than the
issue and says so. Creep looks like extra fronts the issue never
raised: rewriting the surrounding module, migrating a dependency,
adding an option or settings panel, porting the fix to sibling
components, reworking CI or the test harness.

## Executability

Where it lives: the plan's "Change", "Approach", "Changes", or "Files"
lines, in the bundle or the live draft.

What good looks like: a named location (file, function, or call site)
and one chosen approach, so someone who has never spoken to the author
could open the file and begin. Not executable: no location at all,
"somewhere", a choice left open ("upstream or vendored, whichever is
easier", "gocui? tcell? not sure"), or a plan whose action is to poke
around and find out. A small named check still to do before the PR is
fine; an undecided approach is not.

## Test plan

Where it lives: the plan's "Test" or "Test plan" lines, read against
the repro evidence's steps and its "Expected:" / "Actual:" lines.

What good looks like: a specific command or action plus the specific
result that proves the fix (the repro command now prints the match and
exits 0; the color flips at step 3 without leaving the view). Usually
this is the repro re-run with the expected result named. Not decisive:
only "run the full test suite", "should feel fast", "nothing else
should feel broken", or the goal restated as a test ("undo works after
toggling").

## Honesty

Where it lives: the plan's "Risk", "Open question", or unknowns lines,
and the certainty language in the plan and the plan comment
("traced", "verified", "definitely", a delivery date). In live mode,
also any deviation note added to `plan.md` after the build started.

What good looks like: what the author has not checked is labeled as
not checked, and what is claimed as verified has an artifact in the
repro evidence behind it. A deviation from the posted plan is recorded
in `plan.md` with what changed and why, not left to be discovered in
the diff.

## Comms

Where it lives: the "Candidate plan comment" read against two places.
First, the "Thread highlights": only lines whose author is marked
OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR count as maintainer
direction (a culprit pointed at, an approach chosen, proposed, or
rejected, a request made). Second, the repo-facts block's "bug
reports" and "contribution policy" lines, including any AI-use policy.
In live mode: the live issue thread (author badges next to names), and
the repo's `CONTRIBUTING.md`, `AI_POLICY.md`, and templates.

What good looks like: when a maintainer has given direction, the plan
follows it or the comment says why it does not; a comment that would
read the same if the thread were empty, on an issue where a maintainer
has spoken, is not thread-aware. On AI policy, every package is
AI-assisted work: a ban fails; a rule that all AI usage must be
disclosed needs a disclosure in the comment, in the poster's own
words; a rule that only covers the pull request, or only asks for
understanding and human-written comments, or no stated policy, needs
nothing in the comment.
