# Evidence notes for the write-up (gathered, not graded)

These are raw excerpts from my record, for quoting. Delete this file before submitting if you don't want it in the repo.

## Key facts and dates (GitHub dates, UTC)

- Project: CampusOS (NITRR-Official/CampusOS), an open-source campus operations platform (Next.js frontend, plugin-based).
- Issue: https://github.com/NITRR-Official/CampusOS/issues/60, "[Frontend Design] Design Landing Page UI/UX for CampusOS"
- PR: https://github.com/NITRR-Official/CampusOS/pull/135, "docs(frontend): add landing page design spec (#60)"
- PR diff: 1 file added, `docs/frontend/LANDING_PAGE.md`, +402 / -0. Commit `9110cc1`.
- 2026-10-28 05:16: claim comment posted on #60 (after the voice guard stopped the first version)
- 2026-11-05 01:21: reproduction comment posted on #60
- 2026-11-05 01:37: section-hierarchy outline posted on #60
- 2026-11-05 ~01:52: PR #135 opened (through the ship-gate hook)
- 2026-11-05 08:18: maintainer TheFakeCreator (MEMBER) commented on the PR
- 2026-11-05 08:19:27: merged into `dev` by TheFakeCreator, merge commit `91b093a`. #60 closed through `Closes #60`.

## Proof: the reproduction (from my comment on #60, 2026-11-05)

> - `docs/frontend/` has `DARK_MODE.md`, `DESIGN_SYSTEM.md`, and `OVERVIEW.md`, but no landing page design doc. `grep -rliE landing docs/` matches only `OVERVIEW.md` and `docs/project/PROJECT_STRUCTURE.md`, and neither defines the page's sections.
> - The rendered `/` (from `pnpm dev`) has one `h1`, "Campus operations, finally in sync.", and four sections: `#features`, `#modules`, `#workflow`, and `#launch`. It has no "open source" or "contribut…" text, in the page or in `page.tsx`.
>
> So three of the sections this issue proposes have nothing on the page yet: open-source/community, contributor onboarding, and the calendar showcase (the calendar is one card in Modules). The hero stat tiles ("12+", "99.9%") are hard-coded in `page.tsx`.

## Proof: before/after test table (contribution-story.md, Testing Strategy)

| Check | Before (`dev`) | After (branch) |
| --- | --- | --- |
| `ls docs/frontend` | `DARK_MODE.md`, `DESIGN_SYSTEM.md`, `OVERVIEW.md` | the same three plus `LANDING_PAGE.md` |
| `git diff --name-only dev...HEAD` | n/a | only `docs/frontend/LANDING_PAGE.md` |
| `pnpm dev`, fetch `http://[::1]:3000/` | h1 "Campus operations, finally in sync.", four h2s, ids `features modules workflow launch` | identical, so the page is untouched |

Repo checks on the branch: format, lint, type-check, test (232 tests), and `pnpm build`, all exit 0.

## Proof: excerpt from the diff (`docs/frontend/LANDING_PAGE.md`, lines 368-380)

```markdown
### Stat tiles

Decision: **replace the four hero stat tiles with facts that are true of the repo.** Today they show "12+ Active modules", "40+ Teams onboarded", "120+ Automations", and "99.9% Uptime", which are hard-coded with no data source behind them. The replacements:

| Label        | Value                                      | Source                                                        |
| ------------ | ------------------------------------------ | ------------------------------------------------------------- |
| Plugins      | The number of plugin folders in `plugins/` | Count at build time, or update by hand when a plugin is added |
| License      | MIT                                        | `LICENSE`                                                     |
| Built with   | Next.js + Node                             | `frontend/`, `backend/`                                       |
| Contributing | Open, PRs into `dev`                       | `CONTRIBUTING.md`                                             |
```

## Proof: the maintainer's comment on PR #135 (TheFakeCreator, 2026-11-05 08:18 UTC), verbatim excerpts

> First of all, exceptional work on this spec! 👏
>
> Apologies for the delay in getting back to your questions on #60, but going with the Markdown spec in `docs/frontend/LANDING_PAGE.md` was 100% the right call.

