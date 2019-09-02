# QSR-DC

## Goal

This submission for the QSR data challenge focuses in using simple data transformation on the Multivariate time series to augment the original dataset and improve the desired metric, the F-score, evaluated on an unobserved dataset. The classification method is fixed as a Logistic Regression with the hyperparameters given in the problem description.

## Methodology

### Data Split

The different datasets were split into Test and training sets. For “dataset-b”, the split was done preserving the final 10% of all samples as our test set. For “dataset-b” the split is based on the first observed timestamp in the test version of “dataset-b”.

### Data Augmentation

- Different transformations were defined and applied to the train and test versions of “dataset-a” and “dataset-b”. These features do not need any information from the response variable to be computed
- Different clustering methods will be tested in order to augment the dataset
- Additional gradient boosted decision trees were used to create probabilistic scores used as features to add on the logistic regression

### Evaluation Using Logistic Regression

Different iterations/combinations of these augmented features tested using the Logistic Regression will be evaluated on the “dataset-b” to compute the F-score, precision and recall metrics. Given that the fixed hyperparameters make use of Lasso regularization, we can work with a large number of predictors, as feature selection is in effect

## Transformations

The following are applied on the original variables, testing different values for the lag periods. For the exponential weights, different values are tested

- Lagged differences
- Lagged differences of differences
- Percentual lagged difference
- Absolute value of percentual lagged difference
- Exponentially weighted moving average of percentual lagged differences
- Log difference of percentual changes
- Exponentially weighted moving average of log of percentual changes

These are extracted from the timestamps:

-Hour of the day
-Day of the month
-Minute of the hour
-Month of the year
-Time difference from previous time stamp 

## Clustering and Dimensionality Reduction Mappings

Different methods for unsupervised learning and dimensionality reduction are carried out and saved to be used as inputs for the logistic regression classifier. Among the methods explored:
- SVDD
- DBSCAN
- Isolation Forest
- PCA
- ICA
- SVD
- Local Outlier Factor
- TSNE

## XgBoost + BO
The output of the leaflets of three different XgBoost models were used as inputs for the logistic regression. Two of these were fitted to the “dataset-a” observations and one to the “dataset-b” observations. The search for hyperparameters was carried out using Bayesian Optimization and using the area under the test AUC-PR on a 5-fold CV as metric.

## Tests on Logistic Regression

The created features are fed to a Logistic Regression with fixed Hyperparameters. The predictions of the final logistic regression are used as submission. The AUC-PR curve is used for model comparison, besides the challenge metrics. In most cases, this curve showed a rather poor performance and the combination of variables that leads to the best test F-score was selected

## Predictions on New Data

In order to fit the model to new data, the notebook called “5. Prediction on New Data.ipynb” needs to be executed. This notebook takes the saved models from the explained training process to compute the required variables. The trained elements of the process are saved in the directory “03.Models” under different subfolder. 

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

