---
name: web
description: Use when a web search, scrape, or crawl fails, gets blocked, rate-limited, or returns empty/garbage, and when starting any online lookup where blocking is likely (paywalls, bot walls, JS-heavy sites, social platforms). Not for simple factual lookups WebSearch already handles fine.
---

# web

## Overview

Fallback chain for online scraping and research. Every tool below is free: no API key, no paid tier, no signup. A tool blocks or fails, try the next, in order, until one works or all installed ones are exhausted. Never stop after one tool.

**Full 13-tool coverage requires all 13 installed.** Effective chain length = number of tools actually installed, not 13 — a tool that isn't installed gets skipped, it isn't "tried and failed." Report tried vs skipped separately (see Rule) so a 3-tool machine doesn't read as "13 tools failed."

Some tools need extra setup beyond installing them. **agent-reach** works zero-config for 6 channels but needs a cookie/token (`agent-reach configure twitter-cookies`, etc.) for others like Twitter/X and 小红书 — run `agent-reach doctor --json` to see what's configured. Check each tool's own SKILL.md for similar setup steps before assuming a failure is a real block.

## Step 0 — cheap discriminator (direct URL only)

Before invoking any tool, if the input is already a direct URL: do one cheap fetch (`curl -s -o /dev/null -w "%{http_code} %{size_download}" <url>` or WebFetch) to read HTTP status and body size.

- Status 403/429, or a CAPTCHA/"access denied" marker in the small body → treat as blocked, go to Step 1.
- Status 200 and body is empty or near-empty (no redirect, no JS-shell markers you can't tell apart from real emptiness) → the page most likely has no content, not a block. Report that and stop — don't burn the full chain on a wall that isn't there. Only continue into the chain if the user says the content should exist (e.g., known JS-rendered page).
- Anything else → go to Step 1; the chain still applies if the fetched content turns out insufficient or irrelevant.

Skip this step for keyword/topic searches with no single URL to probe.

## Step 1 — domain scoreboard

State file: `~/.claude/unblock-scoreboard.json`, shape `{"<domain>": {"<tool>": <win count>}}`. Missing or unreadable file = empty scoreboard, don't error.

If the request targets a URL, read the file, look up that domain. If it has scored wins, try the highest-scoring tool for that domain **first**, before Step 2's order. Count it as tried; don't try it again when its normal slot comes up in Step 2.

No entry, no domain, or a keyword search with no target site → skip straight to Step 2.

## Step 2 — order

1. **agent-reach**: multi-platform router (小红书, Twitter/X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, general web). Try first for anything platform-specific or general research.
2. **last30days**: recent-discussion pull (Reddit, X, YouTube, TikTok, HN, Polymarket, GitHub, web, keyless core). Good when agent-reach comes back thin or the user wants recent sentiment.
3. **claude-in-chrome**: live browser automation (real Chrome session, clicks, fills, screenshots). Use when a site needs login, interaction, or only renders in a real browser (bot walls that block headless).
4. **crawler**: 3-tier fallback fetcher (Jina Reader free tier, then Scrapling stealth browser; Firecrawl tier skips automatically with no key set). URL in, clean markdown out. Good for paywalled or bot-walled single pages (Medium, WeChat, docs sites).
5. **web-scraping**: recon-first strategy skill. Detects the best approach (traffic interception, sitemap, API, DOM scraping, hybrid) before writing a scraper.
6. **web-scraping-automation**: Playwright/requests-based scraping plus generic REST/GraphQL API calling.
7. **playwright-web-scraper**: multi-page structured extraction via Playwright, with rate limiting, robots.txt compliance, pagination, CSV/JSON/Markdown export built in.
8. **web-scraper**: multi-strategy extractor (WebFetch, browser automation, curl/Bash, WebSearch) for tables, lists, prices, contacts, with a diff/monitoring mode.
9. **site-crawler**: straightforward multi-page site crawl and content extraction.
10. **scraping-skills**: bundle of 6 academic/ethical scraping techniques (rate-limited, dataset-focused).
11. **scrapy-web-scraping**: Scrapy framework guidance. Use for large or multi-page crawls that need a real spider, not a single fetch.
12. **news-extractor**: dedicated news/article extraction across 12 sites (WeChat official accounts, BBC, CNN, Twitter/X, Quora, etc).
13. **Scrapling**: standalone Python lib (already installed, CLI `scrapling`, stealth fetchers). Last resort: write and run a short script when nothing above works, e.g. `python -c "from scrapling.fetchers import Fetcher; print(Fetcher.get(url).status)"` or via Bash.

**best-price is excluded**: separate purpose (price/coupon compare), not a general fetch fallback.

**Excluded (paid, API-key, or account required; never install for this chain):** firecrawl-scrape, just-scrape (`SGAI_API_KEY`), social-scraping (x402, $0.06/call), gzh/xiaohongshu/douyin-crawler (`REDFOX_API_KEY`), starchild web-crawler (tied to a vendor's paid proxy), brightdata/oxylabs/apify/scrapfly/scraperapi-based skills (all paid proxy/API vendors), printing-press-library (meta-installer, off-topic), archive-crawler (local file archivist, not web).

## Rule

Blocked, empty result, CAPTCHA, 403/429, or garbage output: move to the next **installed** tool in the list above. Don't retry the same tool twice. Not installed: skip it, don't count it as a failure.

On success: append/increment `scoreboard[domain][tool]` in the state file from Step 1 (create the file/dirs if missing).

Report, always split into tried vs skipped:
- Success: "Worked: `<tool>`. Tried `<M>` installed tool(s) (`<K>` of 13 not installed, skipped: `<names>`)."
- Total failure: "All installed tools failed. Tried: `<names>`. Not installed, skipped (`<K>`/13): `<names>`."

## When not to use

A simple factual question that WebSearch answers directly doesn't need 13 tools chained behind it.
