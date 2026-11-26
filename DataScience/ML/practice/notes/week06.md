# 6. Modular classification topics in scikit-learn

### Module 1: Classification foundations and evaluation

- **6.1** – Classification functions in scikit-learn
- **6.2** – Evaluating classifiers

### Module 2: Perceptron and linear/probabilistic classifiers

- **6.3** – Demonstration: Binary class image classification with perceptron
- **6.4** – Demonstration: Multi class image classification with perceptron
- **6.5** – Demonstration: MNIST digits classification using SGDRegressor
- **6.6** – Demonstration: MNIST digits classification using logistic regression
- **6.7** – Demonstration: Zero detector with ridge classifier
- **6.8** – Demonstration: Multiclass classifier on MNIST dataset
- **6.9** – Demonstration: Naive Bayes classifier
- **6.10** – Demonstration: Softmax regression with MNIST

### Module 3: Instance-based and kernel classifiers

- **6.11** – Demonstration: KNN with MNIST
- **6.12** – Demonstration: SVC on MNIST dataset

### Module 4: Tree-based and ensemble classifiers

- **6.13** – Decision trees for classification – Abalone
- **6.14** – Decision trees for classification – Iris
- **6.15** – Bagging and random forest classifier on MNIST
- **6.16** – AdaBoost and GradientBoost classifier on MNIST

### Module 5: Neural network classifiers

- **6.17** – MLP classifier on MNIST dataset






# Module 1: Classification foundations and evaluation

## 6.1 – Classification functions in scikit-learn

### Overview of scikit-learn classification functionality

In this week, we will study sklearn functionality for implementing classification algorithms  . We will cover sklearn APIs for specific classification algorithms for least square classification, perceptron, and logistic regression classifier, with regularization, multiclass, multilabel and multi-output setting, various classification metrics, and cross validation and hyper parameter search for classification  .

### Specific classification APIs and `SGDClassifier`

We learnt how to implement the following classifiers with sklearn APIs  :

- Least square classification (`RidgeClassifier`)
- Perceptron (`Perceptron`)
- Logistic regression (`LogisticRegression`)

Alternatively we can use `SGDClassifier` with appropriate loss setting for implementing these classifiers  :

- `loss='log'` for logistic regression
- `loss='perceptron'` for perceptron
- `loss='squared_error'` for least square classification

### Common estimator methods

Classification estimators implement a few common methods like `fit`, `score`, `decision_function`, and `predict`  . Predictors help us in making predictions on data  . `Predict` method takes datasets as an input and returns predictions as an output  . They also provide another method which is the `score` method and its `score` method helps to measure the quality of predictions  . There is a `score` method for evaluating the classifier that takes feature metrics and label as input, and there is an optional argument about sample rate; `score` returns mean accuracy on the given test set and labels  .

There are a few common miscellaneous methods as follows:

- `get_params([deep])` gets parameter for this estimator
- `set_params(**params)` sets the parameters of this estimator
- `densify()` converts coefficient matrix to dense array format
- `sparsify()` converts coefficient matrix to sparse format

<img width="923" height="535" alt="image" src="https://github.com/user-attachments/assets/4a315296-247b-4f44-98c1-81d118aa2c5c" />

### Related scikit-learn API modules

Let us look at some of the concrete scikit-learn APIs:

- **Data API:** Helps us for loading, generating, and preprocessing the training and test set (`sklearn.datasets`, `sklearn.preprocessing`, `sklearn.impute`, feature selection, and `sklearn.feature_extraction`) .
- **Model API:** Helps us to implement supervised as well as unsupervised models, including classification modules (`sklearn.linear_model`, `sklearn.svm`, `sklearn.trees`, `sklearn.neighbors`, `sklearn.naive_bayes`, `sklearn.multiclass`), `sklearn.multioutput` for multi-output classification and regression, and `sklearn.cluster` for clustering algorithms .
- **Model Inspection API:** `sklearn.model_inspection` includes tools for model inspection .
- **Model Evaluation API:** Implements different metrics for model evaluation for classification, regression, and clustering .
- **Model Selection API:** Implements various model selection strategies like cross-validation, hyperparameter tuning, and plotting learning curves .
-


<img width="1400" height="933" alt="image" src="https://github.com/user-attachments/assets/0791f12b-857e-4c69-b4d7-68510343f0ab" />

## 6.2 – Evaluating classifiers

### Cross-validation strategies for classification

Many cross validation and hyper parameter tuning methods discussed in the regression context are also applicable in the classifiers, with additional methods specific to classifiers  .

`sklearn.model_selection` module provides the following three stratified APIs to create folds such that the overall class distribution is replicated in individual folds:

- `StratifiedKFold`
- `RepeatedStratifiedKFold`
- `StratifiedShuffleSplit`

> **Note:** Folds obtained via `StratifiedShuffleSplit` may not be completely different  .
>
>

<img width="600" height="300" alt="image" src="https://github.com/user-attachments/assets/d85cba27-4a59-4bd9-b39f-5baf00e14f1a" />

The `StratifiedKFold` class performs stratified sampling to produce folds that contain a representative ratio of each class  . At each iteration the code creates a clone of the classifier, trains that clone on the training folds, and makes predictions on the test fold  . Then it counts the number of correct predictions and outputs the ratio of correct predictions  .

```python
from sklearn.model_selection import StratifiedKFold
from sklearn.base import clone

skfolds = StratifiedKFold(n_splits=3, random_state=42)

for train_index, test_index in skfolds.split(X_train, y_train_5):
clone_clf = clone(sgd_clf)
X_train_folds = X_train[train_index]
y_train_folds = y_train_5[train_index]
X_test_fold = X_train[test_index]
y_test_fold = y_train_5[test_index]

clone_clf.fit(X_train_folds, y_train_folds)
y_pred = clone_clf.predict(X_test_fold)
n_correct = sum(y_pred == y_test_fold)
print(n_correct / len(y_pred))
```

`cross_val_score` can be called by specifying the estimator, training features, labels, cross-validation folds (`cv`), and scoring scheme (`scoring`). `cross_validate` can also be used with parameters including estimator, data, target, cross-validation iterator (`cv`), scoring scheme, `return_train_score=True`, and `return_estimator=True`, storing results in a Python dictionary with keys `fit_time`, `score_time`, `test_score`, `estimator` (optional), and `train_score` (optional). `cross_val_predict` is used for predicting the labels for examples in different test sets in a cross-validation manner. `RandomizedSearchCV` is used for hyperparameter tuning and searching for the best hyperparameters in a given budget.

