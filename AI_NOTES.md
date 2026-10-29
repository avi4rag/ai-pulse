# AI implementation notes

## Job 1: Tenant isolation

Scoped dashboard reads to the authenticated membership and checked membership before organization selection. Regression tests first failed on both original vulnerabilities. The pre-existing lead-manager test remains failing until job 3.

My mistake: I initially tried reading README.md from the workspace parent directory. The Git repository was actually in its ai-pulse subfolder. Those commands failed without changing files. I corrected the working directory before reviewing the code.
