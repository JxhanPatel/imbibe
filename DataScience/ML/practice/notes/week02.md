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

