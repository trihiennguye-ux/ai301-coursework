# Contribution: [Frontend Design] Design Landing Page UI/UX for CampusOS

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Victor Nguyen
**GitHub Username:** trihiennguye-ux
**Issue:** https://github.com/NITRR-Official/CampusOS/issues/60
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: built issue-scout's search strategy and ran a first wild scan plus three direct-URL runs**

I wrote three `gh search issues` queries traced to my fit profile (Python, JavaScript, and test-titled issues) with a fewest-comments-then-newest ordering, ran a scan that turned 300 results into 5 graded candidates, and graded six hand-picked issues directly (MaterialX, pythondotorg, OpenImageIO). The scan exposed four problems: my rubric still applied the sandbox's "claims don't count" house rule, an assignee filter repeated the queries and dropped nothing, the ordering floated brand-new issues on 0–4-star repos to the top, and the scope check rejects a whole umbrella issue like OIIO #4387 even when a maintainer offers each sub-item as standalone work. I rewrote the "Nobody already on it" check so a recent unreleased claim fails, removed the redundant filter, and logged the ordering gap in `scan-notes.md` and the umbrella gap in `scan-output.md`. I didn't change the ordering, because re-ranking offline didn't fix it and search results carry no star counts.

**Unit 6: installed a voice guard on `gh issue comment` and filled a repo-memory file in my CampusOS clone**

I installed the voice guard as a PreToolUse hook that checks every `gh issue comment` against my voice guide, and its log shows it stopping my first claim post over a report-back date ("by Wednesday, November 4") that broke my rule against timelines I can't back up, then passing the revised comment. That stop showed a gap, since repro-check had accepted the same draft because its rubric only looks for fix timelines, so I changed the line to a promise to post my outline for review before any mockups. I filled `CLAUDE.local.md` in my clone (excluded through `.git/info/exclude`, so `git status` stays clean) with 35 lines covering CONTRIBUTING.md's PR conventions, where the landing page lives, the build and test commands, and the repo's AI stance (none stated). Writing it showed that `frontend/app/page.tsx` is already a full landing page, not the placeholder my claim comment called it, and that I can't run any of the commands yet because `pnpm` isn't installed, so my outline will start from the existing page and name what's missing.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

This week I didn't run a new scan. I picked issues by hand and graded each one with issue-scout in direct mode: OpenImageIO #4720, CTSM #3152, agentic-operator-core #133, CTSM #1390, and CampusOS #60. Three of them dropped out for different reasons. #133 was a seven-item umbrella issue, and my rubric rejected it. On CTSM #3152, a maintainer had closed an AI-written PR for that exact issue six days earlier, saying they don't want AI-written text in their docs. CTSM #1390 was a clean, well-scoped fix, but it's in Fortran and the PR template asks you to build and run the climate model. I chose CampusOS #60, "Design Landing Page UI/UX". The scout accepted it: every required check passed, and the only preferred check it failed was adoption (5 stars). The difficulty note said ambitious, because the design is wide open ("The exact structure may evolve"), every acceptance item is a judgment only the owner can sign off on, and two PRs (#63, #87) have already built a placeholder and the hero section.

The scout also flagged that #60 is a poor fit for my profile, since it asks for design, not code, and I chose it anyway. The repo itself is a React/TypeScript frontend, which is the JavaScript/React side of what I know. My first step is reading `frontend/app/page.tsx` and `docs/frontend/DESIGN_SYSTEM.md`, which is exactly the "reading an unfamiliar codebase" practice I want. I also want to learn how to work an open-ended issue with a maintainer: asking what is still needed and agreeing on a deliverable before building anything. My honest read of the scout is that it graded the rules correctly but missed a few things a person would notice. "Maintainer alive" passed partly on the owner replying to their own issues. The repo is about a week from failing the 90-day activity checks. And scope-feasible has no way to ask whether a design-only issue has a "done" a reviewer can check. I logged all three in `scan-notes.md`. Before posting, repro-check accepted my claim draft, but my voice guard caught a report-back date I couldn't back up, so I replaced it with a promise to post an outline for review before any mockups.

Claim comment: https://github.com/NITRR-Official/CampusOS/issues/60#issuecomment-6461284783

**Toolkit**

