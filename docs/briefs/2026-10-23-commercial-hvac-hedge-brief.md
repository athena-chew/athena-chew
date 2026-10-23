| type | brief |
|---|---|
| title | Commercial HVAC FX Hedging Executive Brief |
| scenario | 13 — commercial-hvac |
| date | 2026-10-23 |
| hypothesis | I expect the forward hedge to provide the most predictable USD proceeds because it locks in the exchange rate, while the EUR put preserves upside potential at a premium cost. |
| llm_used | ChatGPT (GPT-6) |

# Commercial HVAC FX Hedging Executive Brief

**Created by:** Athena Chew  
**Updated by:** Athena Chew  
**Date Created:** 2026-10-23  
**Date Updated:** 2026-10-23  
**Version:** 1.0  
**LLM Used:** ChatGPT (GPT-6)

---

## Executive Summary (≤150 words)

A U.S. commercial-HVAC manufacturer expects to receive EUR 3,400,000 from a Milan office developer in 273 days. Because the payment is in euros but the company measures its cash flow in U.S. dollars, a weaker euro could reduce the dollar value received. This brief evaluates three possible hedging strategies: a forward contract, a money-market hedge, and a EUR put option. The indicative forward rate of 1.1756 would secure USD 3,997,040 before any applicable costs. The EUR put premium is USD 0.0224 per euro, or USD 76,160 for the full receivable. The next stages will obtain current spot and interest-rate data, build and test a comparative model, and validate the results before a final hedge recommendation is made.

## Background & Objectives

The company faces foreign-exchange risk because it will receive euros in the future but needs to account for the proceeds in U.S. dollars. If the euro depreciates against the dollar, the receivable will convert into fewer dollars, potentially affecting cash flow and financial planning.

The objective is to compare three approaches for managing this exposure: a forward contract, a money-market hedge, and a EUR put option. The analysis will measure their expected USD proceeds, costs, benefits, and limitations. The indicative forward rate is EUR/USD 1.1756 for 273 days. The put premium is USD 0.0224 per euro, while the call premium is USD 0.0376 per euro. Both options have a strike at the current spot rate.

## Methods

The project will use the following approach:

1. **Forward hedge:** Calculate the USD proceeds secured by selling EUR 3,400,000 forward at the indicative rate of 1.1756. The resulting amount is USD 3,997,040 before costs.
  Pro: Complete certainty: budget is safe
  Con: Give up all upside if EUR strengthens; counterparty/margin mechanics
2. **Money-market hedge:** Obtain appropriate USD and EUR interest rates, calculate the present value of the receivable, and determine the USD value that can be secured through borrowing, conversion, and investment.
  Pro: Same certainty as a forward; useful when forwards are unavailable or mispriced
  Con: uses borrowing capacity; more moving parts
3. **Option hedge:** Evaluate the EUR put's downside protection, premium cost, and potential benefit if the euro strengthens. The total put premium is USD 76,160. A EUR call will be considered for comparison but is generally less suitable for a company receiving euros.
   Pro: floor under proceeds plus upside participation
   Con: premium paid up front, hedge or not
4. **Risk testing:** Compare outcomes under different EUR/USD settlement rates, including a hypothetical 5% decline from the forward-rate reference. At 1.11682, the unhedged receivable would convert to approximately USD 3,797,188, about USD 199,852 less than the forward reference amount. This is an illustration, not a forecast.
5. **Validation:** Record sources and timestamps for the current spot rate and interest rates, document assumptions, and check formulas and calculations.

## Limitations & Next Steps

The forward quote is indicative, and the current spot rate and USD and EUR interest rates have not yet been verified. The stress test uses the forward rate as a reference rather than the current spot rate. The analysis assumes the full receivable is collected on time and does not yet quantify transaction costs, financing constraints, or counterparty risk.

**Hypothesis:** I expect the forward hedge to provide the most predictable USD proceeds because it locks in the exchange rate, while the EUR put preserves upside potential at a premium cost.

This hypothesis would be challenged if verified calculations show that the money-market hedge provides better net proceeds, or that the put option's flexibility justifies its premium under the company's risk objectives.

**Next steps:**
- **Stage 2:** Define the model structure, inputs, formulas, and validation checks.
- **Stage 3:** Build and audit the hedge-comparison workbook.
- **Stage 4:** Add sourced, timestamped market data and complete the calculations.
- **Stage 5:** Independently validate the results and prepare the final hedge decision memo.

## References

Federal Reserve. (n.d.). *Selected interest rates (H.15).* https://www.federalreserve.gov/releases/h15/

Stauffer, A. (n.d.). *FX hedging project: Stage 1 — Executive brief.* https://adamwstauffer.github.io/ai-lms/fx-hedging-stage1.html

Stauffer, A. (n.d.). *Decision memo template.* https://github.com/adamwstauffer/shidler/blob/main/courses/International-Finance-And-Securities/projects/fx-hedging/_templates/template-decision-memo.md

ChatGPT Prompt log

2026-10-23

Asked: Provided a notes-style outline for a foreign-exchange hedging assignment covering the company’s EUR 3,400,000 receivable, the 273-day payment period, forward contract, money-market hedge, EUR put option, risk testing, limitations, hypothesis, and next steps. Asked ChatGPT to turn the notes into a fully coherent, polished deliverable while maintaining the crucial information and figures.

Produced: A coherent FX hedging brief organized into Executive Summary, Background & Objectives, Methods, Limitations & Next Steps, with the original figures and calculations preserved. The final deliverable explained the three hedging strategies, including the 1.1756 forward rate, USD 3,997,040 secured through the forward, USD 0.0224/EUR put premium and USD 76,160 total premium, as well as the 5% stress test at 1.11682, producing approximately USD 3,797,188 unhedged and a USD 199,852 difference from the forward reference amount. It also retained the hypothesis favoring the forward hedge and the planned validation/model-building stages.

What was wrong and how it was caught: No major calculation or information errors were identified during the conversion. The main transformation was stylistic: the user’s fragmented notes were converted into complete, professional paragraphs while preserving the original assumptions, figures, caveats, and distinction between illustrative stress testing and an actual forecast
