# AI implementation notes

## Job 1: Tenant isolation

Scoped dashboard reads to the authenticated membership and checked membership before organization selection. Regression tests first failed on both original vulnerabilities. The pre-existing lead-manager test remains failing until job 3.

My mistake: I initially tried reading README.md from the workspace parent directory. The Git repository was actually in its ai-pulse subfolder. Those commands failed without changing files. I corrected the working directory before reviewing the code.

## Job 2: Secrets and passwords

Untracked backend/.env, added a development template, required a strong production secret, added salted bcrypt passwords, configurable CORS, a non-root Docker image, and a development/local-database seed guard. All eight new security tests pass; full suite: 16 passed and the existing seniority failure remains. Rotate the old signing key manually; it remains in history. Existing SHA-256 accounts need password resets or a development reseed.

My mistake: my first patch tried deleting and adding the same path in one operation, which the patch tool rejected. No partial edits were applied; I reapplied it as an update.

## Job 3: Scoring and question assignment

Averaged each respondent before departments, shared the <2/<3/<4 maturity scale, ranked seniority explicitly, enforced assigned questions, and used a unique constraint plus SQLite upsert to replace answers. Tests cover unequal answer counts, unassessed groups, thresholds, invalid batches, role/seniority restrictions and database uniqueness. Full suite: 29 passed. Existing databases need a planned migration before using the new constraint; development databases can be reseeded. No new implementation mistake found in this job.

## Job 4: AI validation

Validated all ten canned replies, stripped code fences, required integer scores 1–5 (excluding booleans), filtered flags, defaulted reasons and retried once before saving unscored answers. Separated system instructions from JSON employee data and bounded live requests to 15 seconds per attempt. Tests cover malformed shapes, retries, HTTP/timeout failures, prompt separation and unscored persistence. Prompt separation reduces injection risk; it does not prove model obedience. No new implementation mistake found.

## Job 5: Small-group privacy

Defined completion as a nonempty answer to every currently assigned question, including unscored open answers. Only completers contribute scores and risks. Departments need five completers; smaller departments combine anonymously only if Other teams reaches five, otherwise return just a hidden placeholder without names or counts. Seed now has five completed Sales members and small incomplete departments. Updated earlier tests for this intentional seed change.

My mistake: a privacy test accessed an expired SQLAlchemy object after its session closed, causing DetachedInstanceError. I saved its integer ID before closing the session and reran the suite.

## Job 6: Right to delete

Added DELETE /reports/me scoped to the authenticated membership. Reports and dashboards are computed from remaining rows immediately; repeated deletion is harmless. Tests verify five-to-four suppression, empty reports, authentication, other-user preservation and the caller's other organization remaining unchanged.

My mistake: the cross-organization test counted rows before committing its setup. With autoflush disabled, that excluded the two new rows. I moved the baseline count after commit.

## Job 7: Personal improvement suggestions

Added POST /reports/me/suggestions with three validated actions, mock and live support, one retry and existing role advice as fallback. Sends only the caller's role, seniority, question IDs, scores, allowed flags and fixed answer choices. Raw open text and reasons are excluded to avoid forwarding embedded identifiers. Suggestions regenerate, so deletion leaves no stored copy and subsequent requests return none. Frontend display follows in job 9. Full suite: 80 passed; live calls are mocked.

My mistake: adding a final full stop after a 240-character cap could produce 241 characters. I reduced the pre-punctuation cap and tightened the test. The content filter is conservative basic validation, not a guarantee against every unsafe model suggestion.

## Job 8: Developer experience

Added a Python command runner for backend/frontend/test/build/seed, clean-checkout environment setup, and GitHub Actions for Python tests plus npm ci/build. Added employee/admin, multi-org permissions, expired/forged/missing/malformed claim coverage; malformed signed IDs now return authorization errors. Validation: 90 backend tests pass; npm ci and production build pass. The first build hit a Windows sandbox ancestor-directory access restriction; rerunning with approved filesystem access passed without changing build code. Docker image execution has not been verified locally. No new implementation mistake found.
