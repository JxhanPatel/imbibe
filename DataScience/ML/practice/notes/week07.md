# Week 7: Classification (Multiclass)

### Module 1: Scalability & Multi-Class Setup

- **7.1** - Large Scale Machine Learning
- **7.7** - Multiclass

### Module 2: Classification Evaluation Metrics

- **7.2** - Confusion Matrix
- **7.3** - Accuracy
- **7.4** - Precision
- **7.5** - f1 score
- **7.6** - Recall


---


# Module 1: Scalability & Multi-Class Setup

## 7.1 Large Scale Machine Learning

### Overview of Handling Large-Scale Data

Large-scale Machine Learning differs from traditional machine learning in the sense that it involves processing large amount of data in terms of its size or number of samples, features or classes  . Large-scale datasets may not fit in memory, and we need to devise strategies to handle it in the context of training and prediction use cases  . Although scikit-learn is optimized for smaller data, it does offer a decent set of feature preprocessing and learning algorithms for large scale data such as classification, regression and clustering  .

Online learning algorithms can also be used to train systems on huge datasets that cannot fit in one machine’s main memory (this is called out-of-core learning). The algorithm loads part of the data, runs a training step on that data, and repeats the process until it has run on all of the data.

<img width="949" height="485" alt="image" src="https://github.com/user-attachments/assets/1d0d4ffa-2184-465c-9465-c4b1942f5b3b" />

> [!NOTE] Out-of-core learning is usually done offline (i.e., not on the live system), so online learning can be a confusing name. Think of it as incremental learning.
>
>

### Incremental Learning Scenarios

Incremental learning may be required in the following two scenarios:

- For out-of-memory (large) datasets, where it's not possible to load the entire data into the RAM at once, one can load the data in chunks and fit the training model for each chunk of data.
- For machine learning tasks where a new batch of data comes with time, re-training the model with the previous and new batch of data is a computationally expensive process. Instead of re-training the model with the entire set of data, one can employ an incremental learning approach, where the model parameters are updated with the new batch of data.

### `fit()` vs. `partial_fit()` in Scikit-Learn

Scikit-learn handles large data through `partial_fit()` method instead of using the usual `fit()` method  . To perform incremental learning, Scikit-learn implements `partial_fit` method that helps in training an out-of-memory dataset; in other words, it has the ability to learn incrementally from a batch of instances  .

- This method is expected to be called several times consecutively on different chunks of a dataset so as to implement out-of-core (online) learning .
- This function has some performance overhead, so it's recommended to call it on a considerable large batch of data (that fits into the memory), to overcome the limitation of overhead .
- Standard regression/classification implementations such as batch gradient descent, support vector machines (SVMs), and random forests require loading all the data into memory at once, so they cannot be used in scenarios where we do not have sufficient memory. SGD can deal with large data sets effectively by breaking up the data into chunks and processing them sequentially.

#### `partial_fit()` Attributes

`partial_fit(X, y, [classes], [sample_weight])` where:

- `X`: array of shape `(n_samples, n_features)` where `n_samples` is the number of samples and `n_features` is the number of features.
- `y`: array of shape `(n_samples,)` of target values.
- `classes`: array of shape `(n_classes,)` containing a list of all the classes that can possibly appear in the `y` vector. Must be provided at the first call to `partial_fit`, can be omitted in subsequent calls. For classification tasks, we have to pass the list of possible target class labels in `classes` parameter to cope-up with the unseen target classes in the 1st batch of the data.
- `sample_weight`: (optional) array of shape `(n_samples,)` containing weights applied to individual samples (`1.` for unweighted).
- Returns: object (`self`).

#### Estimators Implementing `partial_fit()`

| Task Category | Estimators |
| --- | --- |
| Classification | `MultinomialNB`, `BernoulliNB`, `SGDClassifier`, `Perceptron` |
| Regression | `SGDRegressor` |
| Clustering | `MiniBatchKMeans` |

`SGDRegressor` and `SGDClassifier` are commonly used for handling large data. `SGDClassifier` uses SGD as an optimization technique, supports multi-class classification by combining multiple binary classifiers in a "one versus all" (OVA) scheme, easily scales up to large scale problems with more than $10^{5}$ training examples and $10^{5}$ features, and also works with sparse machine learning problems  .

### Combining Preprocessing and Incremental Learning

