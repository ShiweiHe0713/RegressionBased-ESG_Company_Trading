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
### Initial exploration
First, I used the simplest model `linear regression`  to predict the next day's price. The features used to train are 'AAPL_Volume(M)', 'AAPL_SMAVG15(M)', 'SPESG', the result are: 
- Coefficients:  [[0.01268778 0.00806344 0.68694972]]
- Variance score: 0.8772854909543317

And I added the last day's price 'AAPL_Px_Shift1' as a feature, the result are:
- Coefficients:  [[-0.00211847  0.00110984  0.01333341  0.98013466]]
- Variance score: 0.9970315820445652

We can see a significant improvement in the variance score, which means the model is more accurate, and the coefficient of 'AAPL_Px_Shift1' is 0.98, which means the last day's price is the most important feature to predict the next day's price. Now it seems that the model is overfitting, so I will try to use a more complex model to predict the next day's price.

After getting such a high score on R squared, I realized somthing is really off, I dug back and found out that I shouldn't predict close price, but instead, I should predict returns. So I change the prediction target to daily returns and retrained the model, then I got a R squred close to 0 on the training set, and negative on a test set. Now I know I'm slightly on the track, but obviously, the model is too simple to predict anything, and my features are not strong enough to predict the one day forward return. 

So I created ret5, ret10, ret30, ret60, ret120, and ret250 as features as well, trying to add more complexity to the model. This time after retrain the linear regression using these features, the R squred value on the training set increased very slightly, but R squred value on the test set become more negative.

Right now, I know I should increase model complexity, experimenting with different models, dealing with outliers, doing cross-validation, using regularization to avoid over-fitting, etc., to both increase the model complexity and have a more accurate prediction.

### Differnt models
|    | R^2 Training set | R^2 Test set | 
|----| -----------------| -------------|
|Logistic regression | 0.06 | -0.09 | 
|Bayesian regression | 0.015 | 0.00 | 
|Decision Tree(depth20) | 0.973 | -1.116|

**We can conclude that the model overfits severly by the metrics given by decision tree, the model has learned noise and specific patterns in the training data that do not generalize to new data. So I will use ensemble models like random forest or XG Boost, etc to reduce the overfitting issue, and use cross-validation to replace train-test split.**

### Random Forest
Cheerfully, after fitting with random forest, the r2 squared values become: r2_train: 0.846, r2_test: -0.118. Which means the model come predict on the training set pretty decently, but it is still over-fits. Even though the model is still very over-fitting, now at least I know the ensemble methods work much better than any other base models on the training sets.


# Terms
- What is R squred value? What does it indicate?
- Does alpha and equity graph apply here?

# Dicussion
- Do companies with high ESG scores are more likely to outperform the market in the long run?