### Classification scoring metrics in `sklearn.metrics`
`sklearn.metrics` implements a bunch of classification scoring metrics based on true labels and predicted labels as inputs. Most scoring functions take actual labels and predicted labels as input and return the score.

| Metric / Utility | Description from Source |
| :--- | :--- |
| `accuracy_score` | Classification scoring metric based on true labels and predicted labels |
| `balanced_accuracy_score` | Helps us to get accuracy score in case of imbalanced data set |
| `top_k_accuracy_score` | Classification scoring metric based on true labels and predicted labels |
| `roc_auc_score` | Gives us the score under the area under the curve of the ROC curve |
| `precision_score` | Measure derived from the confusion matrix |
| `recall_score` | Measure derived from the confusion matrix |
| `f1_score` | Measure derived from the confusion matrix |
| `log_loss` | Loss calculated for logistic regression, also known as cross-entropy loss |

<img width="3042" height="2180" alt="image" src="https://github.com/user-attachments/assets/282f67d3-9c8b-468b-9611-a045334b3f65" />

### Confusion matrix and classification report
`confusion_matrix` evaluates classification accuracy by computing the confusion matrix with each row corresponding to the true class. Entry $(i, j)$ in a confusion matrix is the number of observations actually in group $i$, but predicted to be in group $j$.

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_true, y_predicted)
```

<img width="516" height="387" alt="image" src="https://github.com/user-attachments/assets/3b7cb020-9b50-42c5-89df-144a80240331" />

You can also calculate confusion matrix from an estimator by providing the estimator object and the examples and actual labels (`ConfusionMatrixDisplay.from_estimator`), or plot the confusion matrix from predictions (`ConfusionMatrixDisplay.from_predictions`) and call `plt.show()` to display it.

The `classification_report` function builds a text report showing the main classification metrics. It has precision, recall, and f1-score by the class label, as well as accuracy, macro average, and weighted average (and support).

```python
from sklearn.metrics import classification_report
print(classification_report(y_true, y_predicted))
```

<img width="2445" height="1134" alt="image" src="https://github.com/user-attachments/assets/c0e71080-e531-40e5-aad1-0b657550413c" />

### Classifier performance across probability thresholds
`precision_recall_curve` is implemented in `sklearn.metrics` and gives us the value of precision and recall for every threshold. We do not pass the predicted labels as input to `precision_recall_curve`; instead we need to pass the probability scores or the output from the decision function. When plotted, it has recall on the X-axis and precision on the Y-axis, and from the precision-recall curve we get average precision.

```python
from sklearn.metrics import precision_recall_curve
precision, recall, thresholds = precision_recall_curve(y_true, y_predicted)
```

<img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/eeb5977c-e9bb-4f33-a978-bd05f376a373" />
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/34b959bf-826a-4f14-aa20-18870a0433f1" />

Similarly, `roc_curve` gives false positive rate (FPR) and true positive rate (TPR) for every threshold. When plotted, it has false positive rate (FPR) on the X-axis and true positive rate (TPR) on the Y-axis, and `roc_auc_score` gives the score under the area under the curve of the ROC curve

```python
from sklearn.metrics import roc_curve
fpr, tpr, thresholds = metrics.roc_curve(y_true, y_scores, pos_label=2)
```

<img width="1280" height="1280" alt="image" src="https://github.com/user-attachments/assets/daca2bca-cc31-4668-86ea-2b67c91c18e8" />

### Extending binary metrics to multiclass or multilabel problems
To extend a binary metric to multiclass or multilabel problems:
* Treat data as a collection of binary problems, one for each class.
* Then, average binary metric calculations across the set of classes.








---






# Module 2: Perceptron and linear/probabilistic classifiers

## 6.3 – Demonstration: Binary class image classification with perceptron

### Problem setup: Handwritten digit classification (0-detector)

We are given an image of a digit and we are required to predict the digit present in the image, which is an instance of a multiclass classification problem with 10 digits in total  . Since a single perceptron could only be used for binary classification, we consider only two classes in the first half before extending it to the multiclass setting  .

Suppose we want to recognize whether the given image is of digit zero or not (digits other than zero)  :

- The problem is cast as a binary classification problem (called a **0 detector**) .
- Since the original label vector contains 10 classes, the label `0` is changed to `+1` (`1`), and all other labels (non-zero digits) are changed to `-1` .
- The input matrices are close to sparse (there are lots of `0` / black pixels in the matrix than non-zero pixels) .
-

<img width="704" height="546" alt="image" src="https://github.com/user-attachments/assets/00fac586-d1d0-4fee-9237-aa6c4a3a67e4" />

### Recap (Theory) of Perceptron

1. **Training data**: $(\text{features}, \text{label})$ or $(X, y)$, where label $y$ is a discrete number from a finite set, and features in this case are pixel values of an image .
2. **Model**: 

$$
h_w : y = g(w^T x) = g(w_0 + w_1 x_1 + \dots + w_m x_m) 
$$

where:
- $w$ is a weight vector in $\mathbb{R}^{m+1}$ with components $(w_0, w_1, \dots, w_m)$ .
- $g(\cdot)$ is a non-linear activation function given by a signum function: if the linear combination is $\ge 0$, we assign the label $+1$; otherwise (if the linear combination is negative), we assign the label $-1$ .
3. **Perceptron loss**: 

$$
\text{Loss} = \max\left(-w^T x^{(i)} y^{(i)}, 0\right) 
$$

Internally, `Perceptron()` calls hinge loss with `0` as a threshold and uses SGD to update the weights . Alternatively, `SGDClassifier(loss='perceptron')` can be used to build a perceptron model .
4.

<img width="880" height="376" alt="image" src="https://github.com/user-attachments/assets/6a2647b3-36ce-422f-a963-4d10cbca0225" />

### Baseline model, training, and evaluation

- **Baseline model**: Constructed using `DummyClassifier(strategy='most_frequent')`, which counts the number of samples per class (`5923` positive samples for digit `0` and `54077` negative samples) and always outputs the class with the highest number of samples (`-1`), yielding a baseline accuracy of `0.90` ($90\%$) .
- **Instantiation & Training**:
- Create an instance of binary classifier (`bin_clf = Perceptron()`) and call `.fit()` to train the model .
- Setting `warm_start=True` in the `Perceptron` constructor initializes every new run with the weights obtained from the previous run, which speeds up training and is useful when training in batches on large-scale data .
- On the training set, the model gets an accuracy of `0.99`, and on the test set, an accuracy of `0.98` .
-
- **Metrics & Cross-validation**:
- Using `confusion_matrix` and `classification_report(y_train_0, y_hat_train_0)` (or `precision_score` and `recall_score`), for class `1.0` (digit `0`) the model achieves precision of `0.96`, recall of `0.92`, and F1-score of `0.94` (overall accuracy `0.99`) .
- Using `cross_validate` with $k = 5$ folds (`cv=5`), the estimator is internally cloned 5 times and fitted independently on each fold, yielding:
- `f1`: avg `0.94`, std `0.013`
- `precision`: avg `0.96`, std `0.01`
- `recall`: avg `0.92`, std `0.03`

<img width="877" height="509" alt="image" src="https://github.com/user-attachments/assets/6a4d86a0-fc9d-4a99-8e22-f45f39044ddd" />

## 6.4 – Demonstration: Multi class image classification with perceptron

### Multiclass Classifier (OneVsAll)

- A single perceptron is a binary classifier, whereas the MNIST dataset contains 10 class labels (`0, 1, 2, 3, 4, 5, 6, 7, 8, 9`) .
- **Solution**: Combine multiple binary classifiers (One-vs-All / One-vs-Rest strategy) and devise a suitable scoring metric .
- **Label representation**: In a multiclass setup, each example is marked with a single label out of $k$ labels (shape `(n, 1)`), or transformed into a one-hot encoding representation using `LabelBinarizer` .
- In scikit-learn, `Perceptron` automatically handles multiclass labels without modifying a single line of code written for the binary classifier :
- `clf.coef_.shape` is `(10, 784)` and `clf.intercept_.shape` is `(10,)`, forming a matrix of size $10 \times 784$ where each row represents the weights for a single binary classifier .
- During inference, there is no signum function associated with the multiclass perceptron; the input is passed through all 10 binary classifiers, and the class corresponding to the highest value of `decision_function` is selected as the predicted class .
-

<img width="884" height="406" alt="image" src="https://github.com/user-attachments/assets/a02b084b-a224-4ad4-b516-e4a45818312b" />

### Pipeline, GridSearchCV, and visualizing weights

- **Pipeline**: Created with `MinMaxScaler()` to scale features within the range `(0, 1)` followed by `Perceptron()`: ```python estimators = [('std_scaler', MinMaxScaler()), ('bin_clf', Perceptron())] ```
- **Evaluation**: On the training set, `classification_report(y_train, y_hat)` shows an overall accuracy of `0.90`, with `ConfusionMatrixDisplay.from_predictions(y_train, y_hat, values_format='.5g')` showing that Digit 2 is often confused with Digit 3 .
- **Hyperparameter Tuning**: Using `GridSearchCV` with `scoring=make_scorer(hinge_loss, greater_is_better=False)` across learning rates `lr_grid = [1/2**n for n in range(1, 6)]` discovers a learning rate with much lower training loss than the default learning rate of `1` (`best_bin_clf = bin_clf_gscv.best_estimator_`). However, no matter what the learning rate is, the loss will never converge to zero as the classes are not linearly separable .
- **Visualizing weight vectors**: Reshaping `bin_clf.coef_` into a $28 \times 28$ matrix (`w.reshape(28, 28)`) and plotting with `plt.imshow(w_matrix, cmap='gray')` shows lighter shades for higher weight values and darker shades for lower weight values (e.g., `-60` to `+20`), allowing inspection of what patterns the classifier learned and diagnosing false positives .

## 6.5 – Demonstration: MNIST digits classification using SGDRegressor

### `SGDRegressor` estimator overview and step-wise optimization

- The lecture introduces the demonstration using `SGDClassifier` / `SGDRegressor` iterative training before solving logistic regression analytically/via solvers, noting that `SGDRegressor` implements a stochastic gradient descent algorithm for linear models and is suited for large training setups ($\gt 10\text{k}$ samples) .
- **Key hyperparameters**:
- `loss`: Set to supported values such as `'squared_error'` or `'huber'` .
- `penalty`: `'l1'`, `'l2'`, `'elasticnet'`, or `None` .
- `learning_rate`: `'constant'`, `'optimal'`, `'invscaling'`, or `'adaptive'` .
- `eta0`: Initial learning rate (e.g., `0.01` or `0.001`) .
- `early_stopping`: Stops training when the validation score does not improve by at least `tol` for `n_iter_no_change` consecutive epochs, or after reaching `max_iter` .
- `shuffle`: `True` to shuffle training data after each epoch .
- `average`: Updates the weight vector to the average of weights from previous updates to stabilize noisy single-sample SGD updates .

### Step-wise training and learning curves (`warm_start=True`)

To monitor SGD loss iteration by iteration, set `max_iter=1`, `tol=-np.infty`, and `warm_start=True` inside a `Pipeline` with `StandardScaler()`  :

```python
sgd_pipeline = Pipeline([
("feature_scaling", StandardScaler()),
("SGD", SGDRegressor(max_iter=1, tol=-np.infty, warm_start=True, random_state=42))
])

