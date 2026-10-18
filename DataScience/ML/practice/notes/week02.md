# Week 2

### [Module 1: Basics & Data Loading](https://github.com/JxhanPatel/imbibe/blob/main/DataScience/ML/practice/notes/week02.md#module-1-basics--data-loading-1)

- **2.1**: Introduction to Scikit-Learn
- **2.2**: Data Loading
- **2.3**: Demonstration of sklearn dataset API

### [Module 2: Data Preprocessing & Missing Data](https://github.com/JxhanPatel/imbibe/blob/main/DataScience/ML/practice/notes/week02.md#module-2-data-preprocessing--missing-data-1)

- **2.4**: Data Preprocessing
- **2.4.2**: Handling Missing Data

### [Module 3: Feature Transformers & Scaling](https://github.com/JxhanPatel/imbibe/blob/main/DataScience/ML/practice/notes/week02.md#module-3-feature-transformers--scaling-1)

- **2.5**: Categorical Transformers
- **2.6**: Numeric Transformers
- **2.6.2**: Feature Scaling
- **2.6.3**: Outliers

### [Module 4: Feature Selection & Transformations](https://github.com/JxhanPatel/imbibe/blob/main/DataScience/ML/practice/notes/week02.md#module-4-feature-selection--transformations-1)

- **2.7**: Filter based feature selection
- **2.8**: Wrapper based feature selection
- **2.9**: Heterogeneous features transformations

### [Module 5: Dimensionality Reduction](https://github.com/JxhanPatel/imbibe/blob/main/DataScience/ML/practice/notes/week02.md#module-5-dimensionality-reduction-1)

- **2.10**: Dimensionality reduction by PCA


---


# Module 1: Basics & Data Loading

## L 2.1: Introduction to Scikit-Learn & Concrete APIs

Scikit-Learn provides concrete APIs across various modules to assist in loading, generating, and preprocessing training and test sets  .

- **`sklearn.datasets`**: Helps us load custom data as well as popular reference datasets that come bundled with Scikit-Learn or fetch datasets from the internet .
- **`sklearn.preprocessing`**: Helps in scaling, centering, normalization, and binarization of features .
- **`sklearn.impute`**: Helps in imputing (feeding) missing values in the dataset due to various data capture errors .
- **`sklearn.feature_extraction`**: Contains a collection of feature extractors to extract features from different data types .
- **`sklearn.decomposition`**: Houses a number of APIs for feature reduction algorithms, such as Principal Component Analysis (PCA) .
- **`sklearn.linear_model`**: Contains implementations for different classification and regression models like Perceptron, Logistic Regression, SVM, and Linear Regression .
- **`sklearn.model_selection`**: Provides model selection utilities .
- **`sklearn.metrics`**: Provides scoring metrics for model evaluation .


> **Note**: For classification and regression workflows, the first step is always importing all necessary packages and utilities from their respective Scikit-Learn modules  .
>
>

## L 2.2: Data Loading & Dataset API Interfaces

Scikit-Learn provides a general dataset API with **three main kinds of interfaces**  :

1. **Dataset Loaders**: Used to load small, standard toy datasets bundled with Scikit-Learn . These datasets do not require downloading from external sources .
2. **Dataset Fetchers**: Used to download and load larger datasets from the internet that are not bundled with Scikit-Learn .
3. **Dataset Generators**: Used to generate controlled synthetic datasets with specific statistical properties .

### Structure of Loaders and Fetchers (`Bunch` Objects)

- Both loaders and fetchers return a **Bunch object**, which is a dictionary-like object with key-value attributes .
- The typical dictionary structure includes :
- **`DESCR`**: Full description of the dataset .
- **`data`**: Feature matrix of shape $(n, m)$, where $n$ is the number of examples (instances) and $m$ is the number of features, with one row per instance and one column per feature .
- **`target`**: Label vector of shape $(n,)$ containing the target labels/classes .
- **`feature_names`**: Names of the features .
- **`target_names`**: Names of the classes/targets .
- **`filename`**: Path to the location of the data file .
- You can verify the return type by invoking `type()` on the data object, which returns `sklearn.utils.Bunch` .


