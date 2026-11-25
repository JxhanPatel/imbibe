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



