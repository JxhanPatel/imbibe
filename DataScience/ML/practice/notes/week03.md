# Week 3

**Module 1: Transformer Chaining & Integration**

- **3.1** - Chaining Transformers

**Module 2: Data Extraction, Imputation, Scaling & Visualization**

- **3.2** - Demonstration of Data extraction, imputation, scaling, visualizing feature distribution

**Module 3: Advanced Data Transformations & Composite Pipelines**

- **3.3** - Demonstration of Data transformation, Composite transformers

**Module 4: Feature Selection, PCA, Pipelines & Class Imbalance**

- **3.4** - Demonstration of Feature Selection Demonstration, PCA, Pipelines, Handling class imbalance



---


## Module 1: Transformer Chaining & Integration

### 3.1 Chaining Transformers

<img width="876" height="289" alt="image" src="https://github.com/user-attachments/assets/6dde598a-6363-46e9-b2f6-ce7564763430" />

## Overview of Chaining Transformers

- The preprocessing transformations are applied one after another on the input feature matrix .
- For example, applying `SimpleImputer()` (`si.fit_transform(I)`) followed by `StandardScaler()` (`ss.fit_transform(I_imputed)`) .
- It is important to apply exactly the same transformation on training, evaluation, and test set in the same order .
- Failing to do so would lead to incorrect predictions from the model due to distribution shift and hence incorrect performance evaluation .
- Scikit-Learn provides `pipeline` for making it easier to chain multiple transforms together and apply them uniformly across train, eval, and test sets .

## Composite Estimators: Pipeline and FeatureUnion

- The `sklearn.pipeline` module provides utilities to build a composite estimator, as a chain of transformers and estimators .
- There are two main classes: (i) `Pipeline` and (ii) `FeatureUnion` .

| Class | Usage |
| --- | --- |
| **Pipeline** | Constructs a chain of multiple transformers to execute a fixed sequence of steps in data preprocessing and modelling . |
| **FeatureUnion** | Combines output from several transformer objects by creating a new transformer from them . |

<img width="684" height="421" alt="image" src="https://github.com/user-attachments/assets/b2775a4e-368c-4056-8eee-e1a94f22e767" />

## `sklearn.pipeline.Pipeline`

- Sequentially applies a list of transformers and estimators .
- Intermediate steps of the pipeline must be **transformers**, that is, they must implement `fit` and `transform` methods .
- The final estimator only needs to implement `fit` .
- The purpose of the pipeline is to assemble several steps that can be cross-validated together while setting different parameters .

### Creating Pipelines

- A pipeline can be created with `Pipeline()` . It takes a list of `('estimatorName', estimator(...))` tuples .
- Each name should be unique and should not contain double underscores .
- The pipeline object exposes the interface of the last step .
- Another way to construct a pipeline object is to make use of the `make_pipeline` function, which takes several estimator objects without needing explicit string names/tuples .

> [!NOTE]
> When you call the pipeline’s `fit()` method, it calls `fit_transform()` sequentially on all transformers, passing the output of each call as the parameter to the next call until it reaches the final estimator, for which it calls the `fit()` method  . The pipeline exposes the same methods as the final estimator  .
>
>

### Code Example: Transformation Pipeline

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer

transform_pipeline = Pipeline([
('imputer', SimpleImputer(strategy="median")),
('std_scaler', StandardScaler()),
])

wine_features_tr = transform_pipeline.fit_transform(wine_features)

```

- The output of one step is passed on to the next one in sequence until it reaches the last step .
- Here the pipeline first performs imputation of missing values and its result is passed for standardization .
- The pipeline exposes the method of the last estimator . In this case, the last estimator is `StandardScaler`, and since it is a transformer, we call the `fit_transform` method on the pipeline object .

<img width="689" height="496" alt="image" src="https://github.com/user-attachments/assets/a4c62dab-6802-4cd8-b46b-5c5f743b9e06" />

## Accessing Individual Steps in a Pipeline

- Individual steps can be accessed or defined using an estimators list :

```python
from sklearn.decomposition import PCA
estimators = [
('simpleImputer', SimpleImputer()),
('pca', PCA(2)),
('regressor', LinearRegression())
]
pipe = Pipeline(steps=estimators)

```

- Pipelines can also take a `memory` parameter for caching intermediate computations to avoid redundant computationally expensive transformations during grid search :

```python
pipe = Pipeline(steps=estimators, memory=tempDirPath)

