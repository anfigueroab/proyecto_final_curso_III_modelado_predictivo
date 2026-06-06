# Solution Summary

Conclusions from each of the following sections:

## Data modeling

The training data was loaded from Google Drive using pandas. Basic sanity checks confirmed the expected shape, there are no missing values, and there are no "hidden" categorical variables (neither features nor target). Features and target were separated for modeling.

## Preprocessing

The dataset was split into training (80%) and validation (20%) sets to evaluate generalization performance and mitigate overfitting. A fixed random seed was used to ensure reproducibility.

All features are numerical and showed different scales and distributions, so standardizing is recommended to improve the performance of most models (discussed further in the section). For the models to which it applies: Scaling (using `StandardScaler`) was implemented inside a scikit-learn pipeline to avoid data leakage during training and validation.

Given the relatively small number of features mand the absence of strong multicollinearity or irrelevant/redundant variables, all features were retained. Regularized models were later used to control complexity and reduce overfitting.

## Modeling

Models were compared using R² and RMSE on a validation set. A comparative table was created at the end of the section.

A linear regression model was used as a baseline.

Ridge regression was then evaluated to reduce variance and handle noise (No significant improvements were seen, so variance is probably not a problem).

Random Forest was also tested to capture non-linear relationships, but results show a possible Overfitting.

Extra trees Regression improves the results but has also a big gap between train and validation metrics.

Gradient Boosting Regression achieved a good validation performance while maintaining a reasonable train-validation gap, indicating good generalization despite noise.

XGBoost shows the best performance, However, the improvement isn't as significant compared to Gradient Boost. For this technical test, I'll choose XGBoost as the final model; however, in a production environment, I would run an intermediate test with more data to ensure it's truly worth the computational cost.

Tree-based boosting models outperformed linear approaches by capturing non-linear relationships and feature interactions.

## Model Tuning

A grid search with cross-validation was used to tune the most impactful XGBoost hyperparameters. The search focused on controlling model complexity to balance bias and variance, given the known noise in the target.

The learning curves show a stable gap between training and validation performance, suggesting mild overfitting. Given the known noise in the target and the limited dataset size, this behavior could be expected. Model complexity was adjusted to balance bias and variance, and the final configuration was selected based on validation performance.


## Evaluation

After selecting the best hyperparameters, the final model was retrained on the full training dataset to maximize the information available before generating predictions for the blind test set.

## Prediction

It is just the creation of the CSV file with the "blind test" predictions.