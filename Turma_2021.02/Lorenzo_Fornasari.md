### Final work for the EQM2108 course 
# BTP modeling using Random Forest Regressor
Professor: Amanda Lemette

Student: Lorenzo Fornasari

## Abstract

This work explores the modeling of the Burn Through Point (BTP) of a sintering machine from a Brazilian steel mill. Predicting these parameters can be very helpful in identifying deviations in sinter quality, preventing material recirculation and optimizing the process. The regression methods of XGBoost and Random Forest were experimented, and XGBoost wasn't capable of correctly modeling the data. An optimization of hyperparameters was realized and the best results were selected to be used in our model. The constructed model was able to determine the BTP position with R2 = 0.76, which is similar to the values found in the literature, although it did not have the refinement necessary to make a temporal prediction of such parameter. Thus, this study aims to lay the groundwork for future works of BTP prediction.

## Introduction

Sintering of iron ore is an essential part of a modern integrated steel mill. Unfortunately, it is also a source of a considerable amount of greenhouse gases, particulates, and other harmful gases such as NOx and SOx. Thus, being able to predict parameters of the process that influence its efficiency is an action that not only results in savings for the steel industry, but also lowers its environmental footprint. However, there is currently no instrument to detect sintering end point at present, due to its large lag and time-varying characteristics. This way, the only methods available are estimations of the BTP based on other process parameters, which is prone to error and control issues. Some efforts to improve sintering through machine learning have already achieved considerable accuracy, given the inherent complexity of this process [1,2]. One of them used gradient boosting decision trees and an inference engine to predict BTP and BTT[1].
Given the expected gains and previous successes from modeling using tree-based algorithms, this work aims to build a machine learning framework which may be used to predict sintering end point for this steel mill in the future. Here, XGBoost and Random Forest algorithms were tested and Random Forest yielded satisfactory results. 

## Methodology

The process data average of each minute for approximately one month was obtained from the company's database for the following parameters: exhaust temperatures of various bellows, negative pressure of main pipe, sintering machine speed, mixing roller speed and gas flow at the main exhaust. From the collected dataset, a few hundred entries were excluded due to missing values. The used dataset was reasonably large, with more than thirty thousand entries for each parameter.
Afterwards, the BTP was calculated for each minute using an empirical equation, which uses the highest exhaust gas temperature and the temperatures of adjacent bellows to fit a quadratic equation.

<center><img src="https://github.com/amandalemette/EQM2108/blob/47a190c5dbc51f0159a52995663d2468517e6935/Turma_2021.02/Imagens/LEF_Equa%C3%A7%C3%A3o.png?raw=True"  width=450 height=70 /><center>  
  
Where T is the exhaust gas temperature and X is the number of the bellow. The point of maximum temperature is identified and its position along the sintering process is the BTP.
The data collected was normalized to avoid interference from the parameter's unit sizes and variation on the regression algorithm. The normalization technique used was Z-score normalization, which outputs series with an average value of zero and a variance of one
A preliminary simple trial of XGBoost and Random Forest regressors was realized, and it was verified that while Random Forest was able to model the data, XGBoost failed to do so, and was not considered further.
An inspection of the correlations within the dataset was then realized and all parameters with correlations higher than 0.83 to at least one other parameter were excluded from the model. Subsequently, an hyperparameter optimization was executed using the optuna library.  The optimized hyperparameters were: maximum depth of each tree, minimum of samples in each leaf, the maximum number of leaf nodes and the maximum number of samples used during bootstrapping. Their optimization ranges were, respectively: 1 to 1500; 20 to 2000; 1 to 200000; and 0.001 to 1. The optimized parameters and their tested ranges are shown in Table 1. The best hyperparameters were documented and used for the model. 

_Table 1: hyperparameter ranges tested in optimization_
  
|Parameter        | Minimum| Maximum|
|:---------------:|:------:|:------:|
|Max_depth        |       1|    1500| 
|Min_samples_leaf |      20|    2000| 
|Max_leaf nodes   |       1|  200000| 
|Max_samples      |   0.001|       1| 
  
To conclude, a learning curve was crafted and analyzed to check for possible model overfitting.

## Results and discussion

The correlation matrix in the Figure 1 shows some interesting results. Firstly, adjacent exhaust bellows gas temperatures were very highly correlated. This is expected, as sintering is a linear process and adjacent areas are in contact and transfer heat with one another. Another interesting factor is how negative pressure (‘Depressão’) is increasingly negatively correlated – and gas flow (‘FluxGasSaida’) at the main exhaust is increasingly positively correlated - with exhaust gas temperatures from bellows further on the line. This is likely due to the need to adjust such parameters to the sintering end point, which usually occurs in the final length of the machine.
One observation that stood out and that bears great importance for this study is the low correlation of parameters with the BTP. This shows that there isn’t a single parameter that is the major cause for its value, and that making a regression model for it is likely challenging.

<center><img src="https://github.com/amandalemette/EQM2108/blob/8534c5d1dabbbd148e3689d85dff3b82ec9934e5/Turma_2021.02/Imagens/LEF_Correlation.png?raw=True"  width=900 height=720 /><center>
 
_Figure 1: correlation matrix for all parameters collected_  
  
To reduce noise in the model and facilitate optimization, parameters that had correlation higher than 0.83 with other parameters were removed. In other words, many exhaust gas temperatures were removed, and the 5 that were maintained – bellows 12, 16, 18, 21 and 29 - represented distant and/or non-adjacent areas of the sintering machine. 
Optimization through 300 trials yielded the best values described in Table 2. It should be noted that _max_samples_ is a _float_ rather than an _int_ because it describes the maximum percentage of the total number of samples used in bootstrapping. That is, if a parameter has 100 samples, a _max_samples_ value of 0.5 means that no more than 50 samples may be used to bootstrap it.

