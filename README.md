![image](tresorio_logo.png)

A small project that use Tresorio cloud computing to generate a dataset of BTCUSD charts and train an AI to look into the charts and predict stock movements.

This project is inspired by this one : https://github.com/cderinbogaz/inpredo.

## First Step - Create an account on Tresorio

Tresorio, the eco-friendly and affordable alternative for all your tasks and projects that require a lot of computing power! Each task started on Tresorio contribute to generate hot water to heat community buildings !

Create an account on : https://tresorio.com/en/beta/

![image](tresorioscreen.png)

## Second Step - Clone this repo:

Clone this repo on your personal github to be able to launch the project on Tresorio.

## Third Step - Create a project on your Tresorio account 

Create a new cloud computing project on Tresorio dashboard.

![image](computingscreen.png)

Link your Github account and select patternchart repo.

Choose a powerpack and add the following line to your project command :
`pip3 install -r requirements.txt`

![image](projectscreen.png)

## Fourth Step - Generate your charts dataset

Before start to train a Convolutional Neural Network, first you need to create a
training dataset. As a starting point you can use one of the following timeseries financial data:

- BTC-USD Hourly price data on Poloniex; poloniexclean.csv 
- BTC-USD Hourly price data on Kraken; krakenclean.csv
- BTC-USD Hourly price data on Binance; binanceclean.csv
- BTC-USD Hourly price data on Bitfinex; bitfinexclean.csv
- BTC-USD Hourly price data on Bitstamp; bitstampclean.csv
- BTC-USD Hourly price data on Coinbase; coinbaseclean.csv

Datas are from https://www.cryptodatadownload.com/

Go on your Tresorio dashboard and open patternchart project.
Start a new training and add to you script the following command : 

`mkdir -p /app/artifacts/data/train/sell`
`mkdir -p /app/artifacts/data/validation/sell`
`mkdir /app/artifacts/data/train/buy`
`mkdir /app/artifacts/data/validation/buy`

This will create the path of your output folders so you can dowload the generated dataset on the platform.

In order to convert Poloniex financial datas to charts, add the following command :

`python3 ./src/graphpoloniex.py`

This will create the charts and classify them into train/buy and train/sell folders.

To create the validation set, copy aleatory files from your train folder to your validation folder. 
To do so, in the following commands, replace X by the number of charts you want to transfer to your validation set (20% of the total number of files in your train folder is a good target) and add them to your project commands : 

`shuf -znX -e /app/artifacts/data/train/buy/*.jpg | xargs -0 cp -vt /app/artifacts/data/validation/buy`
`shuf -znX -e /app/artifacts/data/train/sell/*.jpg | xargs -0 cp -vt /app/artifacts/data/validation/sell`

If you want to generate the all dataset, your project commands should look like this : 
`pip3 install -r requirements.txt`
`mkdir -p /app/artifacts/data/train/sell`
`mkdir -p /app/artifacts/data/validation/sell`
`mkdir /app/artifacts/data/train/buy`
`mkdir /app/artifacts/data/validation/buy`
`python3 ./src/graphpoloniex.py`
`python3 ./src/graphkraken.py`
`python3 ./src/graphbinance.py`
`python3 ./src/graphbitfinex.py`
`python3 ./src/graphbitstamp.py`
`python3 ./src/graphcoinbase.py`
`shuf -zn6000 -e /app/artifacts/data/train/buy/*.jpg | xargs -0 cp -vt /app/artifacts/data/validation/buy`
`shuf -zn6000 -e /app/artifacts/data/train/sell/*.jpg | xargs -0 cp -vt /app/artifacts/data/validation/sell`


There is no point to use a GPU for this task so select a pack XS or a pack S.

Start the task

After running `graphwerk.py` it will take some time to write single jpg files under data/train folder.
When script is done writing, then you need to take randomly roughly 20 percent of the training data and put it into validation data.
You need this to be able to train a neural network and yes, I was too lazy to automate that as well.

## Fifth step - Train the AI!

Now we have the training and validation datasets in place, you can start training the AI model.
For this you just need to run `train-binary.py` and this script will start using the dataset make a AI model out of it.
When the model training is complete, it will generate a model and weights file under the models directory.

## Sixth Step - Load Models and Predict

You can run predictions using `predict-binary.py` script. Use the `predict(file)`
and use the path of the jpg file you want to predict. Result of the script will be a buy, sell or not confident message.

## Last words

THIS PROJECT IS FOR EDUCATIONAL PURPOSE ONLY ! DON'T USE IT TO MAKE REAL INVESTMENTS !

For people who wants to go experimental, don't forget that you can lose money in real markets and I am not accountable for your loss if you choose to use this project to trade with your own money.

Medium article for in depth explanation of the project: https://medium.com/@cderinbogaz/making-a-i-that-looks-into-trade-charts-62e7d51edcba