### Dataset Generators Structure

- Unlike loaders and fetchers, generators return a tuple $(X, y)$, where $X$ is a feature matrix of shape $(n, m)$ and $y$ is a label vector of shape $(n,)$ .
- Both $X$ and $y$ are NumPy arrays .

## L 2.3: Demonstration of Sklearn Dataset API & External Formats

### 1. Dataset Loaders Demonstration

There are seven primary dataset loaders in Scikit-Learn for smaller datasets  :

- **Iris Dataset**: Contains 150 samples with 4 features each and a single label for a classification problem .
- **Diabetes Dataset**: Contains 442 samples, 10 features, and a single label . Can be loaded using `load_diabetes` from `sklearn.datasets` .
- **California Housing Dataset**: Can be fetched using `fetch_california_housing(as_frame=True)` from `sklearn.datasets`, returning a `Bunch` data frame .

#### Controlling Loader Return Types

By default, loaders return a `Bunch` object  . If you desire tuples of feature matrices and label vectors instead, you can set the argument `return_X_y=True`  :

```python
from sklearn.datasets import load_iris
X, y = load_iris(return_X_y=True)

```

When set to `True`, it returns a tuple of two NumPy arrays ($X$ of shape $(150, 4)$ and $y$ of shape $(150,)$)  .

### 2. Fetching Larger Datasets from the Internet

Scikit-Learn provides helper functions like `fetch_openml` to download popular external datasets such as MNIST  :

```python
from sklearn.datasets import fetch_openml
mnist = fetch_openml('mnist_784', version=1)
mnist.keys()

```

This returns dictionary keys: `dict_keys(['data', 'target', 'feature_names', 'DESCR', 'details', 'categories', 'url'])`  .


### 3. Loading External & Specialized Data Formats

Depending on the training data format, various specialized utilities are used:

- **Sparse Formats (SVM)**: `datasets.load_svmlight_file()` and `datasets.load_svmlight_files()` load data in `svmlight` and `libSVM` sparse formats, which are optimized for very large, sparse feature spaces used by Support Vector Machines .
- **Images and Videos**: `skimage.io` provides tools to load images and videos into NumPy arrays .
- **Audio**: `scipy.io.wavfile.read` specializes in reading WAV files into NumPy arrays .
- **Optimized Numerical Data**: For managing large numerical datasets, Scikit-Learn recommends using optimized file formats such as **HDF5 (Hierarchical Data Format version 5)** to reduce data load times . Libraries like **Pandas**, **PyTables**, and **H5Py** provide interfaces to read and write data in HDF5 format .




---



# Module 2: Data Preprocessing & Missing Data

## 2.4: Data Preprocessing

### Real-World Data Challenges

The real-world training data is usually not clean and has many issues such as  :

- Missing values for certain features .
- Features on different scales .
- Non-numeric attributes .

Often there is a need to pre-process the data to make it amenable for training the model  . Once you get the training data, the first job is to explore the data and list down preprocessing needed  . Typical problems include  :

1. Missing values in features .
2. Numerical features are not on the same scale .
3. Categorical attributes need to be represented with sensible numerical representation .
4. Too many features, reduce them .
5. Extract features from non-numeric data .

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/5a174f15-e3c7-4866-9882-b637e1aeeb65" />

### Scikit-Learn Library of Transformers

Scikit-Learn provides a rich set of transformers for data preprocessing  . We have to make sure that we apply the same set of transformations to both training, test, as well as on the evaluation set  . Scikit-Learn provides a **Pipeline** class for making it easier to chain multiple transformers together and ensure that we apply these transformations uniformly across training, eval, and test sets  .

Scikit-Learn provides a library of transformers categorized under different modules  :

