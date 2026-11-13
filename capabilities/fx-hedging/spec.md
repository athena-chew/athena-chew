<div style="border-top: 6px solid #024731; border-bottom: 1px solid #B2B2B2; padding: 12px 0; margin-bottom: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif;">
  <div style="color: #024731; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; font-size: 0.85rem;">University of Hawaiʻi at Mānoa · Shidler College of Business</div>
  <div style="color: #000000; font-weight: 700; font-size: 1.25rem; margin-top: 4px;">FIN 321: International Finance and Securities</div>
  <div style="color: #525252; font-weight: 400; font-size: 0.95rem;">FX Transaction Hedging Project — Technical Specification</div>
</div>

<!--
BRAND FORMATTING — applied per docs/_branding/design.json (v1.0.0)
  ┌─ Colors ─────────────────────────────────────────────────────────────┐
  │ Primary green ........... #024731  RGB 2,71,49    Pantone 3435 C     │
  │ Primary black ........... #000000  RGB 0,0,0      Process Black      │
  │ Silver (secondary) ...... #B2B2B2  RGB 178,178,178 Cool Gray 5 C     │
  │ White (secondary) ....... #FFFFFF                                    │
  │ Neutral-600 (muted) ..... #525252  (secondary text, captions)        │
  │ UH-green 700 (hover) .... #013D26  (link hover, pressed state)       │
  │ UH-green 50 (tint) ...... #E6F2EF  (callout backgrounds)             │
  │ Yellow (Excel only) ..... #FFFF00  (input highlight — not brand)     │
  ├─ Typography ─────────────────────────────────────────────────────────┤
  │ Headings (web)  .......... Open Sans Bold (H1/H2) / Semibold (H3/H4) │
  │ Headings (print)  ........ Avenir Bold                               │
  │ Body (web)  .............. Open Sans Regular                         │
  │ Body (print)  ............ Avenir Book                               │
  │ Fallback stack  .......... Helvetica, Arial, sans-serif              │
  │ Monospace  ............... Consolas / ui-monospace                   │
  │ Body minimum size  ....... 10 pt (11–12 pt preferred for print)      │
  │ Leading  ................. 3–5 pt greater than type size (print)     │
  │ Alignment  ............... Flush left, ragged right                  │
  ├─ Accessibility ──────────────────────────────────────────────────────┤
  │ • ADA-compliant contrast ratios for ALL text and UI elements         │
  │ • No red body type                                                   │
  │ • No layouts that are too dark for readability                       │
  │ • No custom palettes or gradients outside the official brand         │
  └──────────────────────────────────────────────────────────────────────┘
  Full brand standard: docs/_branding/design.json · Source: https://manoa.hawaii.edu/brand/
-->

# Commercial-HVAC — FX Transaction Hedge Model · Technical Specification

> <span style="color:#024731; font-weight:700;">Technical specification</span> for the FX transaction hedge model — the named-range contract, calculation flow, and validation checks, precise enough that an AI or a colleague could build (or rebuild) the workbook from this document alone. This spec is the input the AI-assisted build works from.

| Field | Value |
|------|------|
| **Created by** | Athena Chew |
| **Updated by** | Athena Chew |
| **Date Created** | 2026-11-05 |
| **Date Updated** | 2026-11-05 |
| **Version** | 1.1 |
| **LLM Used** | ChatGPT GPT-6 - To edit draft of specifications and check for errors and any missing information |
| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | student build |

---

## 1. Problem Statement

Commercial-HVAC expects to receive a €3,400,000 receivable in 273 days (approximately nine months). Because the receivable is denominated in euros, a depreciation of the EUR against the USD over the settlement period would reduce the amount of USD received upon conversion, creating foreign exchange risk. This specification establishes a framework to quantify and compare four strategies—no hedge, forward hedge, money-market hedge, and put option hedge—to evaluate their respective cash-flow outcomes, risk exposures, and potential benefits. The resulting sensitivity analysis will support the Stage 5 hedging recommendation to the CFO.

---

## 2. Inputs (Known Variables)