loss = []
for epoch in range(100):
sgd_pipeline.fit(train_features, train_labels)  # continues where it left off
loss.append(mean_squared_error(train_labels, sgd_pipeline.predict(train_features)))
```

Because `warm_start=True`, calling `.fit()` continues updating the parameters learned in the previous iteration. Plotting `loss` against iteration number (`Iteration#`) displays the learning curve to inspect convergence across different initial learning rates (`eta0`), where an ideal learning curve shows the training loss reducing monotonically as training progresses.

<img width="905" height="460" alt="image" src="https://github.com/user-attachments/assets/4b6f4eb7-c5c0-4cee-85f4-73a5f1737904" />

---

## 6.6 – Demonstration: MNIST digits classification using logistic regression

### Binary classification setup (0-detector)
* Logistic regression is the workhorse of machine learning and works in binary, multiclass, and multilabel classification setups.
* **Preprocessing**: Features are scaled using `MinMaxScaler()` so each feature lies in the range $[0, 1]$.
> **Note**: Do not apply mean centering as it removes zeros from the data; however, zeros should remain zeros in the dataset.
* **Labels**: Unlike perceptron (where negative labels were set to `-1`), for logistic regression the label `'0'` is changed to `1` and all other labels (`1–9`) are changed to `0`:
```python
y_train_0 = np.zeros((len(y_train)))
y_test_0 = np.zeros((len(y_test)))
```


