A reproducible FX hedge model for a €3.4 million receivable due in 273 days.

The model compares four outcomes across a range of future EUR/USD rates:

- No hedge: proceeds change with the exchange rate.
- Forward hedge: locks in a USD amount.
- Money-market hedge: uses foreign-currency borrowing and USD investment; its result should approximately match the forward under covered interest-rate parity.
- Put-option hedge: establishes a minimum exchange rate while preserving upside, after accounting for the option premium.

The specification defines standardized inputs and calculation steps, plus a sensitivity table and chart, hedge-profit comparisons, summary metrics, and audit checks. It also outlines how to adapt the model for a foreign-currency payable, where the option is a call rather than a put.

The model assumes a full, static hedge and excludes transaction costs, credit risk, taxes, hedge accounting, and probability-based or volatility-based option pricing. Its market inputs are examples that the spec says should be replaced with live data.
