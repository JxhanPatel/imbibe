# Module 1: Regression Models & Evaluation

## 4.1 Linear Regression

<img width="1400" height="933" alt="image" src="https://github.com/user-attachments/assets/e5f996db-44cd-47bb-92ec-dc02a7fc85b1" />

- We start by importing basic Python packages for manipulation and visualization .
- We import `fetch_california_housing` from the `sklearn.datasets` module that helps us to fetch the dataset .
- The linear regression API is imported from `sklearn.linear_model` to help build the linear regression model .
- Model coefficients example output: `[8.443248880-01 1.184639010-01 -3.046195740-01 3.566205030-01 1.74458509-04 -4.23964612-02 -8.960456420-01 -8.68906479-01]` .
- To obtain predictions for test points in cross-validation, we use `cross_val_predict` from `sklearn.model_selection` :
```python
from sklearn.model_selection import cross_val_predict cv_predictions = cross_val_predict(lin_reg_pipeline, train_features, train_labels)
```
- We obtain multiple models in cross-validation and need to select one model that gives the best performance on the evaluation set .
- We report model performance on the test set using the `.score()` method :
```python
 score_cv = selected_model.score(test_features, test_labels) score = lin_reg_pipeline.score(test_features, test_labels) print("R2 score for the best model obtained via cross validation:", score_cv) print("R2 score for model without cv:", score)
```
- The $R^2$ score for the best model obtained via cross validation is `0.5923577635319088` and without CV is `0.5910509795491352` .
- Alternatively, we can use any other metric of interest, such as mean squared error .
- As an exercise, you can change the scoring schemes and compare results across different metrics, or repeat all modeling steps with the `SGDRegressor` API, which trains the linear regression model through iterative optimization .
- We can build a quick baseline model using the `DummyRegressor` class implemented in the `sklearn.dummy` module .
- We instantiate a dummy regressor object by specifying the strategy as mean (`strategy="mean"`), meaning predictions are made based on the mean value of the label learned through the `.fit()` function on the training set .
- We can inspect the model by accessing the weight vector through the `coef_` and `intercept_` member variables of the linear regression object .
- For regression problems, classes and utilities are imported from `sklearn.linear_model`, which has implementations for `LinearRegression`, `SGDRegressor`, `Ridge`, `Lasso`, `RidgeCV`, and `LassoCV` .

### Summary of Linear Regression Training Algorithms

| Algorithm | Large $m$ | Out-of-core support | Large $n$ | Hyperparams | Scaling required | Scikit-Learn |
| --- | --- | --- | --- | --- | --- | --- |
| Normal Equation | Fast | No | Slow | 0 | No | `LinearRegression` |
| SVD | Fast | No | Slow | 0 | No | `LinearRegression` |
| Batch GD | Slow | No | Fast | 2 | Yes | `SGDRegressor` |
| Stochastic GD | Fast | Yes | Fast | $\ge 2$ | Yes | `SGDRegressor` |
| Mini-batch GD | Fast | Yes | Fast | $\ge 2$ | Yes | `SGDRegressor` |

- There is almost no difference after training: all these algorithms end up with very similar models and make predictions in exactly the same way .

## 4.2 Model Evaluation

<img width="576" height="346" alt="image" src="https://github.com/user-attachments/assets/2b869af0-b403-4a21-b989-0c19e8631017" />

### General Steps in Model Evaluation

1. Split data into train and test using the `train_test_split` class from `sklearn.model_selection`, which takes feature matrix $X$ and label vector $y$ (or label matrix in multi-output regression) and a random seed, and returns train features, test features, train labels, and test labels .
2. Fit the linear regression estimator on the training set .
3. Calculate training error (a.k.a. empirical error) .
4. Calculate test error (called generalization error) .
5. Compare training and test error to understand whether our model is the right fit or whether it is underfitting or overfitting .

### Evaluation Using the Score Method

- We call the `.score()` method on the regression estimator, supplying the feature matrix and label vector/matrix of the test set : 
```python
 linear_regressor.score(X_test, y_test)
```
- The score returns $R^2$ or coefficient of determination : $$ R^2 = 1 - \frac{u}{v} $$ where residual sum of squares $u$ is : $$ u = (Xw - y)^T (Xw - y) $$ and total sum of squares $v$ (actual and mean predicted label) is : $$ v = (y - \hat{y}_{mean})^T (y - \hat{y}_{mean}) $$

### Evaluation Metrics in Scikit-Learn

