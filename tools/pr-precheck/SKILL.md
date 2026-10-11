---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit.
---

# pr-precheck: rubric-driven PR grading

## The question
A pr-precheck tool answers exactly one question about exactly one PR package: is this ready to submit?

## Inputs and modes
- Live mode: Inputs are plan.md (with deviation notes), git diff main...HEAD, draft PR title and description in pr_draft.md, captured test outputs in test_evidence.md, read against the issue on Path Review.
- Eval mode: The bundle text is the whole world. Nothing is fetched or read from outside.

## The scope seam (live mode only)
In live mode, read scope.md first. If Repo: contains a bracketed placeholder or does not match the target repo, refuse to grade. In eval mode, ignore scope.md.

## The voice seam (live mode only)
In live mode, read voice-guide.md and check draft text against its rules. In eval mode, ignore voice-guide.md.

## Refusal rules
Refuse to grade if rubric.md or procedure.md has empty content. In live mode, refuse if required input files are missing or empty.

## Grading discipline
1. Evidence first: cite literal quotes or lines for each grade.
2. Grade the thing, not the polish: concise complete PRs can pass.
3. The rubric decides: apply pass/fail conditions strictly.
4. Unclear defaults to fail.

## Verdict and output
End your response with a fenced JSON block matching this schema exactly:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```
