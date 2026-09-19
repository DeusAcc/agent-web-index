# Agent Web Index — how much of the web can AI assistants actually read?

**47,019 domains measured live. 22% of them cannot be read by at least one of
ChatGPT, Claude, Perplexity or Gemini.** Updated daily. Live index: <https://shop.lumnika.com/ai-readiness/>

Every row here is the result of real HTTP requests, not an estimate and not a re-publication of
someone else's crawl: each domain's homepage is requested once as a browser and once as each of the
published AI crawler user-agents, its `robots.txt`, `llms.txt` and sitemap are read, and the
answers are compared.

## The number that exists nowhere else

`robots.txt` says the crawler may come in — and the server refuses it anyway. You can only see
this by making the request as that crawler, which is why no robots.txt study reports it.

| Crawler | Domains measured | Served | robots.txt says no | Server says no anyway |
|---|---|---|---|---|
| claudebot | 47,019 | 84% | 2,067 | 6,526 |
| gptbot | 47,019 | 85% | 2,585 | 6,207 |
| oai-searchbot | 47,019 | 90% | 823 | 4,494 |
| perplexitybot | 47,019 | 90% | 1,230 | 4,374 |
| meta-externalagent | 18,647 | 83% | 826 | 2,941 |
| amazonbot | 18,647 | 79% | 900 | 3,669 |
| bytespider | 357 | 74% | 28 | 81 |
| applebot | 357 | 89% | 5 | 40 |

Broken down by whoever answers in front of the site (read from the response headers of the same
request: `cf-ray`, `akamai-grn`, `x-amz-cf-id`, `x-fastly-request-id`…). Unit: one domain ×
one crawler, counted only where that site's robots.txt allows that crawler — so every refusal below
contradicts the site's own stated policy, and is almost always an edge default nobody chose.

| Edge in front of the site | Domains | Requests robots.txt allows | Refused anyway |
|---|---|---|---|
| Akamai | 265 | 1,233 | 43% |
| Google | 716 | 3,410 | 39% |
| Sucuri | 52 | 267 | 19% |
| AWS CloudFront | 2,896 | 13,707 | 16% |
| Cloudflare | 25,219 | 111,034 | 13% |
| no known edge | 11,265 | 54,260 | 11% |
| DDoS-Guard | 264 | 1,340 | 10% |
| Azure Front Door | 283 | 1,343 | 9% |
| Fastly | 1,335 | 5,883 | 8% |
| Varnish | 253 | 1,192 | 7% |

## Files

| File | What it is |
|---|---|
| `agent-web-index.csv` | one row per domain: score, grade, per-check breakdown, per-crawler verdict, edge vendor, date |
| `aggregate.json` | today's aggregate, exactly as the live index publishes it |
| `daily/<date>.json` | the immutable daily snapshot of the aggregate — this directory is the series |

## Method, and what it does not cover

- One vantage point (Europe), one page per domain (the homepage), 12-second timeout per request.
- Rank-bearing domains come from the [Tranco](https://tranco-list.eu) research list (30-day average
  of five rankings); the rest come from public e-commerce platform seeds.
- `Google-Extended` and `Applebot-Extended` never make requests — they are robots.txt opt-out
  tokens — so for those only robots.txt is reported and no "served" verdict exists.
- Domains that answer with HTTP 429 are excluded from that pass rather than counted as blocking.
- 16,230 measured hosts are infrastructure (CDNs, telemetry, resolvers) rather than
  sites and are counted apart; 22,482 failed to answer and are excluded from every
  percentage.
- Blocking AI crawlers is a legitimate choice, not a failure. This dataset records what is true,
  not what should be.

## Citing

> Agent Web Index, 2026-09-19. 47,019 domains. <https://shop.lumnika.com/ai-readiness/>

Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, say where it came from.

Audit a single site live, for free, no signup: <https://shop.lumnika.com/lab/agentready/index.html>
