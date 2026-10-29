# AI implementation notes

## Job 1: Tenant isolation

Scoped dashboard reads to the authenticated membership and checked membership before organization selection. Regression tests first failed on both original vulnerabilities. The pre-existing lead-manager test remains failing until job 3.

My mistake: I initially tried reading README.md from the workspace parent directory. The Git repository was actually in its ai-pulse subfolder. Those commands failed without changing files. I corrected the working directory before reviewing the code.

## Job 2: Secrets and passwords

Untracked backend/.env, added a development template, required a strong production secret, added salted bcrypt passwords, configurable CORS, a non-root Docker image, and a development/local-database seed guard. All eight new security tests pass; full suite: 16 passed and the existing seniority failure remains. Rotate the old signing key manually; it remains in history. Existing SHA-256 accounts need password resets or a development reseed.

My mistake: my first patch tried deleting and adding the same path in one operation, which the patch tool rejected. No partial edits were applied; I reapplied it as an update.
