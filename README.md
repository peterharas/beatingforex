# Beating Forex

A project investigating whether two STEM students can match J.P. Morgan's 2023 state-of-the-art financial time series forecasting capabilities. Spoiler: we got close! [Jump to the results](#results)

## Chasing J.P. Morgan

The idea for the project was born when we stumbled upon the 2023 paper [Financial Time Series Forecasting using CNN and Transformer](https://arxiv.org/abs/2304.04912) by Zeng et al. from J.P Morgan AI Research. Their method achieves an accuracy of 56.7% when predicting whether the price of an S&P 500 stock goes up or down given the prices in the previous 80 minutes. So by implementing the paper, anybody could program a free money-printing machine, right? Well, not quite. First of all, the paper is not accompanied by any code, hyperparameters are hardly stated, and the description of the implementation details is very vague. Also, the researchers trained their AI on proprietary data unavailable to the public. This kickstarted our mission to fill in the gaps and see for ourselves if we could implement a model that could come close to the paper's performance figures. 

## Stock data vs Forex data

It is difficult to find high quality and minute resolution data of stock prices for free. Data on Forex (foreign currency exchange) currency pair prices however is broady available from sources such as the [Dukascopy Swiss Banking Group](https://www.dukascopy.com/swiss/english/marketwatch/historical/). Further reasons for focusing on Forex are its high liquidity and 24h trading on business days. We used 19 currency pairs (EUR or USD was always part of a pair) and 1 year worth of minute resolution data for training with a train/validation/test split of 75%/12.5%/12.5%. Predictions are made based on 80 minute time windows, the model outputs whether the price will go up or down in the 90th minute. Before training, the data is cleaned of holidays and weekends, the prices are min-max standardized for each day and the class imbalance is fixed using undersampling. The open, close, low and high prices as well as the volume are considered during training and in each prediction. 

## The models
tbd

## Results
<a name="results"></a>
tbd

## What's next
tbd

## Contact us
tbd
