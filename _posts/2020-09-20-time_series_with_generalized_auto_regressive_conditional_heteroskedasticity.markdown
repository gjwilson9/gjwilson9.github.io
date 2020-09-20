---
layout: post
title:      "Time Series with Generalized Auto Regressive Conditional Heteroskedasticity"
date:       2020-09-20 15:17:52 -0400
permalink:  time_series_with_generalized_auto_regressive_conditional_heteroskedasticity
---


Generalized Auto Regressive Conditional Heteroskedasticity (GARCH) is a time series technique that aims to model volatility within a data set. This is achieved by modeling the residual error using the autoregressive moving average process.
<br>
GARCH models are often used in the financial sector to analyze volatility in various data types such as stocks, bonds, and market indicies. Financial institutions use the resulting inforamation to help with price predictions, return on investment estimations, asset allocation, hedging, risk management, and portfolio opimtimization.
<br>
GARCH models are used when the data show nonstationarity in the residual error term, which is to say the error is heteroskedastic. Heteroskedasticity is defined by an irregular pattern of a variable in a statistical model. Meaning there is no linear pattern to the observed data. Therefore, GARCH models work under the assumption that variance of the error term vary sytematically, conditional on the average size of the error terms in previous periods. So, the model generates future volatility as a function of an average of its own past error values.
<br>
Recently, while attempting to forecast future gold prices, I came across a problem with my ARIMA model's inability to capture volatility of the data. This is when I discovered that typical univatiate time series with an autoregressive component do not model change in the variance over time. A time series with modest changes in variance can sometimes be adjusted with a power transform, with a log or Box-Cox transform, but this won't be sufficient with data showing higher volatility such as with gold price. 
<br>
![](https://i.imgur.com/VDM59g3.png)
<br>
In the image above, we see that the ARIMA model was able to account for the overall upward trend of the gold price, but again, did not correctly model the observed volatility. This is where a GARCH model would be more appropriate.
<br>
Configuring a GARCH model is best understood in the context of ACF and PACF plots of the variance of the time series. This is done by subtracting the mean from each observation in the series and squaring the result. The ACF and PACF plots assist in estimating values for the p and q parameters of the model, similarly to what is done for a typical ARIMA model.
<br>
I will work through a GARCH model in the near future using the gold price data seen in the graph above, then provide an update comparing the accuracy of the model forecasts between ARIMA and GARCH.
