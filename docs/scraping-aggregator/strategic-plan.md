# Unified Scraping Aggregator — Strategic Plan & Research Dossier

> Working title: **"the routing layer for web data."**
> A single API/portal that aggregates multiple web-scraping/extraction providers (Apify, Firecrawl, and hungry newer entrants), buys capacity wholesale, routes each job to the cheapest *capable* provider, refines the combined output, and resells it on metered tiers — to humans **and** to autonomous AI agents.
>
> Status: **pre-validation.** This document captures the research, conclusions, assumptions, financial model, technical plan, phased roadmap, and the levers we can pull. It is the artifact to decide against before spending money.
>
> Last updated: 2026-06-15.

---

## 0. Executive Summary

- **What it is:** Not "another scraper." A meta-layer that turns N paid scraping providers into one cheap, reliable, refined endpoint. Margin comes from **buy-side wholesale discounts**, not markup-on-convenience.
- **Why now:** LLM/agent demand for web data is exploding; providers are commoditizing and fragmenting; agent-native payment + discovery rails (MCP, x402) went production-ready in 2025–2026, opening a zero-CAC machine customer channel.
- **Unit economics:** ~62% blended gross margin (conservative). Structurally **profitable from the entry tier** because we buy at ~19–45% of entry-retail rates. No fixed-cost "valley of death."
- **Downside to validate:** bounded to **~$1–1.5k over a 3-month test** (worst realistic case ~$3–5k). Profitable at every modeled scale.
- **Upside:** lifestyle-to-small-business. Conservative net (pre-tax, pre-labor): ~$27k/yr at 250 users, ~$57k at 500, ~$117k at 1,000.
- **The single decisive unknown:** **paid-tier conversion / ARPU** (the $5 → $25 → $75 ladder). Everything between "side project" and "real business" lives in that one number. The cheap test exists to measure it.
- **Grade:** Decision to run the validation = **A**. The business as a long-term venture = **B−**. The gap is exactly why we spend the ~$1.5k.

---

## 1. Origin & Competitive Insight

### 1.1 The trigger: `cporter202/API-mega-list`
- A GitHub repo (6.5k★, 1.26k forks) marketed as "10,498 ready-to-use APIs across 18 categories."
- **Reality:** it is a **directory of affiliate links**, not software. Every entry links to an **Apify Actor** carrying the same FirstPromoter referral code (`?fpr=p2hrc6`). No SDK, no client, no executable product.
- The "10k APIs" figure is inflated: heavy duplication (6+ near-identical "TikTok Profile Scraper" rows), filler ("Business" category = 2 entries), and dead links ("...Actor-Delete").
- It is one of **~22 repos** by the same author running the identical playbook (`scraping-apis-for-devs`, `social-media-scraping-apis`, `agentic-ai-apis`, …), all funneling to Apify affiliate revenue + a paid Skool community.

### 1.2 What's clever (worth stealing)
- **GitHub as SEO host:** free, high domain authority, ranks well, devs trust repos.
- **Portfolio of lists:** each ranks for different long-tail keywords; diversified against takedown.
- **Affiliate residual:** Apify pays 20% (first 3 months) → 30% ongoing, capped $2,500/customer.

### 1.3 What's weak (the opening)
- Owns nothing: no users, no data, no pricing power, no product.
- Pure passthrough — volatile, uncapped downside, decays in ~12 months.
- Pulls **hobbyists**, not the agency whales that carry real spend.

### 1.4 The thesis
**Stop being the signpost; become the road.** Replace "links to other people's scrapers" with "a metered, refined, multi-provider gateway you own." Keep cporter's free distribution engine; bolt it onto a product that captures recurring margin and compounds a moat.

---

## 2. Areas of Research → Conclusions

Each area below states the **question**, the **findings** (grounded where possible), and the **conclusion / strong direction**.

### 2.1 Provider pricing & the discount-arbitrage ratio
**Question:** Is there a real wholesale-vs-retail spread big enough to make a cheap entry tier profitable rather than a loss leader?

**Findings (mid-2026):**

*Firecrawl (first-party, clean economics):*

