# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)
- Where it lives: Compare Plan context (scope, files to touch, approach, deviation notes) against candidate PR Diff and description claims. In live mode, compare plan.md (and Deviations) against git diff main...HEAD and pr_draft.md.
- What good looks like: Every modified file and behavior matches the plan or an explicit deviation note. Disclosed shortfalls pass; unannounced scope expansion or false description claims fail.

## Test evidence (harness category: not-tested)
- Where it lives: Test evidence section of the candidate PR read against Issue repro steps and Plan test plan. In live mode, test_evidence.md and PR Testing section.
- What good looks like: Verifiable commands and terminal outputs demonstrating reproduction on the broken path before the fix, and resolution on that same path after. Assertions like "tests pass" without output fail.

## Diff quality (harness category: unreviewable)
- Where it lives: Diff hunks, comments, formatting, and commit messages.
- What good looks like: Clean code implementing only the fix. No debug print statements (e.g. eprintln, print), no commented-out trial code, no dead helpers, no formatting churn.

## Standards and comms (harness category: standards-wall)
- Where it lives: PR Title, Description, and checklist read against Repo facts (PR template and AI policies).
- What good looks like: Template sections completed, issue reference Closes #<num> present, and explicit AI disclosure included where mandated by repo policy or course requirements.
