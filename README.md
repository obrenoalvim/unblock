# Unblock

[🇧🇷 Leia em Português](README.pt.md)

A Claude Code skill that keeps trying when a web lookup fails. One tool gets blocked, rate-limited, CAPTCHA'd, or returns nothing? It moves to the next, in order, through 13 free tools, until one gets through.

---

## What it does

Use it whenever a web search, scrape, or crawl fails, or whenever blocking looks likely up front: paywalls, bot walls, JS-heavy sites, social platforms.

**Order, most general and robust first:**

1. **[agent-reach](https://buildwithclaude.com/skill/agent-reach)**: multi-platform router (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, general web)
2. **[last30days](https://github.com/mvanhorn/last30days-skill)**: pulls recent discussion from Reddit, X, YouTube, TikTok, HN, GitHub
3. **[claude-in-chrome](https://code.claude.com/docs/en/chrome)**: live browser automation for sites that need login, interaction, or real rendering
4. **crawler**: Jina Reader, then Scrapling stealth browser. Turns a URL into clean markdown
5. **web-scraping**: checks the site first, then picks traffic interception, sitemap, API, or DOM scraping
6. **web-scraping-automation**: Playwright/requests plus generic REST and GraphQL calls
7. **playwright-web-scraper**: multi-page structured extraction, rate-limited, respects robots.txt
8. **web-scraper**: multi-strategy extractor for tables, lists, prices, contacts
9. **site-crawler**: straightforward multi-page crawl
10. **scraping-skills**: bundle of academic and ethical scraping techniques
11. **scrapy-web-scraping**: Scrapy framework for large multi-page crawls
12. **news-extractor**: extraction tuned for news and articles across 12 sites
13. **[Scrapling](https://github.com/D4Vinci/Scrapling)** (Python library, last resort): write and run a short stealth-fetch script

Items 4–12 are generic community skill names — several independent implementations share each name across different marketplaces/registries (buildwithclaude.com, skills.rest, individual GitHub forks), so there's no single canonical source to link with confidence. Search your skill marketplace or plugin registry for the name to install one.

Every tool in the list is free. No API key, no paid tier, no signup. A block or an empty result sends it to the next tool; it never retries the same one twice.

**All 13 need to be installed for full coverage.** A tool that isn't installed gets skipped, not counted as a failure — the report always splits "tried" (installed, actually attempted) from "skipped" (not installed), so a machine with 3 of the 13 doesn't read as "13 tools failed."

Some tools need setup beyond installing them. **agent-reach** is zero-config for 6 channels but needs a cookie/token for others (Twitter/X, 小红书) — run `agent-reach doctor --json` to check. Read each tool's own docs before assuming a failure is a real block.

Before burning the chain, one cheap HTTP status + body-size check on a direct URL tells apart "blocked" from "genuinely empty page" — no point running 13 tools against a page that has nothing on it. The chain also remembers which tool won for a given domain (a small local scoreboard) and tries that one first next time, before falling back to the fixed order below.

**Left out on purpose:** anything that needs an API key, a paid tier, or an account (firecrawl-scrape, x402-based tools, proxy-vendor skills). This chain stays free.

---

## When not to use it

Skip the chain for a plain factual question that a normal web search already answers.

---

## Use it

**No install:**
> "Read https://github.com/obrenoalvim/unblock and follow the Unblock skill."

**As a plugin (available in every session):**
```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Then ask for it: "Use the web skill to find X." It also triggers on its own when a search, scrape, or crawl blocks.

**Copy the file:**
Copy [`skills/web/SKILL.md`](skills/web/SKILL.md) into your skills directory and invoke it through your own skill system.

---

## Works with

Any Claude Code session. Each tool in the chain (agent-reach, crawler, Scrapling, and the rest) needs its own install for that step to run. A tool that isn't installed gets skipped, and the chain moves on.

There's no single command that installs all 13 at once — they live in different sources/marketplaces, so `unblock`'s own `plugin.json` can't safely declare them as hard dependencies (a missing marketplace would break install for everyone). Install each one the normal way for your setup, then check coverage with an install-status pass (ask Claude: "which of the 13 unblock tools are installed?").