- Tool: issue-scout in direct mode. I gave it five hand-picked issue URLs one at a time, and it graded each against my rubric with live `gh` evidence. I also used repro-check on my claim draft.
- What it produced: for #60, an accept with every required check passing, a failed "Adoption scale" (5 stars), and a difficulty note of **ambitious** naming the open edge: which sections are still needed after #63/#87, and in what format. It also produced read-outs for the other four issues in `scan-output.md`.
- Difficulties: the scout can't tell design work from code work. It passed a design-only issue whose acceptance criteria no reviewer can check, and it counted the owner's replies on their own issues as maintainer responses. My AI-policy check only reads policy files, so on CTSM #3152 it passed even though a maintainer had refused AI-written docs on a PR. The attempt-history check missed an unlinked PR that I only found with a PR search.
- What changed: I applied this week's scope patch. My unit-1 "Scope fits a newcomer" check rejected any issue whose design was unsettled, so I replaced it with the `scope-feasible` floor, which only rejects issues that aren't one piece of work, and added a difficulty note (straightforward / moderate / ambitious) under the checks table. Under the old check, #60 would likely have been rejected, since its structure "may evolve". I logged further fixes to make in `scan-notes.md`: require maintainer replies on other people's issues, read maintainers' AI statements on PRs, add a PR search to attempt history, and pass a sub-item the issue sizes as standalone work.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS (Darwin 27.0.0), git 2.54.0, node v24.11.1. I forked NITRR-Official/CampusOS to trihiennguye-ux/CampusOS and cloned the fork. Branch `dev` is at `4263489` ("fix: add header and hero component and auth states for buttons #86 (#87)", 2026-08-05), which matched upstream `dev` on 2026-10-28, with a clean working tree. #60 is a design request, not a bug, so reproducing it means showing that the gap it describes is still real in today's code. All of my checks are static checks on the source. The challenge I haven't solved yet: `pnpm` isn't installed, so I haven't run `pnpm install` or `pnpm dev` and haven't viewed the rendered page at `/`. Viewing the page, including how it responds to screen width and dark mode, is still to do.

**Steps to Reproduce**

From a clean clone at `dev` `4263489`:

1. List the landing page's sections and headings: `grep -nE "<section|<footer|<h1|<h2" frontend/app/page.tsx`
2. Search the page and its two components for the themes the issue asks for: `grep -niE "open.?source|community|contribut|github|calendar|plugin" frontend/app/page.tsx frontend/components/LandingHeader.tsx frontend/components/LandingHeroActions.tsx`
3. Look for a landing page design doc: `ls docs/frontend` and `grep -rliE "landing" docs/`
4. Check the hero's stat tiles: `grep -nE "value: '" frontend/app/page.tsx`
5. Observed result: the page already has a hero, four sections (`#features`, `#modules`, `#workflow`, `#launch`), and a footer, but there's no written design in the repo, and there are no open-source/community or contributor sections.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:** output excerpts from steps 2 and 4:

  ```
  frontend/app/page.tsx:3:  Calendar,
  frontend/app/page.tsx:47:    title: 'Events and calendars',
  frontend/app/page.tsx:49:    icon: Calendar
  frontend/app/page.tsx:364:          <p>CampusOS - Campus management and community operations.</p>

  119:                { label: 'Active modules', value: '12+' },
  120:                { label: 'Teams onboarded', value: '40+' },
  121:                { label: 'Automations', value: '120+' },
  122:                { label: 'Uptime', value: '99.9%' }
  ```

  Step 3 found only `DARK_MODE.md`, `DESIGN_SYSTEM.md`, and `OVERVIEW.md` in `docs/frontend`. The only "landing" mentions are file-tree and route-table lines in `OVERVIEW.md` and `docs/project/PROJECT_STRUCTURE.md`.
- **My findings:** The gap #60 describes is partly real today. A landing page exists in code, more complete than "placeholder" (#63's title) suggests, but none of the issue's design deliverables (section hierarchy, design decisions) exist in the repo. Two of its goals have no section at all: open-source/community and contributor onboarding. Two are only partly covered: the Modules section never mentions the plugin architecture, and the calendar showcase is one module card. The hero's stats are hard-coded marketing numbers. Whether the owner still wants a full design or only the missing sections is the question in my claim comment, which is still unanswered.

**Toolkit**

- Tool: repro-check, in live mode. I ran it on my claim draft before posting, then on the claim plus `repro-draft.md` together, both against my rubric and voice guide.
- What it produced: an accept on the claim-only draft (the three repro checks marked not yet applicable), then an accept on the full package with all six required checks passing.
- Difficulties: my rubric is written for bugs ("trigger", "symptom", "crash"), and #60 is a design request with nothing to trigger. "Behavior matches the issue" passed by reading the issue's acceptance criteria as the symptom, but nothing in the rubric says that's allowed. It also accepted my claim draft with a report-back date that my voice guard then stopped, because "Claims match evidence" only looks for fix timelines. Before grading, I narrowed one sentence myself that claimed more than my grep showed: I had said there was no plugin-architecture section when the page does have a Modules section.
- What changed: I replaced the date in my claim with a promise to post my outline before any mockups. I'd add a row to the rubric for feature and design issues ("the report shows the requested capability or artifact is absent today") and make the timeline check match my voice guide's rule, which covers any timeline I can't back up, not just fix timelines.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
