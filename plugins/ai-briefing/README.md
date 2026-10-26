# ai-briefing

A weekly AI news briefing you can check: every item is backed by **two independent sources**, and every source is a
link.

Ask for "my AI briefing" in a session (or run `/ai-briefing:ai-briefing`). It:

1. searches the past week's news on each topic in `topics.md`,
2. keeps only the claims two different publishers back (one-source claims are dropped and counted),
3. drops duplicates and ranks what's left (usable now > announced; more sources > fewer; newer > older),
4. writes `briefings/briefing-<date>.html` in your current folder: one self-contained page, every source linked.

## Install

Inside a session (Claude Code 2.1.275 or later):

```
/plugin install ai-briefing --marketplace chang47/JoshDevBox
```

Or from a shell: `claude plugin marketplace add chang47/JoshDevBox`, then `claude plugin install ai-briefing@joshdevbox`.

## Change it

- **Topics:** edit `skills/ai-briefing/topics.md`, one per line. Defaults: Claude Code releases, AI coding agents,
  frontier model releases. Ask for "a briefing on X" to run one topic.
- **The look:** the report uses `template.html`. If you have a design-style skill, put its name on the `style:` line in
  `topics.md` and the briefing follows it.
- **Without the plugin:** copy `skills/ai-briefing/` into your project's `.claude/skills/` (or `~/.claude/skills/`).

It uses web search and page fetches, so a full run takes a few minutes and Claude Code will ask before it searches
unless you allow `WebSearch` and `WebFetch`.

Provided as-is, no support.
