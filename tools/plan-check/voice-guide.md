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
