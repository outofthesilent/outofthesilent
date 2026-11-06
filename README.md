## Timothy G. Smith

**Lead AI Automation Engineer.** Twenty-five years building and leading systems
and the organizations that run on them. I build the operational layer
underneath businesses — scheduling, billing, collections, communications,
reporting — and the AI that now works parts of it alongside a person.

I start from what has to be true at the end. Name the outcome, decide what
would count as evidence you got it, and only then decide what to build. The
usual order is to build the thing and then work out how to tell whether it's
right, which is why systems pass their tests and still fail in production: the
tests were written to fit what got built, so they can only confirm it.

So the hard part of a system is a sentence, written before any code:

- *A family must never be charged twice.* → idempotency on every charge path,
  reconciled against the processor rather than against my own records.
- *A spend limit must not be able to fail open.* → four invariants, written as
  tests instead of intentions. That's `llm-breaker`.
- *An agent may never claim it did something a tool did not do.* → *noted*,
  *recorded*, *all set* are legal only after an action actually ran in that
  conversation.

Sole engineer on the platform three brands run their day-to-day operations on —
six locations, four states, a metro that straddles a state line, two delivery
modes with no address at all. One codebase, no per-brand forks: every feature
ships once and has to be right in every combination. That is what 800 tables,
740 row-level security policies and about 200 edge functions actually are — not
effort, but what the matrix costs.

### On low-code, since it always comes up

I built that whole operational layer in self-hosted n8n first — **71 workflows**
carrying every inbound channel, transcription, four classifiers, billing and a
full performance season — then moved it into Postgres and edge functions over
**90 days**. Nothing runs on n8n now.

Measured, not stylistic: one sync made **51 API calls every five minutes**,
rewriting a thousand unchanged rows a run. The native replacement makes **two**
when nothing has changed. I kept the webhook-triggered senders, because a
workflow that only fires when called was still the right choice.

The expensive lesson: **a caller living outside the repo cannot be found by
searching the repo.** Two security fixes listed their callers from the codebase
and missed workflows that weren't in it.

### Public code

**[llm-breaker](https://github.com/outofthesilent/llm-breaker)** — circuit
breaker, cost meter and fallback chain for LLM calls. Zero dependencies, MIT.
An agent of mine ran 15,435 times in a day; my spend meter read $28 while the
provider billed $313, because twelve functions called the API directly and
wrote no usage row. If a call can happen without being counted, the counter is
decoration. `example.ts` replays the incident offline: 15,435 attempts, 60
reach the provider, one alert, $0.38.

**[queue-order](https://github.com/outofthesilent/queue-order)** — the ordering
engine behind a queue a person reads top-down. Age banding from *their* last
contact, twenty state resolutions, a frozen order that holds a row still while
someone works it, and a jittered poll so every open tab doesn't re-rank in
lockstep. Pure functions, 30 tests, MIT.

**[deno-web-push](https://github.com/outofthesilent/deno-web-push)** — Web Push
(RFC 8291/8292) for Deno. No Node crypto shims, no dependencies.

### The closed code, opened up

The source runs a live business, so it stays closed. The screens don't.

**[thefulllifemedia.com/work](https://thefulllifemedia.com/work)** — **nine
interactive replicas of production screens.** Working pages you can click, not
screenshots, each with its documented edge cases wired in and a switch between
the build as it shipped and a corrected one, so you can see which behaviours
are defects, which are deliberate, and which ones I decided not to guess at.

Behind them: multi-tenancy retrofitted into a live single-tenant system with no
downtime, a payment engine that has moved over $900K across 5,000 transactions
built so a retry cannot take the money twice, and a voice agent that earned
three hard rules by being confidently wrong in a replay rather than on a live
call.

tim@thefulllifemedia.com
