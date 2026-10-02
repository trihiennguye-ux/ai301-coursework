# Procedure: how this skill grades a plan package

## Read order

Read the whole package once, in this order, before grading anything.
The order matters: the evidence is read before the plan so the plan's
confidence cannot color what the evidence shows.

1. Repo facts. Note the "contribution policy" line: is there a ban, a
   disclosure requirement, and does the requirement cover comments or
   only the pull request?
2. Issue body. Note in one line the behavior reported and the behavior
   expected. This is what "the issue asks for".
3. Thread highlights. For each line, note the author's role. List
   every piece of maintainer direction (OWNER, MEMBER, COLLABORATOR,
   CONTRIBUTOR): culprit named, approach chosen or rejected, request
   made. Ignore role NONE for direction.
4. Repro evidence. List each step and each control run with its
   result. For each control, write down what it rules in or out
   ("symptom appears with no pager, so the pager is not the cause").
5. Candidate plan. Note its stated cause, its list of changes, its
   named locations, its test plan, and its stated unknowns.
6. Candidate plan comment. Note what it says about the thread and
   about AI use.

In live mode, read `scope.md` first and `voice-guide.md` last; the
issue, thread, and policy come from GitHub and the repro evidence from
the student's posted repro comment, per the evidence guide.

## Evidence gathering

Use the notes from the read; quote the package, never paraphrase from
memory.

- Diagnosis and grounding: put the plan's cause sentence next to the
  control-run notes from step 4. For each control, ask: if the plan's
  cause were true, would this control have come out this way?
- Scope: put the plan's list of changes next to the one-line issue
  summary from step 2. Mark each change as "fixes the reported
  behavior", "test/doc for that fix", or "extra".
- Executability: quote the file/function/site the plan names and the
  approach it commits to. Quote any phrase that leaves the choice
  open.
- Test plan: quote the test plan's command or action and the result
  it expects.
- Comms: put the comment next to the maintainer-direction list from
  step 3 and the policy note from step 1.
- Honesty: quote the plan's risk/open-question lines and any
  certainty or timeline words in the comment.

## Check execution

Run the checks in rubric-table order, one at a time, each against its
own evidence only. A fail on one check never changes how another is
graded: grade all of them even after the first fail.

- Apply the pass condition as written. Do not fail a check for
  something another check owns (a wrong cause is not a scope fail).
- Length and formatting are never evidence. A three-line plan that
  meets a pass condition passes; a long headed one that does not,
  fails.
- A control run that contradicts the stated cause fails "Cause follows
  the evidence" even when a thread commenter proposed that cause.
- If the thread has no maintainer direction, "Engages maintainer
  direction" passes; say "no maintainer direction in thread" as the
  evidence.
- Grade `unclear` only when the part of the package the check reads
  is missing (for example, no repro-evidence block). A part that is
  present but empty of the needed content (a plan with no cause, a
  test plan with no observable) is a `fail`, not `unclear`.
- The notes from the read are enough; re-read a section only to pull
  the exact quote for the evidence line.

## Verdict assembly

1. Collect the grades for the `required` checks. If all are `pass`,
   the verdict is `accept`. If any is `fail` or `unclear`, the verdict
   is `reject`.
2. The `preferred` check is reported with its grade and never enters
   step 1.
3. In the summary, write one line per check, then name the deciding
   check(s) for a reject with the quote that decided each. In live
   mode, add any voice-guide rule the draft comment breaks, quoting
   the rule.
4. Emit the JSON block last, with every check from the table, each
   with a one-line evidence quote or fact.