* **Baseline**: `DummyClassifier(strategy='most_frequent')` yields `0.90` ($90\%$) accuracy because `54,077` out of `60,000` training samples belong to class `0` (non-zero digits).

<img width="890" height="333" alt="image" src="https://github.com/user-attachments/assets/e01461b3-a3b9-49d0-8f90-041a4036b71a" />

### Iterative training with `SGDClassifier(loss='log')`
* **Without regularization**:
* Configured with `SGDClassifier(loss='log', penalty='l2', max_iter=1, warm_start=True, eta0=0.01, alpha=0, learning_rate='constant', random_state=1729)`.
* Training and test accuracy both reach `0.99`.
* On the training set (`classification_report`), class `0.0` achieves precision `1.00`, recall `1.00`, F1-score `1.00`, while class `1.0` (digit `0`) achieves precision `0.98`, recall `0.96`, and F1-score `0.97`.
* 5-fold cross-validation (`cross_validate(..., cv=5, scoring=['precision', 'recall', 'f1'], return_train_score=True, return_estimator=True)`) produces 5 estimators; the best model (index 2 / third model) achieves the highest F1-score (`0.96096096` on split 2).
* **With L2 regularization (`alpha=0.001`)**:
* Configured with `penalty='l2'`, `alpha=0.001`, `eta0=0.01`, `learning_rate='constant'`.
* Training loss is slightly higher than the unregularized model (expected behavior), bias is `-4.43`, and `67` weights equal `0`.
* Training and test accuracy remain `0.99`, while for class `1.0` precision is `0.98`, recall drops from `0.96` to `0.93`, and F1-score is `0.95`.
* **Curves and Hyperparameter Search**:
* Plotting `precision_recall_curve` (recall on x-axis, precision on y-axis) and `roc_curve` (FPR on x-axis, TPR on y-axis) produces an ROC AUC score (`roc_auc_score`) of `0.998`.
* `RandomizedSearchCV` with `loguniform` distribution is used to search for optimal hyperparameters within a fixed budget, while `LogisticRegression` and `LogisticRegressionCV` (which default to L2 regularization controlled by inverse regularization strength `C`, or `C=np.infty` for no regularization) solve the optimization directly using solvers like `'lbfgs'`.

<img width="921" height="552" alt="image" src="https://github.com/user-attachments/assets/51977b55-68a1-46ed-a6eb-343b584c644c" />

---

## 6.7 – Demonstration: Zero detector with ridge classifier

### Formulation and implementation
* `RidgeClassifier` is a classifier variant of the Ridge regressor that casts binary classification as a **least-squares classification** problem and finds optimal weights using matrix decomposition techniques such as **SVD**.
* **Target encoding**: First converts binary targets to $\{-1, +1\}$ and then treats the problem as a regression task, minimizing a penalized residual sum of squares:
```python
y_train_0 = -1 * np.ones((len(y_train)))
y_test_0 = -1 * np.ones((len(y_test)))
indx_0 = np.where(y_train == '0')
y_train_0[indx_0] = 1
```


* **Parameters and Pipeline**:
* By default, `RidgeClassifier` implements L2 regularization with `alpha=1.0`, `fit_intercept=True`, and `solver='auto'` (note: the `normalize` parameter is deprecated).
* To train without regularization, set `alpha=0`:
```python
estimator = RidgeClassifier(normalize=False, alpha=0)
pipe_ridge = make_pipeline(MinMaxScaler(), estimator)
pipe_ridge.fit(x_train, y_train_0)
```


### Evaluation and weight visualization across models
* **Performance**: On the test set (`classification_report`), overall accuracy is `0.98`; for class `1.0` (digit `0`), precision is `0.95`, recall is `0.87`, and F1-score is `0.91`. Using 5-fold `cross_validate` and selecting the best estimator via `np.argmax` on train F1-score (index `3`, the fourth estimator) improves recall by 1 percentage point to `0.88` while precision remains `0.95`.
* **Comparing learned weight vectors**: Reshaping `coef_` into $28 \times 28$ grids and setting negative weights to `0` (`w_matrix[w_matrix < 0] = 0`) across four models (`'sgd'`, `'regularized sgd'`, `'logit'`, `'ridge'`) shows:
* Regularized SGD drives lower weights in certain regions down to `0` compared to unregularized SGD.
* Logistic regression learns pixel weight patterns very similar to SGD.
* Ridge classifier produces a noticeably different weight map with a black patch in the center (where many weights were $< 0$ and clipped to `0`) and positive weights placed on pixels near the boundary.

---

## 6.8 – Demonstration: Multiclass classifier on MNIST dataset

### One-vs-All (OvR) multiclass classification with `SGDClassifier`
* Here we solve the actual 10-class MNIST digit recognition problem (`0` through `9`).
* **Pipeline setup**:
```python
estimator = SGDClassifier(loss='log',
penalty='l2',
max_iter=1,
warm_start=True,
eta0=0.01,
alpha=0,
learning_rate='constant',
random_state=1729)
pipe_sgd_ovr = make_pipeline(MinMaxScaler(), estimator)
```


* **Training & Inference mechanism**:
* Behind the scenes, the library automatically creates **10 binary classifiers** (One-vs-All) and trains them.
* Tracking `log_loss(y_train, pipe_sgd_ovr.predict_proba(x_train))` over 100 iterations shows a smoothly decreasing cross-entropy loss curve.
* The learned weight matrix `pipe_sgd_ovr[1].coef_` has shape `(10, 784)`, where each row corresponds to the weights of a single binary classifier.
* During inference, an input image is passed through all 10 classifiers, and the class with the highest output score is selected as the predicted class.

### Multiclass `LogisticRegression` using solvers and error analysis
* **Solver-based implementation**:
```python
pipe_logit_ovr = make_pipeline(MinMaxScaler(),
LogisticRegression(random_state=1729,
solver='lbfgs',
C=np.infty))
pipe_logit_ovr.fit(x_train, y_train)
```


