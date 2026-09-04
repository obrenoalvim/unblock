# Unblock

[🇧🇷 Leia em Português](README.pt.md)

A Claude Code skill that keeps trying when a web lookup fails. One tool gets blocked, rate-limited, CAPTCHA'd, or returns nothing? It moves to the next, in order, through 13 free tools, until one gets through.

---

## What it does

Use it whenever a web search, scrape, or crawl fails, or whenever blocking looks likely up front: paywalls, bot walls, JS-heavy sites, social platforms.

**Order, most general and robust first:**

1. **agent-reach**: multi-platform router (小红书, X, B站, Reddit, GitHub, YouTube, LinkedIn, RSS, general web)
2. **last30days**: pulls recent discussion from Reddit, X, YouTube, TikTok, HN, GitHub
3. **claude-in-chrome**: live browser automation for sites that need login, interaction, or real rendering
4. **crawler**: Jina Reader, then Scrapling stealth browser. Turns a URL into clean markdown
5. **web-scraping**: checks the site first, then picks traffic interception, sitemap, API, or DOM scraping
6. **web-scraping-automation**: Playwright/requests plus generic REST and GraphQL calls
7. **playwright-web-scraper**: multi-page structured extraction, rate-limited, respects robots.txt
8. **web-scraper**: multi-strategy extractor for tables, lists, prices, contacts
9. **site-crawler**: straightforward multi-page crawl
10. **scraping-skills**: bundle of academic and ethical scraping techniques
11. **scrapy-web-scraping**: Scrapy framework for large multi-page crawls
12. **news-extractor**: extraction tuned for news and articles across 12 sites
13. **Scrapling** (Python library, last resort): write and run a short stealth-fetch script

Every tool in the list is free. No API key, no paid tier, no signup. A block or an empty result sends it to the next tool; it never retries the same one twice. When the chain finishes, it reports which tool worked, or that all 13 failed.

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
Copy `skills/web/SKILL.md` into your skills directory and invoke it through your own skill system.

---

## Works with

Any Claude Code session. Each tool in the chain (agent-reach, crawler, Scrapling, and the rest) needs its own install for that step to run. A tool that isn't installed gets skipped, and the chain moves on.
