# Voice guide: how I talk upstream

## Who I am in threads
I am an engineering student contributing as part of AI301. I approach issues methodically by verifying reproduction before discussing solutions, providing direct evidence, and respecting maintainer time.

## Rules I write by

### Rule: Promise investigation, never assert fixes or delivery dates
Maintainers need to know who is actively investigating, not speculative fix promises.
- Wrong: "I know how to fix this! I will submit a PR by tomorrow rewriting the cache layer."
- Right: "Claiming this issue for AI301. I will set up the local environment, test reproduction of the redundant build behavior on commit HEAD, and post my findings here."

### Rule: Anchor every observation in literal evidence
Do not describe results with vague adjectives or unmeasured impressions.
- Wrong: "The build feels really slow and seems broken when checking hashes."
- Right: "Running the build twice on an unchanged working tree takes 4.2s on the second run and re-executes step 3 (bundling) despite matching artifact checksums."

### Rule: Maintain professional economy
Remove conversational greetings, personal appeals, and student status framing.
- Wrong: "Hello maintainers! Hope you are having a wonderful day. I am a student trying my best, please assign me this issue!"
- Right: "Claiming this issue for AI301. Reproduction steps and baseline output are documented below."

### Rule: Explicit AI disclosure compliance
Whenever a repository requires AI usage disclosure, state tool involvement clearly.
- Wrong: [Omits AI disclosure on a project that explicitly mandates it in CONTRIBUTING.md]
- Right: "Note: In accordance with this repository's contributing guidelines, Claude Code / AI tooling was used to assist in diagnosing this reproduction."

## Things I never post
- Guesses at root causes without reproduction traces
- Promises of pull requests or delivery timelines
- Boilerplate "+1" or "same here" comments without reproducible environment logs
- Apologetic or pleading introductory remarks