* **Confusion matrix & classification report**:
* On the test set (10,000 samples), `SGDClassifier` achieves `0.92` accuracy, with notable confusions between labels `2` and `8`, `5` and `8`, `5` and `3`, and `9` and `4` (Class `8` and Class `5` have F1-scores of `0.87`).
* `LogisticRegression` with `'lbfgs'` achieves `0.93` test accuracy and improves F1-scores on difficult classes such as Class `5` (`0.89`) and Class `8` (`0.88`).
* Visualizing the 10 learned weight vectors ($W_0$ through $W_9$) reshaped to $28 \times 28$ shows the distinct pixel patterns associated with each digit.

<img width="898" height="597" alt="image" src="https://github.com/user-attachments/assets/0be32362-6ff0-4523-b23c-55f1e81f8893" />

---

## 6.9 – Demonstration: Naive Bayes classifier

### Theory and Naive Bayes variants in `sklearn.naive_bayes`
* **Conditional independence assumption**: Naive Bayes classifier applies Bayes' theorem with the "naive" assumption of conditional independence between every pair of features given the value of the class variable. For a given class variable $y$ and dependent feature vector $x_1$ through $x_m$:

$$
P(x_i \mid y, x_1, \dots, x_{i-1}, x_{i+1}, \dots, x_m) = P(x_i \mid y)
$$


Naive Bayes learners and classifiers can be extremely fast compared to more sophisticated methods.
* **List of NB Classifiers in `sklearn.naive_bayes`**:
* `GaussianNB` (for continuous features)
* `BernoulliNB` (for binary features)
* `MultinomialNB` (for multinomial distributions, such as text classification)
* `CategoricalNB` (for categorical features)
* `ComplementNB` (performs better than `MultinomialNB` in cases of class imbalance)
* All classifiers implement the `.fit(X, y)` method to estimate parameters and `.predict(X)` for prediction.

### Demonstration: Text classification on 20 Newsgroups dataset
1. **Data loading**: Fetch 4 categories (`'talk.religion.misc'`, `'soc.religion.christian'`, `'sci.space'`, `'comp.graphics'`) from the 20 Newsgroups dataset using `fetch_20newsgroups`:
```python
from sklearn.datasets import fetch_20newsgroups
categories = ['talk.religion.misc', 'soc.religion.christian', 'sci.space', 'comp.graphics']
train = fetch_20newsgroups(subset='train', categories=categories)
test = fetch_20newsgroups(subset='test', categories=categories)
```

2. **Preprocessing and modeling pipeline**: Because the dataset consists of raw text documents, `TfidfVectorizer()` converts text input into a vector of numerical values before passing it to `MultinomialNB()`:
```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.pipeline import make_pipeline

model = make_pipeline(TfidfVectorizer(), MultinomialNB())
model.fit(train.data, train.target)
```

3. **Evaluation**: Plotting the test set confusion matrix with `ConfusionMatrixDisplay.from_estimator(model, test.data, test.target, display_labels=test.target_names, xticks_rotation='vertical')` shows:
* `comp.graphics` and `sci.space` are well separated.
* Expected confusion occurs between `soc.religion.christian` and `talk.religion.misc`.

---

## 6.10 – Demonstration: Softmax regression with MNIST

### Mathematical formulation of Softmax Regression
Softmax Regression (also called **Multinomial Logistic Regression**) generalizes Logistic Regression to support multiple mutually exclusive classes directly without training and combining multiple binary classifiers.

1. **Softmax score for class $k$**: Given an instance $\mathbf{x}$, the model computes a score $s_k(\mathbf{x})$ for each class $k$, where each class has its own dedicated parameter vector $\boldsymbol{\theta}^{(k)}$ (stored as rows in a parameter matrix $\boldsymbol{\Theta}$):

$$
s_k(\mathbf{x}) = \mathbf{x}^T \boldsymbol{\theta}^{(k)}
$$


2. **Softmax function (normalized exponential)**: Estimates the probability $\hat{p}_k$ that instance $\mathbf{x}$ belongs to class $k$ by computing the exponential of every score and dividing by the sum of all exponentials:


$$
\hat{p}_k = \sigma(\mathbf{s}(\mathbf{x}))_k = \frac{\exp(s_k(\mathbf{x}))}{\sum_{j=1}^{K} \exp(s_j(\mathbf{x}))}
$$


where $K$ is the number of classes and $\mathbf{s}(\mathbf{x})$ is the vector containing the scores of each class for instance $\mathbf{x}$.
3. **Prediction**: Predicts the class with the highest estimated probability (which is the class with the highest score):

$$
\hat{y} = \underset{k}{\operator-name{argmax}} \, \sigma(\mathbf{s}(\mathbf{x}))_k = \underset{k}{\operator-name{argmax}} \, s_k(\mathbf{x}) = \underset{k}{\operator-name{argmax}} \, \left((\boldsymbol{\theta}^{(k)})^T \mathbf{x}\right)
$$

<img width="880" height="615" alt="image" src="https://github.com/user-attachments/assets/b5cd4777-2189-466c-9681-574e9730ee37" />

### Cross-entropy cost function, gradient, and scikit-learn setup
* **Cross-entropy cost function**: Penalizes the model when it estimates a low probability for the target class:


$$
J(\boldsymbol{\Theta}) = -\frac{1}{m} \sum_{i=1}^{m} \sum_{k=1}^{K} y_k^{(i)} \log\left(\hat{p}_k^{(i)}\right)
$$

where $y_k^{(i)}$ is the target probability ($1$ or $0$) that the $i^{\text{th}}$ instance belongs to class $k$. When $K = 2$, this reduces to the binary Logistic Regression log loss.
* **Cross-entropy gradient vector for class $k$**:

$$
\nabla_{\boldsymbol{\theta}^{(k)}} J(\boldsymbol{\Theta}) = \frac{1}{m} \sum_{i=1}^{m} \left(\hat{p}_k^{(i)} - y_k^{(i)}\right) \mathbf{x}^{(i)}
$$

* **Scikit-Learn implementation on MNIST**:
* In `LogisticRegression`, setting `multi_class='multinomial'` switches from One-vs-Rest (`'ovr'`) to Softmax Regression, paired with a compatible solver such as `solver='lbfgs'` (and inverse L2 regularization hyperparameter `C`, or `C=np.infty` for unregularized training) wrapped in a `MinMaxScaler()` pipeline on MNIST (`X_train`, `y_train` across 10 digit classes).






---








# Module 3: Instance-based and kernel classifiers

## 6.11 – Demonstration: KNN with MNIST

### Dataset and setup