- **Data cleaning (`sklearn.preprocessing`)**: Such as standardization, missing value imputation, etc .
- **Feature extraction (`sklearn.feature_extraction`)**: Contains a bunch of feature extractors to extract features from different data types .
- **Feature reduction (`sklearn.decomposition.pca`)**: Houses feature reduction algorithms implemented, such as Principal Component Analysis (PCA) .
- **Feature expansion (`sklearn.kernel_approximation`)**: Methods for feature expansion done especially in kernel methods .

## 2.4.2: Handling Missing Data

### The Missing Data Problem

Missing values occur due to errors in data capture, such as  :

- Sensor malfunctioning .
- Measurement errors .
- Human components involved in recording data .

Many machine learning algorithms do not work with missing data and need all features to be present  . Discarding records containing missing values would result in a loss of valuable training samples (throwing away precious training data), hence we need a mechanism to handle these missing values  .

When handling missing features, you generally have three main options  :

1. **Get rid of the corresponding districts/rows** containing missing values .
2. **Get rid of the whole attribute/feature** .
3. **Set the values to some value** (zero, the mean, the median, etc.) via imputation techniques . If values do not exist, it is better to keep them as `NaN` .

Scikit-Learn provides methods to drop rows containing missing values (`dropna()`, `drop()`), but throwing away data can be costly  .

### The `sklearn.impute` API

The `sklearn.impute` API provides functionality to fill missing values in a dataset  . The key classes implemented in this module include  :

- **`SimpleImputer`**: Fills missing values with statistical strategies .
- **`KNNImputer`**: Imputes missing values using K-Nearest Neighbors.
- **`MissingIndicator`**: Provides indicators for missing values (useful to examine data and evaluate classifier/regressor performance on missing features) .

### Using `SimpleImputer`

The `SimpleImputer` class fills missing values with one of the following strategies  :

- `'mean'`
- `'median'`
- `'most_frequent'`
- `'constant'`

#### Code Implementation Example

To use `SimpleImputer`, first import the class, create an instance specifying the strategy, and fit it to the numerical attributes of your dataset  :

```python
from sklearn.impute import SimpleImputer

# Create an imputer instance using the median strategy
imputer = SimpleImputer(strategy="median")

```

> **Note**: Since the median can only be computed on numerical attributes, you must exclude any text or categorical attributes (such as `ocean_proximity`) before calling the `fit` method  :
>
> ```python
> housing_num = housing.drop("ocean_proximity", axis=1)
>
> ```
>
>

#### Fitting and Transforming

1. **`fit()` method**: Computes the statistic (e.g., median) for each attribute and stores the result in its `statistics_` instance variable : ```python imputer.fit(housing_num) ``` Checking the learned statistics: ```python imputer.statistics_ ```
2. **`transform()` method**: Uses the trained imputer to transform the training set by replacing missing values with the learned medians : ```python X = imputer.transform(housing_num) ``` The result is a plain NumPy array containing the transformed features, which can be converted back into a pandas DataFrame if needed : ```python import pandas as pd housing_tr = pd.DataFrame(X, columns=housing_num.columns, index=housing_num.index) ```





---





# Module 3: Feature Transformers & Scaling

## 2.5: Categorical Transformers

### Converting Text Categories to Numbers

Most machine learning algorithms prefer to work with numbers  . When a dataset contains text or categorical attributes (such as `Sex` or `ocean_proximity`), we need special handling to convert these categories into numerical representations  .

1. **Ordinal Encoding (`OrdinalEncoder`)**:
- We can use Scikit-Learn’s `OrdinalEncoder` class to convert categories to numbers by calling the `fit_transform()` method on the encoder object .
2. **One-Hot Encoding (`OneHotEncoder`)**:
- Scikit-Learn provides the `OneHotEncoder` class to convert categorical values into one-hot vectors .
- Characteristics of one-hot encoding:
- Only one feature is $1$ (hot) and the rest are $0$ (cold) .
- The new features are referred to as **dummy features** .
- When calling `fit_transform()` on a `OneHotEncoder` object, the output is a **SciPy sparse matrix** rather than a NumPy array . This enables us to save a huge amount of space when we have a huge number of categories .
- To convert the sparse matrix into a dense representation, we can use the `.toarray()` method .
- The list of categories can be obtained via the `categories_` member variable .