All inputs should be exposed as workbook **named ranges** so Calculation Flow (§4) reads the same whether implemented in Excel, Python, or an AI prompt. Dates, sources, and access timestamps are recorded in the Notes tab. Market inputs (spot, forward, rates, premia) are the only cells an analyst should adjust for scenario work.

### 2.1 Core Inputs

| Standardized Name | Description | Unit | Example | Stage 4 Source |
|-------------------|-------------|------|---------|-------:|
| `FC_AMT` | Foreign-currency notional (receivable or payable) | FC | 3,400,000 | 
| `S0_in` | Spot exchange rate at inception | USD per FC | 1.11687 (indicative, replaced with live market data at Stage 4.)| fxempire |
| `F0_in` | Forward rate to settlement | USD per FC | 1.1756 |
| `R_USD` | USD interest rate to settlement | Annual % | 4.56% annual, ACT/360 (indicative, replaced with live market data at Stage 4.)| H.15 |
| `R_FC` | Foreign-currency interest rate to settlement | Annual % | 2.91% annual, ACT/360 (indicative, replaced with live market data at Stage 4.)| ECB |
| `T_DAYS` | Days to settlement | Days | 273 |
| `BASIS` | Day-count denominator (single-value simplification) | Days | 360 (USD, EUR) |
| `K_PUT` | Put option strike (receivables) | USD per FC | 1.10499 - (Strikes at spot) |
| `K_CALL` | Call option strike (payables) | USD per FC | 1.10499 - (Strike at spot) |
| `PREM_PUT` | Put premium, USD per 1 FC | USD | 0.0224 |
| `PREM_CALL` | Call premium, USD per 1 FC | USD | 0.0376 |

### 2.2 Derived / Intermediate Values

| Name | Description | Source |
|------|-------------|--------|
| `FV_PREM_PUT` | Future value of put premium at settlement | `−PREM_PUT × FC_AMT × (1 + R_USD × T_DAYS/BASIS)` |
| `FV_PREM_CALL` | Future value of call premium at settlement | `−PREM_CALL × FC_AMT × (1 + R_USD × T_DAYS/BASIS)` |
| `S_T_grid` | Sensitivity spot grid at settlement | Built from `S0_in` ± 5% in 1% steps |
| `USD_NO_HEDGE` | USD proceeds / outlay under no hedge | `S_T × FC_AMT` |

---

## 3. Assumptions & Constraints

State every convention used. Clarity here is what makes the model reproducible.

- **Quote convention:** All rates expressed as **USD per unit of foreign currency** (e.g., USD/GBP, USD/EUR). A higher quote means FC appreciation.
- **Horizon:** Single-maturity model; `T_DAYS = 365` unless otherwise noted. Templates assume a 1-year tenor.
- **Day-count basis:** The default 1-year tenor uses the textbook simplified-annual form `(1 + r)` for both USD and FC legs. The general form is `r × T_DAYS / BASIS`, with `BASIS = 360` for USD and EUR money-market quotes (ACT/360) and `BASIS = 365` for GBP money-market quotes (ACT/365). The template exposes a **single `BASIS`** named range (simplification). A rigorous build should split it into `BASIS_USD` and `BASIS_FC` so each leg applies its own convention. Record it in §6.2.
- **Parity:** Money-market hedge is assumed to replicate the forward hedge under covered interest-rate parity; any gap is a test of parity, not a model error.
- **Option premium:** Paid upfront in USD, quoted per 1 unit of FC (no contract multiplier). Premia are expressed as a **negative cash flow** at t₀ and carried forward at `R_USD` to put them on the same footing as the settlement-date USD proceeds.
- **Counterparty / credit risk:** Excluded. All derivatives assumed frictionless and creditworthy.
- **Transaction costs & bid-ask spreads:** Excluded from the base case. Flagged as a sensitivity candidate in §6.
- **Tax / accounting treatment:** Excluded. Model reports pre-tax cash outcomes only.
- **Scenario construction:** Future spot `S_T` is varied deterministically across a grid; no probability weights and no implied-volatility distribution are applied.

