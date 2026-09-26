# Evidence guide: where proof lives in a reproduction package

## Environment
* **Where it lives:** The environment block or header of the repro report, and any repo-facts runtime notes.
* **What good looks like:** Lists the exact commit SHA or pinned release tag, runtime engine and language versions (e.g., Python 3.11, Node v20.10.0), and operating system/architecture. If testing a newer release or different OS than the original issue, the delta is explicitly stated. Fails if it uses vague placeholders like "latest", assumes unstated global tools, or omits versions.

## Steps
* **Where it lives:** The reproduction steps block in the repro report.
* **What good looks like:** Provides explicit, copy-pasteable terminal commands starting from a clean checkout or standard fixture that leads directly to the trigger. Fails if steps omit crucial setup commands, skip prerequisites, or describe actions purely in vague prose.

## Behavior shown
* **Where it lives:** The artifacts, logs, terminal command outputs, exit codes, and diff excerpts in the repro report, read against the issue description.
* **What good looks like:** Raw, unedited execution output displaying the exact error trace, unexpected return code, or redundant execution described in the issue. In an honest cannot-reproduce scenario, raw terminal logs must demonstrate the exact commands invoked and show the non-triggering output or lack of failure. Fails if no artifact is included, or if the artifact shows an unrelated local failure (such as an unhandled configuration or syntax error).

## Honesty
* **Where it lives:** The conclusion and narrative observations of the repro report compared side-by-side with the output logs.
* **What good looks like:** The author claims only what the artifact proves. An evidenced report stating the bug could not be reproduced under matching conditions passes when accompanied by environment comparison or hypotheses (e.g., OS limits, shell differences). Fails if the author claims successful reproduction when the log displays an unrelated failure or graceful exit.

## Comms
* **Where it lives:** The claim comment, the repro comment, and the repo-facts policy block (such as CONTRIBUTING.md or AI usage policies).
* **What good looks like:** The claim comment promises an investigation rather than an immediate fix or delivery date. Where a repository policy mandates AI disclosure, the package contains an explicit disclosure statement. Fails if required disclosure is omitted or if the text is generic boilerplate over-promising solutions.
