<div align="center">

<img src=".github/logo.svg" alt="Unblock logo" width="120" height="120">

# Unblock

**A Claude Code skill that keeps trying when a web lookup fails.**<br>
One tool gets blocked, rate-limited or returns nothing? It moves to the next of 13 free tools until one gets through.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/obrenoalvim/unblock?style=flat&logo=github&color=8b9cff)](https://github.com/obrenoalvim/unblock/stargazers)
[![Claude Code plugin](https://img.shields.io/badge/Claude_Code-plugin-5B5BD6)](#quick-start)
[![13 free tools](https://img.shields.io/badge/tools-13_free-8b9cff)](#the-chain)

**English** · [Português](README.pt.md) · [Español](README.es.md)

[What it does](#what-it-does) · [Quick start](#quick-start) · [How it works](#how-it-works) · [The chain](#the-chain) · [Install](#install) · [FAQ](#faq)

</div>

---

Unblock is a Claude Code skill for web research that hits a wall. When a web search, scrape, or crawl is blocked, rate-limited, CAPTCHA'd, or returns nothing, it moves to the next tool in a fixed order, through 13 free tools, until one gets through.

## What it does

Use it whenever a web search, scrape, or crawl fails, or up front when blocking looks likely: paywalls, bot walls, JS-heavy sites, social platforms.

Every tool in the chain is free. No API key, no paid tier, no signup.

## Quick start

```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Then ask for it: "Use the web skill to find X." It also triggers on its own when a search, scrape, or crawl blocks.

---

## How it works

1. **A cheap check first.** For a direct URL, one HTTP status and body-size request tells a block (403, 429, CAPTCHA) from a page that is genuinely empty. An empty 200 stops the chain, so it never runs 13 tools against nothing.
2. **A domain scoreboard.** The skill remembers which tool worked for each domain in `~/.claude/unblock-scoreboard.json` and tries that tool first next time.
3. **A fixed order.** The chain runs from the most general and robust tool to the last resort. A block or empty result sends it to the next tool, and it never retries the same tool twice.
4. **An honest report.** It separates tools tried (installed and run) from tools skipped (not installed), so a machine with 3 of the 13 doesn't read as "13 tools failed."

## The chain

Most general and robust first:

| # | Tool | Use it for |
|---|---|---|
| 1 | **[agent-reach](https://buildwithclaude.com/skill/agent-reach)** | Multi-platform router (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, general web) |
| 2 | **[last30days](https://github.com/mvanhorn/last30days-skill)** | Recent discussion from Reddit, X, YouTube, TikTok, HN, GitHub |
| 3 | **[claude-in-chrome](https://code.claude.com/docs/en/chrome)** | Live browser automation for sites that need login, interaction, or real rendering |
| 4 | **crawler** | Jina Reader, then Scrapling stealth browser. Turns a URL into clean markdown |
| 5 | **web-scraping** | Checks the site first, then picks traffic interception, sitemap, API, or DOM scraping |
| 6 | **web-scraping-automation** | Playwright/requests plus generic REST and GraphQL calls |
| 7 | **playwright-web-scraper** | Multi-page structured extraction, rate-limited, respects robots.txt |
| 8 | **web-scraper** | Multi-strategy extractor for tables, lists, prices, contacts |
| 9 | **site-crawler** | Straightforward multi-page crawl |
| 10 | **scraping-skills** | Bundle of academic and ethical scraping techniques |
| 11 | **scrapy-web-scraping** | Scrapy framework for large multi-page crawls |
| 12 | **news-extractor** | Extraction tuned for news and articles across 12 sites |
| 13 | **[Scrapling](https://github.com/D4Vinci/Scrapling)** | Python library, last resort: write and run a short stealth-fetch script |

Items 4 to 12 are generic community skill names. Several independent implementations share each name across different marketplaces and registries (buildwithclaude.com, skills.rest, individual GitHub forks), so there is no single canonical source to link with confidence. Search your skill marketplace or plugin registry for the name to install one.

**All 13 need to be installed for full coverage.** A tool that isn't installed gets skipped, not counted as a failure.

Some tools need setup beyond installing them. **agent-reach** works with no configuration for 6 channels but needs a cookie or token for others (Twitter/X, 小红书). Run `agent-reach doctor --json` to check. Read each tool's own docs before assuming a failure is a real block.

**Left out on purpose:** anything that needs an API key, a paid tier, or an account (firecrawl-scrape, x402-based tools, proxy-vendor skills). This chain stays free.

### Example report

```
Worked: `crawler`. Tried 4 installed tool(s) (9 of 13 not installed, skipped: `web-scraping`, `web-scraping-automation`, `playwright-web-scraper`, `web-scraper`, `site-crawler`, `scraping-skills`, `scrapy-web-scraping`, `news-extractor`, `Scrapling`).
```

## When not to use it

Skip the chain for a plain factual question that a normal web search already answers.

---

## Install

**As a plugin (available in every session):**
```
/plugin marketplace add obrenoalvim/unblock
/plugin install unblock@unblock
```

Then ask for it: "Use the web skill to find X."

**No install:**
> "Read https://github.com/obrenoalvim/unblock and follow the Unblock skill."

**Copy the file:**
Copy [`skills/web/SKILL.md`](skills/web/SKILL.md) into your skills directory and invoke it through your own skill system.

## Works with

Any Claude Code session. Each tool in the chain (agent-reach, crawler, Scrapling, and the rest) needs its own install for that step to run. A tool that isn't installed gets skipped, and the chain moves on.

No single command installs all 13 at once. They live in different sources and marketplaces, so `unblock`'s own `plugin.json` can't safely declare them as hard dependencies (a missing marketplace would break the install for everyone). Install each one the normal way for your setup, then check coverage by asking Claude: "which of the 13 unblock tools are installed?"

---

## FAQ

**Do I need API keys or a paid plan?**
No. Every tool in the chain is free, and anything that needs a key, a paid tier or an account is left out on purpose.

**Do I need all 13 tools?**
No. Missing tools are skipped and listed as skipped in the report. You only get full coverage with all 13.

**Will it hammer a site by retrying?**
No. It never retries the same tool twice, and `playwright-web-scraper` is rate-limited and respects robots.txt.

## More Claude Code skills by the same author

- [**zero-drift**](https://github.com/obrenoalvim/zero-drift): keeps long sessions grounded with named replies and a living `TASK.md`.
- [**keep-improving**](https://github.com/obrenoalvim/keep-improving): an autonomous improvement loop with a ten-role review panel.
- [**findable**](https://github.com/obrenoalvim/findable): SEO and GEO research that applies the safe fixes.
- [**no-watermark**](https://github.com/obrenoalvim/no-watermark): detects and removes invisible Unicode watermarks from text.

## Contributing

Know a free tool that belongs in the chain, or a better order? Open an issue or a PR. See [CONTRIBUTING.md](CONTRIBUTING.md) and the [changelog](CHANGELOG.md).

## License

[MIT](LICENSE)

---

<div align="center">

If Unblock got you through a wall, a ⭐ helps other people find it too.

<sub>**Topics:** claude-code · claude-skill · claude-code-plugin · web-scraping · web-scraper · web-crawler · web-research · data-extraction · playwright · scrapling · mcp</sub>

</div>
