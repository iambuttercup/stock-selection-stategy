+ selected variables: `ideas[['available','symbol','thesis']]`
  + **available** Time of idea publication on SumZero (Epoch UTC). 
    + date range: `[t,t+n]`, like `t = '2014-03-01'`, `n = 500`
    + date frequency: one day, `df.resample('1d')`
  + **symbol**: Ticker of the security, usually in Bloomberg form. 
    + stock range: `US stocks`, 5 
  + **Thesis:** Full text of idea thesis.
  + sentiment of suggestions/comments/likes
+ single-factor model: build the sentiment factor.
+ process the **ideas**(`ideas['thesis'],string`) into numeric data using sentiment analysis (`TextBlob`) and then get sentiment analysis score for each idea. 

+ classify data by **ticker**(`ideas['symbol']`) and **date** (`ideas['available']`) using the `'groupby'` function.
+ calculate sentiment factor exposure in `[t,t+n-split]`:
  + Select all ideas of one individual stock in the last 90 days(rolling windows), calculate sentiment factor exposure for each stock in each day using exponential weighted moving average(ewm).
+ split the data and build a time series model: 
  + `X`: sentiment analysis score in `t`
  + `y`: one stock's return in `t+1`, `yfinance`
+ train the model(i.e. linear regression) and get the factor return for each stock in `[t+1, ,t+n-split]`
+ predict the stock return for each stock in `[t+n-split, t+n]` using the trained model.
+ calculate alpha and select the stocks that has the minium alpha and alpha < 0.
  + `alpha = ((stock return-market return) in [t+n-split, t+n]).mean()`
+ design strategy:
  + `alpha_t > 0`, short
  + `alpha_t < 0`, long
+ backtest: calculate the composite return in `[t+n-split, t+n]` and compare the result with S&P500's return(`yfinance`).

