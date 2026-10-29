## Timothy G. Smith

I build the operational layer underneath businesses — the scheduling, billing,
communications and reporting that a company's tools don't do for each other.

I spent 25 years in education before this: classroom teaching, then curriculum
design, then district-level instructional technology for a district of 3,000+
employees. I came into software sideways, by running sales and CRM on software
I came to understand the limits of, and then building its replacement.

I'm now the sole engineer on a multi-tenant platform that four independent
organisations run their day-to-day operations on, across three separately
branded domains — 793 tables, 740 row-level security policies, 187 edge
functions, and 8,204 commits in eleven months.

### Public code

**[llm-breaker](https://github.com/outofthesilent/llm-breaker)** — a circuit
breaker, cost meter and fallback chain for LLM calls. Zero dependencies,
pluggable storage, MIT.

An agent of mine ran 15,435 times in a day. What made it worth extracting
wasn't the ~$370 — it was that my own spend meter read $28 while the provider
billed $313. Twelve functions were calling the API directly and writing no
usage row, so the alarm that reads that meter stayed quiet. If a call can
happen without being counted, the counter is decoration.

The breaker refuses rather than throttles, and a refusal deliberately does
*not* fall back to a second provider — a fallback would only move the loop to
another bill. `example.ts` replays the incident offline with no API keys:
15,435 attempts, 60 reach the provider, one alert, $0.38.

**[deno-web-push](https://github.com/outofthesilent/deno-web-push)** — Web Push
(RFC 8291/8292) for Deno. No Node crypto shims, no dependencies. Written
because the existing options assumed Node's crypto and I needed it on an edge
runtime.

### What isn't public

Most of what I've built runs a live business, so the code stays closed. The write-ups are not: the architecture, the trade-offs and the
production incidents are at
**[thefulllifemedia.com/work](https://thefulllifemedia.com/work)** — including
what it took to retrofit multi-tenancy into a live single-tenant system with no
downtime, and a payment engine that has moved $817K across 5,019
transactions, built so that a retry cannot take the money twice.

### Contact

tim@thefulllifemedia.com · [thefulllifemedia.com](https://thefulllifemedia.com)
