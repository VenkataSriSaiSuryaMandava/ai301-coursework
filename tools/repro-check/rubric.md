# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment block or header read against the issue's requirements | Pass if commit SHA, release tag, or explicit versions are recorded alongside runtime/OS without unpinned placeholders like "latest", or if version differences against the issue target are explicitly noted; fail if environment details are omitted or unpinned | required |
| followable-steps | The reproduction steps read against a clean checkout state or documented test environment | Pass if commands, inputs, and setup actions form an executable sequence a stranger could re-run from starting state to trigger; fail if steps rely on unstated local state or omit executable commands | required |
| behavior-matches | The raw artifacts (terminal logs, error outputs, or timings) read against the bug described in the issue | Pass if the output demonstrates the reported symptom, OR if an honest cannot-reproduce report displays artifacts confirming the exact commands executed alongside the observed non-triggering output; fail if no artifact is provided or the artifact shows an unrelated failure condition | required |
| honest-reporting | The narrative summary in the repro report read against the actual artifact output | Pass if the stated outcome matches what the output proves, including an evidenced cannot-reproduce with plausible environment/hypothesis divergence analysis; fail if the author asserts reproduction when output shows failure or claims more than the artifact demonstrates | required |
| comms-and-conventions | The claim comment and repro report read against repo contribution rules, voice guidelines, and stated AI policies | Pass if communications are specific, promise investigation rather than unverified solutions, and explicitly comply with repository AI disclosure requirements if stated; fail if AI disclosure is omitted when required or claim over-promises fixes | required |

## Verdict rule

Accept (ready) if and only if every required check passes. If any required check fails or evaluates to unclear, the verdict is reject (hold). Preferred checks never alter the verdict.