**Notes:**
Sources Used:
https://www.ecb.europa.eu/stats/policy_and_exchange_rates/key_ecb_interest_rates/html/index.en.html - EURO interest rate
https://www.federalreserve.gov/releases/h15/ - USD interest rate
https://www.fxempire.com/currencies/eur-usd - EURUSD spot exchange rates

Sources used for initial specification and model build. Will be replaced with live market data at Stage 4. Sources pulled as of market close, November 10, 2026

---

## 4. Calculation Flow

Described in named-range pseudocode so the logic is portable across Excel, Python, and AI prompts. Formulas are written for a **receivable** exposure; Step 7 shows the sign flips required for a payable.

### Step 1 — Derived inputs

1. `DF_USD` = `1 + R_USD × T_DAYS / BASIS` *(accumulation / growth factor, not a fractional discount factor)*
2. `DF_FC` = `1 + R_FC × T_DAYS / BASIS`
3. `FV_PREM_PUT` = `−PREM_PUT × FC_AMT × DF_USD`
4. `FV_PREM_CALL` = `−PREM_CALL × FC_AMT × DF_USD`

### Step 2 — Forward hedge (certainty benchmark)

- `USD_FWD` = `FC_AMT × F0_in`
- Locked-in at t₀; invariant across the `S_T` grid.

### Step 3 — Money-market hedge (parity check)

1. Borrow `FC_AMT / DF_FC` foreign currency today *(the discounted PV of the receivable)*.
2. Convert to USD at the spot rate: `(FC_AMT / DF_FC) × S0_in`.
3. Invest the USD to maturity: `USD_MM` = `(FC_AMT / DF_FC) × S0_in × DF_USD`.
4. At maturity, the FC receivable repays the foreign borrowing exactly — so the USD deposit is the locked-in proceed.

> **Parity check:** `USD_MM ≈ USD_FWD` within rounding (0.05%). A persistent gap indicates a violation of covered interest-rate parity in the quoted inputs.

### Step 4 — Option hedge (floor with upside)

Put-and-hold strategy on the receivable:

- Pay `PREM_PUT × FC_AMT` USD today for a put on FC with strike `K_PUT`.
- At settlement, for each `S_T` on the grid:
  - `USD_PUT(S_T)` = `S_T × FC_AMT` + `MAX(0, (K_PUT − S_T) × FC_AMT)` + `FV_PREM_PUT`
  - Equivalently: `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT`

### Step 5 — Sensitivity table (rows of the grid)

For each `S_T` in `S_T_grid`:

| Column | Output | Formula |
|--------|--------|---------|
| No hedge | `USD_NO_HEDGE(S_T)` | `S_T × FC_AMT` |
| Forward | `USD_FWD` | constant across rows |
| Money market | `USD_MM` | constant across rows |
| Option (put) | `USD_PUT(S_T)` | `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT` |
| Hedge profit | `HEDGE_PROFIT_k(S_T)` | `USD_k − USD_NO_HEDGE` for each of `k ∈ {forward, money market, option}` — one sub-column per strategy |
| Overall winner (incl. no hedge) | label | `ARGMAX(USD_NO_HEDGE, USD_FWD, USD_MM, USD_PUT)` |
| Best active hedge (excl. no hedge) | label | `ARGMAX(USD_FWD, USD_MM, USD_PUT)` |

### Step 6 — Summary metrics (scalar outputs)

- `USD_FLOOR_PUT` = `MIN(USD_PUT)` across `S_T_grid` *(worst-case put outcome on the grid; payable tab uses `USD_CEILING_CALL = MAX(USD_CALL)` instead)*
- `USD_BASE_k` = `USD_k` evaluated at `S_T = S0_in` for each strategy *(the "baseline" row feeds §5.1)*

*`HEDGE_PROFIT_k` is per-row and lives inside the Step 5 grid, not here.*

### Step 7 — Payable variant (sign flips)

For a payable of `FC_AMT` to be settled in FC at maturity, the model mirrors Steps 2–5 with three substitutions: (a) **buy** the forward instead of sell, (b) **borrow USD today, invest in FC** for the money-market leg, and (c) use a **call on FC** with strike `K_CALL` and premium `PREM_CALL`. The sensitivity winner is the strategy that **minimizes** USD outlay at each `S_T`, not the one that maximizes it.

