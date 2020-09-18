---
layout: post
title:      "Predicting Gold Price Using Univariate Time Series Modeling"
date:       2020-09-18 17:50:48 -0400
permalink:  predicting_gold_price_using_univariate_time_series_modeling
---





2020 has been a year of multiple uncertainties mainly driven by the onset and aftermath of the COVID-19 pandemic that has disrupted daily activity for nearly everybody in multiple ways. For one, the global economic impact of such a destructive force has been easily observed and well documented throughout this year. This includes, but is not limited to, a plethora businesses shuttering doors, millions of individuals out of work, unpaid rents, delinquent mortgages, negative gas prices, and bare store shelves. The list goes on.


<br>

The Federal Reserves response was to inject whatever amount of stimulus money into the system (trillions so far in this fiscal year alone) it would take to prop up a failing economy. That said, precious metals have been a historic safe haven for investors during times of increasing volatility and future uncertainty. Most notably, it provides a hedge against the debasement of fiat currency, while preserving the value of ones wealth.

<br>

As the Federal Reserve continues to dilute the value of the American Dollar, many are fleeing for alternative investments outside of the current system; this typically starts with the flight into gold and gold assets, then trickles down to silver along with other precious metals and hard assets. The influx of open interest has already been seen in 2020 with gold reaching all time nominal highs of over $2000/oz in August of this year. 

<br>

Is it possible to accurately predict the gold price one year into the future by just using a univarite time series model, specifically, ARIMA? Lets find out.

<br>