<img width="664" height="647" alt="image" src="https://github.com/user-attachments/assets/5050da82-301c-49ce-a6a1-0cf96a59ce0d" />

### Handling Mixed Feature Types (`ColumnTransformer`)

- In real-world situations, it is very common to have features of different types: some numerical and some categorical .
- Transforming mixed features can be challenging, but Scikit-Learn provides **`ColumnTransformer`** from the `sklearn.compose` module for this exact purpose .
- **Key Principles of `ColumnTransformer`**:
- It applies each transformation to the appropriate column and then concatenates the outputs along the columns .
- All transformers must return the same number of rows .
- The numeric transformers return a dense matrix while the categorical ones return a sparse matrix; `ColumnTransformer` automatically determines the type of the output based on the density of the resulting matrix .

## 2.6: Numeric Transformers

Numeric transformers are applied to numerical features  . There are **three main types of numeric transformers** in Scikit-Learn  :

1. **Feature scaling**
2. **Polynomial transformation**
3. **Discretization**

## 2.6.2: Feature Scaling

### Why Feature Scaling is Necessary

- Most machine learning algorithms do not perform well when input numerical attributes are on very different scales .
- Numerical features with different scales lead to slower convergence of iterative optimization procedures (such as gradient descent, mini-batch gradient descent, and stochastic gradient descent) .
- It is a good practice to scale numerical features so that all of them are on the same scale .
- **Note**: Scaling of target labels is generally *not* required .

### Scikit-Learn Feature Scaling APIs

Scikit-Learn provides three primary feature scaling APIs  :

1. `StandardScaler`
2. `MaxAbsScaler`
3. `MinMaxScaler`

### 1. Min-Max Scaling (Normalization)

- **Definition**: Values are shifted and rescaled so that they end up ranging between $0$ and $1$ . We do this by subtracting the minimum value of a feature from the current value and dividing it by the difference between the maximum and minimum value of that feature : $x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$
- Scikit-Learn provides the **`MinMaxScaler`** transformer for this .
- It has a `feature_range` hyperparameter that lets you change the target range if needed .

### 2. Standardization

- **Definition**: First subtracts the mean value ($\mu$) of each feature from the current value, and then divides by the standard deviation ($\sigma$) so that the resulting feature has a zero mean and unit variance : $x' = \frac{x - \mu}{\sigma}$
- Unlike min-max scaling, standardization **does not bound values to a specific range**, which may be a problem for some algorithms (e.g., neural networks often expect an input value ranging from $0$ to $1$) .
- However, **standardization is much less affected by outliers** compared to normalization . For example, if a district had an extreme erroneous value (such as a median income of $100$), min-max scaling would crush all other values down to a tiny fraction (e.g., $0$ to $0.15$), whereas standardization would not be impacted as severely .
- Scikit-Learn provides the **`StandardScaler`** transformer for standardization .

## 2.6.3: Outliers

### Identifying Outliers

- We can examine the presence of outliers by plotting feature distributions using **boxplots**, where different features show varying ranges and distinct outlier points .
- Calling `.describe()` on a DataFrame provides quartiles, minimum, maximum, and mean values to reconfirm the presence of outliers (e.g., maximum values far higher than the 75th percentile, or minimum values like $0$ in height, which can also indicate potential missing values or data capture errors) .

> **Outlier Impact on Scaling**: Because standardization relies on mean and standard deviation rather than fixed min/max boundaries, it is significantly less sensitive to extreme outliers than min-max normalization  .
>
>

### Outlier Detection Algorithms in Scikit-Learn

Beyond preprocessing adjustments, specialized algorithms can detect and clean up outliers:

1. **Fast-MCD (Minimum Covariance Determinant)**:
- Implemented by the **`EllipticEnvelope`** class .
- Assumes that normal instances (inliers) are generated from a single Gaussian distribution and that the dataset is contaminated with outliers .
- When estimating the parameters of the Gaussian distribution, it ignores instances that are most likely outliers, providing a better estimation of the elliptic envelope .
2. **Isolation Forest**:
- An efficient algorithm for outlier detection, especially in high-dimensional datasets .
- Builds a Random Forest in which each Decision Tree is grown randomly by picking a feature and a random threshold recursively until instances are isolated .
- Anomalies are usually far from other instances, so on average across all Decision Trees, **they tend to get isolated in fewer steps** than normal instances .






---






# Module 4: Feature Selection & Transformations

## 2.7: Filter-Based Feature Selection

Sometimes in a real-world dataset, all features do not contribute well enough towards fitting a model. The features that do not contribute significantly can be removed, leading to a decrease in the size of the dataset and hence the computational cost of fitting a model. The `sklearn.feature_selection` module provides many APIs to accomplish this task.

Feature selection methods are broadly divided into two types: **Filter-based** and **Wrapper-based** methods. Filter-based methods use a scoring function in order to score different features.

### Classes of Filter-Based Feature Selection

The filter-based feature selection methods include:

* **`VarianceThreshold`**

* **`SelectKBest`**

* **`SelectPercentile`**

* **`GenericUnivariateSelect`**

<img width="1200" height="800" alt="image" src="https://github.com/user-attachments/assets/6906cecd-af3f-4cb0-984c-58fb89e17237" />

<img width="931" height="486" alt="image" src="https://github.com/user-attachments/assets/c38b45fc-c5be-4605-b67e-500059b8eb1b" />

### 1. Variance Threshold (`VarianceThreshold`)

* Removes all features with variance below a certain threshold, as specified by the user, from the input feature matrix.


* By default, it removes a feature which has the same value, i.e., zero variance.


* This is useful when all values in a feature column are very close to each other or take a single value, as such features have very low variance and are not useful for training.



### 2. Univariate Feature Selection

Univariate feature selection selects features based on univariate statistical tests for each feature. There are three main APIs for univariate feature selection:

* **`SelectKBest`**: Removes all but the $k$ highest scoring features.


* **`SelectPercentile`**: Removes all but a user-specified highest scoring percentage of features.


* **`GenericUnivariateSelect`**: Performs univariate feature selection with a configurable strategy, which can be found via hyper-parameter search.



Scikit-learn also provides univariate feature selection methods that work on common univariate statistical tests:

* **`SelectFpr`**: Selects features based on a false positive rate test.


* **`SelectFdr`**: Selects features based on an estimated false discovery rate.


* **`SelectFwe`**: Selects features based on family-wise error rate.



#### Univariate Scoring Functions

* Each API needs a scoring function to score each feature.


* Three classes of scoring functions are proposed: **Mutual information (MI)**, **Chi-square**, and **F-statistics**.


* MI and F-statistics can be used in both classification and regression problems.


* Mutual information and Chi-squared feature selection are recommended for sparse data.



> **Caution**: Do not use a regression feature scoring function with a classification problem, as it will lead to useless results.
> 
> 

#### Code Examples for Univariate Selectors

```python
# SelectKBest example
skb = SelectKBest(chi2, k=20)
X_new = skb.fit_transform(X, y)

```

* **`SelectPercentile` example**: Instantiates an object specifying `chi2` as the scoring function and a percentile (e.g., `percentile=20`), then applies `fit_transform()`.


* **`GenericUnivariateSelect` example**:
```python
transformer = GenericUnivariateSelect(chi2, mode='k_best', param=20)
X_new = transformer.fit_transform(X, y)

```


The mode can take values: `'percentile'` (default), `'k_best'`, `'fpr'`, `'fdr'`, or `'fwe'`.



---

## 2.8: Wrapper-Based Feature Selection

Unlike filter-based methods, wrapper-based methods use an estimator class rather than a scoring function to decide the importance of features.

The wrapper-based methods provided in `sklearn.feature_selection` include:

* **RFE** (Recursive Feature Elimination)


* **RFECV** (Recursive Feature Elimination with cross-validation)


