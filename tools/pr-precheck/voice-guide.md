# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time open-source contributor, comfortable in Python, working
through this course's Path Review issues to get better at reading
unfamiliar codebases and writing tests for existing code. Readers should
expect a modest, concrete claim that names exactly what I checked and
what I plan to do next — not an expert tone, and not a promise I can't
back with something I actually ran.

## Rules I write by

### Rule: State scope, not certainty

I say what I've checked and what I plan to check next. I don't promise a
fix or a timeline I can't back up with a shown artifact.

- Wrong: "I'll have this fixed by tomorrow, it's definitely the race
  condition in the debounce logic."
- Right: "I can reproduce the missing `Content-Type` header with exactly
  one custom header set; next I want to trace the header-merge path
  before proposing a fix."

### Rule: Name what I actually ran, not what I assume

Since I'm often reading code I don't yet know well, I don't assert a
root cause before I've traced it. I describe the observed behavior and
my next investigative step instead.

- Wrong: "This is caused by a debounce race in the save handler."
- Right: "The output shows the pane going blank right after step 3; I
  haven't traced the cause yet, but I'm going to check the save-handler
  path next since that's what changed most recently."

### Rule: One honest sentence beats a template

I skip "I'll take this! Assign me" boilerplate and post the one true
thing about my attempt, even when that thing is "I couldn't reproduce
it."

- Wrong: "I'll take this! Will have a fix up soon."
- Right: "Claiming this one — I can reproduce the missing test coverage
  locally; posting the repro report next."

### Rule: Disclose AI use the way the repo asks

I'm working AI-assisted this course, so I say so plainly wherever a
repo's policy asks for disclosure, in my own words rather than a
copy-pasted line.

- Wrong: (posting a claim comment with no mention of AI assistance on a
  repo whose stated policy requires disclosing it)
- Right: "I used an AI coding assistant to help draft this repro and
  reviewed and ran everything myself before posting."

### Rule: Say when I'm out of my depth

When something touches a part of the codebase I haven't learned yet, I
say so instead of waving past it with confident-sounding language.

- Wrong: "This is a simple fix, just update the check."
- Right: "I'm not yet sure how the ingestion pipeline marks a profile as
  empty; I'm reading `models.py` before I say more about the fix."

### Rule: The title says what behavior changed

A maintainer decides from the title whether to open the PR. I name the
behavior that is different after the change, in the form the repo asks
for, and leave out how I feel about it.

- Wrong: "Fixed the header bug!!"
- Right: "Fix JSON Content-Type dropped when exactly one custom header
  is set"

### Rule: The description promises exactly what the diff contains

Every file, test, and entry I say is in the PR is in the diff, and
anything in the diff the plan didn't name is called out with the reason.
I don't write "exactly as planned" unless I've checked the diff against
`plan.md` line by line.

- Wrong: "Implements the plan in full. Tests and changelog included."
  (over a diff with no changelog entry and an extra refactor commit)
- Right: "Implements the plan's one change in `client.py` plus its test.
  One departure: I also touched `utils.py`, because the helper the plan
  named lives there, not in `client.py`."

### Rule: A shortfall is stated as a fact, not an apology

When the PR does less than the plan, I say what is missing, why, and
where it goes next, in one or two plain sentences. No "sorry", no
"hopefully", no burying it at the bottom.

- Wrong: "Sorry, I didn't get to the docs part, hopefully that's okay!"
- Right: "Not in this PR: the docs note from step 2 of the plan. The
  docs page is generated from a file I haven't found yet; I'll open a
  follow-up issue for it."

### Rule: Evidence is shown, not summarized

In a PR I point at the command I ran and the result I saw, before and
after. "Tested" and "works" are conclusions; the reviewer should be able
to draw them from what I pasted.

- Wrong: "Tested locally and everything works now."
- Right: "Before: `http POST :8000/x 'X-A: 1' a=b` sends no
  `Content-Type`. After: the same command sends
  `Content-Type: application/json`. `pytest tests/test_httpie.py`
  passes."

## Things I never post

- A timeline or fix promise I can't back with something I've actually
  run.
- A root-cause claim I haven't traced myself, however plausible it
  sounds.
- A "+1" or "same here" comment that adds no evidence of my own.
- A disclosure omission on a repo whose policy requires it, even when
  disclosing feels like extra friction.
- Boilerplate claim language that would read the same on any issue in
  any repo.
- A description that claims coverage, files, or fidelity to the plan
  that the diff or the test evidence does not show.
- A template section left as its placeholder text, or filled with
  "n/a" and no reason.
- A PR title that could be pasted onto any other PR ("fix bug",
  "update code", "wip").
