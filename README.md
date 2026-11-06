## Timothy G. Smith

I build the operational layer underneath businesses — the scheduling, billing,
communications and reporting that a company's tools don't do for each other.

I start from what has to be true at the end. Name the outcome, decide what
would count as evidence that you got it, and only then decide what to build.
The usual order is to build the thing and then work out how to tell whether
it's right — which is why systems pass their tests and still fail in
production. The tests were written to fit what got built, so they can only
confirm it.

In practice that means the hard part of a system is a sentence, written before
any code:

- *A family must never be charged twice.* → idempotency keys on every charge
  path, and reconciliation against the processor rather than against my own
  records.
- *A spend limit must not be able to fail open.* → four invariants, written as
  tests instead of intentions. That's `llm-breaker`, below.
- *Nobody loses a lesson during the cutover.* → a synthetic twin of the live
  data, so the migration was the last step rather than the test.
- *Any lane of work can be picked up cold.* → 94 protocol documents, because
  "what good output looks like" has to be written down before the work starts,
  not after.

I'm the sole engineer on the platform three brands run their day-to-day
operations on — six locations, four states, a metro that straddles a state
line, and two delivery modes with no address at all. One codebase, no
per-brand forks: every feature ships once and has to be right in every
combination. That is what 794 tables, 740 row-level security policies and
187 edge functions actually are — not effort, but what the matrix costs.

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

Most of what I've built runs a live business, so the code stays closed. The
write-ups are not: the architecture, the trade-offs and the production
incidents are at
**[thefulllifemedia.com/work](https://thefulllifemedia.com/work)** — including
what it took to retrofit multi-tenancy into a live single-tenant system with no
downtime, and a payment engine that has moved $817K across 5,019 transactions,
built so that a retry cannot take the money twice.

### Contact

tim@thefulllifemedia.com · [thefulllifemedia.com](https://thefulllifemedia.com)