* **`SelectFromModel`**

* **`SequentialFeatureSelector`**


### 1. RFE (Recursive Feature Elimination)

RFE operates in two steps:

* **STEP 1**: Fits a model.


* **STEP 2**: Ranks the features; afterwards it removes one or more features (depending upon the `step` parameter).
These two steps are repeated until the desired number of features are selected. RFE requires only a single fit.



```python
from sklearn.feature_selection import RFE
from sklearn.linear_model import LinearRegression

estimator = LinearRegression()
selector = RFE(estimator, n_features_to_select=3, step=1)

```

Once fitting is done, we obtain a boolean array in the `support_` member variable that indicates which features are selected (`True` for selected features).

### 2. `SelectFromModel`

* First trains an estimator on all features.


* Then it selects a desired number of features based on feature importance.


* The maximum number of features that we want to select is specified via the `max_features` parameter.


* The feature importance is obtained via `coef_`, `feature_importances_`, or an `importance_getter` callable from the trained estimator.


* The feature importance threshold can be specified either numerically or through string arguments based on built-in heuristics such as `'mean'`, `'median'`, and float multiples of these like `0.1 * mean`.


* `SelectFromModel` requires a single fit and needs no iterations.



### 3. Sequential Feature Selection (`SequentialFeatureSelector`)

* Performs feature selection by selecting or deselecting features one by one in a greedy manner.


* Uses one of two approaches:


1. **Forward selection**: Starting with zero features, it finds the single feature that obtains the best cross-validation score for an estimator when trained on that feature. It repeats the process by adding a new feature to the set of selected features.


2. **Backward selection**: Starting with all features, it removes least important features one by one following the idea of forward selection.




* **Note on Efficiency**: Sequential feature selection needs more models to evaluate in comparison to recursive feature elimination and `SelectFromModel` (e.g., going from $m$ features to $m-1$ features using $k$-fold cross-validation requires fitting $m \times k$ models).





## 2.9: Heterogeneous Features Transformations & Composite Transformers

Real-world data often has both categorical as well as numerical features, requiring different transformations applied to separate subsets of features. Scikit-Learn provides composite transformers in the `sklearn.compose` module for this exact purpose.

The two main classes focused on in composite transformers are:

1. **`ColumnTransformer`**

2. **`TransformedTargetRegressor`**


### `ColumnTransformer`

* Applies a set of transformers to columns of an array or pandas `DataFrame` and concatenates the transformed outputs from different transformers into a single matrix.


* Useful for transforming heterogeneous data by applying different transformers to separate subsets of features.


* Combines different feature selection mechanisms and transformations into a single transformer object.



#### Format of Each Tuple in `ColumnTransformer`

Each tuple has the format:


$$\text{(estimatorName, estimator(...), columnIndices)}$$

#### Code Snippet Example

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.feature_extraction.text import CountVectorizer

column_trans = ColumnTransformer([
    ('ageScaler', CountVectorizer(), [0]),
    ('genderEncoder', OneHotEncoder(dtype='int'), [1])
], remainder='drop', verbose_feature_names_out=False)

