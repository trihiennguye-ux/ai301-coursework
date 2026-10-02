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

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

[Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it.]

**Trade-offs**

[Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