We use the MNIST dataset, where each datapoint is contained in $x \in \mathbb{R}^{784}$ and the label $y \in \{0, 1, 2, 3, 4, 5, 6, 7, 8, 9\}$  . Data is fetched via `fetch_openml('mnist_784', version=1, return_X_y=True)`, converted to NumPy arrays via `.to_numpy()`, and split into training (`X[:60000]`, `y[:60000]`) and test (`X[60000:]`, `y[60000:]`) sets  .

For binary classification (and eventually extending to a multiclass setup), label `1` is assigned to the digit `0` and the rest of the digits get labeled `0` (or initialized as `-1` with `0` indices set to `1`)  .

<img width="909" height="678" alt="image" src="https://github.com/user-attachments/assets/af76c93a-4cbb-4e72-a552-aa22d74ebf7d" />

### K-Nearest Neighbors (k-NN) algorithm

Nearest neighbor classifiers are a type of instance-based learning (or non-generalizing learning) that do not attempt to construct an explicit parametric model; instead, they store instances of the training data and compute classification from a simple majority vote of the nearest neighbors of each data point . Scikit-Learn provides two implementations of nearest neighbor classifiers: `KNeighborsClassifier` and `RadiusNeighborsClassifier` .

The k-NN classification algorithm steps are:

1. Set $k$ to the desired value (how many neighbors should be allowed to participate in prediction).
2. Calculate the distance between the new example and every example from the data to create a distance vector.
3. Get the indices of the nearest $k$ neighbors.
4. Get the labels of the selected $k$ entries.
5. If it is a classification task, return the majority class by computing the mode of the $k$ labels .

### Applying `KNeighborsClassifier` and `NearestNeighbors`

To apply `KNeighborsClassifier`:

1. Instantiate a `KNeighborsClassifier` estimator (e.g., `kneighbor_classifier = KNeighborsClassifier()` or with `n_neighbors=3`) .
2. Call the `.fit()` method on the `KNeighborsClassifier` object with the training feature matrix and label vector as arguments .
3.

Key implementation details and considerations from the sources:

- **Small-sample / 2D PCA visualization**: Training with a small number of samples (such as 10 samples from the training set) puts datapoints in metric space without building a parameterized model, allowing visualization of test points (marked with stars along with their labels) and combining a PCA pipeline (`pipe_pca_2d`) with `KNeighborsClassifier(n_neighbors=3)` .
- **Distance and connectivity via `NearestNeighbors`**: To inspect the distance and connectivity of $k$ neighbors for each testing point, use the `NearestNeighbors` class; `KNeighborsClassifier` calls `NearestNeighbors` internally to compute distances .
- **Distance algorithms and feature scaling**: The distance computation algorithm can be specified via `algorithm` (`'ball_tree'`, `'kd_tree'`, `'brute'`, or `'auto'`) . Because k-NN uses Euclidean distance computations, having all features on the same scale is crucial so features with wider variance do not disproportionately dominate distance calculations; scalers such as `MinMaxScaler` or `StandardScaler` can be used in a pipeline .
- **Multilabel capability and hyperparameter tuning**: `KNeighborsClassifier` supports multilabel classification (such as predicting `y_multilabel = np.c_[y_train_large, y_train_odd]`), and grid search over `weights` and `n_neighbors` can be used for hyperparameter tuning on MNIST .

<img width="928" height="494" alt="image" src="https://github.com/user-attachments/assets/bea7d60e-520a-49e9-b0eb-7da2d18e0002" />

## 6.12 – Demonstration: SVC on MNIST dataset

### Support Vector Machines in scikit-learn

Support Vector Machines (SVMs) are a set of supervised learning methods used for classification, regression, and outlier detection that construct a hyperplane or set of hyperplanes in a high- or infinite-dimensional space .

In scikit-learn, SVM classification is implemented via three primary classes:

- `SVC` (C-Support Vector Classification)
- `NuSVC`
- `LinearSVC`

| Class | Time Complexity | Out-of-Core Support | Scaling Required | Kernel Trick |
| --- | --- | --- | --- | --- |
| `LinearSVC` | $O(m \times n)$ | No | Yes | No |
| `SGDClassifier` | $O(m \times n)$ | Yes | Yes | No |
| `SVC` | $O(m^2 \times n) \text{ to } O(m^3 \times n)$ | No | Yes | Yes |

- **`SVC` and `NuSVC`**: Based on the `libsvm` library and support the kernel trick, making them suitable for complex small- or medium-sized training sets, though they become very slow when the number of training instances gets large . In `NuSVC`, instead of `C`, $\nu \in (0, 1]$ controls the number of support vectors and margin errors (an upper bound on the fraction of margin errors and a lower bound on the fraction of support vectors) .
- **`LinearSVC`**: Based on the `liblinear` library; a faster implementation of linear SVM using only a linear kernel .
- **`SGDClassifier`**: Setting `loss="hinge"` and `alpha=1/(m*C)` trains a linear SVM classifier via Stochastic Gradient Descent, supporting out-of-core training for huge datasets that do not fit in memory .

### Loading MNIST and inspecting support vectors

In the demonstration, MNIST is loaded via `mnist.load_data()` from `keras.datasets`, returning `(X_train, y_train)` with shape `(60000, 28, 28)` and `(X_test, y_test)` with shape `(10000, 28, 28)`  . Each $28 \times 28$ image is flattened into a vector of length `784` (`60000` by `784` for training and `10000` by `784` for testing) and normalized by dividing each pixel value by `255`  . For faster experimentation in the notebook, the first `10000` images are taken as the training set and the first `2000` images in the test set as the test set  .

After fitting an `SVC` estimator (`clf = SVC_classifier.fit(X_train, y_train)`), attributes revealing support vector details include:

- `clf.support_`: Indices of the support vectors
- `clf.support_vectors_`: The support vectors
- `clf.n_support_`: Number of support vectors for each class
-

### Multiclass strategy, confidence scores, and probability calibration

- **Multiclass classification**: `SVC` and `NuSVC` implement the `"one-versus-one"` (`'ovo'`) approach (or `'ovr'` via `decision_function_shape`), training 45 binary classifiers for 10 classes and selecting the class that wins the most duels . `LinearSVC` implements `"one-vs-the-rest"` (`'ovr'`) or `'crammer_singer'` via `multi_class` .
- **Confidence scores and probability estimates**: Unlike Logistic Regression classifiers, standard SVM classifiers do not output probabilities for each class; instead, they output the distance between the test instance and the decision boundary as a confidence score . Setting `probability=True` when instantiating `SVC` calibrates probabilities using Logistic Regression on the SVM's scores via an additional five-fold cross-validation on the training data, adding `.predict_proba()` and `.predict_log_proba()` methods .

