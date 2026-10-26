---
name: ai-briefing
description: Build a weekly AI news briefing. Searches the past week's news on every topic in topics.md, keeps only claims that two independent sources back, drops duplicates, ranks what's left and writes a dated HTML report that links every source. Use when the user asks for "my briefing", "the briefing", "the AI briefing", "this week's AI news", "what happened in AI this week", or a briefing on one named topic.
---

# AI briefing

A weekly news briefing you can trust a little more than a feed: every item on it is backed by at least two independent
sources, and every source is one click away. The steps are fixed; the topics and the look are yours to change.

Files in this folder:
- `topics.md`: the topics to cover, the time window, how many items to keep, and an optional style skill. Read it first.
- `template.html`: the report's layout and default look. Fill it in; don't redesign it unless a style skill says so.

## 0. Scope
- Read `topics.md`. If the user named one topic ("a briefing on X"), cover only that topic.
- The window is the `window:` line in `topics.md` (default: the past 7 days, ending today). Write today's date and the
  window's start date down; every claim you keep must be dated inside the window.
- If `topics.md` is missing, use the three default topics below and tell the user you did.
  - Claude Code releases
  - AI coding agents
  - Frontier model releases

## 1. Search
- For each topic, run several web searches (at least three, with different wording: the topic, the topic + "release",
  the topic + "announced", named products in the topic). Prefer the past week's results.
- Open the promising pages. A search snippet is not a source: only a page you opened and read counts.
- Prefer primary sources (the company's own post, release notes, changelog, docs, paper) and reputable reporting.
  Skip content farms, pages that only repeat another page, and posts with no date.

## 2. Extract claims
- From each page you opened, write down each concrete claim: what happened, who did it, when, and any number.
- Keep the page's URL, title, publisher and date next to each claim.
- A claim is one fact a reader could check ("X released Y on <date>"), not an opinion or a prediction.

## 3. Keep only claims two sources back
- A claim stays only if at least **two independent sources** state it. Independent = different publishers; a page that
  quotes or links the other as its only basis doesn't count as a second source.
- A company's own post counts as one source. A second publisher reporting the same fact makes two.
- Numbers must match across the sources. If they differ, keep the claim without the number, or drop it.
- Drop every claim with one source. Count them: the report says how many were dropped.

## 4. Drop duplicates
- Merge claims that describe the same event (the same release in two wordings). Keep every source of the merged claims.
- If one event touches two topics, list it once, under the topic it fits best.

## 5. Rank
- Within each topic, rank the kept claims:
  1. Things a reader can use or try now (a release, a new feature, a price change) above announcements and plans.
  2. More sources, and primary sources, above fewer.
  3. Newer above older.
- Keep at most the `max-items:` number per topic from `topics.md` (default 5). A topic with no kept claim says so in
  one line; never pad it.

## 6. Write the report
- Write `briefings/briefing-<YYYY-MM-DD>.html` in the current folder (create `briefings/`; today's date). If the file
  exists, overwrite it.
- **The look:** if `topics.md` names a style skill on its `style:` line and that skill is available, load it and follow
  it for colors, type and decoration, keeping `template.html`'s structure. Otherwise use `template.html` as it is.
- Fill `template.html`: the title, the window, one section per topic, one item per kept claim. Each item has a short
  headline, one or two plain sentences, its date, and **every source as a link** (publisher name as the link text,
  opening in a new tab).
- The footer says how many claims were found, how many were kept, and how many were dropped for having one source.
- Write plain HTML and CSS in one file: no scripts, no external fonts or images, nothing that needs a network to open.

## 7. Check and report back
- Re-open the file you wrote and check: every item has at least two source links, every link is a full `https://` URL
  you opened, every date is inside the window, and no topic is padded.
- Fix anything that fails, then tell the user, in a few lines: the report's path, items kept per topic, claims dropped
  for having one source, and anything you couldn't verify.