Vectorizers are used to convert a collection of text documents to a vector representation, thus helping in preprocessing them before applying any model on these text documents. `HashingVectorizer` is a good choice if we are falling short of memory and resources or we need to perform incremental learning, whereas `CountVectorizer` is a good choice if we need to access the actual tokens.

In combining preprocessing and fitting in incremental learning (demonstrated on a sentiment classification task with `HashingVectorizer` and `SGDClassifier(penalty='l2', loss='hinge')`), data can be processed in parts (such as `X_train[0:400]` and `X_train[400:]`) using `partial_fit` while passing all classes on the first call.

## 7.7 Multiclass

### Basics of Multiclass, Multilabel, and Multioutput Classification

Whereas binary classifiers distinguish between two classes, multiclass classifiers (also called multinomial classifiers) can distinguish between more than two classes  .

- **Multiclass classification**: Classification task with more than two classes, where each example is labeled with exactly one class . It has exactly one output label and the total number of labels $\gt 2$ . Examples from the source:
- **Iris dataset**: Three class labels (`setosa`, `versicolor`, and `virginica`), with each example having one label of the three available class labels .
- **MNIST digit recognition dataset**: Ten class labels (`0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`), with each example having one label of the ten available class labels .
- **For more than one output**, there are two types of classification models:
- **Multilabel**: total #labels $= 2$ .
- **Multiclass multioutput**: total #labels $\gt 2$ .
- We refer both these models as multi-label classification models, where # of output labels $\gt 1$ .
- Multiclass, multilabel, multioutput problems are referred to as multi-learning problems .
-

<img width="704" height="538" alt="image" src="https://github.com/user-attachments/assets/593e7ce6-3b64-4db5-8cff-5292e5a46440" />

### Target Types in Scikit-Learn (`type_of_target`)

`type_of_target` can determine different types of multi-learning targets (as well as targets corresponding to regression and binary classification)  :

| `target_type` | Description in Source |
| --- | --- |
| `'multiclass'` | `y` contains more than two discrete values; not a sequence of sequences |
| `'multiclass-multioutput'` | Each example has more than one label and there are more than two distinct labels |
| `'multilabel-indicator'` | More than one label per sample with two labels (`0` and `1`) |
| `'unknown'` | Target format not matching known types |

### Multiclass Strategies and Estimators

Some algorithms (such as SGD classifiers, Random Forest classifiers, and naive Bayes classifiers) are capable of handling multiple classes natively  . Others (such as Logistic Regression or Support Vector Machine classifiers) are strictly binary classifiers, though there are various strategies that you can use to perform multiclass classification with multiple binary classifiers  .

- **One-versus-the-rest (OvR) / One-versus-all (OVA)**: Trains binary classifiers (such as one per class/digit) and selects the class whose classifier outputs the highest decision score . `LogisticRegression` (with `multi_class='ovr'`), `SGDClassifier`, and `Perceptron` use one-vs-rest strategy to implement multiclass classification . `LogisticRegression` can also set `multi_class='multinomial'` to solve multiclass classification directly with softmax regression using cross-entropy loss (supported by solvers including `'lbfgs'`, `'sag'`, `'saga'`, and `'newton-cg'`) . `RidgeClassifier` and `RidgeClassifierCV` are inherently multiclass / multilabel estimators .
- **One-versus-one (OvO)**: Trains a binary classifier for every pair of classes ($N \times (N - 1) / 2$ classifiers for $N$ classes, or 45 binary classifiers for 10 classes) and selects the class that wins the most duels .
- Scikit-Learn also provides `OneVsOneClassifier` and `OneVsRestClassifier` classes to explicitly use one-versus-one or one-versus-the-rest strategies.

<img width="746" height="666" alt="image" src="https://github.com/user-attachments/assets/fd11fd12-f3a8-4369-a723-3e530d28367c" />

### Multilabel and Multi-Output Strategies (`MultiOutputClassifier` vs. `ClassifierChain`)

| `MultiOutputClassifier` | `ClassifierChain` |
| --- | --- |
| Strategy consists of fitting one classifier per target . | A multi-label model that arranges binary classifiers into a chain . |
| Able to estimate a series of target functions that are trained on a single predictor matrix to predict a series of responses . | Way of combining a number of binary classifiers into a single multi-label model . |
| Allows multiple target variable classifications . | Capable of exploiting correlations among targets; for a multi-label classification problem with $k$ classes, $k$ binary classifiers are assigned an integer between $0$ and $k-1$ defining the order of models in the chain . |


