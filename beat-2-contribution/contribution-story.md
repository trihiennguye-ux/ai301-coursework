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
**Pull Request:** https://github.com/NITRR-Official/CampusOS/pull/135

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

**Unit 7: installed the ship-gate, wrote a five-check manifest from CampusOS's CI, and shipped PR #135 through it**

I wrote `ship-manifest.md` from `.github/workflows/ci.yml` and CONTRIBUTING.md: install, format check, lint, type-check, and test. The gate passed my branch `feature/60-landing-page-design` twice by hand, with all five checks ok and all nine pr-precheck checks passing, and the hook then let `gh pr create` through on the second pass because nothing had changed since. Two things stopped or confused me. The hook test blocked with "no PR text to check", which is expected when there's no `--title`/`--body-file`, but I only knew that because the reason says so. And the repo's own `format:check` would have failed on my untracked local notes (`CLAUDE.local.md`, `plan.md`), not on anything in the PR. So I added `--ignore-path .git/info/exclude` to the format command and wrote down why in its Source column. Looking back, I left CI's Build job out of the manifest, so I ran `pnpm build` by hand for the description, and I'd add it as a sixth check.

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

Nothing is broken. What's missing is the design. CampusOS's landing page was built straight into code (#63's placeholder, then #87's header and hero), and nobody ever wrote down a design for it. So there's no agreed order of sections, no record of why the page looks the way it does, and two of the goals the issue lists (the open-source community and contributor onboarding) have nowhere to go.

**Expected Behavior**

The issue's acceptance criteria: a finalized UI/UX direction, a defined section hierarchy, responsive behavior thought through, a design-system direction, and design decisions concrete enough that someone could build from them. All of it is design and planning. The issue says implementation and frontend code are handled separately.

**Current Behavior**

`/` renders a working page: a hero with four hard-coded stat tiles, then Platform pillars (`#features`), Modules (`#modules`), Workflow (`#workflow`), a Launch call to action (`#launch`), and a footer. The repo has no landing page design document. The rendered page has no open-source, contributor, or GitHub content. The Modules section doesn't mention the plugin architecture, and the calendar appears only as one module card.

**Affected Components**

- `frontend/app/page.tsx`: the page, with its content in the `HIGHLIGHTS`, `MODULES`, and `STEPS` arrays and the stat tiles inline at lines 119–122. I'll read it but not change it.
- `frontend/components/LandingHeader.tsx`: the nav, with four hard-coded anchors (lines 37–55).
- `docs/frontend/DESIGN_SYSTEM.md` and `DARK_MODE.md`: the rules any design has to follow.
- `docs/frontend/`: where the missing design document belongs.

**Toolkit**

- Tool: Claude Code in my CampusOS clone. It read the issue body, `page.tsx`, `LandingHeader.tsx`, and my `CLAUDE.local.md`, then ran `pnpm dev` and fetched `/` to compare what the page serves with the source.
- What it produced: the four answers above, the line numbers for the content arrays and nav anchors, and a rendered-page check showing the same sections and headings as the source, with zero open-source or contributor matches.
- Difficulties: the first fetch returned a different app's page, because an unrelated Next.js server was already on IPv4 port 3000. It took a `lsof` on the port to see that CampusOS had bound only IPv6. A fetch is also not a look: it can't show me spacing, screen widths, or dark mode.
- What changed: I fetched `http://[::1]:3000/` instead, added that step to my repro steps, and listed screen-width and dark-mode behavior as unverified rather than claiming them.

---

## Reproduction Process

**Environment Setup**

macOS (Darwin 27.0.0), git 2.54.0, node v24.11.1. I forked NITRR-Official/CampusOS to trihiennguye-ux/CampusOS and cloned the fork. Branch `dev` is at `4263489` ("fix: add header and hero component and auth states for buttons #86 (#87)", 2026-08-05), which matched upstream `dev` on 2026-10-28, with a clean working tree. #60 is a design request, not a bug, so reproducing it means showing that the gap it describes is still real in today's code. My first checks were static checks on the source, because `pnpm` wasn't installed yet. On 2026-11-04, with `pnpm` and the dependencies installed, I ran `pnpm dev` in `frontend/` (Next.js 16.2.2) and fetched the rendered page at `/`. One snag: my first fetch returned a different app's page ("What are we planning?"). Another Next.js server from an unrelated project was already listening on IPv4 port 3000, and CampusOS had bound only IPv6, so I fetched `http://[::1]:3000/` instead. I still haven't looked at the page in a browser, so how it looks at different screen widths and in dark mode is unverified.

**Steps to Reproduce**

From a clean clone at `dev` `4263489`:

1. List the landing page's sections and headings: `grep -nE "<section|<footer|<h1|<h2" frontend/app/page.tsx`
2. Search the page and its two components for the themes the issue asks for: `grep -niE "open.?source|community|contribut|github|calendar|plugin" frontend/app/page.tsx frontend/components/LandingHeader.tsx frontend/components/LandingHeroActions.tsx`
3. Look for a landing page design doc: `ls docs/frontend` and `grep -rliE "landing" docs/`
4. Check the hero's stat tiles: `grep -nE "value: '" frontend/app/page.tsx`
5. Render the page and check that what's served matches the source: `cd frontend && pnpm dev`, then `curl -s -g 'http://[::1]:3000/'` and grep the HTML for section ids, `<h1>`/`<h2>` text, the stat values, and `open.?source|contribut|github`.
6. Observed result: the page already has a hero, four sections (`#features`, `#modules`, `#workflow`, `#launch`), and a footer, but there's no written design in the repo, and there are no open-source/community or contributor sections. The rendered page (HTTP 200) shows the same thing: the four ids, one `<h1>` ("Campus operations, finally in sync."), four `<h2>`s, the four stat values, and zero matches for open-source, contributor, or GitHub text.

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

  Step 5, rendered page:

  ```
  status: 200
  id="features"  id="modules"  id="workflow"  id="launch"
  h1: Campus operations, finally in sync.
  h2: Built for every campus team.
  h2: Everything your campus runs on.
  h2: From onboarding to outcomes.
  h2: Launch your campus OS.
  12+ 120+ 40+ 99.9%
  open-source/contributor/github matches: 0
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

The page was built in code before anyone designed it. #63 ("Landing Page Placeholder", merged 2026-05-23) created `frontend/app/page.tsx` and changed `docs/frontend/DESIGN_SYSTEM.md` and `OVERVIEW.md`, but added no landing page design document. #87 (merged 2026-08-05) added `LandingHeader` and `LandingHeroActions`. Because no section list was ever agreed in writing, the page's structure is whatever `LandingPage()` in `page.tsx` happens to render: the `HIGHLIGHTS` array (line 21) feeds `#features` (line 224), `MODULES` (line 45) feeds `#modules` (line 261), `STEPS` (line 68) feeds `#workflow` (line 295), and `#launch` (line 330) and the footer (line 362) are written inline. The nav in `LandingHeader.tsx` (lines 37–55) hard-codes those same four anchors. Each goal in #60 that isn't one of these arrays has no section: open-source/community and contributor onboarding have none at all, plugin architecture is missing from the `MODULES` copy, and the calendar is a single card. The stat tiles (lines 119–122) are inline placeholder numbers with no data source. My repro evidence supports this: `docs/frontend/` has no landing document, and the rendered page has the four sections and zero open-source or contributor text.

**Proposed Solution**

Write the missing design as one markdown spec, `docs/frontend/LANDING_PAGE.md`, and open it as a docs PR into `dev`. I chose markdown under `docs/frontend/` because the repo's existing design rules already live there as markdown (`DESIGN_SYSTEM.md`, `DARK_MODE.md`), a reviewer can check a text spec line by line in a PR, and #60 asks for design and planning only. The spec starts from the page that exists rather than from a blank page, and covers each acceptance criterion:

- **Section hierarchy:** the order of sections and what each one has to communicate, mapped against the issue's proposed list. It keeps the existing hero, Platform pillars, Modules, Workflow, Launch, and footer, and specifies the missing ones: an open-source/community section, a contributor onboarding section, plugin architecture in Modules, and a calendar showcase. Each one is a low-fidelity wireframe in a text block.
- **Responsive behavior:** what each section does at the Tailwind breakpoints the page already uses (`sm`, `md`, `lg`), including where the nav, hidden below `lg` today, goes on small screens.
- **Design-system direction:** which tokens, type scale, and components from `DESIGN_SYSTEM.md` each section uses, plus dark-mode notes per `DARK_MODE.md`.
- **Implementation-ready decisions:** for each section, its anchor id, the components it uses, where its content lives, and a decision on the hard-coded stat tiles. Also a list of what's left for the separate implementation issue.

What I won't change: no frontend code. `page.tsx`, `LandingHeader.tsx`, `LandingHeroActions.tsx`, and the design-system package stay exactly as they are, because #60 says implementation is handled separately. No edits to `DESIGN_SYSTEM.md` or any other existing doc, no high-fidelity mockups or Figma file, and no animation or interaction design beyond a short list of ideas the spec labels as unrequired.

Open question: my claim comment asked the owner whether they want a full design or only the sections below the hero, and in what format. There's no answer yet. If the owner picks a different format or scope, I'll follow that and record the change below as a deviation.

**Implementation Plan**

1. Post the section-hierarchy outline on #60 as my plan comment, as my claim comment promised, before writing the full spec.
2. Branch from `dev` in my fork: `docs/60-landing-page-design`.
3. Create `docs/frontend/LANDING_PAGE.md` with five top-level sections, one per acceptance criterion: UI/UX direction, Section hierarchy, Responsive behavior, Design-system direction, Implementation-ready decisions. Under Section hierarchy, give each page section (existing and new) a subsection with its goal, content, and text wireframe.
4. Write the Design-system direction section by citing `DESIGN_SYSTEM.md` sections 1–8 by name, so every color, type, and spacing choice points at an existing rule rather than a new one.
5. Run `pnpm format` so the new file passes the repo's `format:check` (its glob includes `**/*.md`).
6. Verify (each result can be observed):
   - Before, `ls docs/frontend` lists three files; after, it also lists `LANDING_PAGE.md`. Before, `grep -rliE landing docs/` matches only `OVERVIEW.md` and `docs/project/PROJECT_STRUCTURE.md`; after, it also matches the new spec.
   - `grep -nE "^#{2,3} " docs/frontend/LANDING_PAGE.md` shows a heading for each of the five acceptance criteria and a subsection for each of the eight sections the issue proposes, including open-source/community and contributor onboarding.
   - `pnpm format:check` exits 0.
   - `git diff --name-only dev...HEAD` prints only `docs/frontend/LANDING_PAGE.md`, which shows no frontend code changed.
   - Rendering `/` (`pnpm dev`, then fetch `http://[::1]:3000/`) still shows the same `<h1>` and four `<h2>`s as my repro, which confirms the page is untouched.
7. Open the PR into `dev` with `Closes #60`, the template's checklists filled in (type: "Documentation update", no screenshots since there's no UI change), and my AI-use disclosure.

