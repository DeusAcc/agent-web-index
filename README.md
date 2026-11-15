# Agent Web Index — how much of the web can AI assistants actually read?

**51,697 domains measured live. 26% of them cannot be read by at least one of
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
| claudebot | 51,697 | 84% | 2,229 | 7,311 |
| gptbot | 51,697 | 84% | 2,796 | 6,969 |
| oai-searchbot | 51,697 | 90% | 802 | 4,835 |
| perplexitybot | 51,697 | 90% | 1,218 | 4,823 |
| meta-externalagent | 42,956 | 89% | 1,345 | 4,353 |
| amazonbot | 42,956 | 85% | 1,445 | 5,727 |
| bytespider | 26,247 | 85% | 860 | 3,452 |
| applebot | 26,247 | 95% | 107 | 1,235 |

Broken down by whoever answers in front of the site (read from the response headers of the same
request: `cf-ray`, `akamai-grn`, `x-amz-cf-id`, `x-fastly-request-id`…). Unit: one domain ×
one crawler, counted only where that site's robots.txt allows that crawler — so every refusal below
contradicts the site's own stated policy, and is almost always an edge default nobody chose.

| Edge in front of the site | Domains | Requests robots.txt allows | Refused anyway |
|---|---|---|---|
| Akamai | 803 | 4,488 | 39% |
| Google | 814 | 5,168 | 32% |
| Sucuri | 85 | 562 | 26% |
| AWS CloudFront | 3,241 | 18,049 | 16% |
| no known edge | 13,226 | 78,182 | 12% |
| Azure Front Door | 315 | 1,647 | 11% |
| DDoS-Guard | 317 | 1,848 | 11% |
| Cloudflare | 29,155 | 203,990 | 10% |
| Fastly | 1,560 | 8,368 | 9% |
| Qrator | 186 | 1,086 | 9% |

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
- 16,002 measured hosts are infrastructure (CDNs, telemetry, resolvers) rather than
  sites and are counted apart; 22,822 failed to answer and are excluded from every
  percentage.
- Blocking AI crawlers is a legitimate choice, not a failure. This dataset records what is true,
  not what should be.

## Citing

> Agent Web Index, 2026-11-15. 51,697 domains. <https://shop.lumnika.com/ai-readiness/>

Licence: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, say where it came from.

Audit a single site live, for free, no signup: <https://aivis.lumnika.com/en?src=agentindex#scan>
