# JoshDevBox

Free fixes from the channel: copy-paste prompts first, plus skills, plugins and mods I actually use,
published as a by-product of the videos.

> **Provided as-is, no support.** These are my working tools, shared so you can copy the idea.
> Read a prompt or skill before you use it. Issues/PRs may be ignored. MIT licensed.

## Prompts (copy-paste, no install)

Open the file, copy everything below its line, and paste it into your coding agent (Claude Code,
Codex, Cursor, anything that can read your repo and run commands) from your project's root.

| Prompt | What it does | Video |
| :- | :- | :- |
| [`verify-setup`](prompts/verify-setup.md) | Asks you a few questions about your project, then writes a `## Verification` section into your `CLAUDE.md` (or `AGENTS.md`) and one live check, and runs it once, so the agent *proves* its work (real UI / real CLI / real API / real output) instead of claiming it. Unit tests are hygiene, not evidence. | #4 — How I verify AI's work |

## Plugins (optional, for Claude Code)

Some items are also a [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
entry, so you can install one as a skill instead of pasting the prompt. One folder per item under
`plugins/`; install any single item without the rest.

### Install

Add the marketplace once (shell, outside a session):

```bash
claude plugin marketplace add chang47/JoshDevBox
```

Then install whatever you want by `<item>@joshdevbox`:

```bash
claude plugin install verify-setup@joshdevbox                  # for you, every project
claude plugin install verify-setup@joshdevbox --scope project  # just this repo, shared via .claude/settings.json
```

Inside a session the same thing is `/plugin marketplace add chang47/JoshDevBox` then
`/plugin install verify-setup@joshdevbox`. Remove with
`claude plugin uninstall verify-setup@joshdevbox` (or `claude plugin marketplace remove joshdevbox`
to drop everything).

### Items

| Item | What it does | Run it | Video |
| :- | :- | :- | :- |
| [`verify-setup`](plugins/verify-setup) | Interviews you about your project, then writes live-run verification rules into its `CLAUDE.md` so the agent *proves* its work (real UI / real CLI / real API / real output file) instead of claiming it. Unit tests are hygiene, not evidence. | `/verify-setup:verify-setup` | #4 — How I verify AI's work |

## Layout

```
prompts/<slug>.md                 # paste-in prompts (self-contained)
.claude-plugin/marketplace.json   # the catalog: one entry per item
plugins/<item>/
  .claude-plugin/plugin.json      # item manifest (name must match its marketplace entry)
  skills/<skill>/SKILL.md         # the actual skill
  README.md
```

Adding a prompt: drop it in `prompts/` and add a row to the Prompts table.
Adding a plugin: drop it in `plugins/<item>/`, add an entry to `marketplace.json`, then
`claude plugin validate .` must pass.

## License

MIT — see [LICENSE](LICENSE).