<img width="887" height="438" alt="image" src="https://github.com/user-attachments/assets/bf659cc6-bd5d-4f24-ace5-d4a45a451819" />

### Linear SVM, nonlinear RBF SVM, and `GridSearchCV` on MNIST

- **Linear SVM**: A pipeline using `MinMaxScaler` followed by `SVC(kernel='linear', C=1)` trained on MNIST achieves a validation/training accuracy of `91.07%`, and its confusion matrix can be plotted via `disp.plot()` .
- **Nonlinear SVM (`rbf` kernel)**: A pipeline using `MinMaxScaler` followed by `SVC(kernel='rbf', gamma=0.1, C=1)` evaluated with `cross_val_score(..., cv=2)` reports a training accuracy of `82.87%` in the demonstration output .
- **Hyperparameter tuning with `GridSearchCV`**: Using `StandardScaler` with `SVC(kernel='rbf')`, a parameter grid over `gamma` and `C`, and `StratifiedShuffleSplit` validation inside `GridSearchCV`, the best parameters and best score are accessed via `grid.best_params_` and `grid.best_score_` .





---





# Module 4: Tree-based and ensemble classifiers

## 6.13 – Decision trees for classification – Abalone

### Dataset and problem background

In this lecture, we use the Abalone data set for classification  . Abalone is a type of consumable snail whose price varies as per its age, and the aim is to predict the age of abalone from physical measurements  . The age of abalone is traditionally determined by cutting the shell through the cone, staining it, and counting the number of rings through a microscope—a boring and time-consuming task  . Other measurements, which are easier to obtain, are used to predict the age as an alternative method using decision trees  .

The dataset is loaded from the UCI machine learning repository in CSV format using `pandas.read_csv` with the following column names:

- `Sex`
- `Length`
- `Diameter`
- `Height`
- `Whole weight`
- `Shucked weight`
- `Viscera weight`
- `Shell weight`
- `Rings`

### Model training, tuning, and observations

This lecture demonstrates decision trees with continuous and discrete features to predict the number of rings in Abalone  .

- A classification report by class shows a bunch of classes getting `0` precision and recall, with the highest F1-score obtained for number of rings equal to `7` and an overall accuracy of `25%` (`0.25`) .
- Creating a new pipeline with the best identified features, setting `max_depth = 5` and `min_samples_split = 4`, and fitting the pipeline on the training data gives a model score of `0.27`—an improvement over the earlier score of `25%` (`0.25`) .
- In general, accuracy is quite low on this dataset; suggested exercises include evaluating the classifier on the test set and attempting the Abalone problem as a regression problem to check if a better model is obtained .
-

<img width="934" height="453" alt="image" src="https://github.com/user-attachments/assets/4194ae8a-3e63-4962-862e-fcc8471991f1" />

## 6.14 – Decision trees for classification – Iris

### Setup and pipeline training

In this lecture, we demonstrate a decision tree in a classification setup with the Iris dataset  .

- Libraries imported include `pandas`, `NumPy`, `matplotlib.pyplot`, `trees` from `sklearn`, `DecisionTreeClassifier` and `export_text` from `sklearn.tree`, along with `train_test_split`, `GridSearchCV`, `classification_report`, `confusion_matrix`, `ConfusionMatrixDisplay`, `pipeline`, `MinMaxScaler`, and `load_iris` from `sklearn.datasets` .
- The Iris dataset is loaded via `load_iris(return_X_y=True, as_frame=True)` to return a feature matrix and label vector, then split into training and test sets using `train_test_split` .
- A pipeline (`dt_pipeline`) with `feature_scaling` (`MinMaxScaler()`) and `dt_classifier` (`DecisionTreeClassifier(max_depth=3, random_state=42)`) is trained on `(train_features, train_labels)` .
-

### Test set evaluation

Once the classifier is trained, it is evaluated on the test set with:

- `ConfusionMatrixDisplay.from_estimator(dt_pipeline, test_features, test_labels)` displaying true labels on the y-axis and predicted labels on the x-axis
- `classification_report(test_labels, dt_pipeline.predict(test_features))` showing precision, recall, f1-score, and support for each class
-

<img width="914" height="673" alt="image" src="https://github.com/user-attachments/assets/ae8ace4e-a632-4c41-9197-748a764bb0f7" />

## 6.15 – Bagging and random forest classifier on MNIST

### Bagging and pasting concepts

One way to get a diverse set of classifiers is to use very different training algorithms, while another approach is to use the same training algorithm for every predictor and train them on different random subsets of the training set  . When sampling is performed with replacement, this method is called **bagging** (short for bootstrap aggregating), and when sampling is performed without replacement, it is called **pasting**  .

- Both bagging and pasting allow training instances to be sampled several times across multiple predictors, but only bagging allows training instances to be sampled several times for the same predictor .
- Once all predictors are trained, the ensemble makes a prediction for a new instance by aggregating the predictions of all predictors—typically using the statistical mode (most frequent prediction, as in a hard voting classifier) for classification, or the average for regression .
- Each individual predictor has a higher bias than if trained on the original training set, but aggregation reduces both bias and variance; generally, the ensemble has a similar bias but lower variance than a single predictor trained on the original training set .
-

<img width="907" height="526" alt="image" src="https://github.com/user-attachments/assets/b0d8f314-2690-4c8e-bf97-0647e607725d" />

### Random forest classifier and pixel importance on MNIST

On the MNIST dataset (70,000 small images of handwritten digits, each $28 \times 28$ pixels / 784 features, split into 60,000 training and 10,000 test images), `RandomForestClassifier` can be trained on the dataset and evaluated for feature importances  .

- Training a `RandomForestClassifier` (e.g., `rnd_clf = RandomForestClassifier(n_estimators=500, n_jobs=-1)`) on MNIST allows plotting each pixel's importance (`rnd_clf.feature_importances_`) to see which pixels matter most .
- Random Forests are very handy to get a quick understanding of what features actually matter, in particular if you need to perform feature selection .
- Exercises for MNIST include building a classifier achieving over `97%` accuracy on the test set (such as tuning `KNeighborsClassifier` via grid search over `weights` and `n_neighbors`) and applying data augmentation / training set expansion by adding four shifted copies (left, right, up, or down by one pixel) for each image in the training set .
-


