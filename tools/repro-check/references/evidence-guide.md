# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the repro report's "Environment:"
line (tool/library version, OS/platform, relevant dependency versions).
In live mode, the same line in the student's draft repro comment, or
the environment fields the issue's own bug-report template asks for.

What good looks like: the report names a version and platform
sufficient to place the attempt in time and space, and if that version
differs from the one the issue targets, the report says so explicitly
("tested on 3.2.4 since that's what I have installed") rather than
leaving the reader to notice the gap.

## Steps

Where it lives: the repro report's steps section — the numbered
commands or actions, in an eval bundle or a live draft.

What good looks like: a concrete starting state (what's installed,
what file or data exists before step 1) followed by an ordered
sequence of exact commands or actions a stranger could paste and run
without inferring a missing piece. "Ran the same steps as the issue"
without restating them is not sufficient; the reader should never have
to open the issue to find out what step 2 actually was.

## Behavior shown

Where it lives: the report's output excerpt, log, or screenshot, read
against the issue body's stated current result and expected result.

What good looks like: the artifact's trigger is the same trigger the
issue describes (not a modified, simplified, or substituted one) and
the artifact's result is the same symptom the issue names — not an
adjacent error, a different exit code, or a graceful validation
message standing in for the reported crash. When the report claims a
cannot-reproduce, the artifact still shows a genuine attempt at the
issue's own trigger, not a different one.

## Honesty

Where it lives: the claim comment's and repro report's stated
conclusions — root-cause claims, certainty language ("verified",
"guaranteed", "definitely"), fix timelines — read against the
artifacts identified under Behavior shown.

What good looks like: every stated conclusion is backed by a shown
artifact. An honest cannot-reproduce that names what differed from the
issue's setup (a different OS, a config it didn't have, a version it
couldn't match) is a pass. A confident claim with no artifact behind
it, or a conclusion stronger than what the shown artifact actually
demonstrates, is not — however polished or detailed the surrounding
write-up reads.

## Comms

Where it lives: the claim comment against the issue thread's tone and
content; the repro report's shape against the repo-facts block's
stated bug-report template asks and its contribution/AI-use policy
(`CONTRIBUTING.md`, an `AI_POLICY.md`, or a template checkbox).

What good looks like: the comment is specific to this issue — it names
something concrete about the bug or the plan, not interchangeable
"I'll take this" / "+1" boilerplate that would read the same on any
issue. The report supplies whatever the repo's template asks for (e.g.
repro steps, version info). On disclosure: an outright ban on
AI-generated or AI-assisted contributions fails regardless of proof
quality; a policy that requires disclosure passes only if the comment
discloses AI assistance in the poster's own words; silence or a
permissive-with-responsibility policy passes without any disclosure
needed.
