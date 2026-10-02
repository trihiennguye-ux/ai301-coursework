# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

trihiennguye-ux

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36#issuecomment-5959497350

---

## Your branch

**Branch**

fix/36-review-no-ingested-content

**Evidence**

**Steps**
Starting state: a fresh clone, `cp .env.example .env`, `docker compose up -d`, `make setup` (migrations and the seeded `user1@example.com`), then the backend running on port 8000.

```bash
API=http://127.0.0.1:8000
# 1. log in as the seeded user
TOKEN=$(curl -s -X POST $API/auth/login -d 'username=user1@example.com&password=password1' \
  | python3 -c 'import json,sys;print(json.load(sys.stdin)["access_token"])')
# 2. create a profile with no github_username, portfolio_url, or resume
PID=$(curl -s -X POST $API/profiles -H "Authorization: Bearer $TOKEN" \
  | python3 -c 'import json,sys;print(json.load(sys.stdin)["id"])')
# 3. request a review for it
RID=$(curl -s -X POST $API/reviews -H "Authorization: Bearer $TOKEN" \
  -H 'Content-Type: application/json' -d "{\"profile_id\": \"$PID\"}" \
  | python3 -c 'import json,sys;print(json.load(sys.stdin)["id"])')
# 4. read it back after the background task runs
sleep 2; curl -s $API/reviews/$RID -H "Authorization: Bearer $TOKEN"
# 5. count ingested content for the profile
docker compose exec -T db psql -U pathreview -d pathreview_dev \
  -c "select count(*) from ingested_sources where profile_id='$PID';"
```

Before:
- Step 2 created profile `d6b9f207-…` with `"github_username":null,"portfolio_url":null,"resume_filename":null`.
- Step 3: `HTTP 200`, `"status":"pending"`.
- Step 5: `count = 0`, so the profile has no ingested content.
- Step 4: `HTTP 200`, `"status":"complete","overall_score":0.81`, with three feedback sections ("Technical Skills", "Project Experience", "Career Growth"), e.g. `"content":"Detailed feedback on technical skills based on portfolio analysis"`.
- Server log for that review:
  ```
  review_processing_started      review_id=bb1a8754-…
  ingestion_pipeline_completed   review_id=bb1a8754-… sources_count=0
  agent_orchestration_completed  review_id=bb1a8754-… sections_count=2
  review_processing_completed    overall_score=0.81 review_id=bb1a8754-…
  ```
- `grep -rln -E '"/reviews|process_review|_run_ingestion_pipeline|ingested_source' tests/` returns nothing, and `tests/unit/test_review_routes.py` doesn't exist yet. So I can confirm there's no test for this case, as the issue says.


After:
- Step 2 created profile `63d6f1fb-…` with `"github_username":null,"portfolio_url":null,"resume_filename":null`, same as before.
- Step 3: `HTTP 422`, `{"detail":"Profile has no content to review. Add a GitHub username, portfolio URL, or resume first."}`. On `2f4e82f` this was `HTTP 200`, `"status":"pending"`.
- Step 4: `count = 0`, so no review was created for the profile.
- Step 5: `count = 0`, unchanged.
- Server log for that request:
  ```
  review_rejected_no_content     profile_id=63d6f1fb-…
  ```
  There is no `review_processing_started` for that profile.
- Control: profile `5e9edc00-…` with `"github_username":"octocat"`. `POST /reviews` returned `HTTP 200`, `"status":"pending"`, and two seconds later `GET /reviews/$RID2` returned `HTTP 200`, `"status":"complete","overall_score":0.81` with the same three feedback sections. Server log for that review:
  ```
  review_processing_started      review_id=f24eabd9-…
  github_ingestion_failed        error="'raw_data' is an invalid keyword argument for IngestedSource" username=octocat
  ingestion_pipeline_completed   review_id=f24eabd9-… sources_count=1
  agent_orchestration_completed  review_id=f24eabd9-… sections_count=2
  review_processing_completed    overall_score=0.81 review_id=f24eabd9-…
  ```
  `ingested_sources` has `count = 0` for this profile too, because of the `raw_data` error above. That is a separate bug and I haven't changed it.
