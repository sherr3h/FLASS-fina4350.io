---
layout: post
title: Kalman Filter on Power Market Electricity Price Data
gh-repo: sherr3h/sherr3h.github.io
tags: [Kalman Filter, Financial Data, Time Series]
comments: true
---

I use Kalman Filter to forecast next-day electricity price in France and Belgium, with the exogenous variables being the next-day load forecast and the next-day generation forecast. The France's and Belgium's electricity market are both in the European Power Exchange and intertwined.

The problem formulation of Kalman Filter is in the [previous post](https://sherr3h.github.io/2023-01-28-Kalman-Filter-on-Electricity-Prices-and-GPS-Data/).

## Power Market Electricity Price Dataset

I found the dataset from [this paper](https://doi.org/10.1016/j.apenergy.2021.116983) [1]. It contains hourly electricity prices in the EPEX-BE and EPEX-FR market, the day-ahead electricity market in Belgium and France, from 09 Jan, 2011 to 31 Dec, 2016. 

**Features**: The paper suggests the day-ahead grid load forecast and day-ahead generation forecast in France are the best predictors for both France's and Belgium's day-ahead electricity from current literature. This is possibly because of the increasing level of European electricity market integration and the two countries' capacity. According to the 2016 IEA Energy Policies Review of France and Belgium [2][3], France had an estimated total suppy of 91.8 TWh renewable energy while Belgium had a total supply of 13.5 TWh. The feature selection process is worth revisting but I will use these variables for now. 

The electricity price data contains negative and zero prices as well as price spikes. Removing three outliers when the electricity price is over 1000 Euros per MWh, there're 52413 ovservations. (There're still quite a few outliers of very high and negative prices, but I want to see how Kalman Filter is affected by these data points.) The prices in the two markets have a positive correlation of 0.808, which suggests there might be relative value trading opportunities.

<img src="/img/EPEX_BE_FR.png" width="700" >

The two features, the day-ahead grid load forecast and day-ahead generation forecast in France, are available at the website of [RTE](https://www.services-rte.com/en/view-data-published-by-rte.html), France's Transmission System Operator. They exhibit cyclical patterns on levels of years, weeks, and days. The two features also have a high correlation of 0.908 over the period 2011-2016.

<img src="/img/LoadGeneration_2011_16.png" width="700" >

<img src="/img/LoadGeneration_2011.png" width="700" >

<img src="/img/LoadGeneration_2011Jan.png" width="700" >



## Task 1: Use France's Electricity Price to Predict Belgium's Day-Ahead Electricity Price


## Task 2: Use France's Day-Ahead Generation Forecast to Predict France's and Belgium's Day-Ahead Electricity Price



## References

1. Jesus Lago, Grzegorz Marcjasz, Bart De Schutter, Rafał Weron, Forecasting day-ahead electricity prices: A review of state-of-the-art algorithms, best practices and an open-access benchmark, Applied Energy, Volume 293, 2021, 116983, ISSN 0306-2619, https://doi.org/10.1016/j.apenergy.2021.116983.

2. Energy Policies of IEA Countries: France 2016 Review. https://www.iea.org/reports/energy-policies-of-iea-countries-france-2016-review.

3. Energy Policies of IEA Countries: Belgium 2016 Review. https://www.iea.org/reports/energy-policies-of-iea-countries-belgium-2016-review.