Deviations:

- 2026-11-05: Step 1 done differently. I posted the section-hierarchy outline as its own comment on #60 (issuecomment-6647213727), not as the plan-comment.md draft, because the assignment says not to post that. The outline lists 8 sections with proposed new anchors (`#calendar`, `#open-source`, `#contribute`), asks the owner whether the order and scope work, and says I'll use the markdown spec in `docs/frontend/LANDING_PAGE.md` unless they ask for another format.
- 2026-11-05: Step 2 branch name. The branch is `feature/60-landing-page-design`, not `docs/60-landing-page-design`, because CONTRIBUTING.md asks for `feature/issue-number-description`. Branched from `dev` at 4263489, which matched upstream `dev`.
- 2026-11-05: Step 3 has 10 subsections under Section hierarchy, not 8. The issue's "Footer/navigation" is split into Header and navigation (2.0) and Footer (2.9), and the existing Launch CTA (2.8) gets its own subsection.

**Toolkit**

- Tool: Claude Code, drafting the Analysis from the line numbers and PR history it pulled (`gh pr view 63` and `87`), and checking the plan against my plan-check rubric's required checks before I run plan-check itself (assignment step 6).
- What it produced: this Solution Approach, with the cause traced to `LandingPage()` and its three content arrays, one committed deliverable (`docs/frontend/LANDING_PAGE.md`), and a test list where each check has an observable result. The same text is in `plan.md` at the top of my CampusOS clone.
- Difficulties: the owner hasn't answered my format question, so I had to commit to one format without them. My rubric's "A stranger could start" check fails a plan that leaves the choice open. A design-only issue also has no failing test to make pass, so the observable checks are about the file existing and covering the criteria, plus proof that no code changed.
- What changed: I chose the markdown spec and wrote down why, made "follow the owner's answer and record it as a deviation" an explicit rule, and put the outline post first so the owner sees the hierarchy before the full spec exists.

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

