# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-commits | last 5 default-branch commits under Repo facts | At least one human commit or merged pull request within the last 90 days of the capture date | required |
| maintainer-response | maintainer first-response sample under Repo facts | Maintainer has commented, reviewed, or triaged within the last 90 days of the capture date | preferred |
| not-archived | archived line under Repo facts | Repo is not archived | required |
| release-activity | latest release under Repo facts | A release or version tag was published within the last 180 days of the capture date | preferred |
| bounded-scope | issue title, description, and comments | The work is an actionable, bounded change (such as a bug fix, documentation page/updates, test coverage, or isolated feature); not an umbrella or tracking issue, not an open-ended architecture debate, and does not require touching core internals | required |
| not-usage-question | issue title and description | Issue is an actionable task, bug report, or feature request, not a general usage or support question | required |
| unassigned | this issue: assignees line under Repo facts | No contributor is currently assigned | required |
| no-open-pr | linked PRs under Repo facts and comment thread | No open pull request is currently attempting to solve this issue | required |
| no-recent-claim | Comments section | No contributor has claimed the issue within the last 14 days without abandoning it | required |
| ai-policy-allowed | contribution policy under Repo facts | Fully AI-assisted work is not outright banned (conditional use, disclosures, or silent policies pass) | required |

## Verdict rule

Accept if every required check passes. If any required check fails or is unclear, the verdict is reject. Preferred checks never change the verdict, but help rank accepted issues.
