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




