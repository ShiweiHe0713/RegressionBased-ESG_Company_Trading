# ESG Company Trading Strategy

**Training Input:** 3 year of S&P 500 ESG Index, AAPL'S equity Stock Prices 
**Output:** A model that can predict future prices

**Model input:** S&P 500 ESG Index of past {variable} period of time, AAPL'S equity Stock Prices for the past {var2} period of time
**Model Output:** Predicted AAPL'S equity Stock Prices for the next {var3} period of time

**Questions**
- How to predict the next day's price base on SPESG and return of past? 
- And that how big should the window be?
- How to test the result in a systematic way?
  - Draw the Equity Graph, and compore with test set. Calculate accuracy, precision, auc, etc.
  - Calcute the financial to indicators, sharpe ratio, max drawdown, alpha, etc
# Data Massaging

# Modeling
First, I used the simplest model `linear regression`  to predict the next day's price. The features used to train are 'AAPL_Volume(M)', 'AAPL_SMAVG15(M)', 'SPESG', the result are: 
- Coefficients:  [[0.01268778 0.00806344 0.68694972]]
- Variance score: 0.8772854909543317

And I added the last day's price 'AAPL_Px_Shift1' as a feature, the result are:
- Coefficients:  [[-0.00211847  0.00110984  0.01333341  0.98013466]]
- Variance score: 0.9970315820445652

We can see a significant improvement in the variance score, which means the model is more accurate, and the coefficient of 'AAPL_Px_Shift1' is 0.98, which means the last day's price is the most important feature to predict the next day's price. Now it seems that the model is overfitting, so I will try to use a more complex model to predict the next day's price.

# Dicussion
- Do companies with high ESG scores are more likely to outperform the market in the long run?