---





# Module 2: Classification Evaluation Metrics

## 7.2 Confusion Matrix

### Overview and Structure of the Confusion Matrix

Evaluating a classifier is often significantly trickier than evaluating a regressor  . A much better way to evaluate the performance of a classifier is to look at the confusion matrix  . The general idea is to count the number of times instances of class A are classified as class B  . For example, to know the number of times the classifier confused images of 5s with 3s, you would look in the fifth row and third column of the confusion matrix  .

`confusion_matrix` evaluates classification accuracy by computing the confusion matrix with each row corresponding to the true class . Each row in a confusion matrix represents an actual class, while each column represents a predicted class . Entry $(i, j)$ in a confusion matrix is the number of observations actually in group $i$, but predicted to be in group $j$ .

```python
from sklearn.metrics import confusion_matrix
confusion_matrix(y_true, y_predicted)

```

<img width="907" height="506" alt="image" src="https://github.com/user-attachments/assets/18415bd2-ae64-41ec-af4b-c548b2d0f651" />

### Interpreting Entries and Classifying Digits

In the illustrated confusion matrix for a 5-detector (where the first row considers non-5 images as the negative class and the second row considers images of 5s as the positive class):

- **True negatives (TN)**: Non-5 images correctly classified as non-5s (top left; e.g., $53,057$ in `array([[53057, 1522], [1325, 4096]])`)
- **False positives (FP)**: Non-5 images wrongly classified as 5s (top right; e.g., $1,522$)
- **False negatives (FN)**: Images of 5s wrongly classified as non-5s (lower left; e.g., $1,325$)
- **True positives (TP)**: Images of 5s correctly classified as 5s (lower right; e.g., $4,096$)

A perfect classifier would have only true positives and true negatives, so its confusion matrix would have nonzero values only on its main diagonal (top left to bottom right)  . In multi-class confusion matrices, off-diagonal entries signify the examples/digits that are incorrectly classified  .

### Computing and Displaying the Confusion Matrix in Scikit-Learn

To compute the confusion matrix, you first need to have a set of predictions so that they can be compared to the actual targets . Instead of using the test set before you are ready to launch, you can use `cross_val_predict()`, which is used for predicting the labels for examples in different test sets in a cross-validation manner .

You can also calculate the confusion matrix from an estimator by providing the estimator object, the examples, and the actual labels (`ConfusionMatrixDisplay.from_estimator`), or plot the confusion matrix from predictions (`ConfusionMatrixDisplay.from_predictions`) and call `plt.show()` to display it  . In addition, `classification_report(y_true, y_predicted)` builds a text report showing precision, recall, and f1-score by class label, along with accuracy, macro average, and weighted average  .


## 7.3 Accuracy

### Accuracy in Classification and Cross-Validation

For classification, performance measures include precision, recall, F1-score, and accuracy . `accuracy_score` is a classification scoring metric in `sklearn.metrics` based on true labels and predicted labels as inputs, and estimators also provide a `score` method that takes feature matrices and labels as input and returns mean accuracy on the given test set and labels .

A good way to evaluate a model is to use cross-validation  . `cross_val_score` can be called by specifying the estimator, training features, labels, cross-validation folds (`cv`), and scoring scheme (`scoring`)  . `cross_validate` can also be used with parameters including estimator, data, target, cross-validation iterator (`cv`), scoring scheme, `return_train_score=True`, and `return_estimator=True`, storing results in a Python dictionary with keys `fit_time`, `score_time`, `test_score`, `estimator` (optional), and `train_score` (optional)  .

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

### Why Accuracy Alone Is Not Always a Good Measure and Balanced Accuracy

Accuracy alone is not always a good measure . Before proceeding further, we need to check whether the dataset is balanced or imbalanced . When evaluating a model against a baseline model that always outputs the class with the highest number of samples (for example, `DummyClassifier(strategy='most_frequent')`), a high accuracy can occur simply because one class dominates the sample counts .

`balanced_accuracy_score` in `sklearn.metrics` helps us get an accuracy score in the case of an imbalanced dataset .

<img width="906" height="514" alt="image" src="https://github.com/user-attachments/assets/6c92b3ec-fdb8-4c37-8e2e-99ee062cfbf7" />

