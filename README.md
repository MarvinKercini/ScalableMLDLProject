# ScalableMLDLProject

For this final project of our Scalable ML and DL course we decided to combine the knowledge acquired during the course. Our idea is to train a model on the historical stock prices of the NASDAQ composite index, combined with other features such as inflation rate and US dollar strenght (FOREX USD/EUR). This model will then be deployed on an app online and predict daily the upcoming price of the NASDAQ stock, by using the latest data and a clean interface.

## Features

The dataset is composed of the main features of any given stock, plus some extras. The main features of the NASDAQ index are downloaded by using an API called yfinance. For our training we have used start date "2003-01-01" and end date '2022-10-31' (almost 20 years). The model can of course be trained on other dates. The main features we got from this library were:

Open

High

Low

Close	Volume


To add more features to our dataset we also forex_python, from which we got the daily exchange rate of EUR/USD to use as a metric for the strenght of the US dollar. Furthermore, we added another feature, thanks to the library CPI, which stands for Consumer Price Index. This is a metric used to calculate the the price of a weighted average market basket of consumer goods and services purchased by households. Changes in measured CPI track changes in prices over time, similar to inflation rate.

Lastly, we computed a feature ourselves, a boolean value for if the month of the date given is the end of a quartile. We choose this since we read that in this months the companies prepare the quartile performance reports and it can have an influence on the stock market.

After cleaning and preparing everything we saved our features in the huggingface as a dataset.



## Training

Before training anything we scaled our features using a min-max scaler. The scaler was trained on the training set and then used to scale not only it, but also the validation and test sets. This same scaler is later saved on huggingface and used to scale the input of the model at the online predictions.

Then we build the sequences to feet to our time series forecasting deep learning model. The model has a window of 10 (days) and predicts the next day. Note that this are working days (when the stock market is open and prices are changing).

After some fine tuning the final model we are using is composed of three Bidirectional GRU layers of 64 neurons each. This type of layer is known for its performance on time series forecasting. We are also using early stopping and reducing learning rate on plateau.

We tried saving the model on both huggingface and hopsworks. Both these solutions work and we could load our model in another colab file and use it to make the daily predictions. Unfortunately neither of this options worked when using gradio or streamit space in huggingface, so at the end we went with saving our model on google drive and then using it in the streamit in huggingface.



## Prediction

For the prediction we used a streamit app huggingface space. We load our model and also do the same combination of featueres as in the training but with the last 10 days or latest values. Then the model predicts on this interface and shows a graph of the latest values and the models prediction. In addition there is a slider where you can select to see the latest historical features, on which the model was based on.

You can find the final version of our app here: https://huggingface.co/spaces/Victorlopo21/StockProject



Group 51

Victor Lopo

Marvin Kercini
