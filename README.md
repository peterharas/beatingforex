# Beating Forex

A project investigating whether two STEM students can match J.P. Morgan's 2023 state-of-the-art financial time series forecasting capabilities. Spoiler: we got close!

## Chasing J.P. Morgan

The idea for the project was born when we stumbled upon the 2023 paper [Financial Time Series Forecasting using CNN and Transformer](https://arxiv.org/abs/2304.04912) by Zeng et al. from J.P Morgan AI Research. Their method achieves an accuracy of 56.7% when predicting whether the price of an S&P 500 stock goes up or down given the prices in the previous 80 minutes. So by implementing the paper, anybody could program a free money-printing machine, right? Well, not quite. First of all, the paper is not accompanied by any code, hyperparameters are hardly stated, and the description of the implementation details is very vague. Also, the researchers trained their AI on proprietary data unavailable to the public. This kickstarted our mission to fill in the gaps and see for ourselves if we could implement a model that could come close to the paper's performance figures. 

