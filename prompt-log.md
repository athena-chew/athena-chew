# Prompt log

## 2026-09-30

- **Asked:** Set up the athena-chew portfolio repository: folder skeleton, a tailored AGENTS.md based on the course baseline, CLAUDE.md pointer, prompt-log.md, RESUME.md from my resume, a placeholder README.md, and .gitignore, all shown for review before any commit.
- **Produced:** README.md, RESUME.md, AGENTS.md, CLAUDE.md, prompt-log.md, .gitignore, and one-line README.md files in capabilities/, docs/briefs/, docs/decisions/, data/, and analysis/figures/.
- **What was wrong and how it was caught:** (1) The first two requests arrived without the resume text; the upload folder was checked, found empty, and nothing was drafted until the resume was pasted. (2) The "never paste into a model" categories and the "How I work" section were inferred from job titles and coursework, not from my own statements; this was flagged at the time and left for me to correct before committing. (3) The request said graduate course but the resume lists a B.B.A. (May 2027); the files say "a business course". (4) The resume includes my phone number, email, and permanent address, which would be public in this repository; this was flagged during review and left as my decision.

## 2026-10-23

- **Asked:** Provided a notes-style outline for a foreign-exchange hedging assignment covering the company’s EUR 3,400,000 receivable, the 273-day payment period, forward contract, money-market hedge, EUR put option, risk testing, limitations, hypothesis, and next steps. Asked ChatGPT to turn the notes into a fully coherent, polished deliverable while maintaining the crucial information and figures.

- **Produced:** A coherent FX hedging brief organized into Executive Summary, Background & Objectives, Methods, Limitations & Next Steps, with the original figures and calculations preserved. The final deliverable explained the three hedging strategies, including the 1.1756 forward rate, USD 3,997,040 secured through the forward, USD 0.0224/EUR put premium and USD 76,160 total premium, as well as the 5% stress test at 1.11682, producing approximately USD 3,797,188 unhedged and a USD 199,852 difference from the forward reference amount. It also retained the hypothesis favoring the forward hedge and the planned validation/model-building stages.

- **What was wrong and how it was caught:** No major calculation or information errors were identified during the conversion. The main transformation was stylistic: the user’s fragmented notes were converted into complete, professional paragraphs while preserving the original assumptions, figures, caveats, and distinction between illustrative stress testing and an actual forecast

## 2026-11-05

- **Asked:** Shared the Stage 2 FX Hedging Technical Specification for the Commercial-HVAC scenario and asked ChatGPT to review it against the assignment website's instructions. Specifically, wanted to identify missing information that could make an AI uncertain when building the Excel model and improve the professionalism and clarity of the wording.
  
- **Produced:** A detailed review identifying technical corrections, missing requirements, and recommended wording revisions. Recommendations included: (1) correcting the distinction between option strike prices (K_PUT, K_CALL) and premiums (PREM_PUT, PREM_CALL); (2) clarifying ACT/360 day-count conventions for USD and EUR; (3) documenting how option premiums are carried forward to settlement to avoid double-counting; (4) defining the missing STEP_FRAC variable; (5) adding a workbook tab architecture; (6) establishing explicit validation rules; (7) improving the problem statement to explain the EUR 3,400,000 receivable and its foreign-exchange risk; and (8) clarifying that the primary model should focus on the receivable exposure. Also recommended revising the document structure to match the website's eight required sections, completing the model review, and maintaining a prompt-log.md documenting AI-assisted revisions.
  
- **What was wrong and how it was caught:** (1) The original specification listed option strikes of 0.023 and 0.038 USD/EUR, which appeared to confuse the assigned option premiums with strike prices. (2) The day-count instructions offered multiple conventions, potentially causing inconsistent AI calculations. (3) The workbook tab architecture was missing, leaving the AI too much discretion over workbook organization. (4) STEP_FRAC appeared in the auditability checklist but was not defined in the inputs. (5) The problem statement used informal and financially imprecise wording, particularly regarding gross margin and lost conversion value. (6) The validation requirements were partially included in the auditability checklist rather than presented as a dedicated section. These issues were identified during the specification review, with suggested corrections provided before the Stage 3 AI-assisted model build.

# 2026-11-13

- **Asked:** You wanted a workbook built from your model specification, and every item in its build contract was binding: all ten named ranges formulas only, with no pasted values a cover page a Legend/Key tab with the colour convention all three hedge families (forward, money market, option)
the ±5% sensitivity table with a chart validation checks computed in the workbook It had to be a downloadable .xlsx, and you wanted questions before building if anything in the spec was ambiguous. The spec file itself wasn’t uploaded here. Only the two workbooks were, so I worked from the spec references quoted inside them (spec.md v1.1, §2.1, §3, §4, §5.1, §6.4).

- **Produced:**
Two near-identical 10-tab workbooks, FX_Hedge_Model_Commercial-HVAC.xlsx and model_xlsx.xlsx. The tabs are Cover, Legend-Key, Inputs, Forward Hedge, Money-Market Hedge, Option Hedge, Payable Hedge, Sensitivity, Validation and Notes & Assumptions. The exposure is EUR 3.4M over 273 days. The workbooks have 68 named ranges, two line charts, and no cached errors. Arial is used throughout. Every calculation tab is formulas only. The only hardcoded numbers are on Inputs, Validation (check numbers and tolerances) and the Notes regression table. The base-case numbers recompute correctly. The values below are the receivable proceeds at spot, and the Option row is the same figure after the future-valued put premium:

| Strategy |	USD proceeds |
|----------|---------------|
| No hedge |	3,797,358 |
| Forward |	3,997,040 |
| Money market |	3,843,847 |
| Option	| 3,718,564 |
Validation shows 35 PASS and 1 FAIL out of 36 checks.

- **What was wrong and how it was caught:** Legend contradicted the formatting. It said “Green text = formula calculated on this tab”, but the cells use black for same-tab formulas and green for cross-tab links. I caught it by comparing the legend text with the actual font colours cell by cell. The corrected file fixes the legend. The original also tracked forward points, which were omitted in the second. The parity check fails by 3.83% (about $153k), and that comes from the inputs rather than the formulas. The bank forward of 1.1756 is far above the covered-interest-parity forward of 1.1305. The workbook’s own parity check and the Validation tab flag it. The workbook leaves F0_in unchanged, documents the gap and recommends requesting a requote.
The tab is “Legend-Key” because Excel doesn’t allow “/” in sheet names.
