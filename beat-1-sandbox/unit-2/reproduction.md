# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

trihiennguye-ux

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36
**Then paste the text of that comment underneath the link** 
Hi @Aburke225 could I claim this one? I'm going to read the profile/ingestion models in the review-routes code to see how "no ingested content" is represented, then add a test in tests/unit/test_review_routes.py that hits POST /reviews with a profile that has no ingested documents and checks it returns a proper error instead of crashing. I'll post my repro report once I've run it.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36
**Then paste the text of that comment underneath the link** 
Repro report for this issue, following up on my claim above.

**Environment**
- macOS 26.5.2 (arm64), Docker 28.5.2, Docker Compose v2.40.3
- Python 3.13.7 (SETUP.md asks for 3.11+; 3.13 is what I have installed)
- fastapi 0.141.1, starlette 1.7.0, SQLAlchemy 2.1.1, asyncpg 0.31.0, pydantic 2.13.5
- Postgres 16-alpine and Redis 7-alpine from `docker-compose.yml`, `.env` copied unchanged from `.env.example` (`LLM_PROVIDER=mock`)
- Code: my fork's `main` at `2f4e82f`, same as upstream `main`
- Deviations from SETUP.md: I started only the backend (`uvicorn api.main:app --port 8000`, the backend half of `make run`). The `vector-db` container (chromadb 0.4.22) exits on startup with ``AttributeError: `np.float_` was removed in the NumPy 2.0 release``. Nothing on the `POST /reviews` path uses the vector store (the RAG step in `review_service.py` is a placeholder), so I don't think it affects this, but I'm noting it.

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

**What I observed**
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

**What this means for the test**
On this commit, a profile with no ingested content doesn't crash and doesn't error. The request is accepted, and the review finishes as `complete` with a score and feedback, even though `sources_count=0`. That's different from what I assumed in my claim ("returns a proper error instead of crashing"). Because `POST /reviews` returns `pending` right away and processing happens in `process_review` as a background task, the "appropriate error" the issue asks for could go in one of two places:
1. `POST /reviews` rejects the request up front (e.g. a 4xx when the profile has nothing to ingest), or
2. `process_review` marks the review `failed` with an `error_message` when ingestion returns no sources.

@Aburke225, which behavior should the new test assert? I'm leaning toward (1) since the client finds out immediately, but I haven't looked at how the frontend handles either case yet, so I'd rather check with you before writing the test.


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (2026-09-25T03:16:48Z, graded from `~/.claude/skills/repro-check`): `agreement: 19/20 scored items  (bar: 18/20: PASS)`. The only miss was pkg-12.
2. Run 2 (2026-09-25T03:22:57Z, graded from `tools/repro-check`, the committed `eval-run.txt`): `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

The two runs have the same fingerprints (`rubric.md  sha256:41354750104c7539`, `evidence-guide.md  sha256:e65b0e2bec7a3ff5`, `SKILL.md  sha256:094c7ec26a0f22ed`), so the rubric didn't change between them. The one-point drop is pkg-05 flipping from `accept` to `reject` (`failed: Comms respect the repo`). The grader gave a different answer on identical inputs; I didn't change anything.

**Package analysis**

**pkg-12** (prettier/prettier#19795). My rubric decided **reject**, and the gold label is **accept**. Both runs gave the same result: `pkg-12  accept  reject   NO     failed: Steps followable, Comms respect the repo`.

My rubric read it that way because of how the report writes its steps:

> Steps: ran the issue's script verbatim in an empty directory:
> `$ node repro.mjs`
> with `repro.mjs` containing the issue's two `prettier.format` calls (shape A: rangeStart 19, rangeEnd 32; shape B: rangeStart 18, rangeEnd 31; both `parser: "babel"`).

The report never prints the contents of `repro.mjs`. Instead it points back at "the issue's script." My Steps followable check says "'same steps as the issue' without restating them fails this," so the grader treated "ran the issue's script verbatim" as a pointer, not a restated step. The same gap failed Comms respect the repo. The repo-facts line says the prettier template "asks for the prettier version, a playground or runnable reproduction, the input, the actual output, and the expected output," and the report shows no runnable file or playground link.

The gold note reads it differently: "both shapes reproduced on the current release with outputs shown; version delta stated; next step concrete." The report does give the rangeStart/rangeEnd values and the parser, and the inputs are exact strings in the issue. A stranger could rebuild `repro.mjs` without guessing anything, so my check was stricter than the gold standard here.

**Check rationale**

> | Steps followable | The repro report's steps section (Steps family) | A concrete starting state plus an ordered sequence of exact commands or actions a stranger could re-run without inferring a missing piece; "same steps as the issue" without restating them fails this | required |

The last clause is there because the most common way a repro comment goes wrong is "I followed the steps in the issue and it happened." That tells a maintainer nothing about whether the poster actually hit the trigger or changed it without saying so. My evidence guide puts the principle directly: "the reader should never have to open the issue to find out what step 2 actually was."

Run 1 showed this clause costs me pkg-12, and I kept it anyway. The alternative was to loosen the check to something like "restating the issue's steps by reference passes if the parameters are named." I rejected that because the check can't tell a careful reference (pkg-12 names every range offset and the parser) from a lazy one without the grader making the same judgment call I was trying to take away from it. Run 2 keeps the wording unchanged (same `rubric.md` hash), which records that decision.

**Trade-offs**

The Steps followable check trades pkg-12 for strictness. It's a clear-accept package that my rubric rejects in both runs, which is why `clear-accept` reads `7/8` in run 1 and `6/8` in run 2. The case I accept it will miss is any report that faithfully reruns the issue's own script and names its key parameters but doesn't paste the script. On gold labels that's a good package; under my rubric it's held.

What I gain is that nothing in the reject categories slipped through. Both runs score `no-evidence 4/4`, `unfollowable-comms 3/3`, and `wrong-target 4/4`, and those are the packages where "same steps as the issue" hides a skipped or changed trigger. I know the strictness didn't cost anything elsewhere because every other package's verdict is identical across the two runs except pkg-05. The pkg-05 flip is in a different check (Comms respect the repo) and happened with byte-identical skill files, so it's grader variance, not a result of this clause.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
