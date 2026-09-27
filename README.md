# Agent Web Index — how much of the web can AI assistants actually read?

**49,941 domains measured live. 26% of them cannot be read by at least one of
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
| claudebot | 49,941 | 84% | 2,039 | 7,125 |
| gptbot | 49,941 | 85% | 2,581 | 6,785 |
| oai-searchbot | 49,941 | 90% | 792 | 4,705 |
| perplexitybot | 49,941 | 90% | 1,203 | 4,571 |
| meta-externalagent | 41,199 | 89% | 1,153 | 4,144 |
| amazonbot | 41,199 | 86% | 1,253 | 5,498 |
| bytespider | 24,490 | 86% | 647 | 3,100 |
| applebot | 24,490 | 95% | 100 | 1,079 |

Broken down by whoever answers in front of the site (read from the response headers of the same
request: `cf-ray`, `akamai-grn`, `x-amz-cf-id`, `x-fastly-request-id`…). Unit: one domain ×
one crawler, counted only where that site's robots.txt allows that crawler — so every refusal below
contradicts the site's own stated policy, and is almost always an edge default nobody chose.

| Edge in front of the site | Domains | Requests robots.txt allows | Refused anyway |
|---|---|---|---|
| Akamai | 789 | 4,382 | 38% |
| Google | 805 | 5,094 | 33% |
| Sucuri | 75 | 482 | 21% |
| AWS CloudFront | 3,197 | 17,692 | 16% |
| no known edge | 12,731 | 74,414 | 11% |
| Azure Front Door | 314 | 1,639 | 11% |
| Cloudflare | 28,020 | 195,830 | 10% |
| DDoS-Guard | 309 | 1,786 | 10% |
| Fastly | 1,531 | 8,169 | 9% |
| Varnish | 414 | 2,155 | 8% |

`no known edge` is an upper bound, not a vendor: response headers are not kept, so a domain read before a signature was
added to the table stays in that bucket until it is re-requested (a 250-domain sample on 19 Sep 2026: 14% already carry a
signature the current table recognises). A weekly pass re-reads them, so the named vendors above are undercounts.

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
- 15,998 measured hosts are infrastructure (CDNs, telemetry, resolvers) rather than
  sites and are counted apart; 22,702 failed to answer and are excluded from every
  percentage.
- Blocking AI crawlers is a legitimate choice, not a failure. This dataset records what is true,
  not what should be.

## Citing

> Agent Web Index, 2026-09-27. 49,941 domains. <https://shop.lumnika.com/ai-readiness/>

Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, say where it came from.

Audit a single site live, for free, no signup: <https://aivis.lumnika.com/en?src=agentindex#scan>
