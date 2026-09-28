# Still Open

[Live app](https://still-open.bjrn-norman.chatgpt.site/) · [Interactive rehearsal](https://still-open.bjrn-norman.chatgpt.site/demo/)

## Complete source

The complete 31-file source tree, including schemas, tests, lockfile and setup instructions, is in [still-open-source.zip](./still-open-source.zip). Download and unzip it, then open the `still-open` directory. The archive contains no credentials or dependency/build folders.

**Evidence before effort.** A Next.js + Sanity opportunity desk built with an AI coding agent. It makes stale listings, missing currencies, credit-only rewards, and unconfirmed payout routes visible before delivery work begins.

All six example opportunities and all source excerpts are fictional. All example source links use `example.com`.

## Run locally

Requires Node.js 22.12+ (tested with 24.19) and pnpm 11.

```sh
pnpm install --frozen-lockfile
cp .env.example .env.local
pnpm dev
```

Open http://localhost:3333/demo for a complete local rehearsal. No account, API key, or network model call is required for this route.

To connect your own Sanity project, set `NEXT_PUBLIC_SANITY_PROJECT_ID` and `NEXT_PUBLIC_SANITY_DATASET` in `.env.local`. The normal `/` route reads the actual public dataset. A failed configured connection displays an error; it never silently replaces the source with fixtures.

The embedded Studio is at http://localhost:3333/studio. Sign in to your Sanity project. The project must allow that exact localhost origin with credentials. The owner account used during development already had this default origin configured. No write token belongs in this app or in a `NEXT_PUBLIC_` variable.

## Try the workflow

1. Open the missing-currency case. Propose a next step: ask for clarification.
2. Simulate a buyer clarification. The budget becomes 10,000 SEK; the original proposal cannot be approved because the source revision changed.
3. Reassess. Review the new proposed step: prepare a proposal. Recording it sends no message and creates no agreement.
4. Use **Fast-forward 14 days**. Previously fresh evidence becomes stale without any database mutation.
5. Open the closed-listing case. A newer secondary listing does not override the buyer's direct closure.

Public board interactions last only in the current tab. The authenticated **Review desk** writes shared records to Sanity. Its seed action creates the six fictional examples only if their IDs do not already exist; it never replaces existing records.

## Content model

- `opportunity`: identity, scope summary, synthetic marker, source observations and Sanity `_rev`.
- `evidence`: structured observation objects, each with a claim, value, source authority, URL, excerpt, publication date and capture date. They are embedded to keep the facts and their revision atomic.
- `reviewProposal`: a reference to the opportunity, exact source revision, action, reason codes, reason snapshots, budget snapshot, proposal/expiry timestamps and review status.

The pure evaluator covers eight facts: availability, numeric budget, currency, reward type, buyer identity, payment route, scope and AI acceptance. Direct sources outrank third-party listings; equal-authority simultaneous contradictions remain unresolved. Publication time controls freshness. A recrawl does not refresh an old promise.

Approval checks the opportunity and proposal revisions in one Sanity transaction. A write racing with a source edit or another review is rejected by the database. The app does not retry blindly. This protects the custom workflow, not against a project administrator intentionally bypassing it.

## Validation

```sh
pnpm test
pnpm typecheck
pnpm build
```

25 domain and transaction-contract tests passed during development. They include stale-source rejection, timezone-equivalent conflicts, changed-revision approval, one-time review, proposal expiry, retained reason snapshots, a surfaced transaction conflict, and unsafe source-link rejection.

Browser checks also verified that filters and empty searches never leave an unrelated detail panel visible, reset restores the full initial view, and the detail tabs support arrow/Home/End navigation.

Browser checks covered desktop and 390px mobile layouts, search, proposal creation, simulated clarification, stale-proposal blocking, reassessment, review recording, time travel, and keyboard dismissal/focus restoration of help.

Live integration verified on 28 September 2026: six synthetic records were created through authenticated Studio; a proposed review was saved, a synthetic currency clarification changed its source revision, the stale approval was blocked, and a fresh review was saved. A separate public read confirmed all six records, the old pending proposal and the approved proposal with matching review timestamps. This verifies real persistence, not live concurrency or network-race stress testing.

## Static hosting option

The app can export a read-only public frontend without buying a server:

```sh
STILL_OPEN_STATIC=1 NEXT_PUBLIC_BASE_PATH=/still-open pnpm build
```

The `out/` directory contains static assets for hosting under `/still-open`. Set `NEXT_PUBLIC_BASE_PATH` to empty when hosting at the domain root. Public data is fetched from Sanity in the browser. If the origin is not already allowed, configure a read-only CORS origin without credentials. The static export deliberately does not host the authenticated Studio; its editor page explains how to run the repository locally.

No hosting provider, publication, or paid plan is automatically configured by this repository.

## Deliberate limitations

- Manual source entry. Recorded verification is not automatic identity verification.
- No runtime LLM, scraping, outgoing messages, contracts, or payment processing.
- Zero recorded payments in the example dataset; advertised amounts are never summed as income.
- A conservative supported-currency allowlist; unsupported currencies require review.
- Shared data is refreshed manually. No realtime synchronization claim.
- Application-level append-only observations are not a cryptographic audit log.
- Public synthetic data only. This configuration is not suitable for private client information.
- Local rehearsal uses a fixed 27 September 2026 clock; the connected board uses load time and the editor uses current time.

## Build context

Prepared for the Sanity DEV Challenge, Path Two, which accepts Next.js or Astro frontends with Sanity behind them. A valid entry still needs its final public deployment/repository links and the owner's final submission. Project creation or a passing build does not constitute entry or prize income.

Original application code and scenarios were produced with an AI agent. Libraries are used under their respective licenses. The source is published for inspection and challenge review; no additional open-source license is granted for the original application.

Primary references:
- https://www.sanity.io/docs/content-lake/transactions
- https://www.sanity.io/docs/studio/embedding-sanity-studio
- https://nextjs.org/docs/app/guides/static-exports
- https://dev.to/challenges/sanity-2026-09-16
