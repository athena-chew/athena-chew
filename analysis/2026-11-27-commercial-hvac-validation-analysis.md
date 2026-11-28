**Stage 4 Live Market Data (taken from 2026-11-19-commercial-hvac-market-data-memo.md)**

|  Name     |  Value  |  Source       |  Retrieved on              |  Computation Used      |
|-----------|---------|---------------|----------------------------|------------------------|
| S0_in     | 1.1164  | investing.com | 2026-11-10 16:00:00 ET     | Market close spot rate |
| R_USD     | 4.56%   | H.15          | 2026-11-10 16:00:00 ET     | 1-year treasury bills are used, since they reflect risk-free and is purely based on yield curve. |
| R_FC      | 2.91%   | ECB           | 2026-11-10 16:00:00 ET     | 1-year AAA euro-area yield curve is used, since it reflects the most stable central governments of the euro-area, making it nearly risk-free. 
| F0_in     | 1.1756  | Given         | -                          | Given by bank          |
| F0_CIP    | 1.1300  | Calculated    | -                          | S0 × (1 + R_USD × T/360) / (1 + R_FC × T/360) | 
| K_PUT     | 1.1164  | S0_in         | -                          | Strikes at spot        |
| K_CALL    | 1.1164  | S0_in         | -                          | Strikes at spot        |
| PREM_PUT  | 0.0224  | Given         | -                          | Given by scenario (retail-accessible option quotes are unreliable) |
| PREM_CALL | 0.0376  | Given         | -                          | Given by scenario (retail-accessible option quotes are unreliable) |
| FC_AMT    | 3400000 | Given         | -                          | Given by scenario      |
| T_DAYS    | 273     | Given         | -                          | Given by scenario      | 

**LLM vs Workbook Comparison Table**
Spec and market data memo fed to a fresh LLM conversation. Results listed below
|    S_T = -3%   |  No Hedge  | Forward Hedge |   MM Hedge | Put Option | Call Option |
|----------------|------------|---------------|------------|------------|-------------|
|    LLM Value   | $3,681,887 |  $3,997,040   | $3,842,229 | $3,716,966 | 3,814,148	  |
| Workbook Value | $3,681,887 |  $3,997,040   | $3,842,229 | $3,716,966 | 3,814,148	  |

|    S_T = 0%    |  No Hedge  | Forward Hedge |   MM Hedge | Put Option | Call Option |
|----------------|------------|---------------|------------|------------|-------------|
|    LLM Value   | $3,795,760 |  $3,997,040   | $3,842,229 | $3,716,966 | 3,928,021   |
| Workbook Value | $3,795,760 |  $3,997,040   | $3,842,229 | $3,716,966 | 3,928,021   |

|    S_T = +3%   |  No Hedge  | Forward Hedge |   MM Hedge | Put Option | Call Option |
|----------------|------------|---------------|------------|------------|-------------|
|    LLM Value   | $3,909,633 |  $3,997,040   | $3,842,229 | $3,830,839 | 3,928,021	  |
| Workbook Value | $3,909,633 |  $3,997,040   | $3,842,229 | $3,830,839 | 3,928,021	  |

All values match correctly between the fresh LLM and the workbook's values. LLM recommends **Forward Hedge** due to it being most money earned among all of the possible outcomes. 

**Hand Verification Table**

Calculated by hand

*Unhedged*

USD Proceeds(-2) = F_AMT * S_T = 3400000 * 1.0940 = 3719845 - For S_T = -2% S_0

USD Proceeds(-1) = F_AMT * S_T = 3400000 * 1.1052 = 3757802 - For S_T = -1% S_0

USD Proceeds(0) = F_AMT * S_T = 3400000 * 1.1164 = 3795760 - For S_T = +0% S_0

USD Proceeds(+1) = F_AMT * S_T = 3400000 * 1.1276 = 3833718 - For S_T = +1% S_0

USD Proceeds(+2) = F_AMT * S_T = 3400000 * 1.1387 = 3871675 - For S_T = +2% S_0

*Forward Hedge* 

USD Proceeds = F_AMT * F0_in = 3,400,000 * 1.1756 = $3,997,040

Forward hedge is locked in across all sensitivity, so this number spans across the whole table.

*Money Market Hedge*

Borrow FC, Convert at spot, Invest in USD

Borrow = FC_AMT / (1 + R_FC * T_DAYS/360) = 3400000 / (1 + 0.0291 * 273/360) = 3326590.46

USD_now = Borrow * S0_in = 3326590.46 * 1.1164 = 3713805.60

USD Proceeds = USD_now * (1 + R_USD * T_DAYS/360) = 3713805.60 * (1 + 0.0456 * 273/360) = $3,842,229

Money Market is locked in across all sensitivity, so this number spans across the whole table.

*Put Option* 

DF_USD = 1 + R_USD * T_DAYS / BASIS = 1 + 0.0456 * 273/360 = 1.03458

FV_PREM_PUT = −PREM_PUT * FC_AMT * DF_USD = -0.0224 * 3400000 * 1.03458 = -78793.61

USD Proceeds(-2) = MAX(S_T, K_PUT) * FC_AMT + FV_PREM_PUT = MAX(1.0940, 1.1164) * 3400000 - 78793.61 = 3716966 - For S_T = -2% S_0

USD Proceeds(-1) = MAX(S_T, K_PUT) * FC_AMT + FV_PREM_PUT = MAX(1.1052, 1.1164) * 3400000 - 78793.61 = 3716966 - For S_T = -1% S_0

USD Proceeds(0) = MAX(S_T, K_PUT) * FC_AMT + FV_PREM_PUT = MAX(1.1164, 1.1164) * 3400000 - 78793.61 = 3716966 - For S_T = +0% S_0

USD Proceeds(+1) = MAX(S_T, K_PUT) * FC_AMT + FV_PREM_PUT = MAX(1.1276, 1.1164) * 3400000 - 78793.61 = 3755046 - For S_T = +1% S_0

USD Proceeds(+2) = MAX(S_T, K_PUT) * FC_AMT + FV_PREM_PUT = MAX(1.1387, 1.1164) * 3400000 - 78793.61 = 3792882 - For S_T = +2% S_0

| Sensitivity | No Hedge | Forward Hedge | MM Hedge | Put Option |
|-------------|----------|---------------|----------|------------|
| S_0 = -2%   | 3719845  | 3997040       | 3842229  | 3716966    |
| S_0 = -1%   | 3757802  | 3997040       | 3842229  | 3716966    |
| S_0 = 0%    | 3795760  | 3997040       | 3842229  | 3716966    |
| S_0 = +1%   | 3833718  | 3997040       | 3842229  | 3755046    |
| S_0 = +2%   | 3871675  | 3997040       | 3842229  | 3792882    |

