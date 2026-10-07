---
name: verify-setup
description: Set up how an AI agent must PROVE its work in this project. Interviews the user (stack, what "done" means, what can be run live — browser UI, CLI, API, device, output files), then writes a Verification section into the project's CLAUDE.md plus one runnable live-run verifier, and runs it once to prove it works. Use when the user says "verify-setup", "set up verification", "make the agent prove its work", "stop trusting green tests", or "how should Claude verify changes in this repo".
argument-hint: "[optional: answers to the interview, if you already know them]"
---

# Verification setup

You are setting up **how work gets proven in this project**. The output is (1) a
`## Verification` section in the project's `CLAUDE.md` and (2) at least one runnable live-run
verifier that you create AND run once, pasting its real output. Keep it small. Reason from the
user's answers; this file is principles + a short interview + templates, not a framework.

## The opinions (bake these into everything you write)

1. **Every task is three jobs: spec, implementation, proof.** If the agent writes all three, the
   proof can't fail: a check written against code it just wrote only certifies what the code
   already does.
2. **Green is not correct.** Unit tests, type checks and lint are *hygiene* — keep them, run them,
   but they are never the evidence that a change works.
3. **Prove with a live run of the real thing**, at the layer the change lives in:
   - UI → drive the real page in a browser (Playwright / a browser tool): click it, read the
     accessibility snapshot, check the console for errors, screenshot to a named path.
   - CLI → run the real binary on real input and show the real output.
   - API / service → hit the running endpoint and show status code + body.
   - Files / media / data → open the real output (probe it, parse it, count rows, render it).
   - Hardware / audio / "does it feel right" → a human checks; say so explicitly, never fake it.

   *A unit test can't see a dead button; a browser can't see a silent speaker.*
4. **Pasted output or it didn't happen.** "It passes" without the pasted output of the command run
   *in this session* counts as not done. Otherwise a claimed success and a real one look identical.
5. **Two layers.** Layer 1: the agent proves its own work with pasted live output. Layer 2:
   something that didn't do the work re-runs the check (a fresh session, a subagent with no
   context, CI, or the human).
6. **Reproducing a number is not proving it.** Every number in a claim must be read from its
   source in this session (the command output, the file, the page) — never carried over, never
   recomputed from the agent's own earlier claim.
7. **Silent skips are failures.** A green run that skipped the real check (no data loaded, test
   filtered out, server not running) proves nothing. Presence is not a pass.
8. **Verifier tiers** — prefer the top:
   - **V1 executable**: a command that exits 0/1 against the live thing.
   - **V2 observable**: a command that prints the properties of a real artifact (status code,
     duration, row count, screenshot path + size).
   - **V3 judged**: written criteria for genuinely subjective quality, each MET only with quoted
     evidence. **Never V3 alone** — if there is no V1/V2, the task isn't specified yet.
9. **Freeze the definition of done before work starts**; the agent never edits it. A goalpost the
   agent can move always passes.
10. **Match effort to the change.** A copy tweak needs a look, not a harness.

## Step 1 — Interview (short)

If `$ARGUMENTS` already answers these, don't re-ask. Otherwise first look at the repo yourself
(package.json / pyproject / Makefile / README / existing CLAUDE.md / test setup / installed tools
such as Playwright) so you only ask what you can't infer. Then ask **in one message**, numbered,
with your best guess pre-filled so the user can just say "yes" or correct it:

1. **What is it and how do users touch it?** (web UI, CLI, HTTP API, library, job/pipeline,
   mobile/device, generated files)
2. **What does "done" mean here** for a typical change? One concrete example of a recent change
   and how you'd *know* it worked.
3. **What can be run live from this machine?** Dev server command + URL, CLI entry point, API base
   URL, sample real inputs, a staging env — and what can't (hardware, paid APIs, prod).
4. **What already exists?** Test/lint commands, browser automation, fixtures/sample data, CI.
5. **Who/what does the independent re-check?** (fresh agent session, CI job, you, a teammate)
6. **Anything that must never be touched or run** (prod data, deploys, paid calls, secrets)?

Stop and wait for the answers. Only in a non-interactive run where no answers can come back, use
your inferred guesses, mark each one `(assumed)` in the CLAUDE.md section, and continue.

## Step 2 — Write the verifier

Create at least one **V1 live-run verifier** for the project's main surface, in a `verify/`
directory (or wherever the user prefers), using only tools already installed — **don't install
new tools without asking**. If the ideal tool (e.g. Playwright) is missing, pick a live check that
uses what exists and say what you'd add.

A good verifier:
- exercises the **real** entry point (starts/uses the real server, runs the real CLI, opens the
  real output) on **real or realistic input** — no mocks of the thing under test;
- asserts concrete, falsifiable expectations (exact output, status code, element text, row count)
  taken from the user's definition of done — not from what the code currently happens to do;
- prints what it observed (so the paste *is* the evidence) and exits non-zero on any mismatch;
- writes bulky output (screenshots, logs) to `verify/evidence/` and prints only paths + a tail.

Then **run it now** and paste the real output. If it fails, that's a finding — report it; don't
change the expectation to match the code. If it's cheap, also show it going red once (feed it a
known-bad input or a deliberately broken copy) so everyone knows it can fail, then restore.

## Step 3 — Write the CLAUDE.md section

Append to the project's `CLAUDE.md` (replace an existing Verification section; create the file if
missing). Keep it tight; fill every `<>` from the interview:

```markdown
## Verification — prove it, don't claim it

Tests/lint/typecheck (`<cmd>`) are hygiene: run them, but they are NOT evidence a change works.

**Evidence = a live run of the real thing, pasted.** A change is done only when you have run the
matching check below IN THIS SESSION and pasted its output. "It passes" without the paste = not done.

| Change touches | Prove it by | Command / how |
| :- | :- | :- |
| <surface, e.g. CLI output> | <V1: run real CLI on real input> | `<verify command>` |
| <UI> | <drive the page: snapshot + console errors + screenshot> | `<how>` |
| <API> | <hit running endpoint, show status + body> | `<curl …>` |
| <hardware / subjective> | HUMAN CHECK — say so, never fake it | <who> |

Rules:
- Write down "done" (expected output/behavior) BEFORE changing code; never edit it to make a check pass.
- Every number you report is read from this session's output — never carried over or recomputed.
- A skipped/empty run is a failure (0 rows, server not up, test filtered out ≠ pass).
- Never V3 alone: a "looks good" judgment needs a V1/V2 check beside it.
- Independent re-check: <who/what> re-runs `<verify command>` before this counts as shipped.
- Never: <forbidden actions from the interview>.
```

## Step 4 — Report

Reply with: the interview answers you used (mark assumed ones), the files you created or changed,
the CLAUDE.md section as written, and the verifier's **real pasted output** from Step 2. End with
the one-line command the independent re-checker should run.