| Tier | Price/mo | Credits | $/page |
|---|---|---|---|
| Free | $0 | 1,000 | — |
| Hobby (entry paid) | $16 | 5,000 | $0.0032 |
| Standard | $83 | 100,000 | $0.00083 |
| Growth | $333 | 500,000 | $0.00066 |
| Scale | $599 | 1,000,000 | $0.0006 |

- **discount_ratio (Scale ÷ Hobby) ≈ 0.19** → buying at high tier costs ~19% of entry-retail → **~81% gross margin** on entry-tier-equivalent value.

*Apify (thinner, partly passthrough):*
- CU rate ~$0.30 entry (Starter) → ~$0.20 Business (some sources ~$0.13). **discount_ratio ≈ 0.43–0.67 → ~33–57% margin headroom.**
- **Critical caveat:** much of Apify cost is **third-party actor rental fees** (e.g. "$1 / 1,000 comments") set by actor authors — these **do not** get our volume discount. Apify arbitrage is real but shallow and not fully in our control.

**Conclusion / direction:**
- **Lean margin on first-party + OSS providers** (Firecrawl, Jina Reader, self-hosted Crawl4AI) where the spread is clean (~80%).
- **Use Apify for breadth/long-tail** (unmatched actor catalogue) accepting thinner margin.
- **Blended entry discount_ratio ≈ 0.3–0.45 → ~55–70% blended gross margin.** The $5 tier is structurally profitable. Thesis confirmed.

### 2.2 Terms of Service / legality of resale
**Question:** Can we legally resell / multi-tenant on top of these providers?

**Findings:**
- **Apify:** ⛔ prohibits multiple Personal Accounts ("even with different emails"). ✅ BUT Organization Accounts are sanctioned shared workspaces, **and Apify runs a Solution Provider / Partner program where you set your own prices and keep 100% of managed-service revenue** — i.e. our model is explicitly supported via the partner path. Affiliate program available for the offboarding valve.
- **Firecrawl:** public ToS neither blesses nor bans resale (only a specific FCRA-use prohibition). Operates via **Enterprise / Order Form** for non-standard use; already on the **Vercel Marketplace** (understands being-a-backend-for-others).

**Conclusion / direction:**
- **There is a sanctioned door on both — walk through it, don't go around it.** Before any build: (a) apply to Apify's partner/Solution-Provider program; (b) obtain a Firecrawl order-form/enterprise OK for multi-tenant resale.
- This is the **#1 gate** and it is free to clear. Do not quietly multi-tenant a personal account.

### 2.3 Provider landscape (incumbents, challengers, OSS)
**Question:** Is Apify+Firecrawl enough, or is there value in aggregating smaller/newer providers?

**Findings:**

| Provider | Type | Why interesting |
|---|---|---|
| Firecrawl | Incumbent, first-party | Clean ~80% arbitrage; LLM-ready markdown |
| Apify | Incumbent, marketplace | Breadth (actors); thin/passthrough margin |
| ScrapegraphAI | Challenger | LLM-native prompt→structured; Firecrawl peer |
| JigsawStack | Challenger | Better success on protected sites → fallback value |
| Scrape.do / Decodo | Value picks | Cheap unit cost → fattens arbitrage |
| Zyte | Quality | Leads AI structured extraction |
| Jina Reader | Cheap/near-free | Drives entry COGS toward zero |
| Crawl4AI | Open-source/self-host | ~100% margin on commodity scrapes |
| Diffbot / Bright Data / Oxylabs | Enterprise/proxy | Heavy targets, enterprise tier |

**Conclusion / direction:**
- **Aggregating the long tail is the actual moat**, for three reasons:
  1. **Routing intelligence** ("which provider wins for target X") is itself a product — research the user won't do.
  2. **Hungry challengers give better terms** (aggressive affiliate/wholesale + co-marketing) — improving both arbitrage and residual.
  3. **OSS blending** (Crawl4AI, Jina) for commodity scrapes pushes blended COGS down, reserving paid providers for hard targets.

### 2.4 Unit economics & margin structure
**Question:** Is the margin from breakage or from real spread? Is it robust?