_Table 2: best values for the optimized hyperparameters_
  
|Parameter        | Best value|
|:---------------:|:---------:|
|Max_depth        |        952|
|Min_samples_leaf |         20|
|Max_leaf nodes   |     174160|
|Max_samples      |     0.9727|
  
It is interesting to note that the best parameters showed vey low _min_samples_leaf_ and very high _max_leaf_nodes_. This suggests that the best trees had a higher number of leaf nodes, and smaller leaves, what could point to an overfitting of the model. However, another explanation is that this is due to the inherent complexities of the sintering process, and that these highly branched trees are simply needed to account for all process instabilities encountered. This last hypothesis is backed by the fact that there is overall low correlation between all parameters and the BTP values.
Figure 2 shows the optimization history plot. It shows that around 60 trials were needed to achieve R2 values similar to the best one, and that only one other trial shared the same R2 value as the best one. A possible reason for this is the low correlation between parameters and values, or overfitting.

<center><img src="https://github.com/amandalemette/EQM2108/blob/8534c5d1dabbbd148e3689d85dff3b82ec9934e5/Turma_2021.02/Imagens/LEF_OptimizationHistoryPlot.png?raw=True"  width=900 height=552 /><center>

_Figure 2: optimization history plot_  
  
Figure 3 shows hyperparameter importances. It reveals that the parameter for the minimum number of samples in leaves shows a far higher importance than any other parameter, at 0.91. Maximum number of samples for bootstrapping was also important, with 0.08 importance rating. Maximum depth of the regression tree minimally important, and the maximum number of leaf node was not important. It should be noted that the _min_samples_leaf_ parameter is correlated the _max_leaf_nodes_, as trees that are allowed to have smaller leaves will tend to have more nodes, and trees that are not allowed to have many nodes will have bigger leaves.

<center><img src="https://github.com/amandalemette/EQM2108/blob/8534c5d1dabbbd148e3689d85dff3b82ec9934e5/Turma_2021.02/Imagens/LEF_HyperparameterImportances.png?raw=True"  width=900 height=443 /><center>

_Figure 3: hyperparameter importances_  
  
Figure 4 shows the slice plot for each parameter. It makes it very clear that the model highly favors low minimum number of samples in leaves. It also suggests that the max depth of the trees is best around 1000. This is probably correlated to the minimum number of samples in leaves and to the sample size, which is about 30 thousand. For example, a tree with 1000 levels, and each level with a leaf with 20 samples would contain 20 thousand data points, which is very similar to the actual number of data points used. Furthermore, maximum number of leaf nodes showed its better results for higher values, which was already discussed. Finally, the maximum number of samples used for bootstrapping also showed better results for higher values, which means that more of the dataset was needed to accurately bootstrap the model.

<center><img src="https://github.com/amandalemette/EQM2108/blob/8534c5d1dabbbd148e3689d85dff3b82ec9934e5/Turma_2021.02/Imagens/LEF_SlicePlot.png?raw=True"  width=900 height=372 /><center>

_Figure 4: slice plot_  
  
A model was built using the best values found and tested. It yielded R2 values of 0.768 and 0.763 for training and testing, respectively. These values are similar to the ones found in the literature for BTP prediction through gradient boost decision tree (GBDT)[1].   
The same optimum parameter values were then used to build a learning curve for the described regressor model through 5-fold cross validation. The resulting learning curve presented cross-validation scores lower than training scores, which is a sign that the model is not overfit. The graph also showed significant standard deviation for the cross-validation, reinforcing the idea that the process modeled presents parameter’s oscillation as a challenging feature.

<center><img src="https://github.com/amandalemette/EQM2108/blob/8534c5d1dabbbd148e3689d85dff3b82ec9934e5/Turma_2021.02/Imagens/LEF_LearningRate.png?raw=True"  width=900 height=581 /><center>

_Figure 5: learning curve_   
  
## Conclusion

Although parameter optimization showed various signs that the model was overfitted to the data, given its preference for highly branched trees with small leaves, and for taking a bigger part of the data for bootstrapping, the learning rate curve implies that it isn’t overfit. An alternative explanation proposed is that these parameters were selected because the modeled process is complex, with many different parameters affecting its performance and adding to its variability. In this interpretation, many different logic arguments would be needed to deal with the various process instabilities that may be reflected on the data.
Overall good coefficients of determination were found it this study, their values being similar to other literature on the subject. This suggests that the crafted model is realistic and may be of value for future investigations on BTP prediction for this sintering process.
Future studies should focus on acquiring a more diverse set of parameters for modeling and using auxiliary codes to make predictions of the sintering end point in the near future. This would allow for better control, cost savings, and better environmental indicators for the steel mill.

## References

[1] Liu, S., Lyu, Q., Liu, X., Sun, Y. & Zhang, X. A prediction system of burn through point based on gradient boosting decision tree and decision rules. ISIJ Int. 59, 2156–2164 (2019).

[2] Wang, B., Fang, Y., Sheng, J., Gui, W. & Sun, Y. BTP prediction model based on ANN and regression analysis. Proc. - 2009 2nd Int. Work. Knowl. Discov. Data Mining, WKKD 2009 108–111 (2009) doi:10.1109/WKDD.2009.179.








