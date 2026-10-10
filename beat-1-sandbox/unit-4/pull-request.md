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

20/20 scored items  (bar: 18/20: PASS)

**Package analysis**

Package: `pkg-01`
Rubric decision: `reject`
Gold label: `reject`
Reasoning: `pkg-01` belongs to the `standards-wall` category. The PR omitted required template sections and skipped mandatory repo requirements specified in the project guidelines without disclosing reasons for omission. The rubric check `repo-standards-compliance` evaluated the draft against the template requirements, identified missing mandatory sections, and assigned a `reject` verdict matching the gold label.

**Check rationale**

Check from `rubric.md`:
| `decisive-test-evidence` | `test_evidence.md`, PR testing section | Pre-fix execution exhibits the failure mode described in the issue; post-fix execution demonstrates passing tests; all repo checks (unit, lint, typecheck) pass or have documented valid environment exemptions. | required |

Rationale: This check requires test evidence to demonstrate both the pre-fix failure path and post-fix resolution rather than accepting a raw pytest output alone. In earlier revisions, runs could pass simply by showing a single green test, which failed to catch silent regressions or false-positive tests that never exercised the actual bug.

**Trade-offs**

Enforcing strict pre-fix failure evidence in `decisive-test-evidence` risks rejecting clean fixes where contributors did not capture the broken state before editing code. However, canary testing across packages confirmed that requiring before-and-after failure verification is essential to catch `not-tested` packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) without falsely rejecting valid contributions.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