```






---




# Module 5: Dimensionality Reduction

## 2.10: Dimensionality Reduction by Principal Component Analysis (PCA)

### Overview of PCA

* **Principal Component Analysis (PCA)** is an unsupervised, linear dimensionality reduction technique.


* It is used to decompose a multivariate dataset into a set of successive **orthogonal components** that capture the maximum amount of variance.


* **Core Mechanics**:
* The **first principal component (PC)** lies in the direction of maximum variance in the data, capturing the bulk of the variance.


* **Subsequent principal components** are orthogonal to the first PC and gradually capture lesser and lesser variance in the data.


* We can select the first $k$ PCs such that we capture a desired amount of variance in the data.




* **Benefits**: Reduces the dimensions of a dataset, thereby lowering the computational cost of subsequent steps such as training models and cross-validation.



<img width="680" height="679" alt="image" src="https://github.com/user-attachments/assets/9bb31bc7-aa8f-460b-83bd-be73d964ec8c" />

---

### Mathematical Foundation: Singular Value Decomposition (SVD)

* PCA uses a standard matrix factorization technique called **Singular Value Decomposition (SVD)** to project the feature matrix or data onto a lower-dimensional space.


* SVD decomposes the training set matrix $X$ into the matrix multiplication of three matrices:



$$X = U \Sigma V^T$$


* Here, matrix $V$ contains the unit vectors that define all the principal components we are looking for:



$$V = \begin{bmatrix} \vert{} & \vert{} & & \vert{} \\\ c_1 & c_2 & \dots & c_n \\\ \vert{} & \vert{} & & \vert{} \end{bmatrix}$$



* **Data Centering**: PCA assumes that the dataset is centered around the origin. Scikit-Learn's PCA classes automatically handle data centering, but manual implementations require centering first ($X_{\text{centered}} = X - X_{\text{mean}}$).



---

### Projecting Down to $d$ Dimensions

* Once principal components are identified, you can reduce dimensionality down to $d$ dimensions by projecting the dataset onto the hyperplane defined by the first $d$ principal components.


* Selecting this hyperplane ensures that the projection preserves as much variance as possible.



<img width="1452" height="737" alt="image" src="https://github.com/user-attachments/assets/d0ba4b4e-a73e-4894-abca-4bec1fcd95c2" />


### Using Scikit-Learn's PCA API

Scikit-Learn provides the **`sklearn.decomposition.PCA`** class to perform PCA-based dimensionality reduction.

#### Basic Implementation

```python
from sklearn.decomposition import PCA 

# Instantiate PCA to reduce dimensions to 2
pca = PCA(n_components=2) 
X2D = pca.fit_transform(X) 
```

* **`components_` attribute**: Holds the transpose of $W_d$. For example, the unit vector defining the first principal component is equal to `pca.components_.T[:, 0]`[cite: 4].
* **Inverse Transformation**: To bring the transformed data back into the space of the original dimensions, you can use the `inverse_transform` method of the PCA object, or compute it via:
  $$X_{\text{recovered}} = X_{d\text{-proj}} W_d^T$$

#### Explained Variance Ratio
* The **`explained_variance_ratio_`** variable provides the proportion of the dataset's variance that lies along each principal component[cite: 4].
* **Choosing the Right Number of Dimensions**: Instead of manually specifying the number of components, you can set `n_components` to be a float between $0.0$ and $1.0$, indicating the ratio of variance you wish to preserve (e.g., preserving $95\%$ of variance)[cite: 4]:
  ```python
  pca = PCA() 
  pca.fit(X_train) 
  cumsum = np.cumsum(pca.explained_variance_ratio_) 
  d = np.argmax(cumsum >= 0.95) + 1
  ```


### Advanced PCA Variants in Scikit-Learn

#### 1. Randomized PCA
* If you set the `svd_solver` hyperparameter to `"randomized"`, Scikit-Learn uses a stochastic algorithm called **Randomized PCA** that quickly finds an approximation of the first $d$ principal components[cite: 4].
* **Computational Complexity**: $\mathcal{O}(m \times d^2) + \mathcal{O}(d^3)$ instead of $\mathcal{O}(m \times n^2) + \mathcal{O}(n^3)$ for full SVD, making it dramatically faster when $d$ is much smaller than $n$[cite: 4].
* **Default Behavior**: By default, `svd_solver` is set to `"auto"` (Scikit-Learn automatically uses randomized PCA if $m$ or $n > 500$ and $d < 80\%$ of $m$ or $n$, otherwise it uses full SVD)[cite: 4].

#### 2. Incremental PCA (IPCA)
* Standard PCA implementations require the entire training set to fit in memory[cite: 4].
* **Incremental PCA (IPCA)** allows you to split the training set into mini-batches and feed an IPCA algorithm one mini-batch at a time using the `partial_fit()` method[cite: 4].
* This is especially useful for large training sets and for applying PCA online (on the fly as new instances arrive)[cite: 4].

```