- I also ran the original steps against `2f4e82f` side by side on port 8001. It still gives `HTTP 200`, `"status":"pending"` at step 3 and `"status":"complete","overall_score":0.81` at step 4, so the difference comes from the branch.
- `grep -rln -E '"/reviews|process_review|_run_ingestion_pipeline|ingested_source' tests/` now returns `tests/unit/test_review_routes.py`.
- `.venv/bin/pytest tests/unit/test_review_routes.py -v -m unit`: `2 passed`. `make test-unit` equivalent (`pytest tests/unit -m unit`): `377 passed, 53 xfailed`, up from `375 passed, 53 xfailed` on `2f4e82f`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (2026-10-02T18:43:26Z, graded from `~/.claude/skills/plan-check`, the committed `eval-run.txt`): `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

That is the only run. The category line reads `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`, so the category floor holds too. The fingerprints in the header (`rubric.md  sha256:0e8ad8edbd8bfecd`, `evidence-guide.md  sha256:ab6a99a025d3dbf8`, `procedure.md  sha256:20cff6aa8bbc28dc`, `SKILL.md  sha256:4688d0d0f4cf6cb3`) match the files now in `tools/plan-check/`, so the run graded the same components I uploaded.

**Package analysis**

**pkg-20** (ghostty-org/ghostty#11261). My rubric decided **reject**, and the gold label is **reject**: `pkg-20  thread-convention  reject  reject   yes`.

I picked it because it is the package where every plan-quality check has nothing to object to, so the verdict rests on one check. The plan follows the maintainer's direction in the thread ("proposes instead recomputing `prev` only if the underlying page capacity changed during an operation"), stays bounded ("Not in scope: recomputing `prev` unconditionally (the approach the thread already rejected as too expensive), or any wider refactor of page memory management"), names its site (`Terminal.print`), and has an observable test ("`zig build test` with both fuzz-derived cases passing and the control unchanged").

What my rubric reads against it is the repo-facts policy line:

> contribution policy (CONTRIBUTING.md + AI_POLICY.md): strict AI rules. All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance

The plan comment runs from "I reproduced both fuzz cases on current main" to "pending the print benchmark" and never mentions AI use. My AI-policy compliance check says a policy that requires disclosing AI use in general "passes only if the comment discloses it in the poster's own words", and that "every package is treated as AI-assisted work", so the package is held. The verdict rule does the rest: "Any `required` check that fails, or comes back `unclear`, rejects the package." The gold note agrees with that reading: "excellent bounded plan that follows the thread's direction, but the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage".

**Check rationale**

> | AI-policy compliance | The repo-facts contribution-policy line read against the plan comment text (Comms family); every package is treated as AI-assisted work | An outright ban on AI-assisted contributions fails. A policy that requires disclosing AI use in general ("all AI usage must be disclosed") passes only if the comment discloses it in the poster's own words. A policy that only asks for disclosure in the pull request, only asks for understanding/responsibility/human-written comments, or states no AI policy, passes without a disclosure in the comment | required |

It has three tiers because the policy lines in the packages come in three kinds, and only one of them asks anything of a plan comment. The version I rejected was the simpler one: "if the repo states any AI policy, the comment must disclose." That would have been easier for a grader to apply, but it reads conditions as disclosure requirements when they aren't. pkg-05's policy is "generative AI tools welcome; you are responsible for all contributions and must review and understand AI-generated content before including it in a pull request", and pkg-09's says contributors "must state the tool and the extent of its use in the pull request" and then "the policy states no disclosure ask for issue comments". Neither comment discloses anything, and both are gold accepts. So the last sentence of the pass condition names those cases out loud (PR-only disclosure, understanding/responsibility, human-written comments, no policy) so the grader doesn't have to decide on its own whether a condition counts as a disclosure requirement.

I also kept "every package is treated as AI-assisted work" in the evidence column. Without it the check has no way to fail pkg-20, since nothing in the comment says AI was used.

**Trade-offs**

The packages this wording changes, compared with the simpler "any AI policy means disclose" version, are pkg-05 and pkg-09: both would flip from accept to reject, and `clear-accept` would drop from `7/7` to `5/7`. With the three-tier wording they agree with gold, and pkg-20 still rejects, so `thread-convention` stays `2/2`.

What I give up is the comment itself on PR-only policies. pkg-09's policy also says "comments to maintainers are expected to be in the contributor's own words and voice", and my check passes that tier "without a disclosure in the comment" without testing whether the comment is in the poster's own voice. A templated, AI-sounding comment on a repo like that would get through this check. I accept that miss in eval mode because voice can't be graded from a bundle; in live mode my `voice-guide.md` rule "Disclose AI use the way the repo asks" is what covers it, and that only reports, it doesn't change the verdict.

I did not re-run a canary with `--only`, because the single full run already covers both small categories and the rubric hasn't changed since (same `rubric.md` hash as the committed run).

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
