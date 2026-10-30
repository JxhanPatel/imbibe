# Week 4: Polynomial Regression & Regularized Models

**Module 1: Regression Models & Evaluation**

- **4.1** - Linear regression
- **4.2** - Model Evaluation
- **4.3** - Polynomial Regression

**Module 2: Regularization & Tuning**

- **4.4** - Regularization
- **4.5** - Hyper parameter tuning

**Module 3: Linear & Distance-Based Classifiers**

- **4.6** - Multi Learning Classification
- **4.7** - Naive Bayes Classifier
- **4.8** - K Nearest Neighbours
- **4.9** - SVM in scikit-learn

**Module 4: Decision Trees & Ensemble Methods**

- **4.10** - Decision Trees
- **4.11** - Voting, Bagging and Random Forest
- **4.12** - Boosting: AdaBoost, GradientBoost

**Module 5: Deep Learning Foundations**

- **4.13** - Neural Networks: Multi-layer Perceptron



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




---



# Module 2: Regularization & Tuning

## 4.4 Regularization

<img width="715" height="403" alt="image" src="https://github.com/user-attachments/assets/8f602248-79b9-4cbc-a6ac-e7eb79d636b8" />
<img width="602" height="399" alt="image" src="https://github.com/user-attachments/assets/08ddc4e3-a8a2-4ccc-9505-a6f21668d2a4" />

### Ridge Regression (Tikhonov Regularization)

- Ridge Regression is a regularized version of Linear Regression where a regularization term is added to the cost function .
- The Ridge Regression cost function is defined as : $$ J(\mathbf{\theta}) = \text{MSE}(\mathbf{\theta}) + \alpha \frac{1}{2} \sum_{i=1}^{n} \theta_i^2 $$
- The hyperparameter $\alpha$ controls the extent of model regularization . If $\alpha = 0$, Ridge Regression collapses to standard Linear Regression; if $\alpha$ is very large, all weights end up very close to zero, producing a flat line through the data's mean .
- The bias term $\theta_0$ is not regularized (the summation begins at $i = 1$) .
- **Scikit-Learn Implementation Options for Ridge**:
- **Option #1 (Using Ridge Class)**: ```python from sklearn.linear_model import Ridge ridge = Ridge(alpha=1e-3) ```[cite: 5] *Fit, score, and predict work exactly like other linear regression estimators[cite: 5].* ```
- **Option #2 (Using SGDRegressor)**: ```python from sklearn.linear_model import SGDRegressor sgd = SGDRegressor(alpha=1e-3, penalty='l2') ```[cite: 5] ```

### Lasso Regression (Least Absolute Shrinkage and Selection Operator)

- Lasso Regression adds a regularization term to the cost function using the $\ell_1$ norm of the weight vector instead of half the square of the $\ell_2$ norm : $$ J(\mathbf{\theta}) = \text{MSE}(\mathbf{\theta}) + \alpha \sum_{i=1}^{n} \vert{}\theta_i\vert{} $$
- **Key Characteristic**: Lasso Regression tends to eliminate the weights of the least important features by setting them to zero, automatically performing feature selection and outputting a sparse model .
- The subgradient vector for Lasso Regression is given by : $$ g_{\mathbf{\theta}, J} = \nabla_{\mathbf{\theta}} \text{MSE}(\mathbf{\theta}) + \alpha \begin{pmatrix} \text{sign}(\theta_1) \\ \vdots \\ \text{sign}(\theta_n) \end{pmatrix} $$ where $\text{sign}(\theta_i) = -1 \text{ if } \theta_i \lt 0$, $0 \text{ if } \theta_i = 0$, and $+1 \text{ if } \theta_i \gt 0$ .
- **Scikit-Learn Implementation Options for Lasso**:
- **Option #1 (Using Lasso Class)**: ```python from sklearn.linear_model import Lasso lasso_reg = Lasso(alpha=0.1) lasso_reg.fit(X, y) ```[cite: 4] ```
- **Option #2 (Using SGDRegressor)**: ```python from sklearn.linear_model import SGDRegressor sgd = SGDRegressor(alpha=1e-3, penalty='l1') ```[cite: 5] ```

### Elastic Net

- Elastic Net serves as a middle ground between Ridge and Lasso Regression, where the regularization term is a convex combination of both penalties .
- Controlled by the mix ratio $r$: when $r = 0$, Elastic Net is equivalent to Ridge Regression; when $r = 1$, it is equivalent to Lasso Regression .
- The Elastic Net cost function is defined as : $$ J(\mathbf{\theta}) = \text{MSE}(\mathbf{\theta}) + r\alpha \sum_{i=1}^{n} \vert{}\theta_i\vert{} + \frac{1-r}{2}\alpha \sum_{i=1}^{n} \theta_i^2 $$
- In Scikit-Learn, the mix ratio is set via `l1_ratio` (where `l1_ratio` $= r$, and the weight assigned to Ridge/$\ell_2$ is $1 - r$) .

### Regularization in Logistic Regression

- Logistic Regression models can be regularized using $\ell_1$, $\ell_2$, or Elastic Net penalties . Scikit-Learn adds an $\ell_2$ penalty by default .
- The amount of regularization is controlled by parameter $C$, which is the **inverse of the regularization rate** ($\alpha$) : $$ \arg\min \left( \text{regularization penalty} + C \times \text{cross entropy loss} \right) $$
- $C$ must be positive: a **smaller value** leads to stronger regularization, while a **larger value** leads to weaker regularization . Setting $C$ to infinity implements unregularized logistic regression .
- Default Scikit-Learn parameters for `LogisticRegression` include an $\ell_2$ penalty, $C = 1$, and the `lbfgs` solver . Supported solvers include `liblinear`, `newton-cg`, `sag`, `saga`, and `lbfgs` .

## 4.5 Hyper Parameter Tuning (HPT)

<img width="1000" height="563" alt="image" src="https://github.com/user-attachments/assets/237c8daa-2148-456a-8b74-cbb1c4620bf4" />

- **Hyperparameter Selection via Cross-Validation**: Regularization parameters such as $\alpha$ in Ridge and Lasso, or mix ratios in Elastic Net, must be tuned to minimize cross-validation errors . For instance, cross-validation can identify optimal regularization values (e.g., $\alpha = 0.0078$) .
- **Hyperparameter Spaces**: Tuning involves specifying distributions or grids for parameters such as polynomial degree, learning rate strategies (`constant`, `adaptive`, `invscaling`), and regularization mix ratios (`l1_ratio`) .
