# Procedure: how this tool grades a PR package

## Read order
1. Read Repo facts and Issue: identify template asks, checklists, and AI disclosure rules.
2. Read Plan context: extract scope boundaries, files, approach, and deviation notes.
3. Read Candidate PR Title, Description, and Checklist: note claims and disclosures.
4. Read Candidate PR Diff and Commits: inspect diff file-by-file against plan and description.
5. Read Test evidence: compare against repro steps and bug path.

## Evidence gathering
1. Scope pairing: compare diff files and features against plan scope; identify any unannounced drift.
2. Diff hygiene: inspect diff lines for debug statements (print/eprintln), commented-out code, and formatting churn.
3. Evidence verification: confirm terminal outputs exercise the bug path before and after.
4. Standards gathering: check that required checklist items, issue link, and AI disclosure are present.
5. Truthfulness gathering: verify description claims match actual diff contents.

## Check execution
1. Grade bounded-scope: Pass if diff matches plan or deviation notes; fail on silent drift.
2. Grade decisive-test-evidence: Pass if before/after outputs exist on bug path; fail on vague assertions or control-path tests.
3. Grade reviewable-diff: Pass if diff is clean without debris; fail on debug prints, dead code, or churn.
4. Grade repo-standards-compliance: Pass if template asks, checklists, and AI disclosure policies are met; fail if ignored.
5. Grade truthful-description: Pass if description claims match diff reality; fail on false claims.

## Verdict assembly
1. Review all five check grades.
2. Accept if and only if all five required checks pass.
3. Reject if any required check is fail or unclear.
4. Output summary followed by the required JSON block.