---

## 5. Outputs

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Input panel | All named-range inputs with units, sources, and access dates | Top of each tab | Single source of truth |
| Strategy summary | `USD_FWD`, `USD_MM`, `USD_BASE_k` per strategy, plus `USD_FLOOR_PUT` (receivable) / `USD_CEILING_CALL` (payable) | Table above sensitivity grid | Executive at-a-glance |
| Sensitivity table | USD proceeds / outlay for each strategy across `S_T_grid` ± 5% | Table on each tab | Core analytical evidence |
| Hedge-profit columns | `USD_k − USD_NO_HEDGE` for each strategy per row | Sub-table | Isolates hedge value-add |
| Winner / best-hedge labels | `ARGMAX` / `ARGMIN` labels per row | Two label columns | Quick-read decision cue |
| Sensitivity chart | Line chart of USD outcome vs. `S_T` for all four strategies | Embedded chart | Visual comparison |
| Executive summary (Stage 5 decision memo) | 1–2 paragraph narrative with explicit recommendation | Separate memo | Downstream deliverable |

### 5.1 Computed Base-Case Values

Record the base-case outcome at `S_T = S0_in` once the model is built. This block serves as a regression checkpoint for the Stage 3 build and the Stage 4 repopulation.

| Strategy | USD Proceeds (Receivable) | USD Outlay (Payable) | Hedge Profit vs. No Hedge |
|----------|--------------------------:|---------------------:|--------------------------:|
| No hedge | 3797358 | 0 | — |
| Forward | 3997040 | 0 | + 5.0% |
| Money market | 3843847 | 0 | + 1.2%|
| Option (put / call) | 3797358 | 78793 | -2.11% |

---

## 6. Model Review — What Worked & What to Improve

Record candidly what the model gets right and where it needs work. If you are writing this spec before the build, use the notes below as a known-risks register — these pitfalls are common in FX hedge models and are worth designing against from the start; if iterating after a first build, treat them as a review of what to fix.

### 6.1 What Worked

### 6.2 What to Improve

### 6.3 Auditability Checklist

- [ ] Every input has a standardized named range from §2.1
- [ ] Every formula in §4 uses named ranges — no bare `$F$n` references
- [ ] Money-market hedge ties to forward hedge within 0.05% (parity check)
- [ ] Put payoff at `S_T = K_PUT` equals `K_PUT × FC_AMT + FV_PREM_PUT` (kink verification)
- [ ] Sensitivity grid is symmetric around `S_T = S0_in` and driven by from range -5% to +5% (inclusive) at 1% interval.
- [ ] Notes tab records spot / forward / rate sources with access dates
- [ ] Cell colors match the legend: <span style="background:#FFFF00;">yellow</span> inputs, <span style="color:#0000FF;">blue</span> assumptions, black formulas, <span style="color:#024731;">green</span> cross-tab links

### 6.4 Validation Checklist
The completed workbook must pass the following validation checks before its outputs are used for the Stage 5 decision memo.

- Input validation: All ten standardized named ranges must be present, correctly defined, and linked to the appropriate input cells.
- Forward hedge: USD proceeds must equal FC_AMT × F0_in and remain constant across all sensitivity scenarios.
- Money-market hedge: The foreign-currency borrowing must equal the present value of the receivable. The loan must be fully repaid by the EUR receivable at maturity.
- Interest rate parity: Money-market hedge proceeds should approximately equal forward hedge proceeds when the inputs satisfy covered interest rate parity, subject to rounding and consistent market conventions.
- Option payoff: The put option must establish a minimum exchange-rate conversion value at the strike price, less the premium cost. The option must participate in favorable EUR appreciation above the strike.
- Sensitivity validation: The sensitivity grid must range from 95% to 105% of S0_in, using 1% increments, with 11 scenarios.
- Formula integrity: All calculated outputs must use formulas rather than manually entered results. No spreadsheet error values should appear.
- Output verification: All strategy proceeds, hedge-profit calculations, and scenario winners must reconcile with their respective formulas.

---

## 7. Sensitivity Plan

