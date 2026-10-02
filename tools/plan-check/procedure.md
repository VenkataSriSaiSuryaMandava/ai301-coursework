# Procedure: how this skill grades a plan package

## Read order

1. Read `Repo facts` first to extract repo rules, contribution requirements, and specific AI usage/disclosure policies.
2. Read `Issue` and `Thread highlights` next to identify the original bug report, maintainer decisions, and explicit instructions from repo owners.
3. Read `Repro evidence` to establish the ground truth of the bug, noting the exact commands, error traces, and runtime environments that triggered the failure.
4. Read `Candidate plan` (diagnosis, scope, approach, files, and test plan) to evaluate technical substance.
5. Read `Candidate plan comment` to verify external communication against thread context and repo guidelines.

## Evidence gathering

1. **Diagnosis Evidence**: Extract the plan's stated root cause. Compare it against the reproduction logs/traces in `Repro evidence`. Note any divergence where the plan blames an unrelated component.
2. **Scope Evidence**: Extract all files and architectural changes proposed in the plan. Compare them against the minimal fix surface needed for the issue. Note any library upgrades, multi-file re-architecting, or unrelated cleanups.
3. **Executability Evidence**: Locate the target files, functions, and algorithmic steps in the plan. Determine if concrete files and code paths are identified, or whether the plan proposes open-ended investigation.
4. **Testability Evidence**: Locate the verification / test plan section. Identify the exact verification commands and the expected observable post-fix outcome.
5. **Thread & Convention Evidence**: Compare the plan comment against maintainer remarks in `Thread highlights` (did maintainers reject a workaround or request a specific code fix?) and contribution rules in `Repo facts` (is AI disclosure strictly required?).

## Check execution

Grade checks in this sequence:
1. Grade `grounded-diagnosis`: Compare stated cause against repro logs. If it contradicts proven repro behavior, mark `fail`.
2. Grade `bounded-scope`: Review change surface. If it includes drive-by migrations or refactors, mark `fail`.
3. Grade `executable-approach`: Check file and approach clarity. If vague or undecided, mark `fail`.
4. Grade `observable-test`: Check test assertions. If non-observable or missing post-conditions, mark `fail`.
5. Grade `thread-convention`: Check maintainer guidance and AI disclosure requirements. If violated, mark `fail`.
6. For any check where evidence is missing or ambiguous, mark `unclear`.

## Verdict assembly

1. Review grades for all five checks: `grounded-diagnosis`, `bounded-scope`, `executable-approach`, `observable-test`, `thread-convention`.
2. If every check is `pass`, set verdict to `accept`.
3. If any check is `fail` or `unclear`, set verdict to `reject`.
4. Construct the summary notes and the final fenced JSON block adhering strictly to the skill's output specification.