```

## Advantages of Pipelines

- Helps us combine multiple steps of end-to-end machine learning pipeline into a single object .
- Enables joint grid search of parameters of all the estimators in the pipeline .
- Makes configuration and tuning of end-to-end machine learning pipeline quick and easy .
- Offers convenience as the developer has to call only `fit` and `predict` methods on the pipeline object .
- Reduces code duplication; with pipeline objects, one does not have to repeat code for pre-processing and transforming on the test set .





---




## **Module 2: Data Extraction, Imputation, Scaling & Visualization**

### 3.2 Demonstration of Data Extraction, Imputation, Scaling & Visualizing Feature Distribution

<img width="1024" height="816" alt="image" src="https://github.com/user-attachments/assets/c33d316a-1787-45ac-9613-d9933f471f85" />

## 1. Motivation and Overview of Data Processing Steps

- In machine learning algorithms, different features used to represent an example may exist at different scales, and the current data distribution might not be amenable to learning .
- To address data capture errors and prepare data for modeling, four key data processing steps are applied :
1. Separating features and labels .
2. Handling missing values and outliers (e.g., fixing sensor malfunctions or measurement errors) .
3. Feature scaling to bring all features onto the same scale .
4. Applying mathematical transformations (such as log or square root) to achieve a data distribution more amenable to learning .
- **Best Practice**: Always make a copy of the data and apply preprocessing on that copy, ensuring that the original dataset remains intact if anything goes wrong .

## 2. Scikit-Learn Data and Preprocessing APIs

- Scikit-Learn provides dedicated modules for data preparation and feature engineering :
- **`sklearn.datasets`**: Helps in loading custom datasets as well as popular reference datasets bundled with Scikit-Learn .
- **`sklearn.preprocessing`**: Provides utilities for feature scaling, centering, normalization, and binarization .
- **`sklearn.impute`**: Provides tools like `SimpleImputer` to handle and fill missing values resulting from data capture errors .

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/4105b1b5-0889-4b8d-b89b-209bccb5da7e" />

## 3. Handling Missing Values

- Missing values often arise due to sensor malfunction, measurement errors, or human recording errors .
- Many machine learning algorithms cannot process missing data, making imputation or removal necessary .
- **`SimpleImputer`**: Fills missing values using univariate statistical strategies such as `mean`, `median`, `most_frequent`, or `constant` .

## 4. Visualizing Feature Distributions and Relationships

Various visualization techniques help inspect data distributions and feature correlations during exploratory data analysis  .

### 4.1 Scatter Visualization

- Scatter plots can be created using the **Seaborn** library to visualize the relationship between two specific features .
- Users specify the x-axis, y-axis, and the data source . For example, plotting **fixed acidity** on the x-axis against **density** on the y-axis .

### 4.2 Scatter Matrix (`scatter_matrix`)

- The `scatter_matrix` function from the `pandas.plotting` library plots relationships between multiple features in a grid of scatter plots .
- **Code Example**: 
```python
from pandas.plotting import scatter_matrix attribute_list = ['citric acid', 'pH', 'alcohol', 'sulphates', 'quality'] scatter_matrix(exploration_set[attribute_list])
```
- **Diagonal vs. Off-Diagonal Interpretation**:
- **Diagonal**: Displays histograms representing the distribution of individual features (e.g., the distribution of the `citric acid` feature) .
- **Off-Diagonal**: Displays scatter plots showing pairwise relationships between different features .

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/87b937ca-30b6-4e9b-8318-194ff6e9469d" />

### 4.3 Feature Histograms & Outlier Inspection

- Plotting histograms for numerical attributes helps analyze feature spreads :
```python
import matplotlib.pyplot as plt california_housing.frame.hist(figsize=(12, 10), bins=30, edgecolor="black") plt.subplots_adjust(hspace=0.7, wspace=0.4)
```
- **Key Observations from Histograms**:
- Features like median income may display a long-tail distribution .
- House age can show a more uniform distribution with specific spikes .
- Features such as average rooms, average bedrooms, population, and average occupancy can exhibit large value ranges with noticeable extreme tails, indicating potential **outlier values** .





---




## Module 3: Advanced Data Transformations & Composite Pipelines

### 3.3 Demonstration of Data Transformation & Composite Transformers

<img width="876" height="289" alt="image" src="https://github.com/user-attachments/assets/84abea42-51f3-4bda-a238-5aa71e412a2f" />

## 1. Overview of Composite Transformers (`sklearn.compose`)

- The `sklearn.compose` module provides useful classes and methods to apply transformations on subsets of features and combine them.
- Real-world data often contains heterogeneous features (both categorical and numerical features), requiring different transformations applied to separate subsets of features.
- Two main classes in `sklearn.compose` are:
1. **ColumnTransformer**
2. **TransformedTargetRegressor**

## 2. ColumnTransformer

<img width="680" height="828" alt="image" src="https://github.com/user-attachments/assets/7a22c0ae-1895-4130-a1d3-2b3e294f38d4" />

### 2.1 Definition and Purpose

- **ColumnTransformer** applies a set of transformers to columns of an array or a pandas DataFrame and concatenates the transformed outputs from different transformers into a single matrix.
- It is useful for transforming heterogeneous data by applying different transformers to separate subsets of features.
- It combines different feature selection mechanisms and transformations into a single transformer object.

### 2.2 Syntax and Tuple Format

- Each tuple in a `ColumnTransformer` has the following format: $$ \text{(estimatorName, estimator(...), columnIndices)} $$
- **Arguments & Parameters**:
- `remainder`: Specifies what to do with columns not specified in the transformers list (e.g., `'drop'` to drop them).
- `verbose_feature_names_out`: Controls feature naming behavior (e.g., `False`).

### 2.3 Concrete Example: Transforming Heterogeneous Features

- Consider a feature matrix representing the weight and gender of a class of students: $$ \mathbf{X}_{6\times2} = \begin{bmatrix} 20.0 & \text{'male'} \\ 11.2 & \text{'female'} \\ 15.6 & \text{'female'} \\ 13.0 & \text{'male'} \\ 18.6 & \text{'male'} \\ 16.4 & \text{'female'} \end{bmatrix} $$
- Since the first column is numeric (weight) and the second column is categorical (gender), different transformers must be applied.
- **Code Example**: ```python from sklearn.compose import ColumnTransformer from sklearn.preprocessing import MaxAbsScaler, OneHotEncoder column_trans = ColumnTransformer([ ('scaler', MaxAbsScaler(), [0]), ('encoder', OneHotEncoder(dtype='int'), [1]) ], remainder='drop', verbose_feature_names_out=False) column_trans.fit_transform(X) ```
- **Transformed Output Matrix ($\mathbf{X}'_{6\times3}$)**: $$ \mathbf{X}'_{6\times3} = \begin{bmatrix} 1.0 & 1.0 & 0.0 \\ 0.56 & 0.0 & 1.0 \\ 0.78 & 0.0 & 1.0 \\ 0.65 & 1.0 & 0.0 \\ 0.93 & 1.0 & 0.0 \\ 0.82 & 0.0 & 1.0 \end{bmatrix} $$
- *Note*: `MaxAbsScaler` scales the numeric weight column into the range $[-1, 1]$ (or based on maximum absolute value, scaling $20.0$ to $1.0$), and `OneHotEncoder` binarizes the categorical gender column into separate indicator columns.

## 3. FeatureUnion

<img width="612" height="866" alt="image" src="https://github.com/user-attachments/assets/a7cc4469-6f20-451c-8814-c6599ecf873f" />

- **FeatureUnion** is another composite utility that applies a list of transformer objects in **parallel** and then concatenates their outputs side by side to form a larger transformed feature matrix.
- FeatureUnion and Pipeline constructs can be combined together to create complex transformers.

| Construct | Execution Style | Primary Purpose |
| --- | --- | --- |
| **Pipeline** | Sequential chain | Executes a fixed sequence of preprocessing and modeling steps one after another. |
| **FeatureUnion** | Parallel execution | Concatenates outputs from several transformer objects side by side. |

## 4. TransformedTargetRegressor

<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/52d60718-f8d2-4dba-a21a-535fbd6b7a3a" />

- **TransformedTargetRegressor** transforms the target variable $y$ before fitting a regression model.
- The predicted values are mapped back to the original space via an **inverse transform**.
- It takes the regressor and the transformer to be applied to the target variable as arguments.

## 5. FunctionTransformer

- **FunctionTransformer** constructs transformed features by applying a user-defined function directly within the transformation workflow.
