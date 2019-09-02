# QSR-DC

## Goal

This submission for the QSR data challenge focuses in using simple data transformation on the Multivariate time series to augment the original dataset and improve the desired metric, the F-score, evaluated on an unobserved dataset. The classification method is fixed as a Logistic Regression with the hyperparameters given in the problem description.

## Methodology

### Data Split

The different datasets were split into Test and training sets. For “dataset-b”, the split was done preserving the final 10% of all samples as our test set. For “dataset-b” the split is based on the first observed timestamp in the test version of “dataset-b”.

### Data Augmentation
We applied 3 different types of data augmentation techniques:

- Transformations independent of the response variable: simple operations on the time series data in the train and test versions of “dataset-a” and “dataset-b”
- Clustering: outputs of different clustering and data reduction techniques
- Tree based probability: one-step ahead failure probability score of tree-based prediction methods

### Evaluation Using Logistic Regression

Different iterations/combinations of these augmented features tested using the Logistic Regression will be evaluated on the “dataset-b” to compute the F-score, precision and recall metrics. Given that the fixed hyperparameters make use of Lasso regularization, we can work with a large number of predictors, as feature selection is in effect

## Transformations

The following are applied on the original regressor variables:

- Lagged differences
- Lagged differences of differences
- Percentual lagged difference
- Absolute value of percentual lagged difference
- Exponentially weighted moving average of percentual lagged differences
- Log difference of percentual changes
- Exponentially weighted moving average of log of percentual changes

We tested different values for the lag periods and the exponential weights.

In addition, the following variables are extracted from the timestamp:

-Hour of the day
-Day of the month
-Minute of the hour
-Month of the year
-Time difference from previous time stamp 

## Clustering and Dimensionality Reduction Mappings

The clustering and dimensionality reduction methods used to extract features are:

- SVDD
- DBSCAN
- Isolation Forest
- PCA
- ICA
- SVD
- Local Outlier Factor
- TSNE

## XgBoost + BO
The output of the leaflets of three different XgBoost models were used as inputs for the logistic regression. Two of these were fitted to the “dataset-a” observations and one to the “dataset-b” observations. The search for hyperparameters was carried out using Bayesian Optimization and using the area under the test AUC-PR on a 5-fold CV as the performance metric.

## Tests on Logistic Regression

The created features are fed to a Logistic Regression with fixed Hyperparameters. The predictions of the final logistic regression are used as submission. The AUC-PR curve is used for model comparison, besides the challenge metrics. In most cases, this curve showed a rather poor performance and the combination of variables that leads to the best test F-score was selected

## Best Results
Logistic regression with only the original data obtained an F-Score of 0.0115. The model with all the created variables and scaling of the original variables obtained an F-Score of 0.0242. Based on the relative feature importance of the logistic regression, the most relevant variables are the exponential transformations of the one-step ahead failure probability and the absolute percentual lagged differences of some of the variables.  This results is explained by the capacity of the tree-based methods to capture complex covariance structures in the data while the exponential smoothing captures the history of the time series.

![Feature Relevance](03.Models/Relevance.png?raw=true "Feature Relevance for best model")

## Predictions on New Data

In order to fit the model to new data, execute the notebook “5. Prediction on New Data.ipynb” in the "02.Scripts" directory. The trained elements of the process are saved in the directory “03.Models”.

## Note on Large Files
GitHub web did not allow the upload of very large files and the configuration and use of GIT LFS is needed. An alternative proposes to circumvent this is to download the larger files, with all the other scripts and directories, from this [google drive shared folder](https://drive.google.com/drive/folders/1mis7aakZz0-mr-FXZQNkU_40815U3o2b?usp=sharing).

## Library Versions

The different libraries used:

- pandas 0.25.1
- numpy 1.16.4
- sklearn 0.21.2
- pickle 4.0
- xgboost 0.82
- bayes_opt 1.0.1
- funcsigs 1.0.2
- datetime
- Matplotlib 2.2.2

