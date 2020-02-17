---
layout: post
title: Regression & Time Series: Domestic Flight Data

---

*an overview of my linear regression & time series models on predicting flight delay.

-----

### Project Scope

For a project on linear regression, I chose to predict departure delay for flights. As a frequent traveler, I was interested in learning about trends in delays. Unfortunately the data available was only for domestic flights. This data came from Department of Transportation data available for download. My data analysis looks at all flights with an origin at SFO, LAX, and SJC airports in the timeframe of 2017 through 2018.


### Data Cleaning

The initial metrics that I included in the download were carrier, time, origin, month, day, departure delay, and arrival delay. Upon seeing that there was a direct linear relationship between arrival and departure delay, I chose to eliminate arrival delay and just focus on departure delay. Not only is this because of the linear relationship, but also because the flight would not know their arrival delay prior to departure, so it is not a valid statistic to use the future.

I found nulls in the data set where not all flight carriers were leaving from my chosen origin airports (SFO, LAX, and SJC). I dropped those carriers and ended up with 9 flight carriers for my analysis.

### Methodology 

Initially, I modeled this data with linear regression. I transformed the training data and predicted the test data. Following the linear regression I used time series analysis to explore trends in the data. Lastly, I modeled the data with an ARIMA model.

#### Linear Regression

In order to use Linear Regression modeling I had to make sure my data had only continuous variables. I did this by eliminating categorical variables and replacing them with dummy variables. This was done with: carrier, day of week, and origin airport.

Putting my raw data into the linear regression model, I got an R^^2 score of 0.012 which is indicative of a model not better than guessing the mean. In addition to a low R^^2 score, I also found low p-values, low coefficients, and high multicollinearity. Great. Not what is needed for an accurate prediction.

So, to combat some of these metrics, I fit and transformed my train data with standard scaler. Using lasso and ridge regression models did not reveal better results.

Lasso regression can help with feature selection because it changes the regularization parameter and reduces coefficients to zero for those that don't affect the dependent target variable.

Ridge regression penalizes the model for number of variables and multicollinearity.

Neither of these regularization methods made any significant difference in the R^^2 value nor the coefficients.

#### Time Series Modeling

Taking into consideration my features, my model is more of a candidate for time series modeling than linear regression. For my simple time series model, I need a dataframe with just my target variable and the date. I dropped all other exogenous variables except for departure delay. Next, I set my date column as the index.

My initial plot of the data at the daily level showed a lot of variance in delay time. This high variance is difficult to predict, so attempting to smooth the data is the next step. I tried resampling the data at the weekly, monthly, and even hourly level. Trying all three of these resampling periods allowed me to see that there was too much noise at the hourly level, but not enough data to really see a pattern in 2 years of monthly data. So, I chose weekly data.

Modeling this data allowed me to pick up on peaks and valleys in the months of August and September.

After initial analysis, I put my weekly sampled data into the SARIMA model and adjusted the parameters for autocorrelation. Ultimately, the model with the lowest AIC score was a weekly model with not much seasonal lag. This means it was essentially an ARIMA model.

My final prediction model received a Mean Absolute Error of 4.85 minutes. While the graph didn't look excellent, a Mean Absolute Error of 4.85 minutes is tolerable to me when calculating flight delay.

### Results & Recommendations

My final recommendations were to fly in the month of September with Alaska from San Jose airport. These are preliminary results as I would like to take further steps and create more narrow models. 

This current model for all 3 airports with 9 carriers had a lot of variance and I was not able to create a very accurate prediction. Through this data set I found that the least delayed airport was San Jose Airport. For my next model, I would like to focus only on San Jose airport and extend my time period analyzed.

-----

Learn more and see our code on [GitHub]({{ site.github.repo }}).
