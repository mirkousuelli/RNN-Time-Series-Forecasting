# Recurrent Neural Network: Time Series Forecasting
Artificial Neural Networks and Deep Learning competition 2021/2022 - Politecnico di Milano.

*Authors: **Fabio Tresoldi** and **Mirko Usuelli***

## 1. Introduction
The proposed dataset has been split into a Training Set(first 90% time steps) and Test Set (last 10%) in purpose ofestimating  forecasting  conclusions.  During  the  training  weadopted a Cross-Validation 90% - 10% at run time.

## 2. Data Pipeline
### 2.1.  Pre-Processing
First of all, we adopted a Min-Max scaling with respectto the Training Set in order to normalize the data. Next, weapplied it to the Test Set.

#### 2.2. Decomposition
We  developed  from  scratch  an  additive  STL  decompo-sition  using  justnumpyand  we  have  been  able  to  extract–  from  each  time  series  –  their  respectiveTrend,SeasonalandResidualcomponents.  Before  applying  it  we  neededto discover the characteristic period of each time series: weexploited Moving Average properties related to the Autocor-relation  Function  to  identify  this  information  on  the  entiredataset.periods= [192,96,96,96,95,86,96]This  achievement  was  made  in  purpose  of  gaining  betterforecasting  capabilities  by  working  on  a  modular  architec-ture  rather  than  a  compact  one  with  a  more  sophisticatedinput to deal with.

![image](/img/stl-3.png)

## 3.  Model
The design approach that we had was purely incrementalas it is going to be explain in the model section: starting froma  baseline  we  progressively  improved  it  by  trying  severaltechniques  and  made  decisions  according  to  RMSE  as  ourprimary metric. All  the  results  that  will  be  shown  were  obtained  with window=200, stride=10 and direct forecasting of 864 timesteps on the Test Set.

### 3.1.  Encoder
We  then  compared  5  base-line  architectures  starting  from  a  basic  RNN  –  namely  Bi-LSTM – then improved through a CNN. We replicated thewell-known  architecture  WaveNet  consisting  in  just  Con-volutional  layers  to  obtain  further  hints  on  the  direction  totake into account. At the end, we also tried the TransformerEncoder  –  including  a  temporal  Positional  Encoding  –  inorder to fully exploit its architectural benefits.

![image](/img/mae_comparison.png)
![image](/img/mse_comparison.png)

As shown by the two metrics, the lowest convergence hasbeen achieved by the CNN-RNN Encoder, thus we decidedto improve this baseline with further experiments.

### 3.2. Tuner
We  adopted  a  Bayesian  Optimization  for  the  modelhyper-parameters  in  purpose  of  finding  the  best  possible performance  with  the  design  choices  we  discussed.  Thisprocess has been done with KerasTuner.

## 4. Conclusion
Resuming,  we  started  from  a  basic  CNN-Bi-LSTM,afterwards  we  trained  an  Autoencoder  in  order  to  improvepattern  recognition  performance  in  an  unsupervised  way.Later  on,  we  unplugged  the  Encoder  to  be  joined  with  theForecaster to estimate final predictions in a direct approach.

### 4.1. Final Model
A  the  end,  the  Bayesian  optimization  tuning  producedthe following model:

![image](/img/model-3.png)

### 4.2. Performances
![image](/img/preds.png)
![image](/img/futs.png)

## 5. Leadboard Evaluation
- Development phase RMSE : **4.1252**
- Final phase RMES : **---**
