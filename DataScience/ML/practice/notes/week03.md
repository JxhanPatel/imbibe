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
