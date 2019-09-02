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

### Evaluation using Logistic Regression

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





