**Stage 4 Live Market Data**

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

**The CIP-implied forward**
- Equation: F0_CIP = S0 × (1 + R_USD × T/360) / (1 + R_FC × T/360)
- Rate: 1.1300 USD per FC
- Forward hedge payable: FC_AMT * F0_CIP = 3400000 * 1.1300 = 3842000
- Difference from rate F0_in: -0.0456
- Difference from forward hedge payable: 155040 USD

**Workbook results (S_T = S_0)** 
|        Name         |    Value   | 
|---------------------|------------|
| Unhedged            | $3,795,760 |
| Forward hedge       | $3,997,040 |
| Money Market hedge  | $3,842,229 |
| Put hedge           | $3,716,966 |
| Implied forward     | 1.1301     |
| Bank quoted forward | 1.1756     |

**Lab results (S_T = S_0)**
|        Name         |    Value   | 
|---------------------|------------|
| Unhedged            | $3,795,760 |
| Forward hedge       | $3,997,040 |
| Money Market hedge  | $3,842,229 |
| Put hedge           | $3,719,600 |
| Implied forward     | 1.1301     |
| Bank quoted forward | 1.1756     |


**Workbook checks**

24 out of 29 validations were correct and the table was repopulated correctly using the new data. All the data within the sensitivity graph was compared to the FX hedging lab calculator, and all values correctly matched (S_T = S_0) modeled here. The only validations that weren't correct were the parity check and 4 of the base case validations against 5.1. The latter is to be expected because the workbook inputs were changed to the live market values. They no longer match the base case values and therefore the check fails. It is not a result of the workbook failing, just a sign that the data was changed. 

For the parity check, the relative distance between the money market and the forward hedge was 3.87%, calculated in B21 of Money Market Hedge. This is much higher than the accepted threshold of 0.05%, which the parity check correctly flags. Calculating the forward hedge and the money market hedge manually gives a difference of 154,811.01, a percentage difference of 3.87%, which are both correctly calculated in the workbook. This is correctly reflected in the lab, which cites a difference of $154,811, the same as the hand calculated data. This suggests not an issue with the workbook itself, but with the bank proposed forward hedge. This works to our benefit, as the quote would receive more money than the money market hedge. 

The lab results and the workbook results match except for the put hedge, which differ by 2634. Checking the math of the lab, this is because it takes the premium of the put at face value, and then subtracts it from the spot induced value. This differs from the specification, which takes the future value of the premium, which multiplies by the basis, (1 + R_USD * DAYS/BASIS). This causes the premium to increase by the difference, $2634. In this scenario we assume that the premium will be based on the future value of the premium, so the difference is just a difference in algorithm. 
