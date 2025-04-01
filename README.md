# MarketNeutralSimulationPython
Independent Market Neutral Strategy Simulation programmed in a Jupyter Notebook in Python.

Independent Market Neutral Strategy Simulation to a S&P500 portfolio, using data of the last 12 years from Yahoo Finance, in Python requiring the use of both Pandas and NumPy. I cleaned and then manipulated the data to determine which stocks to long or short daily, depending on their performance over a period of 30 days. Presented data using graphical visualisation and performance metrics. Programmed in a Jupyter Notebook.


Design and Approach of Market-Neutral Mean Reversion Strategy: 
Collecting and preparing data: 
1)	In order to select 1500 US Stocks I found the tickers of the stocks from the S&P 1500 index, which combines the S&P 500 (large-cap), S&P 400 (mid-cap), and S&P 600 (small-cap) from wikipedia. 
2)	I Used the Yahoo Finance API through the yfinance library to retrieve the closing prices over a 12-year period. The yahoo finance library mentioned in documentation seems to be abandoned, so I used yfinance library https://yfinance-python.org instead. 
3)	I then structured the closing prices against each date in a 2D NumPy array – any stocks with missing data was removed. 
Mean Reversion Algorithm: 
1)	First I had to understand the strategy that prices tend to return to average levels over a period of time – which I decided to make 30 days as this is a sufficient length for mid-frequency/daily trading 
2)	I used my knowledge of Statistics in A-Level Mathematics to implement the z-score of each datapoint in the 2D array using the formula z=x-µ/σ, where x is the closing price, mu is the 30-day moving average of closing prices and sigma is the 30 day standard deviation in closing prices. This provides a quantifiable way of comparing the performance of each stock and has no forward bias as it only uses past data. 
3)	The z-score was used as it avoids forward bias, has high computational efficiency but can also scale all assets and can be used across different price ranges so is a very adaptable method to use. However, it can be slightly sensitive to extremities. Bollinger was another method to consider however it cannot easily compare stocks of differing prices and is less volatile as it lags in trends. 
Constructing my Portfolio: 
1)	I now had to decide how to structure my portfolio out of the 1500 stocks that I had selected. 
2)	Every day we rank each stock by their z-score, we then go long(buy) on the bottom 150 stocks and go short(sell) on the top 150 stocks. This is because we are suggesting that higher z-scores mean they have been overvalued in the last 30 days and by the mean reversion strategy are likely to fall. 
3)	I chose to sell the top 10% and buy the bottom 10% of stocks as mean reversion strategy suggests that extremes are hard to maintain whereas the middle ground could be feasible growth. 
4)	One form of market neutrality is keeping the portfolio dollar-neutral, therefore if we buy long and short the bottom and top 150 stocks with a dollar value(position size) of $1000 each then we can drive the portfolio performance regardless of the market moving up or down. 
5)	We then update the rankings daily and hold the same stocks intraday. 6) Portfolio has limitations: 
a)	Asset indivisibility 
b)	Intraday market volatility 
c)	Low liquidity 
d)	M&A 
PnL Calculation: 
1)	In order to calculate profit and loss we have to use the price change for each stock and yesterday’s number of shares(positions) in order to avoid forward bias. 
2)	We then calculate the daily PnL by doing the shifted position x the difference in price for each stock and then sum in order to calculate total PnL for the portfolio. 
3)	PnL ignores:  
a)	Transactions costs like brokerage, stamp duty, leverage interest etc. 
b)	Dividends, in long positions stocks will receive dividends, while in short positions shorts will give dividends 
Performance Metrics: 
1)	In order to determine how well our portfolio has performed I used some important metrics such as Sharpe ratio, Annualised returns and maximum drawdown. 
2)	First I calculated the daily returns by using my daily PnL/ Gross Exposure and made sure to remove any NaN entries from this calculation to avoid errors in the code. 
3)	The simulation’s annualised returns were calculated by doing the mean daily returns x 252 trading days per year. 
4)	To find my sharpe ratio, I assumed a risk free rate of 0 when using the formula mean daily returns/standard deviation of daily returns x sqrt(252) - as the risk free rate is dependent on macroeconomic factors which we have assumed to be a constant value of 0 for the period of the simulation. 
5)	To calculate my cumulative returns until a certain point in time I used the cumprod function in NumPy. 
6)	In order to calculate the maximum drawdown, I used the cumulative wealth at that point in time and stored the highest wealth seen so far (looking back on past events) as the running maximum. We then calculate the running max - wealth/running max in order to find the drawdown, which we wish to minimise as this is the worst case loss. 
7)	The sharpe ratio and drawdown were key in understanding how my returns fared in context of the risk taken which is a key factor to consider. 
Plotting and Visualisation of PnL: 
1)	In order to display the strategy’s performance I plotted a graph of cumulative returns against time. 
2)	I used the matplotlib.pyplot library in order to plot an easy on the eyes graph. 
3)	This helps display the strategies overall growth over time while also evidencing any areas of high over/under performance very clearly.  
  
 
Evaluation: 
While the annualized returns and maximum drawdown are sufficient, the sharpe ratio is slightly low, demonstrating that this programmed simulation may carry higher risk due to the high standard deviation in daily returns meaning there is high volatility in the portfolio. 
Improvements: 
1)	Incorporate volatility 
2)	Incorporate sector information in stock selection 
3)	Machine learning algorithms to optimise parameter selection for better prediction of future returns. Use z-score, momentum, and volatility features in ML Model 
4)	Incorporate fundamental data for stock selection 
5)	Use data from a source other than Yahoo to verify the Strategy 
6)	Effect of varying moving average window from 30 to something else on performance metrics or adaptive window based on volatility. 
7)	Effect of varying number of stocks from 150 to something else on performance metrics 
8)	Adding a filter so that extremely low volume stocks are not traded. Or Volume-Weighted Z-Scores 
9)	Incorporate means to rate limit and retry data collection 
10)	Incorporate transaction costs and dividends in PnL 
11)	Momentum-Filtered Mean Reversion 
 
