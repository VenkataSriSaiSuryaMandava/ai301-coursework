# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1

**Verdict output**

```json
[
  {
    "issue": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1",
    "checks": [
      {"name": "maintainer-commits", "grade": "pass", "evidence": "Last 5 commits on main are all within the last few days"},
      {"name": "maintainer-response", "grade": "pass", "evidence": "Maintainers actively triage and merge recent PRs within 30 days"},
      {"name": "not-archived", "grade": "pass", "evidence": "archived: false"},
      {"name": "release-activity", "grade": "pass", "evidence": "Recent release/tags published within 180 days"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "The issue requests a bounded optimization/fix to skip unnecessary rebuild steps when content hash is unchanged"},
      {"name": "not-usage-question", "grade": "pass", "evidence": "Actionable bug/enhancement request with clear reproduction and problem description"},
      {"name": "unassigned", "grade": "pass", "evidence": "assignees: []"},
      {"name": "no-open-pr", "grade": "pass", "evidence": "Repo has 0 pull requests in any state; issue timeline shows no cross-referenced PR"},
      {"name": "no-recent-claim", "grade": "pass", "evidence": "comments: 0; timeline contains only label events from 2026-09-10"},
      {"name": "ai-policy-allowed", "grade": "pass", "evidence": "No CONTRIBUTING.md/AI_POLICY.md at root; docs/CONTRIBUTING.md, README, and PR template contain no AI-use restriction"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Run 1 (full suite): 17/20 scored items (bar: 18/20: below the bar). Disagreed on issue-06, issue-14, and issue-19.
- Run 2 (targeted re-test): python3 run_eval.py --rubric ~/.claude/skills/issue-select/rubric.md --only issue-06 -> agreement: 1/1 scored items (accept).
- Run 3 (confirming full suite with --save-run): 18/20 scored items (bar: 18/20: PASS). Exactly matches the agreement line in committed eval-run.txt.

**Issue analysis**

- Scored issue id: `issue-06`
- Rubric decision: `accept`
- Gold label: `accept`
- Reasoning: In Run 1, issue-06 failed because maintainer-response was required and evaluated against an issue sample with response latency over 60 days, despite the repository being actively maintained with default-branch commits within 90 days. Changing maintainer-response to preferred while keeping maintainer-commits required enabled issue-06 to correctly evaluate to accept, matching the gold label.

**Check rationale**

Quoted check from `tools/issue-select/rubric.md`:
`| maintainer-commits | last 5 default-branch commits under Repo facts | At least one human commit or merged pull request within the last 90 days of the capture date | required |`

Reasoning behind current form: A repository with no human commit activity will leave opened pull requests stranded indefinitely. Requiring a human commit or merged PR on the default branch within 90 days establishes a reliable signal of active project maintenance without false-failing healthy repos that have longer issue-triage cadences.

**Trade-offs**

What this check gives up: A 90-day window cannot detect repositories that became abandoned within the last 30 to 60 days. In exchange, it prevents false negatives on mature, stable projects that receive infrequent releases. Making maintainer-commits the primary required check and maintainer-response preferred flipped canary test issue-06 to accept without causing regressions across dead-repo benchmark items.

---

## Selection rationale

**Selection rationale**

1. Fit to interests and time: Issue #1 focuses on content hash checks and eliminating redundant build processing. This aligns directly with my interests in software tooling, build pipelines, and runtime efficiency, fitting within the scope of Unit 2 reproduction and Unit 3 implementation.
2. What verdict identified correctly vs what I weighed: The verdict confirmed essential preconditions: active maintenance, no assignees, zero competing PRs, and clear AI-use guidelines. I additionally weighed that the issue isolates an unambiguous hash validation step, making local reproduction reliable.
3. Anticipated difficulty in claiming: Low difficulty. The issue has no assignees, zero linked pull requests, and no competing claims, leaving it open to claim under the Unit 2 claiming guidelines.