> 1. **Format:** The Markdown spec with ASCII wireframes, responsive matrices, and component mappings is complete, rigorous, and implementation-ready. We don't require an external Figma file for this.
> 2. **Section Hierarchy & Order:** The 10-section breakdown (including separating Header/Footer and giving `#launch` its own CTA space) flows very logically from product capabilities down to community & contributor onboarding.
> 3. **Honest Metrics Principle:** Huge +1 on replacing the hardcoded vanity stat tiles (`99.9% uptime`, `120+ automations`) with verifiable repository facts (plugin counts, MIT license, stack, and contribution status).

> - We will go ahead and **approve & merge** this spec.
> - We'll track the actual frontend component build, mobile `Sheet` navigation, and plugin strip in a follow-up implementation issue referencing this spec.

## Candidate "change I made" #1: the voice guard stopped my claim comment

`~/.claude/voice-guard/guard-log.md`:

```text
- 2026-10-28 01:14 [hook] stop: rule broken: "A timeline or fix promise I can't back with something I've actually run." | target: gh issue comment https://github.com/NITRR-Official/CampusOS/issues/60 --body-file
- 2026-10-28 01:16 [hook] pass | target: gh issue comment https://github.com/NITRR-Official/CampusOS/issues/60 --body-file
```

Before (stopped draft line):

> I'll post that outline, or an update on where I'm stuck, here by Wednesday, November 4.

After (posted line, in the live claim comment):

> I'll post the outline here for review before I start on any mockups.

Context: repro-check had passed the same draft, because its rubric only looked for fix timelines.

## Candidate "change I made" #2: the ship-gate format check (manifest)

The repo's `format:check` checks every `.md` in the working tree, so my untracked local notes (`plan.md`, `test_evidence.md`, etc.) would have failed it. I added `--ignore-path .git/info/exclude` to the manifest's format command. The command as it ran in `~/.claude/ship-gate/gate-log.md` (2026-11-04 20:48):

```text
  - Format check: `pnpm exec prettier --check "**/*.{ts,tsx,js,jsx,json,md}" --ignore-path .prettierignore --ignore-path .git/info/exclude` -> ok (4s)
```

## Candidate "change I made" #3: scope changed from my plan

The plan said 8 subsections under Section hierarchy. While writing the spec I split "Footer/navigation" into 2.0 Header and navigation and 2.9 Footer, and gave the existing Launch CTA its own subsection (2.8), so it became 10. I logged that as a deviation. The maintainer's comment names this split specifically ("including separating Header/Footer and giving `#launch` its own CTA space").

Also: my claim comment called the page a "placeholder". Reading `page.tsx` for `CLAUDE.local.md` (unit 6) showed it was already a full landing page, so the design started from what existed.

## Tool proof: ship-gate pr-precheck lines (gate-log.md, 2026-11-04 20:48 manual PASS)

```text
- verdict: PASS
  - Diff stays inside the plan [pass]: Only docs/frontend/LANDING_PAGE.md changed, the plan's single deliverable; branch-name and 10-subsection differences are recorded in the plan's deviation notes and the description.
  - Evidence shows the fix [pass]: test_evidence.md shows before/after ls and grep, the heading list, git diff --name-only, unchanged h1/h2/section ids on rendered /, and format, lint, type-check, test and build outcomes.
```

## Material for Limitations (from contribution-story.md)

- The PR is a design spec only. No frontend code was changed. The build (components, mobile `Sheet` nav, plugin strip) is left to a follow-up implementation issue, per the maintainer.
- I didn't verify the calendar section's copy against what the `calendar`, `event`, and `scheduling` plugins actually do. I marked it "for example" and listed it under "Left for the implementation issue".
- No test can check whether the design is good. Only the owner could judge that.
- The owner never answered my claim questions on #60 before the PR, so every scope call was mine.
- Ship manifest left out CI's `pnpm build` job. I ran it only by hand.
- Environment set up late: no `pnpm` on day one led to the wrong "placeholder" claim.
- Possible next step: pick up the follow-up implementation issue and build the spec.