**PR Link:** https://github.com/NITRR-Official/CampusOS/pull/135

**PR Description:** "docs(frontend): add landing page design spec (#60)", opened 2026-11-05 from `feature/60-landing-page-design` into `dev`, with `Closes #60`. The PR adds one file, `docs/frontend/LANDING_PAGE.md`. The description explains why the spec is needed: the page was built in code (#63, #87) before any design was written down, three of #60's proposed sections were missing from the page, and the stat tiles are hard-coded. It lists what the spec covers, one top-level section per acceptance criterion:

- UI/UX direction
- section hierarchy, which keeps the existing sections and adds `#calendar`, `#open-source`, and `#contribute`
- responsive behavior, including a `Sheet` menu below `lg`
- design-system direction, citing `DESIGN_SYSTEM.md` and `DARK_MODE.md` with no new tokens
- implementation-ready decisions

It also says what isn't in the PR (frontend code, edits to existing docs, a Figma file) and how the PR differs from my plan (the branch name, and 10 subsections instead of 8). It shows before/after output for `ls docs/frontend`, the landing grep, the heading list, and `git diff --name-only`, plus proof that the rendered `/` didn't change, and a table of format, lint, type-check, build, and test results (all passing). It ends with my AI-use disclosure.

**Maintainer Feedback:**

- 2026-11-05: none yet. The only comment is Vercel's preview bot, and its preview check passed.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (`~/.claude/ship-gate/check_gate.py`), installed as a PreToolUse hook on `gh pr create`. It runs my `ship-manifest.md` checks, then pr-precheck against `plan.md`, the issue, and my test evidence. I ran it by hand with `--branch` first, then let the hook check the real `gh pr create`.
- What it produced: a log in `gate-log.md`. A BLOCK on the hook test ("no PR text to check"). Then two manual PASSes on `feature/60-landing-page-design` at head `9110cc1`, at 20:48 and 20:51 (I re-ran after editing the PR body). Each pass had all five manifest checks ok and all nine pr-precheck checks passing, for example "Diff stays inside the plan: Only docs/frontend/LANDING_PAGE.md changed". Last, a hook PASS at 20:52 that "honored the manual pass at 20:51", because the head, base, title, body, manifest, plan, evidence, and issue were all unchanged.
- Difficulties: the repo's `format:check` checks every `.md` file in the working tree, so my untracked local notes would have failed it even though the PR was fine. The first block confused me until I read its reason. And because any edit to the body changes the body fingerprint, each wording fix to the description meant another full manual run before the hook would pass.
- What changed: I added `--ignore-path .git/info/exclude` to the manifest's format command and recorded why. I re-ran the gate after my last body edit so the hook's pass matched the text I actually submitted. The next change I'd make is adding CI's `pnpm build` job to the manifest, since I ran it only by hand.

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
