---
name: web
description: Use when a web search, scrape, or crawl fails, gets blocked, rate-limited, or returns empty/garbage, and when starting any online lookup where blocking is likely (paywalls, bot walls, JS-heavy sites, social platforms). Not for simple factual lookups WebSearch already handles fine.
---

# web

## Overview

Fallback chain for online scraping and research. Every tool below is free: no API key, no paid tier, no signup. A tool blocks or fails, try the next, in order, until one works or all are exhausted. Never stop after one tool.

## Order

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

Blocked, empty result, CAPTCHA, 403/429, or garbage output: move to the next tool in the list above. Don't retry the same tool twice. Don't stop until all 13 are tried or one succeeds. Report which tool worked, or that all 13 failed, so the user knows.

## When not to use

A simple factual question that WebSearch answers directly doesn't need 13 tools chained behind it.
