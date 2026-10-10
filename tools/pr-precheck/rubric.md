# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| bounded-scope | Diff changed files and hunks read against Plan context scope boundary and deviation notes | Pass if all code changes fall within the plan's stated scope, files, or explicit deviation notes; fail if the diff touches unmentioned files, adds unannounced CLI flags/config options, or includes silent functional refactoring (silent drift). Honest disclosed shortfalls pass if documented in both plan deviations and PR description. | required |
| decisive-test-evidence | Test evidence section read against the plan's repro steps, test plan, and repo checks | Pass if the evidence shows observable test execution, test runner pass outputs, or before-and-after command runs demonstrating the fix, and exercises the changed bug path; fail if evidence is mere unbacked conversational assertions ("tested locally, works"), exercises only an untouched path, or shows no execution output at all. | required |
| reviewable-diff | Diff hunks, formatting, comments, and commit messages | Pass if the diff contains clean implementation code free of debugging debris (e.g. debug prints, println/eprintln logging), commented-out dead code/experiments, unformatted churn, or unrelated whitespace changes; fail if extraneous noise, dead code, or unrelated churn buries the fix. | required |
| repo-standards-compliance | PR description checklist, issue link, and AI disclosure read against Repo facts and template asks | Pass if the PR satisfies explicit template requirements and issue links, and includes AI disclosure when the repository's stated contribution policy explicitly mandates AI disclosure; fail if explicit template checklists, mandatory sections, issue references, or explicitly mandated AI policies are ignored. In eval mode, if the repo states no AI policy, omit the AI requirement. | required |
| truthful-description | PR description summary and claims read directly against the actual diff contents | Pass if the description accurately reflects the change without claiming uncommitted modifications or misrepresenting scope; fail if the description claims changes absent from the diff (e.g. claiming docs were updated when diff omits them) or falsely claims no changes beyond plan when unannounced hunks exist. Terse or concise descriptions are acceptable. | required |

## Verdict rule

A pull request package is **accept** if and only if all five required checks (`bounded-scope`, `decisive-test-evidence`, `reviewable-diff`, `repo-standards-compliance`, `truthful-description`) receive a grade of `pass`.

If any required check receives a grade of `fail` or `unclear`, the verdict is **reject**. Preferred checks never alter the verdict.