- Scikit-Learn provides a bunch of regression metrics in the `sklearn.metrics` module :
- **`mean_absolute_error`**: Imported via `from sklearn.metrics import mean_absolute_error`, calculated by supplying actual labels and predicted labels .
- **`mean_squared_error`** (or `mean_squarred_error`): Evaluates performance on the evaluation set .
- **`r2_score`**: Gives the same output as the score method, imported via `from sklearn.metrics import r2_score` .
- **`mean_squared_log_error`**: Imported via `from sklearn.metrics import mean_squared_log_error` .
- These metrics can also be used in a multi-output regression setup .
- Example computing RMSE on the housing dataset using Scikit-Learn's `mean_squared_error()` function : 
```python
 from sklearn.metrics import mean_squared_error housing_predictions = lin_reg.predict(housing_prepared) lin_mse = mean_squared_error(housing_labels, housing_predictions) lin_rmse = np.sqrt(lin_mse) ```[cite: 5] This results in `lin_rmse` equal to `68628.19819848922`[cite: 5].
 ```
- A typical prediction error of $68,628 when median housing values range between $120,000 and $265,000 is an example of a model **underfitting** the training data . When this happens, it can mean that the features do not provide enough information .
- Cross-validation evaluation scores can be obtained using `cross_val_score` or `cross_validate` (specifying estimator, data, target, cross-validation iterator, scoring scheme such as `scoring="neg_mean_absolute_error"`, `return_train_score=True`, and `return_estimator=True`) .
- Cross-validation results are stored in a Python dictionary with keys: `fit_time`, `score_time`, `test_score`, `estimator` (optional), and `train_score` (optional) .

## 4.3 Polynomial Regression

<img width="734" height="482" alt="image" src="https://github.com/user-attachments/assets/0fbede6d-60bc-438f-a908-dbf70ef2bfe7" />

- What if your data is more complex than a straight line? Surprisingly, you can use a linear model to fit nonlinear data by adding powers of each feature as new features, then training a linear model on this extended set of features; this technique is called **Polynomial Regression** .
- `PolynomialFeatures(degree=d)` transforms an array containing $n$ features into an array containing $\frac{(n + d)!}{d!n!}$ features (where $n!$ is the factorial of $n$) . Beware of the combinatorial explosion of the number of features !
- Polynomial transformation generates a new feature matrix consisting of all polynomial combinations of the features with degrees less than or equal to the specified degree .
- For example, given $X = [x_1, x_2]$ :
- `degree = 2` transforms $X$ into $X' = [x_1, x_2, x_1x_2, x_1^2, x_2^2]$ .
- `degree = 3` transforms $X$ into higher-degree polynomial combinations up to degree 3 .

### How Polynomial Regression is Trained

Polynomial regression is a two-step process  :

1. Apply polynomial transformation on the feature matrix .
2. Learn linear regression model (via normal equation or SGD) on the transformed feature matrix .

- Implementation tip: Make use of the `Pipeline` construct for polynomial transformation followed by linear regression estimator .

### Code Implementation Examples

- Setting up polynomial regression model with normal equation : 
```python
 from sklearn.linear_model import LinearRegression from sklearn.pipeline import Pipeline from sklearn.preprocessing import PolynomialFeatures poly_model = Pipeline([ ('polynomial_transform', PolynomialFeatures(degree=2)), ('linear_regression', LinearRegression()) ]) poly_model.fit(X_train, y_train)
 ```
- Setting up polynomial regression model with SGD : 
```python
 from sklearn.linear_model import SGDRegressor from sklearn.pipeline import Pipeline from sklearn.preprocessing import PolynomialFeatures poly_model = Pipeline([ ('polynomial_transform', PolynomialFeatures(degree=2)), ('sgd_regression', SGDRegressor()) ]) poly_model.fit(X_train, y_train)
```
- Training a pipeline with degree 2 polynomial features, standard scaling, and linear regression : 
```python
 poly_reg_pipeline = Pipeline([ ("poly", PolynomialFeatures(degree=2)), ("feature_scaling", StandardScaler()), ("lin_reg", LinearRegression()) ])
 ```
- Mean absolute error of linear regression model on the train set: `0.461 +/- 0.003` and on the test set: `0.485 +/- 0.030` .
- Notice that the training and validation errors have reduced after using the second-order polynomial features to represent the model .
- Instead of using all polynomial features, we can use only interaction feature terms in the polynomial model : 
```python
poly_reg_pipeline = Pipeline([ ("poly", PolynomialFeatures(degree=2, interaction_only=True)), ("feature_scaling", StandardScaler()), ("lin_reg", LinearRegression()) ])
```
- Polynomial models have a tendency to **overfit** if higher-order polynomial features are used . Ridge regression can be used to penalize excessive model complexity in polynomial regression by adding a regularization term .
