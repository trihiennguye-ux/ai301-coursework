---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement and the issue that plan belongs to.

Answer that question and no other. Do not review the code for style,
do not suggest a better fix, do not grade the plan or the issue
themselves: they are what the PR is read against, not what is graded.
Grade exactly one package per run; if you are handed more than one,
stop and ask which one to grade. Do not answer from gut feel: you
answer by executing `procedure.md`, which applies the checks in
`rubric.md` to evidence located per `references/evidence-guide.md`.
`CONTRACT.md` in this directory is the fixed interface this file
implements; where this file and the contract disagree, the contract
wins.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
or JSON file whose text starts `# Eval package: <id>`, or that text
inlined in the prompt), you are in eval mode. Otherwise you are in
live mode.

**Live mode**: the student's own submission, checked before the pull
request is opened. Run from the student's working copy of the scoped
repo, on the branch the PR will come from. Read all of the following
and nothing else:

1. `scope.md`, then `voice-guide.md`, in this skill directory (the two
   seams below).
2. The files named in the command that invoked you. Between them they
   carry the student's `plan.md` (the posted week-3 plan, deviation
   notes included), the draft PR title and description
   (`pr_draft.md`), and the captured test evidence
   (`test_evidence.md`). Read every file the command names, in full.
   A file the command names that does not exist or is empty is not a
   reason to refuse: say which one is missing in the summary and let
   the checks that depend on it grade the absence.
3. The branch's diff: run `git diff main...HEAD` (three dots) from the
   working copy. That output, everything the branch changes relative
   to the default branch, is the diff. Do not grade uncommitted
   changes, and do not substitute the description's account of the
   diff for the diff.
4. The issue's thread: the issue the plan names, in the scoped repo,
   with all of its comments (`gh issue view <number> --repo <scoped
   repo> --comments`). Take the issue number from `plan.md`.
5. The repo's stated asks, read from the working copy: the repo's PR
   template and `docs/CONTRIBUTING.md`.

A house-chain student (one the instructor routed to the house issue)
has no posted plan or repro of their own: there, read the house plan
and the house repro pack in place of the student's `plan.md` and
repro evidence. The same checks grade the same things.

Read the drafts the way a maintainer will read the opened PR: the
package is what these sources contain, not other files in the working
directory.

**Eval mode**: the bundle is the whole world. It contains the repo
facts (PR-template asks and stated policy), the issue and its thread
highlights, the plan context (the accepted plan and the repro evidence
it built on), and the candidate PR (title, description, commits, diff,
test evidence). Use ONLY the bundle text as evidence. Fetch nothing,
run no git or `gh` command, read no other file, and do not look up the
real issue the bundle's `source:` line names: where the live page and
the bundle would disagree, the bundle wins. Eval mode always grades a
complete package: every check in the rubric, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. Its `Repo:` line names the one repository the student's pull
request may target, and its house rules say how that environment
works; a house rule changes how evidence is read there, so carry the
rules into the checks they touch.

Refuse to grade in two cases, and in both stop before reading any
other input:

- The `Repo:` line still carries an unfilled bracketed placeholder
  (`<ORG>/<PATH-REVIEW-REPO>`). Tell the student to fill the `Repo:`
  line in `scope.md` with their section's Path Review repo. Never
  guess a scope.
- The work is outside the scoped repo: the working copy's remotes do
  not point at the scoped repo or a fork of it, or the issue the plan
  names lives in a different repository. Say which repo you found and
  which repo the scope names.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the outgoing PR text, the draft title
and the draft description, against those rules. For every rule the
draft breaks, report it in the summary: quote the rule, quote the
words that break it. Nothing else is gated by the voice guide: not the
plan, not the commits, not the diff.

The voice guide never changes the verdict on its own. It is the
student's personal standard, not the readiness bar; a broken voice
rule moves a grade only if `rubric.md` has a check that reads the
voice guide. If `voice-guide.md` has no rules filled in, say so in the
summary and move on.

In eval mode, ignore `voice-guide.md` entirely: voice is personal and
carries no gold labels, and the universal communication-quality checks
live in the rubric.

## Component reads

Three files in this skill directory do the grading, and you read all
three before grading anything:

- `rubric.md` is the brain. Its table defines the checks: each row
  names the check, the evidence to look at, the pass condition, and
  the weight (`required` checks gate the verdict; `preferred` checks
  never change it). Below the table it states the verdict rule: how
  the grades combine into accept or reject. Grade every check in the
  table and only the checks in the table.
- `references/evidence-guide.md` is the map. For each evidence family
  a check names, it says where that evidence lives in a package (in a
  bundle, and live in the working copy and on GitHub) and what good
  looks like there. Look where it says.
- `procedure.md` is the hands. Execute it as written: its read order,
  its evidence gathering, its check execution, its verdict assembly.

Where the procedure is silent on a step you need, do not improvise
around it: name the gap in the summary ("procedure gap: ...") and do
the least the rubric requires to finish the check. A procedure gap is
feedback the student needs.

Refuse to grade if `rubric.md` has no checks filled in or no verdict
rule, or if `procedure.md` has no steps filled in. Stop and say which
file is empty. Headings and the instruction comments inside the
templates are not content; only the student's own written text counts.
Never invent checks or steps to cover for an empty file: a verdict
from made-up checks looks like judgment and is noise.

## Verdict and output

The verdict space is binary: `accept` (ready to submit: open the pull
request) or `reject` (hold: fix it first). There is no third verdict,
no "accept with reservations", no score. A reservation belongs in the
evidence line of the check it concerns.

Before the block, show a short readable summary: one line per check
with its grade and deciding fact, the check the verdict turned on,
any procedure gaps, and in live mode any voice-guide rules the draft
breaks. Then end your reply with the fenced JSON block below. It must
be present, valid, and last: nothing follows it. The eval harness
parses the last fenced JSON block in your output.

Fill `item` with the bundle id in eval mode (for example `pkg-07`). In
live mode use the PR's URL if a draft PR already exists; otherwise the
scoped repo, issue number, and branch (for example
`codepath/pathreview-ai301-fa26-s3#12 (fix/12-slug)`). List one entry
in `checks` per rubric row, in rubric order, with `name` matching the
rubric's check name exactly. Do not add, rename, or reorder fields.

A refusal (unfilled or out-of-scope `scope.md`, empty `rubric.md` or
`procedure.md`, more than one package) is not a graded run: say why
you stopped and emit no JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it, and where in the package it sits. "Looks
  fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready
  and a beautiful confident one can be hiding drift. Read the diff,
  the evidence, and the description themselves against the plan, the
  issue, and the repo's stated asks, never the formatting or the
  length.
- A disclosed shortfall is not a hidden one: a PR that does less than
  the plan and says so, in a deviation note or in the description, is
  read by the rubric's conditions like any other. Do not turn "less
  than everything" into `reject` on your own.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  does not say, treat `unclear` as `fail`: a PR you cannot verify from
  the package is a PR that is not ready to submit.