- **Grid:** `S_T_grid` spans `S0_in × (1 ± 5%)` in 1% increments → 11 rows (including the baseline).
- **Strategies plotted:** no hedge, forward, money market, option (put for receivable / call for payable).
- **Primary chart:** line chart with `S_T` on the x-axis and USD proceeds (or outlay) on the y-axis. Forward and money-market series are horizontal by construction; no-hedge is a straight line through the origin; option is piecewise-linear with a kink at the strike.
- **Secondary table:** hedge profit vs. no hedge for each strategy, to make the visual intuition numeric.
- **What the chart should communicate:** the trade-off between **certainty** (forward / money-market, flat lines), **optionality** (put / call, kinked payoff), and **naked exposure** (no hedge, unbounded on both sides).

---

## 8. Limitations & Next Steps

**Limitations.** This specification does not incorporate:
- Partial / layered / dynamic hedging (treated as static, full-notional hedge at t₀)
- Credit, counterparty, and settlement risk
- Implied-volatility-based option pricing (premia are scenario inputs, not Black-Scholes outputs)
- Accounting treatment (ASC 815 / IFRS 9 hedge accounting designation)
- Multi-currency or multi-horizon portfolio effects

**Next steps — later stages will:** (a) translate the sensitivity evidence into the Stage 5 decision memo to the CFO, (b) formalize the AI prompt using §4 as the instruction block and §6.2 as the improvement brief, and (c) implement at least one of the §6.2 improvements (default priority: standardized named ranges + chart).

---

## 9. Writing a Strong Specification

> <span style="color:#024731; font-weight:600;">The spec should read like a handoff document, not a lab notebook.</span>

- **Communicate like a professional:** clear, structured, no filler.
- **Think one stage ahead:** the spec feeds directly into the Stage 3 AI build prompt and the Stage 5 decision memo.
- **Be internally consistent:** variables, labels, and steps must align with the actual workbook.
- **Be reproducible:** another treasury analyst — or an AI — should be able to rebuild the model from this spec alone.
- **Be reflective:** §6 should show honest assessment of the model's strengths and gaps, not self-congratulation.
- **Be executive-relevant:** the CFO should understand *what was built* and *why it matters* for the hedging decision.

**Workbook Tab Architecture**

The workbook must contain the following tabs, each with a clearly defined purpose:

| Tab | Purpose |
|---|---|
| Cover | Identify the project, company, course, author, and model version. |
| Legend/Key | Explain cell colors, input conventions, formulas, and workbook navigation. |
| Inputs | Store all market inputs, assumptions, units, sources, and access dates. |
| Forward Hedge | Calculate the locked-in USD proceeds using the forward exchange rate. |
| Money-Market Hedge | Display the foreign-currency borrowing, spot conversion, and USD investment calculations separately. |
| Option Hedge | Calculate the put option payoff, premium cost, and net USD proceeds. |
| Sensitivity | Compare all four strategies across the specified future exchange-rate scenarios. |
| Notes & Assumptions | Document data sources, market-data timestamps, assumptions, limitations, and model conventions.

---

## 10. How This Sets Up Stages 3–5

| What's Written in Stage 2 | What It Enables in Stages 3–5 |
|---------------------------|----------------------------|
| Standardized named ranges with precise definitions | AI uses standardized variable names; no improvisation |
| Step-by-step calculation flow | AI generates correct, auditable hedge formulas |
| Model review and improvement notes (§6.2) | AI builds the *improved* version, not just a replica |
| Explicit output requirements + chart spec | AI produces the exact tables, chart, and memo sections needed |
| Base-case output values (§5.1) | Regression checkpoints for the refined model |

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 0.1 | 2026-11-05 | Athena Chew | Initial draft, based on https://github.com/adamwstauffer/shidler/blob/main/courses/International-Finance-And-Securities/projects/fx-hedging/_templates/template-spec.md |
| 1.0 | 2026-11-05 | Athena Chew | Revised draft, fed through ChatGPT GPT 6.0 to look for any modeling errors or better way to phrase data |
| 1.1 | 2026-11-12 | Athena Chew | Added validation, sources from which live data is pulled from, defined BASIS in context table, and fixed up some leftover indicators.
