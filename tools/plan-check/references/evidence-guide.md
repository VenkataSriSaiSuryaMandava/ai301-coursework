# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- **Where it lives**: In an eval bundle, find the root cause in `Candidate plan` (under Diagnosis, Cause, or Root Cause) and compare it with the logs, terminal traces, and commands in `Repro evidence`. In live mode, locate the cause in `plan.md` and check against the posted repro comment on the issue.
- **What good looks like**: The root cause specifically addresses the failure mechanism demonstrated in the reproduction output. A poor diagnosis blames an unrelated module, an uncalled method, or an unrelated build artifact.

## Scope

- **Where it lives**: In `Candidate plan` under Scope, Files touched, or Proposed Changes.
- **What good looks like**: The proposed modifications touch only the files and functions necessary to fix the isolated bug. Scope creep involves refactoring adjacent modules, migrating dependencies, redesigning state machines, or adding unrequested features.

## Executability

- **Where it lives**: In `Candidate plan` under Approach, Implementation, or Files.
- **What good looks like**: Concrete target files, functions, or specific logic branches are named with a clear algorithmic fix. An unbuildable plan says "investigate further", "profile to find out where", or offers multiple conflicting options without choosing one.

## Testability

- **Where it lives**: In `Candidate plan` under Test plan or Verification.
- **What good looks like**: The test plan specifies exact reproduction steps or test suites to run, along with an observable post-fix outcome (e.g. exit status 0, absence of specific exception, or expected return value). Merely writing "manually test" or "ensure it works" fails.

## Thread and conventions

- **Where it lives**: In `Candidate plan comment`, read against `Thread highlights` and `Repo facts` (CONTRIBUTING.md, AI_POLICY.md).
- **What good looks like**: The plan comment aligns with maintainer consensus and instructions (e.g., executing a code fix if the maintainer requested one, rather than a docs workaround). If `Repo facts` states a mandatory AI disclosure policy, the comment must explicitly include the required disclosure statement.
