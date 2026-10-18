# Week 2

### Module 1: Basics & Data Loading

- **2.1**: Introduction to Scikit-Learn
- **2.2**: Data Loading
- **2.3**: Demonstration of sklearn dataset API

### Module 2: Data Preprocessing & Missing Data

- **2.4**: Data Preprocessing
- **2.4.2**: Handling Missing Data

### Module 3: Feature Transformers & Scaling

- **2.5**: Categorical Transformers
- **2.6**: Numeric Transformers
- **2.6.2**: Feature Scaling
- **2.6.3**: Outliers

### Module 4: Feature Selection & Transformations

- **2.7**: Filter based feature selection
- **2.8**: Wrapper based feature selection
- **2.9**: Heterogeneous features transformations

### Module 5: Dimensionality Reduction

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
