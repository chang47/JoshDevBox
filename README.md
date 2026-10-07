# channel-kit

Free tools from the channel — skills, plugins, and mods I actually use, published as a
by-product of the videos. One folder per item under `plugins/`, and the repo is a
[Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces), so
you can install any single item without the rest.

> **Provided as-is, no support.** These are my working tools, shared so you can copy the idea.
> Read a skill before you install it. Issues/PRs may be ignored. MIT licensed.

## Install

Add the marketplace once (shell, outside a session):

```bash
claude plugin marketplace add <owner>/<repo>
```

Then install whatever you want by `<item>@channel-kit`:

```bash
claude plugin install verify-setup@channel-kit                  # for you, every project
claude plugin install verify-setup@channel-kit --scope project  # just this repo, shared via .claude/settings.json
```

Inside a session the same thing is `/plugin marketplace add <owner>/<repo>` then
`/plugin install verify-setup@channel-kit`. Remove with
`claude plugin uninstall verify-setup@channel-kit` (or `claude plugin marketplace remove channel-kit`
to drop everything).

## Items

| Item | What it does | Run it | Video |
| :- | :- | :- | :- |
| [`verify-setup`](plugins/verify-setup) | Interviews you about your project, then writes live-run verification rules into its `CLAUDE.md` so the agent *proves* its work (real UI / real CLI / real API / real output file) instead of claiming it. Unit tests are hygiene, not evidence. | `/verify-setup:verify-setup` | #4 — How I verify AI's work |

## Layout

```
.claude-plugin/marketplace.json   # the catalog: one entry per item
plugins/<item>/
  .claude-plugin/plugin.json      # item manifest (name must match its marketplace entry)
  skills/<skill>/SKILL.md         # the actual skill
  README.md
```

Adding an item: drop it in `plugins/<item>/`, add an entry to `marketplace.json`, then
`claude plugin validate .` must pass.

## License

MIT — see [LICENSE](LICENSE).
