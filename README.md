# Agent Web Index — how much of the web can AI assistants actually read?

**46,907 domains measured live. 22% of them cannot be read by at least one of
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
| claudebot | 46,907 | 84% | 2,066 | 6,503 |
| gptbot | 46,907 | 85% | 2,583 | 6,190 |
| oai-searchbot | 46,907 | 90% | 823 | 4,490 |
| perplexitybot | 46,907 | 90% | 1,229 | 4,366 |
| meta-externalagent | 18,535 | 83% | 826 | 2,931 |
| amazonbot | 18,535 | 79% | 897 | 3,652 |
| bytespider | 245 | 74% | 25 | 54 |
| applebot | 245 | 88% | 5 | 30 |

Broken down by whoever answers in front of the site (read from the response headers of the same
request: `cf-ray`, `akamai-grn`, `x-amz-cf-id`, `x-fastly-request-id`…). Unit: one domain ×
one crawler, counted only where that site's robots.txt allows that crawler — so every refusal below
contradicts the site's own stated policy, and is almost always an edge default nobody chose.

| Edge in front of the site | Domains | Requests robots.txt allows | Refused anyway |
|---|---|---|---|
| Google | 619 | 2,857 | 44% |
| Akamai | 229 | 1,021 | 42% |
| AWS CloudFront | 2,239 | 10,022 | 16% |
| DDoS-Guard | 204 | 995 | 11% |
| Cloudflare | 22,417 | 94,552 | 11% |
| no known edge | 8,580 | 38,843 | 11% |
| Azure Front Door | 208 | 925 | 9% |
| Fastly | 1,089 | 4,606 | 8% |
| Varnish | 186 | 839 | 7% |
| Alibaba | 65 | 327 | 7% |

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
- 16,227 measured hosts are infrastructure (CDNs, telemetry, resolvers) rather than
  sites and are counted apart; 22,477 failed to answer and are excluded from every
  percentage.
- Blocking AI crawlers is a legitimate choice, not a failure. This dataset records what is true,
  not what should be.

## Citing

> Agent Web Index, 2026-09-19. 46,907 domains. <https://shop.lumnika.com/ai-readiness/>

Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, say where it came from.

Audit a single site live, for free, no signup: <https://shop.lumnika.com/lab/agentready/index.html>
