# Procedure: how this tool grades a PR package

## Read order

Read the whole package once, in this order, before grading anything.
The order matters twice over. The plan is read before the diff so
there is a fixed list to hold the diff against, and the title and
description are read last so their account of the change ("exactly
the plan", "tests added") cannot color what the diff and the evidence
actually show.

1. Repo facts. From the "pull requests" line, list every stated ask
   as its own item: each template section, each checklist item, the
   issue-reference form, any changelog or release-note entry, tests,
   a title convention. Mark each ask "checkable from the package" or
   "process" (a signed CLA, a vouch flow, prior approval). From the
   "contribution policy" line, note the AI rule: a ban, a disclosure
   requirement and what it must state, or none.
2. Issue body. Note in one line the behavior reported and the behavior
   expected.
3. Thread highlights. For each line, note the author's role. List
   every piece of maintainer direction (OWNER, MEMBER, COLLABORATOR,
   CONTRIBUTOR): an approach chosen, an approach or API rejected, a
   policy restated. Ignore role NONE and bots for direction.
4. Plan context, repro evidence first. List each repro step and each
   control with its result. Then, from the plan, write four lists:
   - deliverables: each change the scope promises, plus each test and
     doc it names;
   - files: every path on the "Files" line;
   - not in scope: every item the plan excludes or defers, and any
     stated limit;
   - test plan: one row per item (each failure mode to re-run, each
     control, the suite or checks to run), with the result the plan
     expects for it.
   Copy any deviation note found in the plan.
5. Diff and commits. List every changed file. Under each file, note
   in a few words what each hunk does. List each commit message.
6. Test evidence. List each thing the evidence says was run and the
   result it reports, and whether it is a transcript, a named action
   with a named outcome, or only an assertion ("works now").
7. Title and description, last. List every claim about what the diff
   contains (files, tests, entries, "no other changes", "exactly",
   "in full"), every departure from the plan it states, where it
   meets each ask from step 1, and what it says about AI use.

In live mode the same order holds with the live sources: read
`scope.md` first; steps 1 to 3 come from the repo's PR template,
`docs/CONTRIBUTING.md`, and the issue thread; step 4 from `plan.md`;
step 5 from `git diff main...HEAD`; steps 6 and 7 from the evidence
and draft files the command names; read `voice-guide.md` last.
`references/evidence-guide.md` says where each one lives.

## Evidence gathering

Work from the lists made during the read. Each gather below puts two
lists side by side; quote the package for anything that decides a
grade, never paraphrase from memory.

- Plan fidelity, diff against plan (for "Diff stays inside the
  plan"): go down the changed-file and hunk list from step 5. Tag
  each hunk as "planned" (the plan's scope or Files line names it),
  "test for the change", "repo-asked artifact" (matches an ask from
  step 1), "covered by a deviation note" (quote the note), "debris"
  (whitespace-only, debug, commented-out), or "extra". Check the
  commit list for commits whose message names work the plan does not
  (a `feat:` or `refactor:` beside the fix).
- Plan fidelity, plan against diff (for "Plan delivered or shortfall
  disclosed"): go down the deliverables list from step 4. Beside each
  one, write the file and hunk that delivers it, or "missing". For
  each "missing", look in the plan's deviation note and in the
  description for a sentence that says it is not in this PR; quote it
  or write "no note".
- Plan fidelity, description against diff (for "Description matches
  the diff"): go down the claims list from step 7. Beside each claim,
  write the file or hunk that makes it true, or "not in the diff".
  For a fidelity claim ("exactly", "in full", "no other changes"),
  write whether the first two gathers found any "extra" or "missing".
- Test evidence, evidence against test plan (for "Evidence shows the
  fix"): go down the test-plan rows from step 4. Beside each row,
  write what the evidence list from step 6 reports for it and in what
  form, or "not mentioned". Mark the row that re-runs the reproduced
  behavior; for that row, compare the reported result with the
  expected result the plan states. Note any evidence that matches no
  row (a different case than the one reproduced).
- Diff quality (for "Diff is reviewable" and "Title and commits name
  the change"): take the hunks tagged "debris" or "extra" above and
  quote one line from each. Scan added lines in non-test files for
  debug output, commented-out code, and TODO or WIP markers. Copy the
  title and the commit messages.
- Standards and comms, asks against description and diff (for "Repo
  asks honored"): go down the checkable asks from step 1. Beside
  each, write where the description or the diff meets it (quote the
  section label, the "closes #N" line, the changelog file path), or
  "not met". Leave "process" asks out.
- Standards and comms, policy against description (for "AI-policy
  compliance"): put the AI rule from step 1 next to what the
  description says about AI use from step 7, or "nothing said".
- Standards and comms, thread against PR (for "Maintainer direction
  respected"): go down the maintainer-direction list from step 3.
  Beside each item, write whether the diff or description follows it,
  goes against it, or goes a different way with a stated reason.

## Check execution

Run the checks in rubric-table order, one at a time, each against its
own gather only. Grade every check even after the first fail; a fail
on one check never changes how another is graded.

- Apply the pass condition as written. Do not fail a check for
  something another check owns: an unplanned feature is "Diff stays
  inside the plan", not "Repo asks honored"; a whitespace-only hunk
  is "Diff is reviewable", not a plan-fidelity fail. One problem may
  fail two checks only when each check's own condition says so.
- Length and formatting are never evidence. A three-line description
  that meets every ask passes; a long headed one hiding an extra
  commit fails.
- "Diff stays inside the plan" fails on any hunk tagged "extra".
  "Plan delivered or shortfall disclosed" fails on any deliverable
  marked "missing" with "no note", and passes when every missing
  deliverable has a quoted note.
- "Evidence shows the fix" fails if the reproduced-behavior row has
  no named action and result, if its result is not the plan's
  expected one, or if any failure-mode, control, or suite row reads
  "not mentioned". A named action with its exact outcome in prose
  counts the same as a transcript.
- If the thread has no maintainer direction, "Maintainer direction
  respected" passes; write "no maintainer direction in thread" as the
  evidence. If the repo states no checkable asks, "Repo asks honored"
  passes with "no template, no stated asks".
- Grade `unclear` only when the part of the package the check reads
  is missing altogether (no plan context, no diff, no repo-facts
  block). A part that is present but empty of the needed content
  ("Test evidence: (none provided)", a description that never
  mentions a required section) is a `fail`, not `unclear`.
- The lists from the read are enough to grade. Re-read a section only
  to pull the exact quote for an evidence line.

## Verdict assembly

1. Collect the grades for the `required` checks. If all are `pass`,
   the verdict is `accept`. If any is `fail` or `unclear`, the verdict
   is `reject`; `unclear` counts the same as `fail`.
2. The `preferred` check is reported with its grade and never enters
   step 1.
3. The deciding check for a reject is the first `required` check in
   rubric-table order that did not pass. Quote the fact or line that
   failed it. List any other failing checks after it by name.
4. In the summary, write one line per check with its grade and
   deciding fact, then the deciding check from step 3 for a reject,
   then any step this procedure did not cover. In live mode, add any
   voice-guide rule the draft title or description breaks, quoting
   the rule.
5. Emit the JSON block last, with every check from the table in table
   order, each with a one-line evidence quote or fact.
