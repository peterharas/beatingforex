# Beating Forex

A project investigating whether two STEM students can match J.P. Morgan's 2023 state-of-the-art financial time series forecasting capabilities. Spoiler: we got close, our models **beat the market with an accuracy of 54%**! 

[Jump to the results](#results)

## Chasing J.P. Morgan

The idea for the project was born when we stumbled upon the 2023 paper [Financial Time Series Forecasting using CNN and Transformer](https://arxiv.org/abs/2304.04912) by Zeng et al. from J.P Morgan AI Research. Their method achieves an accuracy of 56.7% when predicting whether the price of an S&P 500 stock goes up or down given the prices in the previous 80 minutes. So by implementing the paper, anybody could program a free money-printing machine, right? Well, not quite. First of all, the paper is not accompanied by any code, hyperparameters are hardly stated, and the description of the implementation details is very vague. Also, the researchers trained their AI on proprietary data unavailable to the public. This kickstarted our mission to fill in the gaps and see for ourselves if we could implement a model that could come close to the paper's performance figures. 

## Stock data vs Forex data

It is difficult to find high-quality and minute-resolution data on stock prices for free. Data on Forex (foreign currency exchange) currency pair prices however is broadly available from sources such as the [Dukascopy Swiss Banking Group](https://www.dukascopy.com/swiss/english/marketwatch/historical/). Further reasons for focusing on Forex are its high liquidity and 24-hour trading on business days. We used 19 currency pairs (EUR or USD was always part of a pair) and 1 year's worth of minute-resolution data. Predictions are made based on 80-minute time windows, the model outputs whether the price will go up or down in the 90th minute. The open, close, low, and high prices as well as the volume are considered during training and in each prediction. 

Further technical details:
- Train/validation/test split: 75%/12.5%/12.5%
- Data preprocessing:
  - Removal of holidays and weekends
  - Min-max standardization of prices for each day
  - Undersampling is used to fix class imbalance  

## The models

We implemented two working models: the **CNN and Transformer based Time Series Model** (referenced as CTTS) and a variation of it that involves a **Long short-term memory** (referenced as CLSTM).

### CTTS

A convolutional neural network (CNN, often used in computer vision) acts as the initial part of the CTTS architecture. It captures various patterns of the 80-minute input time series and creates tokens carrying those patterns for the architecture's next building block, the Transformer Encoder (yes, the same architecture also used in ChatGPT). To learn the long-term dependencies between the tokens, the same positional embedding is used as in Vaswani et al.'s 2017 [Attention Is All You Need](https://arxiv.org/abs/1706.03762) paper. The Transformer encoder learns the best possible representation (latent embedding vector) of the time series that is then passed to the final building block, a Multilayer Perceptron (MLP), that performs the classification and outputs the probability of the currency's price going up. 

Further technical details:
- The CNN has 5 channels: High, Low, Open, Close, Volume
- 128 different patterns are captured by the CNN for each token
- Patterns are analyzed for 16-minute windows (the result of CNN kernel size 16 and stride 8)
- Transformer parameters: depth 4, 4 self-attention heads, embedding dimension 128, drop rate 0.3
- Optimizer: AdamW

### CLSTM

While developing the CTTS model we also experimented with incorporating an LSTM in the architecture. The outputs of the CNN are fed into an LSTM, the MLP head is replaced with a single fully connected layer. This modified architecture performed comparably to the original CTTS architecture. 

## Results
<a name="results"></a>

While we did not succeed in matching the performance figures of J.P. Morgan's AI research team, both our algorithms are capable of beating the market and would perform better than randomly placing bets. The table below shows the achieved (test) accuracies:

| Algorithm       | Data            | Accuracy        |
| --------------- | --------------- | --------------- |
| CTTS            | Forex           | 53.997%         |
| CLSTM           | Forex           | 54.215%         |
| J.P. Morgan     | S&P 500 stocks  | 56.7%           |

Looking at the loss curves proves that the model learned actual patterns and is not overfitting to the noise in the training data. Initially, the training and validation loss decrease jointly, once the model learns everything there is to learn from the data the validation loss begins to stagnate. The red line is the loss of a model that would randomly guess "up" or "down", both models outperform that baseline after a few epochs.

CTTS loss curves:
![CTTS Loss](/assets/ctts_loss.png)

CLSTM loss curves:
![CLSTM Loss](/assets/clstm_loss.png)

While predicting, the models return the probability of the price of a currency going up based on an observed period. The plot below shows the distribution of the predictions for the CTTS model. The further away a prediction is from the 0.5 mark, the more confident the model is about the veracity of its claim. When only considering the observations where the model is 75% sure about its prediction, the accuracy of the prediction increases to even **69.23%**. 

![CTTS Violin](/assets/ctts_violin.png)


## What's next

A trading strategy maximizing returns could be developed based on the observation that the accuracy increases with higher model confidence. Instead of only classifying whether the price goes up or down (buy or sell), ternary classification could be implemented with a third option: hold. Once that is implemented, simulations that also consider the bid-ask spread could be run on more data to evaluate whether the models are already capable of generating revenue. 

## Contact us

Got interested? Feel free to add us on LinkedIn or to reach out to us by e-mail!

Eugenio: 
[Mail](mailto:eugenio.cainelli@gmail.com) - 
[LinkedIn](https://www.linkedin.com/in/eugenio-cainelli)

Peter:
[Mail](mailto:peter.haraszti@protonmail.com) - 
[LinkedIn](https://www.linkedin.com/in/peter-haraszti-476a03325/)

