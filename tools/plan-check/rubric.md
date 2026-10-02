# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-diagnosis | Plan's stated diagnosis/root cause read against `Repro evidence` (and `Issue` description) | Pass if the identified root cause and error mechanism directly explain and align with the failure symptoms, logs, or error traces shown in the reproduction evidence; fail if the diagnosis contradicts, ignores, or misattributes the proven breakdown to an unaffected component or layer. | required |
| bounded-scope | Plan's scope and file touch list read against the reported problem in `Issue` and `Repro evidence` | Pass if the proposed edits are strictly limited to fixing the isolated bug; fail if the plan introduces scope creep, drive-by refactoring, unrequested migrations, or system rewrites beyond what is necessary to resolve the issue. | required |
| executable-approach | Plan's files, approach, and implementation steps read as an actionable work order | Pass if the plan specifies concrete target files or identifiable code locations and an unambiguous, actionable strategy that an engineer could begin executing without guessing; fail if the plan is vague, exploratory ("poke around", "profile and see"), or leaves the core approach undecided. | required |
| observable-test | Plan's test plan read against the reproduction steps and expected results | Pass if the test plan defines concrete steps with clear, observable success criteria (expected vs. actual output, error disappearance, or automated test assertion) confirming the bug is resolved; fail if the test plan specifies no observable verification criteria or merely asserts "verify it works". | required |
| thread-convention | Candidate plan comment read against `Thread highlights` and `Repo facts` (including contribution/AI policy) | Pass if the comment adheres to explicit maintainer instructions/direction in the issue thread (e.g. not substituting a workaround for a requested code fix) and complies with documented repository contribution policies (including mandatory AI disclosure rules if stated); fail if it defies maintainer direction or omits mandatory policy disclosures. | required |

## Verdict rule

A plan package is **accept** if and only if all five required checks (`grounded-diagnosis`, `bounded-scope`, `executable-approach`, `observable-test`, `thread-convention`) receive a grade of `pass`.

If any required check receives a grade of `fail` or `unclear`, the package is **reject**.