**Findings / conclusion:**
- **Margin is unit-level and structural, from the buy-side discount — NOT breakage.** Each $5 of earliest-tier-retail-equivalent value costs us ~$0.94–2.25 (blended ~$1.5–2). Entry-tier gross margin guaranteed, independent of utilization.
- The **risk inverts**: not cost-overrun (breakage model) but **inventory wastage** (buying credits you don't resell before monthly expiry). This is *benign* — bounded, self-correcting (don't buy the next tier until current sells through), and unused capacity is just headroom.
- **Pricing ladder sells refinement, not volume.** Tiers climb on aggregate capability (providers blended, refinement depth, success-rate SLA), which has far higher willingness-to-pay than raw quantity.

### 2.5 Conversion / ARPU (the decisive unknown)
**Question:** Will free users convert, and will they climb the tier ladder?

**Findings / direction:**
- **Unvalidated.** Assumed free→paid 3–5%; conservative blended ARPU ~$18.
- This is the **single point of failure** for the whole model — the gap between "side project" and "real business."
- **Must be measured in the cheap test** (target: ≥8–12% upgrade from $5 → a refined tier).
- If people only ever want $5, the venture collapses to a reseller with overhead → kill.

### 2.6 Agent-native distribution (payments + discovery)
**Question:** Can autonomous agents discover and pay for this, becoming customers with no human signup?

**Findings (mid-2026, production-ready rails):**
- **Discovery = MCP.** Agents (Claude, Codex, Cursor, …) discover/call tools via MCP. **27,000+ servers** indexed across Official Registry, Smithery (~7k), mcp.so (~20k), Glama, GitHub MCP Registry, `awesome-mcp-servers`. Discovery is **semantic** — the tool name + description is the ranking surface.
- **Payment = x402.** Coinbase's x402 (HTTP 402 + stablecoins) enables pay-per-call API access **with no account/signup**. x402 V2 launched Dec 2025; **Stripe integrated x402 on Base, Feb 2026**; Cloudflare supports it. Purpose-built for "APIs and MCP servers."
- **Authorization/trust = AP2** (Google Agent Payments Protocol): delegated, bounded spend mandates ("agent may spend ≤$20/day") — also our spend-cap/fraud control.
- Visa, Mastercard, Google have agent-payment programs; McKinsey projects agentic commerce $3–5T by 2030.

