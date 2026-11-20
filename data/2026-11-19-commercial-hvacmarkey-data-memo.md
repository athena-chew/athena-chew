**Stage 4 Live Market Data**

|  Name     |  Value  |  Source       |  Retrieved on              |  Computation Used      |
|-----------|---------|---------------|----------------------------|------------------------|
| S0_in     | 1.1164  | investing.com | 2026-11-10 16:00:00 ET     | Market close spot rate |
| R_USD     | 4.56%   | H.15          | 2026 - 11 -10 16:00:00 ET  | 1-year treasury bills are used, since they reflect risk-free and is purely based on yield curve. |
| R_FC      | 2.91%   | ECB           | 2026 - 11 - 10 16:00:00 ET | 1-year AAA euro-area yield curve is used, since it reflects the most stable central governments of the euro-area, making it nearly risk-free. 
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

**Workbook checks**
10 of the 12 validations were correct and the table was repopulared correctly using the new data. The only validations that weren't correct were the EUR borrowing fully repaid validation and the parity within 0.05% validation. The EUR borrowing was solely because the formula was selecting B13 of the money market hedge tab, which corresponded to the completed check, and then subtracting from the original recievable. This step already does this check, so adding the extra subtraction made the check incorrect. Instead, I changed the formula so that it would just take the final check at face value.


