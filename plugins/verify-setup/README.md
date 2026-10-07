# verify-setup

Make the agent **prove** its work in your project instead of claiming it.

Run `/verify-setup:verify-setup` in a project. It looks at the repo, asks you six short questions
(what the thing is, what "done" means, what can be run live, what exists, who re-checks, what's
off-limits), then:

1. writes a live-run verifier (`verify/…`) for your main surface — real CLI on real input, real
   page in a browser, real endpoint, real output file — and **runs it once, pasting the output**;
2. adds a `## Verification — prove it, don't claim it` section to your `CLAUDE.md`.

## The opinions it bakes in

- Tests, lint and typecheck are hygiene, not evidence.
- Evidence = a live run of the real thing, with the output pasted from this session.
- Two layers: the agent proves its own work, then something that didn't do the work re-runs it.
- Reproducing a number is not proving it. Silent skips are failures.
- V1 executable > V2 observable > V3 judged — and never V3 alone.

Pass answers up front to skip the questions:
`/verify-setup:verify-setup it's a Node CLI, done = correct totals on the sample CSV, …`

Provided as-is, no support.
