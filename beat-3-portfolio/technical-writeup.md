# Writing the landing page design spec for CampusOS, merged as PR #135

## Overview

CampusOS is an open-source campus operations platform built with Next.js and a plugin system. Issue [#60](https://github.com/NITRR-Official/CampusOS/issues/60) asked for a landing page UI/UX design, but the page had already been built in code (PRs #63 and #87) without any design written down. Three of the sections the issue proposed were missing, and the hero's stat tiles showed made-up numbers. I reproduced that gap, posted a section outline for review, and wrote `docs/frontend/LANDING_PAGE.md`, a 402-line spec with one section per acceptance criterion. I opened it as [PR #135](https://github.com/NITRR-Official/CampusOS/pull/135), and the maintainer merged it into `dev` on 2026-11-05. The actual frontend build will be tracked in a follow-up issue that references the spec.

## What happened

### Claiming the issue, and getting it wrong at first

I claimed #60 on 2026-10-28 with two questions for the owner: is a full design still wanted after #63 and #87, and what format should it be in? My first claim comment was built on an assumption I hadn't checked:

> #63 added a landing page placeholder and #87 added the header and hero component, so is a full design still wanted, or should I design only the sections below the hero?

I hadn't installed `pnpm` yet, so I couldn't run the app. Later that day I read `frontend/app/page.tsx` to fill in my repo notes and found that it wasn't a placeholder at all. It was a full landing page with a hero, four sections, and a launch CTA. That changed my plan. Instead of designing a page from nothing, the spec would start from what already existed and name what was missing.

### Reproducing the gap

A design issue has no failing test, so I had to show what "missing" meant. On 2026-11-04 I installed the dependencies, ran `pnpm dev`, and fetched the page. My first fetch returned "What are we planning?", which came from a different Next.js app I had running on IPv4 port 3000. CampusOS was on IPv6, so I switched to `http://[::1]:3000/`. I posted the result on #60:

> - `docs/frontend/` has `DARK_MODE.md`, `DESIGN_SYSTEM.md`, and `OVERVIEW.md`, but no landing page design doc. `grep -rliE landing docs/` matches only `OVERVIEW.md` and `docs/project/PROJECT_STRUCTURE.md`, and neither defines the page's sections.
> - The rendered `/` (from `pnpm dev`) has one `h1`, "Campus operations, finally in sync.", and four sections: `#features`, `#modules`, `#workflow`, and `#launch`.
>
> So three of the sections this issue proposes have nothing on the page yet: open-source/community, contributor onboarding, and the calendar showcase.

### Deciding without an answer

The owner hadn't answered either of my questions, so I had to choose a format myself. I picked a markdown spec in `docs/frontend/`, next to the existing design docs, and wrote down why in my plan, with a rule to follow the owner's answer if it came later. I posted a section-hierarchy outline on #60 before writing the full spec, so the owner could see the structure first.

### Changing the plan while writing

My plan had 8 subsections under the section hierarchy. While writing, I saw that the issue's "Footer/navigation" was really two different parts of the page, and that the existing launch CTA needed its own entry. So I split it into 2.0 Header and navigation, 2.8 Launch CTA, and 2.9 Footer, which made 10 subsections, and I logged it as a deviation from the plan. I also made one firm decision about the stat tiles. This is from the diff:

```markdown
Decision: **replace the four hero stat tiles with facts that are true of the repo.** Today they show "12+ Active modules", "40+ Teams onboarded", "120+ Automations", and "99.9% Uptime", which are hard-coded with no data source behind them.

| Label        | Value                                      | Source                                                        |
| ------------ | ------------------------------------------ | ------------------------------------------------------------- |
| Plugins      | The number of plugin folders in `plugins/` | Count at build time, or update by hand when a plugin is added |
| License      | MIT                                        | `LICENSE`                                                     |
```

### Testing a document

To show the spec did what the issue asked without touching the page, I ran the same checks on `dev` and on my branch:

| Check | Before (`dev`) | After (branch) |
| --- | --- | --- |
| `ls docs/frontend` | `DARK_MODE.md`, `DESIGN_SYSTEM.md`, `OVERVIEW.md` | the same three plus `LANDING_PAGE.md` |
| `git diff --name-only dev...HEAD` | n/a | only `docs/frontend/LANDING_PAGE.md` |
| rendered `/` | h1 "Campus operations, finally in sync.", ids `features modules workflow launch` | identical |

Format, lint, type-check, the 232 tests, and `pnpm build` all passed. I opened PR #135 on 2026-11-05.

### The result

A few hours later, maintainer TheFakeCreator commented on the PR and answered the questions I had asked on #60:

> going with the Markdown spec in `docs/frontend/LANDING_PAGE.md` was 100% the right call.

> The 10-section breakdown (including separating Header/Footer and giving `#launch` its own CTA space) flows very logically from product capabilities down to community & contributor onboarding.

> Huge +1 on replacing the hardcoded vanity stat tiles (`99.9% uptime`, `120+ automations`) with verifiable repository facts

They merged the PR into `dev` at 08:19 UTC on 2026-11-05 (merge commit `91b093a`), and #60 closed automatically. The two decisions I made on my own, the format and the 10-section split, were the ones they called out.

## Limitations and what's next

The spec is only a design. No frontend code changed, so the page still shows the old stat tiles and is still missing the three new sections. The maintainer said the build will be "a follow-up implementation issue referencing this spec." I also never checked the calendar section's wording against what the `calendar`, `event`, and `scheduling` plugins actually do. I marked that copy "for example" and listed it under what's left for the implementation issue, but it's still unverified.

If I did it again, I'd set up the environment on day one. Not having `pnpm` is why my claim comment called the page a placeholder, which was wrong. I'd also add CI's build job to my pre-PR checks instead of running it by hand.

Next, I'd like to take on the implementation issue myself. I'd start by checking the calendar plugins against the spec's copy, and then build the new sections and the honest stat tiles the spec describes.
