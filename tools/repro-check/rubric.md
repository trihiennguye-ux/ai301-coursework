# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment line (Environment family in `references/evidence-guide.md`): tool/library version, platform, relevant dependency versions | The report names versions and platform sufficient to place the attempt, and any delta from the issue's target version is stated out loud, not left silent | required |
| Steps followable | The repro report's steps section (Steps family) | A concrete starting state plus an ordered sequence of exact commands or actions a stranger could re-run without inferring a missing piece; "same steps as the issue" without restating them fails this | required |
| Behavior matches the issue | The report's output excerpt/log/screenshot read against the issue's stated current and expected result (Behavior-shown family) | The artifact's trigger is the issue's trigger (not a modified or simplified one) and its result is the issue's described symptom, not an adjacent error, a different exit code, or a graceful validation message standing in for the reported crash | required |
| Claims match evidence | The claim comment's and report's stated conclusions (root cause, "verified", "guaranteed", a fix timeline) read against the artifacts named above (Honesty family) | Every stated conclusion is backed by a shown artifact. An honest cannot-reproduce that names what differed from the issue's setup is a pass; a confident claim with no artifact behind it, or stronger than what the artifact actually shows, is not | required |
| AI-disclosure compliance | The repo-facts contribution/AI-policy line read against the claim comment and report text (Comms family) | An outright ban on AI-generated/assisted contributions fails this regardless of proof quality. A policy that requires disclosure passes only if the comment discloses AI assistance in the poster's own words. Silence or a permissive-with-responsibility policy passes without any disclosure needed | required |
| Comms respect the repo | The claim comment against the issue thread; the report's shape against any stated bug-report template asks (Comms family) | The comment is specific to this issue — it names something concrete about the bug or the plan, not interchangeable "I'll take this" / "+1" boilerplate — and the report supplies whatever the repo's template asks for (e.g. repro steps, version info) | required |

## Verdict rule

Accept only if every `required` check passes. Any `required` check that
fails, or comes back `unclear`, rejects the package — `unclear` is
treated the same as fail, since a stranger reading the posted comment
can't verify a claim I can't verify myself. There are no `preferred`
checks in this rubric: every family above is load-bearing enough that
a miss should hold the package, not just rank it lower.
