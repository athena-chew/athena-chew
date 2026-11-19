Audit Notes

- The AI created its own system for coloring texts and text boxes, yellow fill blue text for inputs, blue fill blue text for assumptions, etc, all defined in cells A5-C9 on the "Legend/Key" tab. Changed the spec to specifically define the color of the text box and what it should be used for: Yellow fill = inputs · Blue fill = assumptions · Green fill = formulas · Gray fill = outputs, applied throughout.
  
- The AI marked the basis, parity check, and S_T distribution as assumed in cell C11, C19, and C19/20. It notes correctly where that assumption takes place and only marks it because it is not explicit. The basis was checked and confirmed to match 360 for both USD and EUR. The Parity check was confirmed to be the part 4 step 3 assumption of 0.05%. The sensitivity correctly showed 5 positive and negative steps and in 1% increments and correctly showed all 11 scenarios in the sensistivity tab.

- The AI added forward points (PIPS) into the forward hedge and money market hedge in B12/13 and B23, respectively in their own tabs. This was not a measure defined within the spec to track. It was explicitly told to be left out of the spec and to not calculate it at any time within the sheet.
  
- The money market does fail the parity check, but this was the case with the pre-check table as well. The calculated value shown of $3,843,847 in B16 of the money market tab does match the one in the spec, and the difference that is calculated within the sheet is also equal to the one that is produced when subtracting the two values within the specification check. The issue seems to be with the money market quote itself, which the notebook also mentions in the validation tab.
  
- All textboxes and equations are entered correctly. Checked by changing a variable within the inputs, and checked each section to see what is different. Further validated by going through each of the 511 equations and making sure they matched the equation that is listed beside it, which also matched the specification equation for that section. All 511 equations changed and correctly pointed to a value gained from a named value or from an explicit value defined in the spec.
 
- All other validation besides the parity check from earlier is shown in the validation tab to work and be correct throughout the spreadsheet. I went through all of the checks that it uses and confirmed that it correctly takes the value and applies it to the check that it defines. I also checked that all definitions matched to those within the specification. All 35 validation checks other than the parity check is correct, and the parity check is correctly stated to be because of the forward bank quote, not because of the calculation, which matches the specification. This was further validated by checking each individual value all the final calculated forward and money market hedge, which matched the spec. 

- Changed the fixes above into the spec and then fed it into ChatGPT for a revised notebook.

- On second pass the AI fixed all issues above except for parity, due to the same reason as above.
