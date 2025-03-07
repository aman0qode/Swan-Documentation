#Nifty Correlation

Objective: To exploit the correlation between Nifty and it's constituents. We go long both calls and puts of the index(monthly expiry) and short both calls and puts of the stocks. Ideally, with perfect correlation, the stocks should run up or fall equally with the index, in which case both our longs and shorts will make money. It is a delta neutral strategy. We trade the Implied correlation between the index and it's constituents.

Implied Correlation is the correlation between the implied volatilities of index options and the weighted implied volatilities of options on the index components. When the ratio is low there is low correlation between the index and it's constituents and when the ratio is high there is high correlation between the index and it's constituents. Ideally, we want the ratio to be low when we are opening positions




#Backtest:

Data used: 
- EoD options data for Nifty and it's constituents from 2016 to 2021.
- Expiry dates
- Stock weights

Strike Selection: We have used strikes closest to the delta. due to inavailablity of data/low liquidity in some stocks)

Leverage=10x

Position Sizing: 
- In this backtest, we have assigned quantities based on the market caps of the stocks, using minimum share logic. 
- Index exposure is calculated by dividing MinShareExposure  by MinShareValue. Margin required to open 1 unit is calculated, which is then 
- Index exp= minShareExp/MinShareValue
- Individual stock exposure= Index exp/stock weight
- Individual stock qty=Individual stock exposure/ Equity close of that stock
- Index exposure and total stock exposure will be the same
- The margin required for 1 unit will be Index exposure + total stock exposure
- No of units= equity available/margin required for 1 unit


Data holes have been filled with the last available values for that month.
On Expiry day, intrinsic value has been calculated and used for in the money options and for out of the money options, option price is set to 0.05 .  

Entry: First day of the month
Exit: Monthly expiry
Adjustments: None
Optimisations: The backtest was run for all deltas in the range of 5 to 50.