![](https://i.imgur.com/qVW4iXf.png)


<br>

The above image shows daily gold prices fron April of 1968 through August of 2020. Right off the bat, It's clear to see an upward trend in the data as well as periods of extreme volatility. Again, can this be captured through time series modeling, or does another technique need to be implemented?


<br>
**Data**
<br>

<br>
Gold prices are set twice a day (A.M. and P.M.) by the London Bullion Market and are based in nominal U.S. dollars starting from 1968 up to the present. Data for gold prices used in this project are located on the Federal Reserves website (https://fred.stlouisfed.org/series/GOLDAMGBD228NLBM).

<br>
<br>

**Exploration**

<br>

One assumption for time series modeling is that the data should have a Gaussian distribution for more accurate results. This can be visualized using various methods outlined below.

<br>

-KDE Plot-

![KDE](https://i.imgur.com/o5SfWhI.png)

<br>

An initial look at the kde plot reveals that distributions of gold price are clearly not Gaussian. In fact, there seems to be a bimodal distribution heavily skewed toward the left. This means that some type of transform should be performed on the data to ensure it fits with time series assumptions.

<br>

-Box Plot-

<br>

Grouping the data by year and visualizing the box plots may help get an idea of the spread of gold price annually and how it may be changing over time. 

<br>

![](https://i.imgur.com/PssuxQo.png)

<br>


Box plot observations reveal two major periods of high volatility and price variance between 1979-1982 and 2006-present. The majority of years show a very different signature of low volatility and variance. It's possible that prices prior to 2000 may not be beneficial for forecasting and can possibly be clipped prior to modeling.

<br>

-Lag Plot-

<br>


Time series modeling assumes a relationship between an observation and the previous observation. Previous observations in a time series are called lags, with the observation at the previous time step called lag1, the observation at two time steps ago lag=2, and so on. If the points cluster along a diagonal line from the bottom-left to the top-right of the plot, it suggests a positive correlation relationship.

<br>

![](https://i.imgur.com/sOh4YjX.png)

<br>

As expected, a quick look at the lag1 plot suggests a string, positive correlation between observations. This indicates some level of differencing must occur during the modeling process.


<br>

-Stationarity Check-

<br>

Time series modeling for accurate forecasting requires the data to be stationary. This is achieved when the statistical properties of mean, variance, and covariance remain constant over time. Stationarity can be tested in a variety of ways, however in this case the Dickey-Fuller test will be used. The null hypothesis for this test assumes that the provided data are not stationary. To reject the null hypothesis and confirm that the data are stationary, the test statistics must be less than the critical value of p < 0.05. 

<br>

![](https://i.imgur.com/zJLW26N.png)

<br>

![](https://i.imgur.com/wNqoBzF.png)

<br>

From the above graph, neither the mean nor the standard deviation are constant. Also, the p-value of the Dickey-Fuller test is well above the 0.05 threshold; this confirms that the gold data are not stationary, and as confirmed by the above graphs, must be transformed prior to modeling.

<br>

**Model**

<br>

-Baseline Model-

<br>

Prior to manipulating the data, or getting into analysis, it's good practice to establish a baseline level performance known as a naive forecast in time series modeling. This provides the lowest benchmark for future model evaluation. With this naive forecast, observations from the previous time step are used as the prediction for the next observed time step.

<br>

![](https://i.imgur.com/b9Ih5Qa.png)

<br>

The baseline model achieved an RMSE of 14.114. The lower the RMSE value (closer to 0), the better. Moving forward, achieving a value below 14.114 means a better model than the baseline has been established for forecasting gold prices.

<br>

-ARIMA Model-

<br>

ARIMA stands for AutoRegressive Integrated Moving Average.

<br>

*     The AR portion of the model uses the dependent relationship between an observation and some number of lagged observations and is denoted by the variable 'p'.
*     The MA model uses the dependency between an observation and a residual error from a moving average model applies to lagged observations and is denoted by the variable 'q'.
*     I uses the differencing of raw observations in order to make sure the time series is stationary. It is denoted by the variable 'd'.

<br>

The grid search process can be used to determine optimal (p, d, q) orders for an accurate time series forecast of future gold price. This is done by running through all of the possible model combinations for the provided variables eventually providing insight into the most favorable ARIMA model parameters.

<br>

The initial grid seach ran for far too long, so I ended up elimiating data prior to the year 2000 from here on out.

<br>


Grid searching on data after 2000, resulted in an ARIMA model with pdq values of (3, 1, 3) that have generated the most optimal modeling parameters with the given information.

<br>

**Forecast**

<br>

The data was split 75/25 into train/test sets for the ARIMA (3, 1, 3) model. Next, rerun the one-step ahead forecast with optimal pdq parameters to see if model performance has increased from the baseline model.  This resulted in a decrease in rmse by 3. Though, this is an improvement, it may not be statistically significant from the baseline model. Especially for the amount of time and computational power it took compared to the first iteration. Here are the visual results:

<br>

![](https://i.imgur.com/dWkgMX1.png)

<br>

The one-step ahead forecast aligns with the test values as seen in the above graph. However, it's difficult to see that the predictions are shifted one day into the future from the the observations. 

<br>

-Dynamic Forecast-

<br>

Looking deeper into the model will provide more insight into the true predictive power of the generated model. This can be achieved through dynamic forecasting where only information from the time series up to a certain point is used, and after that, forecasts are predicted using values from the previous forecast. 

<br>

![](https://i.imgur.com/VDM59g3.png)

<br>



Though the dynamic forecast captures the upward trend of gold prices between 2015 and today, the fluctuation and volatility seen in the test set is not captured.
		
<br>

This result could come from a few different issues:

<br>

*    The provided data have no associated detectable trend or seasonality -- lack of complexity
*    Inappropriate use of modeling parameters leading to a disruption in trend and seasonality detection
*    With only a single variable, ARIMA models have a hard time accounting for volatility
* 	 Undetected seasonal component to data due to data sampling interval

<br>

These issues will need to be addressed in subsequent model iterations to provide more accurate forecasting of gold price.
		
<br>

**One Year Forecast**

<br>

Although the predicted dynamic forecasts don't properly reflect the test set, it would still be appropriate to visualize the one year into the future forecast to get a general sense of where gold prices could go if it follows the same upward trend.

<br>

![](https://i.imgur.com/CvGHCbQ.png)

<br>

Again as expected, the gold price volatility is not captured and a straight line with an upward trend is projected one year into the future. However, the forecast isn't all for nothing, the confidence interval can provide insight into the upper and lower limits of future gold prices. These limits alone can help furnish the starting point for the extend of risk versus reward for near term gold investing.

<br>

**Conclusion**

<br>

If one were to invest a percentage of their portfolio in gold using the current forecast, the model projects a potential 1 year ROI of as high as 32.54% and a potential loss of -12.2%, with an average ROI of 10.17%. At first glance, these numbers seem to deliver promising returns, though it's important to remember volatility is not captured in this model. 

<br>

Computing power was a major limitation during the modeling process. This greatly increase modeling computational time and may have hindered the models ability to find the most optimal parameter orders for an accurate forecast. It's recommended to invest in more powerful hardware to decrease computational time while increasing the ability for to search more thoroughly for optimal modeling parameters.

<br>

The ARIMA modeling technique may not be the correct selection for this specific task. It had a difficult time predicting the volatility of gold price seen in more recent years. One possible solution is to try different models that specifically account for more variance in price action such as the Autoregressive Conditional Heteroskedasticity (ARCH) method. The ARCH model is specifically designed to take into account volatility by modeling residual error variance at each step in time.

<br>

The value of gold is widely known to be influenced by many factors including: inflation rates, U.S. dollar strength, investment demand, mine supply, central bank purchases, and overall economic sentiment. Forecasting gold may be too complex of a problem for univariate time series modeling. Therefore, switching to a multivariate time series method could more accurately predict volatility and capture the momentum of gold either to the up or the down side.








