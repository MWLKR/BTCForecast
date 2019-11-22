# BTCForecast
Bitcoin Forecasting Using SARIMA
By Hansaka Wijaya (1706985962), Muhammad Adisatriyo (1706986031) & Hilman Maulana (1706985975)
* [Description](#description)
* [Overview](#overview)
* [License](#license)
* [Power Point Presentation](https://github.com/MWLKR/BTCForecast/raw/master/Forecasting%20Bitcoin%20Price%20with%20ARIMA%20Model.pptx) 

## Description

This is a sample project for Bitcoin Price Forecasting using SARIMA method. Source code and data can be downloaded through this github repo above.

Seasonal ARIMA is an ARIMA model that contains seasonal factors. Season means that the data has a tendency to repeat the pattern of behavior in the season period. Usually it can be weekly, monthly, quarterly, semester. For example, seasonal one year for monthly data. Therefore, the seasonal time series has characteristics that are indicated by the strong continuous correlation in the seasonal distance (season period), that is, the time associated with many observations on the season period.

There are three trend elements that require configuration.

They are the same as the ARIMA model,specifically:

p - the number of lag observations to include in the model, or lag order. (AR)
d - the number of times that the raw observations are differenced, or the degree of differencing. (I)
q - the size of the moving average window, also called the order of moving average.(MA)

The Seasonal Autoregressive Integrated Moving Average (SARIMA) model is a development of the ARIMA model. The following are the seasonal steps for SARIMA: 

1. Identify seasonal data using ACF / PACF in seasonal lags. 
2. Perform differencing of data in accordance with the season taken differencing on season is used to eliminate seasonality because there is a possibility that the data used requires differencing.


## Overview

### Stationarity check and Seasonal decomposition
If a timeseries is stationary, it implies the lack of broad trends (changes in mean and variance over time) in the data. This is important as a consideration in time series forecasting.

The statsmodels library provides a seasonal_decompose method, an implementation of the naive decomposition method, which we will use to examine our timeseries.
![1st](https://user-images.githubusercontent.com/36689886/69425478-8738fd00-0d5d-11ea-8d5c-4c7181a190c7.png)

The p-value indicates that series is not stationary.

Most interesting is the seasonal component which was extracted, which looks like it repeats itself on a yearly basis.

We can also see that the residuals are fairly constant until around the start of 2017.

### Box-Cox Transformation
We use the Box-Cox transformation to suppress some of the variance.

The Box-Cox transformation is a family of power transformations indexed by a parameter lambda. Whenever you use it the parameter needs to be estimated from the data. In time series the process could have a non-constant variance. if the variance changes with time the process is nonstationary. It is often desirable to transform a time series to make it stationary. Sometimes after applying Box-Cox with a particular value of lambda the process may look stationary. It is sometimes possible that even if after applying the Box-Cox transformation the series does not appear to be stationary, diagnostics from ARIMA modeling can then be used to decide if differencing or seasonal differencing might be useful to to remove polynomial trends or seasonal trends respectively. After that the result might be an ARMA model that is stationary. If diagnostics confirm the orders p an q for the ARMA model, the AR and MA parameters can then be estimated.
![2nd](https://user-images.githubusercontent.com/36689886/69425583-c6ffe480-0d5d-11ea-8fbd-5dfdb3890b75.png)
The p-value indicates that series is still not stationary.

### Differencing
When building models to forecast time series data (like ARIMA), another pre-processing step is differencing the data until we get to a point where the series is stationary. Models account for oscillations but not for trends, and therefore, accounting for trends by differencing allows us to use the models that account for oscillations.

One method of differencing data is seasonal differencing, which involves computing the difference between an observation and the corresponding observation in the previous year.
![3rd](https://user-images.githubusercontent.com/36689886/69425632-e3038600-0d5d-11ea-9fce-8f99727aa329.png)
The p-value indicates that series is still not stationary.

### Regular differentiation
Sometimes it may be necessary to difference the data a second time to obtain a stationary time series, which is referred to as second order differencing.
![4th](https://user-images.githubusercontent.com/36689886/69425744-17774200-0d5e-11ea-946c-663c540b4ae9.png)
The p-value indicates that series is stationary as the computed p-value is lower than the significance level alpha = 0.05.

### Autocorrelation
Autocorrelation is the correlation of a time series with the same time series lagged. It summarizes the strength of a relationship with an observation in a time series with observations at prior time steps.

We create autocorrelation factor (ACF) and partial autocorrelation factor (PACF) plots to identify patterns in the above data which is stationary on both mean and variance. The idea is to identify presence of AR and MA components in the residuals.

![5th](https://user-images.githubusercontent.com/36689886/69425941-82c11400-0d5e-11ea-948d-6c518ae4977e.png)

![6th](https://user-images.githubusercontent.com/36689886/69425988-a5532d00-0d5e-11ea-85ec-b3f773bb61d6.png)

### ARIMA Model
We will iteratively explore different combinations of parameters. For each combination we fit a new ARIMA model with SARIMAX() and assess its overall quality.

We will use the AIC (Akaike Information Criterion) value, returned with ARIMA models fitted using statsmodels. The AIC measures how well a model fits the data while taking into account the overall complexity of the model. A model that fits the data very well while using lots of features will be assigned a larger AIC score than a model that uses fewer features to achieve the same goodness-of-fit. Therefore, we are interested in finding the model that yields the lowest AIC value.

![7th](https://user-images.githubusercontent.com/36689886/69426198-42ae6100-0d5f-11ea-99df-6f250b7b6c66.png)

### Analysis of Results
The coef column shows the weight (i.e. importance) of each feature and how each one impacts the time series. The P>|z| column informs us of the significance of each feature weight. Here, each weight has a p-value lower or close to 0.05, so it is reasonable to retain all of them in our model.

![8th](https://user-images.githubusercontent.com/36689886/69426410-c9633e00-0d5f-11ea-9ae7-de025e412afb.png)

### Monthly Prediction
The prediction starts from 31/10/2015 to 31/7/2020. As we can see, in 2020 the price of the bitcoin is forecasted to be raising.
![8th](https://user-images.githubusercontent.com/36689886/69426569-2232d680-0d60-11ea-8cb7-6ffd74515f35.png)



## License

MIT License

Copyright (c) 2019 MWLKR 

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

The software is provided "as is", without warranty of any kind, express or
Implied, including but not limited to the warranties of merchantability,
Fitness for a particular purpose and noninfringement. In no event shall the
Authors or copyright holders be liable for any claim, damages or other
Liability, whether in an action of contract, tort or otherwise, arising from,
Out of or in connection with the software or the use or other dealings in the
Software.