**Conclusion / direction:**
- This is **Funnel C** and it attacks both grade-card weaknesses: **CAC → ~0** and **moat hardens** (becoming the agent's routed default is stickier than any human funnel).
- We are **uniquely suited**: agents route to cheapest-capable (our arbitrage wins), prefer fewer tools (our aggregation wins), and we already designed metered/prepaid billing (agent-payability mostly done).
- **Bet on MCP + x402 first; add AP2 for spend-control; ignore the rest until proven.**

### 2.7 Marketing / go-to-market
**Question:** How do we get distribution — especially to whales, not just hobbyists?

**Findings / direction:**
- **Position on the wedge, not the category:** "One key for every scraper. Cheaper than Firecrawl/Apify's minimum — and output neither gives alone." Three proof hooks: price, reliability (failover success rate), refinement.
- **Two human funnels + one agent funnel:**
  - **Funnel A (PLG/volume):** SEO content, GitHub repo network, Show HN, Reddit, Product Hunt → free/$5 signups, conversion signal. ~$0.
  - **Funnel B (whales/ARPU):** workflow-tool integrations (n8n, Make, Zapier, Vercel Marketplace), buyer-intent comparison SEO, cold outreach, case studies → $75–250 tiers.
  - **Funnel C (agents):** MCP + x402 (see 2.6).
- **Research-as-marketing:** publish routing/benchmark data ("we tested 8 scrapers on 1,000 protected sites — which wins per target"). One asset = top-ranking SEO + GEO/AEO citations + proof of moat + the data is a byproduct of running the service.
- **GEO/AEO:** ChatGPT indexes via Bing, Claude via Brave; ensure `OAI-SearchBot`, `PerplexityBot`, `Claude-SearchBot` can crawl; get mentioned on Reddit/GitHub (their retrieval sources). `llms.txt` = do once, low impact.

---

## 3. Core Assumptions (explicit, with confidence + validation)

| # | Assumption | Value used | Confidence | How we validate |
|---|---|---|---|---|
| A1 | Blended buy-side discount_ratio | 0.30–0.45 | **High** (from live pricing) | Direct provider pricing; recompute monthly |
| A2 | Blended gross margin | 62% (conservative) | High | Actual COGS vs revenue once live |
| A3 | Blended ARPU | $18/user/mo (conservative) | **Low** | The test — tier-mix telemetry |
| A4 | Free→paid conversion | 3–5% | **Low** | The test |
| A5 | $5→higher-tier upgrade rate | ≥8–12% (target) | **Low** | The test |
| A6 | Resale legally sanctioned | Yes, via partner/order-form | Medium | Apply to programs **before build** |
| A7 | Fixed provider-sub floor | ~$200–800/mo, stepped | High | Provider plan selection |
| A8 | Free-tier COGS/user (capped) | ~$0.20/mo | Medium | Telemetry; shrink bucket if exceeded |
| A9 | Agent channel viable | Yes (MCP+x402 live) | Medium | Ship MCP server; measure agent calls |
| A10 | Apify passthrough caps margin | margin → ~50% if Apify-heavy | High | Provider-mix telemetry |
| A11 | Distribution to whales exists | Unproven | **Low** | Funnel B test (outreach + integrations) |

**Assumptions most likely to break the model if wrong:** A3, A4, A5, A11 — all about demand/conversion, all measured cheaply in Phase 0.

---

## 4. Financial Model

### 4.1 Conservative net-income by milestone
Assumptions: ARPU **$18**, gross margin **62%** (COGS 38%), mid-range business opex (tooling, monitoring, accounting/admin, payment processing ~3%). **Excludes owner labor and tax** — net = operating profit before owner comp.

| Users | Turnover /mo | Turnover /yr | Gross margin % | Gross profit /mo | Mid opex /mo | **Net /mo** | **Net /yr** |
|---|---|---|---|---|---|---|---|
| 25 | $450 | $5.4k | 56%* | $250 | $150 | **$100** | **~$1.2k** |
| 50 | $900 | $10.8k | 62% | $558 | $200 | **$358** | **~$4.3k** |
| 100 | $1,800 | $21.6k | 62% | $1,116 | $300 | **$816** | **~$9.8k** |
| 250 | $4,500 | $54.0k | 62% | $2,790 | $500 | **$2,290** | **~$27.5k** |
| 500 | $9,000 | $108.0k | 62% | $5,580 | $800 | **$4,780** | **~$57.4k** |
| 1000 | $18,000 | $216.0k | 62% | $11,160 | $1,400 | **$9,760** | **~$117.1k** |

\* At 25 users the fixed provider-sub floor (~$200/mo) binds, compressing margin to ~56%; above ~30 users COGS tracks 38% and margin holds at 62%. Net margin settles ~50–54% of turnover past the floor.

### 4.2 ARPU sensitivity (annual net contribution)
ARPU is the whole ballgame (set by upgrade rate):

| Users | ARPU $12 (weak) | ARPU $18–20 (base) | ARPU $30 (strong) |
|---|---|---|---|
| 50 | $4.1k | $4.3–6.8k | $10.3k |
| 100 | $8.2k | $9.8–13.9k | $20.5k |
| 250 | $20.5k | $27.5–35.4k | $51.3k |
| 500 | $41.0k | $57.4–70.8k | $102.6k |
| 1000 | $82.1k | $117.1–141.6k | $205.2k |

Add affiliate residual (offboarded users): **+5–15%** on top of all cells.

### 4.3 Take-home (500-user case, illustrative)
- ~$57k conservative net → less ~$6k lean overhead → ~$51k pre-tax (solo, no hired help).
- After tax (generic personal-income, 25–45% band): **~$28–43k take-home solo.**
- Note: this *is* the owner's whole comp — judge against day-rate/opportunity cost. Icelandic *ehf* (20% corp + dividend) may optimize vs. personal income — TBD by jurisdiction.

### 4.4 Downside / cost exposure (pessimistic ~1% conversion)
Design caps this hard (minimum subs, tiny non-refilling free buckets, conversion trickle offsets free COGS):

| Free signups | Converts @1% | Paid rev /mo | Free COGS /mo | Fixed (subs+infra) | **Monthly burn** |
|---|---|---|---|---|---|
| 500 | 5 | $90 | ~$100 | ~$350 | **−$360** |
| 1,000 | 10 | $180 | ~$200 | ~$350 | **−$370** |
| 2,500 | 25 | $450 | ~$500 | ~$350 | **−$400** |
| 5,000 | 50 | $900 | ~$1,000* | ~$350 | **−$450** |

\* cap this via free-seat limits.

- **Burn stays flat ~$350–450/mo** even as free signups scale 10× (each free user ≈ −$0.09/mo net; burn dominated by fixed floor).
- **Total exposure to test: ~$1–1.5k over 3 months.** Worst realistic (6 months, 5k free, loose caps): **~$3–5k**.
- Capital-at-risk to learn whether it works ≈ price of a cheap laptop.

---

## 5. Technical Architecture Plan (high level)

```
                 ┌─────────────────────────────────────────────┐
   Humans  ──────▶  Portal / Dashboard (auth, billing, keys)    │
   Agents  ──────▶  API Gateway  +  MCP Server  +  x402 endpoint │
                 └───────────────┬─────────────────────────────┘
                                 │  (normalized request schema)
                          ┌──────▼───────┐
                          │  Router /    │  ← capability + cost + success-rate routing
                          │  Orchestrator│  ← cache, dedup, fallback/failover
                          └──────┬───────┘
            ┌───────────┬────────┼────────┬───────────┬───────────┐
        Firecrawl    Apify   ScrapegraphAI  Jina    Crawl4AI    (others)
       (first-party)(actors)  (LLM extract)(cheap)  (self-host)
                          ┌──────▼───────┐
                          │ Refinement   │  ← dedup, schema-normalize, merge, LLM-structure, score
                          └──────┬───────┘
                          ┌──────▼───────┐
                          │ Metering /   │  ← prepaid credits, per-job caps, usage events
                          │ Billing      │  ← Stripe (humans) + x402 (agents) + AP2 mandates
                          └──────────────┘
```

**Components:**
1. **Ingress:** REST API (OpenAPI spec) + **MCP server** (tools: `scrape`, `crawl`, `extract`, `search`) + **x402** pay-per-call endpoint. `/.well-known/mcp.json` manifest.
2. **Normalization:** one request schema → provider-specific adapters. One response schema out.
3. **Router/orchestrator:** picks provider per job on (capability × cost × measured success-rate); cache + cross-user dedup of public pages; **fallback** to next provider on block/failure (this powers the "98% delivery" SLA).
4. **Refinement layer (starts thin):** v0 = schema-normalize + dedup. v1 = cross-source merge. v2 = LLM-structure + enrichment + scoring. This is the moat — deepen only after demand is proven.
5. **Metering/billing:** prepaid credit buckets (never postpaid), per-job budget caps, rate limits, usage-event log. Stripe for human tiers; x402 for agent pay-per-call; AP2 mandates for delegated agent spend.
6. **Observability:** per-key/source usage, provider mix (→ margin), free-tier burn, conversion-by-source, success-rate per provider/target (feeds router *and* benchmark content).

**Build principles:**
- Provider adapters behind a clean interface → adding a provider is cheap (this is the moat-widening unit of work).
- Everything metered in **outcomes** (results/pages/profiles), never raw calls.
- **Hard caps everywhere** (free seats, per-key spend, per-job budget) — exposure control is a first-class feature, not an afterthought.

---

## 6. Phased Plan

### Phase 0 — Validation (the cheap test). Target ~6–10 weeks. Budget ≤ ~$1.5k.
**Goal:** measure the one unknown — conversion/ARPU — at minimum cost.
- Clear the **ToS gate**: apply to Apify partner/Solution-Provider; request Firecrawl order-form/enterprise OK. *(blocking, free)*
- Carry **minimum paid tiers** on 2–3 providers (Firecrawl + Apify + one challenger/OSS).
- Thin product: normalized API + 2–3 tiers ($5 / $25 / $75), prepaid buckets, hard caps. Refinement = v0 (normalize + dedup).
- Capped free tier (≤1,000 seats, tiny non-refilling bucket, identity gate).
- Distribution: Show HN, r/webscraping, r/SaaS, Indie Hackers, 2–3 buyer-keyword benchmark posts, 2–3 GitHub repo-lists.
- **Ship a minimal MCP server** + list on registries (cheap, opens Funnel C early).
- **Instrument everything by source** (the whole point).

**Exit gate (need ≥3 of 4):** ≥~80 paying users · ≥8–12% $5→higher upgrade · free-tier utilization <~50% · no ToS blocker.
**Kill condition:** people only ever buy $5 (ARPU ≈ $6–7) → stop; it's a reseller with overhead.

### Phase 1 — Product (if Phase 0 passes). ~Q+1.
- Deepen **refinement layer** to v1 (cross-source merge) — the proven-demand upsell.
- Add **2–4 more providers** (incl. OSS Crawl4AI/Jina to lower COGS).
- Add **x402 pay-per-call** + basic **AP2** spend-control → Funnel C live for real.
- Build **integrations** (n8n / Make / Zapier / Vercel Marketplace) → Funnel B opens.
- Formalize **own referral program** (viral loop) + affiliate offboarding valve.

### Phase 2 — Scale to ~500 users. ~Q+2–3.
- Refinement v2 (LLM enrichment + scoring) → unlock $75–250 tiers.
- Whale motion: outreach + case studies + managed/done-for-you tier.
- Router optimization on accumulated success-rate data (compounding moat).
- Publish recurring benchmark reports (GEO + content engine).
- Step provider subscriptions *behind* demand (avoid wastage).

### Phase 3 — Business (~1,000+ users). ~Q+4+.
- Agent channel as primary growth (MCP ubiquity + x402 volume).
- Team: part-time support, infra hardening, SLA tiers.
- Evaluate raising margin on value-add (refinement) where BYO-key can't replicate.
- Consider entity/tax structure optimization (e.g. Icelandic *ehf*).

---

## 7. Levers We Can Pull

**Pricing levers**
- Tier prices ($5 / $25 / $75 / $250) and what each unlocks (capability, not just volume).
- Take rate on value-add vs passthrough (push higher on refinement where BYO can't escape).
- Promotional entry pricing to spike conversion signal.

**Capacity / COGS levers**
- Which provider tiers to carry, and *when* to step up (behind demand → minimize wastage).
- Provider mix (shift volume to first-party/OSS to lift blended margin off the ~50% Apify floor).
- Cache/dedup aggressiveness (don't pay twice for public pages).
- Route-to-cheapest-capable weighting (cost vs success-rate trade-off).

**Free-tier / exposure levers**
- Free-seat cap (turns open liability into fixed).
- Free-bucket size (shrink if free COGS/user > ~$0.10).
- Identity gating strength (kills abuse — the real tail risk).
- Burn kill-switch threshold.

**Demand / channel levers**
- Funnel A spend (organic, ~$0) vs Funnel B effort (integrations, outreach).
- Benchmark-content cadence (SEO + GEO + moat proof, all at once).
- Own referral reward size (viral coefficient).
- Affiliate offboarding routing (capture residual on churned whales).

**Agent levers**
- MCP tool name/description optimization (semantic discoverability = the new SEO).
- Registry coverage (Smithery/mcp.so/Glama/GitHub/awesome-list).
- x402 per-call price (agents route on it — be cheapest-capable).
- AP2 mandate defaults (spend caps balancing trust vs friction).

**Moat levers**
- Refinement depth (thin → merge → LLM enrich → score).
- Provider-count breadth (more adapters = more "only we have this").
- Routing intelligence from accumulated success-rate data (compounds).

---

## 8. Risk Register

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| **Low conversion / ARPU collapse** | Med | **Critical** | Cheap test measures it first; kill condition defined |
| Provider ToS blocks resale | Low–Med | Critical | Apply to partner/order-form **before build** (free gate) |
| Apify passthrough caps margin (~50%) | Med | Med | Skew volume to first-party/OSS; price value-add |
| Free-tier abuse (bot credit farming) | Med | Med | Identity gate + caps + kill-switch (the real tail) |
| Inventory wastage (over-buy capacity) | Low | Low | Step subs behind demand; self-correcting |
| Race-to-bottom on agent price routing | Med | Med | Route on capability/success, not raw price; refinement moat |
| Thin MVP easily cloned (no moat) | **High** | Med–High | Deepen refinement + provider breadth + routing data post-validation |
| Standards fragmentation (x402/AP2/ACP) | Med | Low | Bet on MCP+x402 first; ignore rest until proven |
| Whale channel doesn't exist | Med | High | Funnel B test (outreach/integrations) gates the upside case |
| Provider competes / cuts us off | Low–Med | Med | Multi-provider routing = hedge + genuine value-add |
| Owner-labor opportunity cost > return at <250 users | Med | Med | Treat sub-250 as validation only; commit at 500+ |

---

## 9. Go / No-Go Decision Framework

**Proceed past Phase 0 only if:**
1. Conversion proven: ≥~80 payers **and** ≥8–12% $5→higher upgrade.
2. Margin holds: blended ≥ ~55% in live data.
3. ToS clear: partner/order-form secured on both providers.
4. At least one signal of **whale interest** (one $75+ account or one integration-sourced upgrade).

**Strong-go (commit as real business):** 500-user trajectory within ~18 months *and* a working whale channel → ~$57k+ net, owned customers + data. Worth becoming a company.

**Lifestyle-only (proceed cautiously):** profitable but capped <250 users, hobbyist-only demand → fine as side income, not worth full-time vs opportunity cost.

**No-go / kill:** $5-only demand (ARPU ≈ $6–7), or ToS blocker with no partner path, or free-tier abuse uncontrollable → stop. You've avoided building a higher-overhead version of cporter's affiliate farm.

---

## 10. Open Questions / Next Research

1. **Exact Apify partner-program terms** — margin share, multi-tenant permission, white-label rights. *(contact Apify)*
2. **Firecrawl resale stance in writing** — order-form/enterprise terms. *(contact Firecrawl)*
3. **Challenger affiliate/wholesale terms** — ScrapegraphAI, JigsawStack, Scrape.do, Decodo (hungry = better deals?).
4. **x402 integration specifics** — Stripe-on-Base path vs direct; settlement, fees, custody.
5. **AP2 mandate UX** — how humans delegate bounded spend without friction.
6. **Real blended margin by use-case** — map top intended jobs (e.g. TikTok ingestion) to cheapest capable provider; how much leans on paid Apify actors (margin drag).
7. **Whale sourcing channel** — do we have *any* path to agencies/data teams beyond GitHub hobbyist traffic? (gates the upside case)
8. **Jurisdiction/tax structure** — sole-trader vs Icelandic *ehf* vs other; take-home optimization.
9. **18-month ramp model** — monthly growth + early wastage drag + cumulative cash position + break-even month + max float required.

---

## 11. Appendix — Key Sources (mid-2026)

- Firecrawl pricing teardown — scrapegraphai.com/blog/firecrawl-pricing
- Firecrawl & Apify pricing — costbench.com (web-scraping)
- Apify CU/free pricing — use-apify.com/docs
- Apify Affiliate Program — affiliate.apify.com ; Partner program — apify.com/partners/join
- Apify General Terms — docs.apify.com/legal/general-terms-and-conditions
- Firecrawl Terms of Service — firecrawl.dev/terms-of-service
- Firecrawl alternatives — scrape.do/blog/firecrawl-alternatives ; scrapecreators.com/blog/web-scraping-apis
- Agentic payment protocols compared — crossmint.com/learn/agentic-payments-protocols-compared
- x402 + MCP payments — zuplo.com/blog/mcp-api-payments-with-x402 ; allium.so (x402 explained)
- MCP registries 2026 — roxyapi.com (MCP registries) ; github.blog (GitHub MCP Registry)
- GEO/AEO — pixelmojo.io (GEO playbook) ; addyosmani.com/blog/agentic-engine-optimization

---

*This is a living document. The numbers are conservative-leaning estimates, not guarantees; every "Low confidence" assumption is designed to be cheaply tested in Phase 0 before meaningful capital or time is committed.*