<img width="896" height="467" alt="image" src="https://github.com/user-attachments/assets/89eac7b4-9d34-4bb9-90ab-96e9e6244241" />



## 6.16 – AdaBoost and GradientBoost classifier on MNIST

### Boosting overview and AdaBoost algorithm

Boosting (originally called hypothesis boosting) refers to any Ensemble method that can combine several weak learners into a strong learner by training predictors sequentially, each trying to correct its predecessor  .

In the **AdaBoost** algorithm:

1. Each instance weight $w^{(i)}$ is initially set to $\frac{1}{m}$, and a first predictor is trained to compute its weighted error rate $r_j$ on the training set : 

$$
r_j = \frac{\sum_{\substack{i=1 \\ \hat{y}_j^{(i)} \neq y^{(i)}}}^{m} w^{(i)}}{\sum_{i=1}^{m} w^{(i)}}
$$

where $\hat{y}_j^{(i)}$ is the $j^{\text{th}}$ predictor's prediction for the $i^{\text{th}}$ instance .

2. The predictor's weight $\alpha_j$ is computed using: 

$$
\alpha_j = \eta \log \frac{1 - r_j}{r_j} 
$$

where $\eta$ is the learning rate hyperparameter (defaults to `1`); more accurate predictors receive higher weight, random guessing yields weight near zero, and worse-than-random accuracy yields negative weight .

3. Instance weights are updated to boost the weights of misclassified instances : 

$$
w^{(i)} \leftarrow \begin{cases} w^{(i)} & \text{if } \hat{y}_j^{(i)} = y^{(i)} \\ w^{(i)} \exp(\alpha_j) & \text{if } \hat{y}_j^{(i)} \neq y^{(i)} \end{cases}
$$


for $i = 1, 2, \dots, m$, and then all instance weights are normalized by dividing by $\sum_{i=1}^{m} w^{(i)}$ .
4. A new predictor is trained using the updated weights, repeating until the desired number of predictors is reached or a perfect predictor is found .
5. To make predictions, AdaBoost weighs the predictions of all predictors using $\alpha_j$ and selects the class receiving the majority of weighted votes : 

$$
\hat{y}(\mathbf{x}) = \underset{k}{\operator-name{argmax}} \sum_{\substack{j=1 \\ \hat{y}_j(\mathbf{x}) = k}}^{N} \alpha_j 
$$

where $N$ is the number of predictors .


### Gradient boosting considerations and MNIST ensemble workflow

- **Gradient Boosting**: Another popular boosting method that trains predictors sequentially to correct predecessors; when a Gradient Boosting ensemble overfits the training set, decreasing the learning rate is a key regularization adjustment .
- **MNIST dataset and classifier comparison**: Using the MNIST dataset (with options such as splitting into 50,000 training, 10,000 validation, and 10,000 test instances), multiple classifiers—such as a Random Forest classifier, an Extra-Trees classifier, and an SVM classifier—can be trained and combined into an ensemble (using soft or hard voting) and compared against individual classifiers on validation and test sets .

<img width="928" height="575" alt="image" src="https://github.com/user-attachments/assets/effd365e-7188-4705-837c-fcaaadd9aa32" />






---











# Module 5: Neural network classifiers

## 6.17 – MLP classifier on MNIST dataset

### Introduction to neural networks and Multilayer Perceptrons (MLP)

Artificial Neural Networks (ANNs) are at the very core of Deep Learning; they are versatile, powerful, and scalable, making them ideal for tackling large and highly complex Machine Learning tasks such as classifying billions of images, powering speech recognition services, recommending videos, or playing the game of Go . When an ANN contains a deep stack of hidden layers, it is called a **deep neural network (DNN)**, and the field of Deep Learning studies DNNs and models containing deep stacks of computations .

An MLP is composed of:

- One (passthrough) **input layer**
- One or more layers of Threshold Logic Units (TLUs), called **hidden layers**
- One final layer of TLUs called the **output layer**

The layers close to the input layer are usually called the **lower layers**, and the ones close to the outputs are called the **upper layers** . Every layer **except the output layer** includes a bias neuron and is fully connected to the next layer . Because the signal flows only in one direction (from the inputs to the outputs), this architecture is an example of a **feedforward neural network (FNN)** .

<img width="887" height="508" alt="image" src="https://github.com/user-attachments/assets/cd543fe2-2eea-4661-86b8-b4786fefe62a" />

### Hyperparameters and configuration for classification tasks

For classification tasks on datasets such as MNIST (which consists of 70,000 small images of digits handwritten by high school students and employees of the US Census Bureau, labeled with the digit represented, across classes `0` through `9` for digit image classification):

- If each instance can belong only to a single class out of three or more possible classes (e.g., classes `0` through `9` for digit image classification), you need one output neuron per class and should use the **softmax** activation function for the whole output layer .
- The softmax function ensures that all estimated probabilities are between `0` and `1` and add up to `1`, which is required when classes are exclusive (multiclass classification) .
- Since we are predicting probability distributions, **cross-entropy loss** (also called **log loss**) is generally a good choice .
- When using one-hot target vectors per class, `"categorical_crossentropy"` is used; when using sparse target class indices (from `0` to `9` for exclusive classes), `"sparse_categorical_crossentropy"` is used . For binary classification (with one or more binary labels), `"sigmoid"` (logistic) activation is used in the output layer along with `"binary_crossentropy"` loss .

| Hyperparameter / Setting | Binary Classification | Multilabel Binary Classification | Multiclass Classification |
| --- | --- | --- | --- |
| **Input and Hidden Layers** | Same as regression | Same as regression | Same as regression |
| **Number of Output Neurons** | $1$ | $1$ per label | $1$ per class |
| **Output Layer Activation** | Logistic | Logistic | Softmax |
| **Loss Function** | Cross entropy | Cross entropy | Cross entropy |

<img width="878" height="508" alt="image" src="https://github.com/user-attachments/assets/5736d24a-1df7-4aec-9efe-0288b6bd912f" />

### Exploration and visualization tools

Playing with various neural network architectures using visualization tools like the **TensorFlow Playground** is useful for understanding MLPs, including the effects of hyperparameters such as the number of layers and neurons and activation functions .
