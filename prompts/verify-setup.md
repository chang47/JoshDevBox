# verify-setup (paste-in prompt)

Copy everything below the line into your coding agent (Claude Code, Codex, Cursor, anything that
can read your repo and run commands), from the root of your project. It asks you a few questions,
then writes a `## Verification` section into your `CLAUDE.md` (or `AGENTS.md`) plus one live check,
and runs that check once so you see it work. Same thing as the `verify-setup` plugin, no install.

---

Set up how you (the agent) must PROVE your work in this project, instead of claiming it.
The output is (1) a `## Verification` section in this project's agent instructions file and
(2) one runnable live check that you create AND run once, pasting its real output. Keep it small.

**The rules to bake in:**
1. Every task is three jobs: what done means, the code, and the check. If you write all three, the
   check can't fail, because a check written against code you just wrote only confirms what it does.
2. Unit tests, type checks and lint are hygiene. Run them, but they are never the evidence that a
   change works.
3. Prove it with a live run of the real thing, at the layer the change lives in: UI → drive the real
   page in a browser and look; CLI → run the real command on real input; API → hit the running
   server, show the status + body (and the log if there is one); files/data → open the real output.
   Hardware, audio or "does it feel right" → a human checks; say so, never fake it.
4. Pasted output or it didn't happen. "It passes" without the output of a command run in this
   session means not done.
5. Every number in a claim is read from this session's output, never carried over or recomputed.
6. A skipped or empty run is a failure (0 rows, server not up, test filtered out).
7. Write down "done" before changing code, and never edit it to make a check pass.
8. Two layers: you prove your own work, then something that didn't do the work (a fresh session,
   CI, or me) re-runs the check.

**Step 1: interview.** First look at the repo yourself (package files, README, existing
instructions, test setup, tools already installed) so you only ask what you can't tell. Then ask me
these in ONE message, numbered, each with your best guess filled in so I can just say "yes":
1. What is it, and how do people use it (web UI, CLI, API, library, job, mobile, generated files)?
2. What does "done" mean for a typical change? One recent example and how I'd know it worked.
3. What can run live on this machine (dev server + URL, CLI entry point, sample inputs), and what
   can't (hardware, paid APIs, prod)?
4. What already exists (test/lint commands, browser automation, fixtures, CI)?
5. Who re-checks independently (fresh session, CI, me)?
6. Anything that must never be touched or run (prod data, deploys, paid calls, secrets)?
Then stop and wait for my answers. (If I already answered them after this prompt, don't re-ask.)

**Step 2: write one live check** for the main surface, in `verify/`, using only tools that are
already installed (don't install anything without asking). It must use the real entry point on
real or realistic input with no mocks of the thing under test, assert concrete expectations taken
from my definition of done (not from what the code happens to do now), print what it saw, and exit
non-zero on any mismatch. Run it now and paste the output. If it fails, that's a finding: report
it, don't change the expectation to match the code.

**Step 3: write the section.** Append this to `CLAUDE.md` (or `AGENTS.md` if that's what the
project uses; replace an existing Verification section), filling every `<>` from my answers:

```markdown
## Verification: prove it, don't claim it

Tests/lint/typecheck (`<cmd>`) are hygiene: run them, but they are NOT evidence a change works.
A change is done only when you've run the matching check below IN THIS SESSION and pasted its output.

| Change touches | Prove it by | Command / how |
| :- | :- | :- |
| <surface> | <live run of the real thing> | `<command>` |
| <hardware / subjective> | HUMAN CHECK, say so | <who> |

- Write down "done" before changing code; never edit it to make a check pass.
- Every number you report is read from this session's output.
- A skipped or empty run is a failure.
- Independent re-check: <who> re-runs `<command>` before it counts as shipped.
- Never: <off-limits actions>.
```

**Step 4: report** the answers you used, the files you created or changed, the section as
written, and the check's real pasted output. End with the one command someone else should run to
re-check it.
