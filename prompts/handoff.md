# handoff (paste-in prompt)

Copy everything below the line into your coding agent (Claude Code, Codex, Cursor, anything that
can write files in your project) when you're about to stop mid-task or start a fresh chat. It writes a
`HANDOFF.md` in your project root that a brand-new session can pick up from on its own: your rules
first, then what's built, the decisions, where things live, the dead ends, and the next step. Start
the fresh chat with the one line it prints at the end.

---

We're stopping this chat and picking it up in a fresh one. Write HANDOFF.md in the project root (overwrite it if it
exists). A fresh session with no memory of this chat will read it first, so if it isn't in this file, it's lost.

Keep it under 60 lines. Real file names, function names, commands and error text. No "various fixes".

Use these sections, in this order:

## Project rules (apply to everything)
Every rule or convention I gave you, word for word. These come first so the next session reads them before it plans.

## What's built
What works, what's half done, what's broken. The last command you ran to check it, and what it printed.

## Decisions
Each decision with a one-line why, so it doesn't get argued again.

## Where things live
The files that matter and what's in each. Mark the 2 to 5 to read first.

## Dead ends
What we tried that didn't work, and why, so it isn't tried again.

## Next
The single next step first, then the rest in order.

## Start the next session with
One line to paste into the fresh chat, e.g. "Read HANDOFF.md first, then <the next step>."

Date it at the top. When it's written, read it back and check a fresh session could pick up from it alone. Then print
the path and a one-line summary.
