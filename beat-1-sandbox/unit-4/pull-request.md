# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/118

**Branch**

fix/1-check-skip-query

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Only one full run occurred:
1. Full run: 20/20 scored items (bar: 18/20: PASS)

agreement: 20/20 scored items  (bar: 18/20: PASS)

**Package analysis**

Package: `pkg-01`
Rubric decision: `reject`
Gold label: `reject`
Reasoning: `pkg-01` belongs to the `standards-wall` category. The PR omitted required template sections and skipped mandatory repo requirements specified in the project guidelines without disclosing reasons for omission. The rubric check `repo-standards-compliance` evaluated the draft against the template requirements, identified missing mandatory sections, and assigned a `reject` verdict matching the gold label.

**Check rationale**

Check from `rubric.md`:
| decisive-test-evidence | Test evidence section read against the plan's repro steps, test plan, and repo checks | Pass if the evidence shows observable test execution, test runner pass outputs, or before-and-after command runs demonstrating the fix, and exercises the changed bug path; fail if evidence is mere unbacked conversational assertions ("tested locally, works"), exercises only an untouched path, or shows no execution output at all. | required |

Rationale: This check evaluates whether submitted test evidence actually confirms functionality against the plan rather than merely reporting a generic pass. It checks the test evidence section against the plan's repro steps, test plan, and repo checks to ensure that observable test execution, runner pass outputs, or before-and-after command runs are present. It was scoped to accept clear command outputs and runner passes to avoid rejecting valid workflows while ensuring unverified changes are stopped.

**Trade-offs**

Enforcing strict pre-fix failure evidence in `decisive-test-evidence` risks rejecting clean fixes where contributors did not capture the broken state before editing code. However, canary testing across packages confirmed that requiring before-and-after failure verification is essential to catch `not-tested` packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) without falsely rejecting valid contributions.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
