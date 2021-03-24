<p align="center"><img width=12.5% src="https://github.com/anfederico/Clairvoyant/blob/master/media/Logo.png"></p>
<p align="center"><img width=60% src="https://github.com/anfederico/Clairvoyant/blob/master/media/Clairvoyant.png"></p>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![Python](https://img.shields.io/badge/python-v3.6+-blue.svg)
[![Build Status](https://travis-ci.org/anfederico/Clairvoyant.svg?branch=master)](https://travis-ci.org/anfederico/Clairvoyant)
![Dependencies](https://img.shields.io/badge/dependencies-up%20to%20date-brightgreen.svg)
[![GitHub Issues](https://img.shields.io/github/issues/anfederico/Clairvoyant.svg)](https://github.com/anfederico/Clairvoyant/issues)
![Contributions welcome](https://img.shields.io/badge/contributions-welcome-orange.svg)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## Basic Overview

Using stock historical data, train a supervised learning algorithm with any combination of financial indicators. Rapidly backtest your model for accuracy and simulate investment portfolio performance. 
<p align="center"><img width=95% src="https://github.com/anfederico/Waldo/blob/master/media/Schematic.png"></p>

<br>

## Visualize the Learning Process
<img src="https://github.com/anfederico/Clairvoyant/blob/master/media/Learning.gif" width=40%>

<br>

## Last Stable Release
```python
pip install clairvoyant
```

## Latest Development Changes
```bash
git clone https://github.com/anfederico/Clairvoyant
```

## Backtesting Signal Accuracy
During the testing period, the model signals to buy or sell based on its prediction for price
movement the following day. By putting your trading algorithm aside and testing for signal accuracy
alone, you can rapidly build and test more reliable models.

```python
from clairvoyant.engine import Backtest
import pandas as pd

features  = ["EMA", "SSO"]   # Financial indicators of choice
trainStart = 0               # Start of training period
trainEnd   = 700             # End of training period
testStart  = 701             # Start of testing period
testEnd    = 1000            # End of testing period
buyThreshold  = 0.65         # Confidence threshold for predicting buy (default = 0.65) 
sellThreshold = 0.65         # Confidence threshold for predicting sell (default = 0.65)
continuedTraining = False    # Continue training during testing period? (default = false)

# Initialize backtester
backtest = Backtest(features, trainStart, trainEnd, testStart, testEnd, buyThreshold, sellThreshold, continuedTraining)

# A little bit of pre-processing
data = pd.read_csv("SBUX.csv", date_parser=['date'])
data = data.round(3)                                    

# Start backtesting and optionally modify SVC parameters
# Available paramaters can be found at: http://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html
backtest.start(data, kernel='rbf', C=1, gamma=10)
backtest.conditions()
backtest.statistics()  
backtest.visualize('SBUX')
```

#### Output
```text
------------ Data Features ------------

X1: EMA
X2: SSO

---------------------------------------

----------- Model Arguments -----------

kernel: rbf
C: 1
gamma: 10

---------------------------------------

---------  Engine Conditions ----------

Training: 2013-03-01 -- 2015-12-09
Testing:  2015-12-10 -- 2017-02-17
Buy Threshold: 65.0%
Sell Threshold: 65.0%
Continued Training: False

---------------------------------------

------------- Statistics --------------

Total Buys: 170
Buy Accuracy: 68.24%
Total Sells: 54
Sell Accuracy: 59.3%

---------------------------------------
```

<img src="https://github.com/anfederico/Clairvoyant/blob/master/media/SBUX.png">

## Simulate a Trading Strategy
Once you've established your model can accurately predict price movement a day in advance, 
simulate a portfolio and test your performance with a particular stock. User defined trading logic
lets you control the flow of your capital based on the model's confidence in its prediction
and the following next day outcome.

```python
def logic(account, today, prediction, confidence):
    
    if prediction == 1:
        Risk         = 0.30
        EntryPrice   = today['close']
        EntryCapital = account.BuyingPower*Risk
        if EntryCapital >= 0:
            account.EnterPosition('Long', EntryCapital, EntryPrice)

    if prediction == -1:
        ExitPrice = today['close']
        for Position in account.Positions:  
            if Position.Type == 'Long':
                account.ClosePosition(Position, 1.0, ExitPrice)


simulation = backtester.Simulation(features, trainStart, trainEnd, testStart, testEnd, buyThreshold, sellThreshold, continuedTraining)
simulation.start(data, 1000, logic, kernel='rbf', C=1, gamma=10)
simulation.statistics()
simulation.chart('SBUX')
```

#### Output

```text
------------- Statistics --------------

Buy and Hold : -6.18%
Net Profit   : -61.84
Strategy     : 5.82%
Net Profit   : 58.21
Longs        : 182
Sells        : 168
Shorts       : 0
Covers       : 0
--------------------
Total Trades : 350

---------------------------------------
```

<img src="https://raw.githubusercontent.com/anfederico/Clairvoyant/master/media/Chart.png">

## Other Projects
#### Intensive Backtesting
The primary purpose of this project is to rapidly test datasets on machine learning algorithms (specifically Support Vector Machines). While the Simulation class allows for basic strategy testing, there are other projects more suited for that task. Once you've tested patterns within your data and simulated a basic strategy, I'd recommend taking your model to the next level with:
```text
https://github.com/anfederico/Gemini
```

#### Social Sentiment Scores
The examples shown use data derived from a project where we are data mining social media and performing stock sentiment analysis. To get an idea of how we do that, please take a look at:
```text
https://github.com/anfederico/Stocktalk
```

## Notes
#### Multivariate Functionality
Remember, more is not always better!
```python
variables = ["SSO"]                            # 1 feature
variables = ["SSO", "SSC"]                     # 2 features
variables = ["SSO", "SSC", "RSI"]              # 3 features
variables = ["SSO", "SSC", "RSI", ... , "Xn"]  # n features
```

## Contributing
Please take a look at our [contributing](https://github.com/anfederico/Clairvoyant/blob/master/CONTRIBUTING.md) guidelines if you're interested in helping!
#### Pending Features
- Export model
- Support for multiple sklearn SVM models
- Visualization for models with more than 2 features











Autokeras Version 1.0.2

## List of files and folders

| File name     | Description   |
| ------------- | ------------- |
| GUI.py        | Execute this file to start the GUI. All functions of the GUI are defined in here.                                        |
| pruning.py    | This script executes the pruning. The functions pruning and pruning_for_acc start the pruning and return a pruned model. |


### Auto_TF_to_uC

This directory contains the scripts for the automated implementation of the project.

| File/Folder name       | Description   |
| ---------------------- | ------------- |
| Converted_model_files  | This folder contains the converted TensorFlow lite models which are converted through this tool.                                                                                                                                 |
| TensorFlow_library     | The TensorFlow library with all neccesary source and header files.                                                                                                                                                                  |
| convert_keras_to_cc.py | The function convert_model_to_tflite() converts a keras model to a TensorFlow lite file. The function convert_model_to_cpp() converts the TensorFlow lite model into a cpp model so that it can be executed on the microcontroller. |
| create_project.py      | This script converts the model, creates the project directory and generate all files that are needed to compile the project.                                                                                                        |
| write files_uc.py      | In this script the project directory is created and all files are generated which are used for the project.                                                                                                                         |


### Images

All images required for the GUI design are stored in this folder.


### Threads

This directory contains all threads which are needed to run the GUI correctly.

| File name                | Description   |
| ------------------------ | ------------- |
| Create_project_thread.py | This thread calls the function from create_project.py to create all files of the project. It then executes the bash script of the created project to compile and load the project onto the microcontroller.               |
| Loading_images_thread.py | In the UILoadWindow of the GUI the project folder with all necessary files is created and the binary file is loaded onto the microcontroller. Meanwhile, this thread performs the display of a loading screen in the GUI. |
| Prune_model_thread.py    | If the neural net is to be reduced by pruning, this is done in this thread.                                                                                                                                               |


### UIWindows

The design of the individual windows of the GUI are defined in these files.

| File name              | Description   |
| ---------------------- | ------------- |
| UIHelperWindow.py      | The purpose of this window is to help the user select the right target platform based on a few parameters such as design, price or performance.                                                   |
| UILoadWindow.py        | The last window of the GUI in which the project is finally created and loaded onto the microcontroller.                                                   |
| UIMarcusWindow.py      | ---                                                   |
| UIMarcusWindow1.py     | ---                                                   |
| UIMarcusWindow2.py     | ---                                                   |
| UIMarcusWindow3.py     | ---                                                   |
| UIMarcusWindow4.py     | ---                                                   |
| UIMarcusWindow5.py     | ---                                                   |
| UIOptiWindow.py        | The window where the optimizations (pruning, quantization, knowledge distillation and huffman coding are optional) of the neural network are selected.                                                                       |
| UIRestrictionWindow.py | ---                                                   |
| UIStartWindow.py       | Here the project name, output path of the project, the Keras model and if the neural network is to be optimized a script for re-training the model is passed. |
| UITargetWindow.py      | In this window the target platform on which the neural network is to be executed is selected.                                                 |