## 7.4 Precision

### Definition and Formula

Precision is a measure derived from the confusion matrix . In the illustrated confusion matrix (Figure 3-2), precision is illustrated vertically along the Positive predicted column (for example, 3 out of 4) . Manually from a confusion matrix, precision is computed as:

$$
\text{precision} = \frac{TP}{TP + FP}
$$

### Computing Precision and Threshold Trade-offs

Scikit-Learn provides `precision_score` in `sklearn.metrics` to compute precision :

```python
from sklearn.metrics import precision_score, recall_score
precision_score(y_train_5, y_train_pred)

```

In the binary 5-detector example from *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*, `precision_score(y_train_5, y_train_pred)` equals `4096 / (4096 + 1522)` (approximately `0.7291`, or 72.9%), meaning that when the classifier claims an image represents a 5, it is correct only 72.9% of the time .

`precision_recall_curve` in `sklearn.metrics` gives the value of precision and recall for every threshold . Rather than passing predicted labels as input to `precision_recall_curve`, you need to pass the probability scores or the output from the decision function . When plotted, it has recall on the X-axis and precision on the Y-axis, and from the precision-recall curve we get average precision . Setting a high threshold (such as `y_scores >= threshold_90_precision`) can raise precision to about 90%, though doing so can lower recall if pushed too high .

<img width="898" height="367" alt="image" src="https://github.com/user-attachments/assets/aac854d0-e46d-4816-99c6-c04caf642546" />


## 7.5 f1 score

### Definition and Harmonic Mean Formula

It is often convenient to combine precision and recall into a single metric called the F1 score, in particular if you need a simple way to compare two classifiers  . The F1 score is the harmonic mean of precision and recall (Equation 3-3)  . Whereas the regular mean treats all values equally, the harmonic mean gives much more weight to low values; as a result, the classifier will only get a high F1 score if both recall and precision are high  .

$$
F_1 = \frac{2}{\frac{1}{\text{precision}} + \frac{1}{\text{recall}}} = \frac{2 \times \text{precision} \times \text{recall}}{\text{precision} + \text{recall}} = \frac{TP}{TP + \frac{FN + FP}{2}}
$$

### Computing and Interpreting the F1 Score

To compute the F1 score in Scikit-Learn, call `f1_score` from `sklearn.metrics` :

```python
from sklearn.metrics import f1_score
f1_score(y_train_5, y_train_pred)

```

In the binary 5-detector example, `f1_score(y_train_5, y_train_pred)` is `0.7420962043663375` . The F1 score favors classifiers that have similar precision and recall . However, this is not always what you want: in some contexts you mostly care about precision (such as detecting videos that are safe for kids, where rejecting many good videos for high precision is preferred over letting bad videos through), whereas in other contexts you really care about recall (such as detecting shoplifters in surveillance images, where catching almost all shoplifters with high recall matters even if precision is lower) .


## 7.6 Recall

### Definition and Formula

Recall is a measure derived from the confusion matrix . In the illustrated confusion matrix (Figure 3-2), recall is illustrated horizontally along the Positive actual row (for example, 3 out of 5) . Manually from a confusion matrix, recall is computed as:

$$
\text{recall} = \frac{TP}{TP + FN}
$$

### Computing Recall and Extending Metrics to Multiclass / Multilabel Settings

Scikit-Learn provides `recall_score` in `sklearn.metrics` to compute recall . In the binary 5-detector example, `recall_score(y_train_5, y_train_pred)` equals `4096 / (4096 + 1325)` (approximately `0.7556`, or 75.6%), meaning it detects 75.6% of the 5s .

To extend a binary metric to multiclass or multilabel problems, you treat the data as a collection of binary problems (one for each class) and average the binary metric calculations across the set of classes . This is typically done with the `average` parameter, which takes values including `macro`, `weighted`, `micro`, `samples`, and `none` :

- `macro` calculates the mean of binary metrics
- `weighted` computes the average of binary metrics weighted by the presence of the number of samples in each class
- `micro` gives each sample-class pair an equal contribution to the overall metric
- `samples` calculates the metric over true and predicted classes for each sample in the evaluation data and returns their averages
- `none` returns an array with the score for each class

<img width="889" height="481" alt="image" src="https://github.com/user-attachments/assets/d69f7de9-46d7-4a3d-975e-bc5564cc774a" />
