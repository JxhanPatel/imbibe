# Week 5: Logistic Regression

**Module 1: Exploratory Data Analysis & Baseline**
* 5.1 - Explore California Housing Dataset
* 5.4 - Baseline Model


 **Module 2: Linear Models & Optimization**
* 5.2 - Linear Regression on California housing Dataset
* 5.3 - Linear Regression Demonstration
* 5.5 - SGDRegressor Demonstration


 **Module 3: Non-Linear & Distance-Based Models**
* 5.6 - Demonstration: KNN with California Housing Dataset
* 5.7 - Decision Trees for Regression


 **Module 4: Ensemble Methods**
* 5.8 - Bagging and Random Forest Regressor on California Housing Dataset
* 5.9 - AdaBoost and GradientBoost Regressor on California Housing Dataset


**Module 5: Neural Networks**
* 5.10 - MLP Regressor on california housing dataset





# Module 1: Exploratory Data Analysis & Baseline

## 5.1 - Explore California Housing Dataset

- This colab introduces the California Housing dataset that is used for regression demonstration .
- The steps for typical dataset exploration can be applied broadly to any dataset .
- The dataset can be fetched from sklearn with the `fetch_california_housing` API .
- To analyze the dataset, it is loaded as a dataframe by setting the `as_frame` parameter to `True` .
- Checking the type of `california_housing` returns `sklearn.utils.Bunch` .


### Attributes of the Bunch Object

- **`data`**: A pandas object where each row corresponds to 8 feature values .
- **`target`**: Contains the average house value in units of $100,000 as a pandas object .
- **`feature_names`**: An array of ordered feature names used in the dataset .
- **`DESCR`**: Contains the description of the dataset .

### Dataset Description (STEP 1)

- The description can be accessed via `print(california_housing.DESCR)` .
- **Number of Instances**: 20,640 .
- **Number of Attributes**: 8 numeric, predictive attributes and the target .
- **Attribute Information** :
- `MedInc`: median income in block group .
- `HouseAge`: median house age in block group .
- `AveRooms`: average number of rooms per household .
- `AveBedrms`: average number of bedrooms per household .
- `Population`: block group population .
- `AveOccup`: average number of household members .
- `Latitude`: block group latitude .
- `Longitude`: block group longitude .
- **Missing Attribute Values**: None .
- The dataset was obtained from the StatLib repository (`https://www.dcc.fc.up.pt/~ltorgo/Regression/cal_housing.html`) .
- The target variable is the median house value for California districts, expressed in hundreds of thousands of dollars ($100,000) .
- This dataset was derived from the 1990 U.S. census, using one row per census block group .
- A block group is the smallest geographical unit for which the U.S. Census Bureau publishes sample data (a block group typically has a population of 600 to 3,000 people) .
- A household is a group of people residing within a home .
- Since the average number of rooms and bedrooms in this dataset are provided per household, these columns may take surprisingly large values for block groups with few households and many empty houses, such as vacation resorts .
- Reference: Pace, R. Kelley and Ronald Barry, Sparse Spatial Autoregressions, Statistics and Probability Letters, 33 (1997) 291-297 .

### Data Frame & Statistics

- Accessing `california_housing.frame.head()` displays rows of aggregated data about each district in California .
- The median house value is expressed in terms of 100s of 1000s of dollars (e.g., a price of 4.526 means the price of the house is $452,600) .
- Aggregation is done via mean and median .
- Looking at statistics via `california_housing.frame.describe()` provides count, mean, std, min, 25%, 50%, 75%, and max values for all features and `MedHouseVal` .

### Observations Based on Pairplot

- `MedIncome` seems to be useful in distinguishing between low and high valued houses .
- A few features have extreme values .
- `Latitude` and `longitude` together seem to distinguish between low and high valued houses .

## 5.4 - Baseline Model

### Instance-Based Versus Model-Based Learning

- Most Machine Learning tasks are about making predictions, meaning given training examples, the system needs to generalize well to examples it has never seen before .
- Having a good performance measure on training data is insufficient; the true goal is to perform well on new instances .
- There are two main approaches to generalization: instance-based learning and model-based learning .
- Model-based learning algorithms search for an optimal value for the model parameters such that the model will generalize well to new instances .

### Linear Regression Baseline Model

- A simple linear model can be represented by Equation 1-1 : $$ \text{life\_satisfaction} = \theta_0 + \theta_1 \times \text{GDP\_per\_capita} $$

<img width="757" height="330" alt="image" src="https://github.com/user-attachments/assets/90c03d8c-d62f-4075-9a34-4a8f10c6b380" />

<img width="734" height="338" alt="image" src="https://github.com/user-attachments/assets/a5c43984-dbde-4d0c-bad4-b7e07abb2943" />

- By convention, the Greek letter $\theta$ (theta) is frequently used to represent model parameters .
- This model has two model parameters, $\theta_0$ and $\theta_1$ .
- By tweaking these parameters, you can make your model represent any linear function .
- Before using your model, you need to define the parameter values $\theta_0$ and $\theta_1$ by specifying a performance measure .
- You can define a utility function (or fitness function) that measures how good your model is, or a cost function that measures how bad it is .
- For Linear Regression problems, people typically use a cost function that measures the distance between the linear model’s predictions and the training examples, with the objective to minimize this distance .

### Training the Model

- Training the model involves feeding training examples into the Linear Regression algorithm so it finds the parameters that make the linear model fit best to your data .
- The algorithm finds that the optimal parameter values are $\theta_0 = 4.85$ and $\theta_1 = 4.91 \times 10^{-5}$ .
- The word "model" can refer to a type of model (e.g., Linear Regression), to a fully specified model architecture (e.g., Linear Regression with one input and one output), or to the final trained model ready to be used for predictions .
- Model selection consists in choosing the type of model and fully specifying its architecture .
- Training a model means running an algorithm to find the model parameters that will make it best fit the training data and make good predictions on new data .












# Module 2: Linear Models & Optimization

## 5.2 & 5.3 - Linear Regression on California Housing Dataset & Demonstration

### Linear Regression Model Definition & Prediction

- A linear model makes a prediction by computing a weighted sum of the input features, plus a constant called the bias term (also called the intercept term) : $$ \hat{y} = w_0 + w_1 x_1 + w_2 x_2 + \dots + w_m x_m = w^T x $$
- In simpler regression models (such as life satisfaction), the model is represented as : $$ \text{life\_satisfaction} = \theta_0 + \theta_1 \times \text{GDP\_per\_capita} $$
- Here, $\theta_0$ and $\theta_1$ (or $w_0, w_1$) are the model's parameters .
- Training a model means setting its parameters so that the model best fits the training set .

### Cost Function (MSE)

- To train a Linear Regression model, we need a performance measure of how well or poorly the model fits the training data .
- The most common performance measure of a regression model is the Root Mean Square Error (RMSE) .
- To train the model, we need to find the value of $\theta$ that minimizes the RMSE .
- In practice, it is simpler to minimize the Mean Squared Error (MSE) than the RMSE, leading to the same result because the value that minimizes a function also minimizes its square root .
- The MSE cost function for a Linear Regression model hypothesis $h_\theta$ on a training set $X$ is calculated using Equation 4-3 : $$ \text{MSE}(X, h_\theta) = \frac{1}{m} \sum_{i=1}^{m} (\theta^T x^{(i)} - y^{(i)})^2 $$
- To simplify notations, we write $\text{MSE}(\theta)$ instead of $\text{MSE}(X, h_\theta)$ .

<img width="584" height="400" alt="image" src="https://github.com/user-attachments/assets/ddf7e3d5-a354-41bb-9cf6-ce8a516c5e95" />
<img width="2560" height="1750" alt="image" src="https://github.com/user-attachments/assets/5b2946f0-874e-44e9-b3aa-74ba2ca83375" />

### Pipeline Setup & Training with Normal Equation

- The California housing dataset can be fetched using Scikit-Learn's `fetch_california_housing()` function .
- Linear regression models can be set up using Scikit-Learn's `Pipeline` and `StandardScaler` : ```python lin_reg_pipeline = Pipeline([ ("feature_scaling", StandardScaler()), ("lin_reg", LinearRegression()) ]) ```
- The model is trained using the training feature matrix and label vector : ```python lin_reg_pipeline.fit(train_features, train_labels) ```
- Target values can also be scaled/transformed using `TransformTargetRegressor` from `sklearn.compose` along with an estimator and a transformer (such as `MaxAbsScaler`) .

### Model Inspection & Evaluation

- **Weights ($w_1, w_2, \dots, w_m$)**: Stored in the `coef_` member variable of the linear regression object (`linear_regressor.coef_` or `lin_reg_pipeline[-1].coef_`) .
- **Intercept ($w_0$)**: Stored in the `intercept_` member variable (`linear_regressor.intercept_` or `lin_reg_pipeline[-1].intercept_`) .
- These code snippets work for both `LinearRegression` and `SGDRegressor`, and for all regression estimators .
- **Evaluation**: We fit the linear regression estimator on the training set, calculate the training error (empirical error), and calculate the test error (generalization error) to determine whether the model is underfitting, overfitting, or is the right fit . Evaluation can be performed using score methods ($R^2$ score), `cross_val_score` with different scoring parameters, or mean absolute error .

## 5.5 - SGDRegressor Demonstration

### SGDRegressor Estimator Overview

- The `SGDRegressor` implements a stochastic gradient descent algorithm for linear regression .
- It is used for large training setups with more than 10,000 samples (`> 10k` samples) .
- It provides greater control on the optimization process through hyperparameter settings :
- **`loss`**: Set to supported values such as `'squared_error'` (studied in this course) or `'huber'` .
- **`penalty`**: Set to `'l1'`, `'l2'`, `'elasticnet'`, or `None` .
- **`learning_rate`**: Options include `'constant'`, `'optimal'`, `'invscaling'`, and `'adaptive'` .
- **`early_stopping`**: Can be set to `True` or `False` . The SGDRegressor stops when the validation score does not improve by at least the tolerance value (`tol`) for a specified number of consecutive epochs (`n_iter_no_change`), or after reaching `max_iter` .
- **`shuffle`**: Set `shuffle=True` in the constructor to shuffle training data after each epoch .
- **`average`**: Averaged SGD updates the weight vector to the average of weights from previous updates (e.g., `average=True` or `average=10` after seeing 10 samples) . SGD updates can be unstable when looking at one sample at a time; averaging provides better weight updates .

<img width="898" height="582" alt="image" src="https://github.com/user-attachments/assets/26f12535-1a6c-448b-a684-07405751ca15" />

### Baseline SGDRegressor Implementation

- **Step 1**: Instantiate a baseline `SGDRegressor` model with default parameters (`sgd = SGDRegressor(random_state=42)`) .
- **Step 2**: Train the model with the training feature matrix and labels (`sgd.fit(train_features, train_labels)`) .
- **Step 3**: Obtain the score/error on the training and development sets using mean absolute error (`mean_absolute_error`) .

### Step-Wise Training & Monitoring Loss Iteration by Iteration (`warm_start=True`)

- To monitor SGD loss iteration after iteration and implement early stopping mechanics, set `max_iter=1`, `tol=-np.infty`, and `warm_start=True` .
- Using a pipeline setup : ```python sgd_pipeline = Pipeline([ ("feature_scaling", StandardScaler()), ("SGD", SGDRegressor(max_iter=1, tol=-np.infty, warm_start=True, random_state=42)) ]) ```
- **Training Loop**: ```python for epoch in range(100): sgd_pipeline.fit(train_features, train_labels) # continues where it left off loss.append(mean_squared_error(train_labels, sgd_pipeline.predict(train_features))) ```
- Because `warm_start=True`, calling `.fit()` uses the parameter values learned in the previous iteration and updates them .
- Learning curves can then be plotted (e.g., `plt.plot(np.arange(len(loss)), loss, 'b-')`) to visualize convergence across iterations and examine the impact of different learning rates ($\eta_0$) .

> [!NOTE]
> Tuning hyperparameters such as learning rate ($\eta_0$), penalty terms, and early stopping criteria via validation curves is essential to ensure proper convergence when using `SGDRegressor`  .
>
>















# Module 3: Non-Linear & Distance-Based Models

## 5.6 - Demonstration: k-NN with California Housing Dataset

### Overview of k-NN in Regression

- k-NN can be used in addressing regression problems .
- This demonstration applies k-NN in a regression setup using the California housing dataset to predict the price of a house based on its features .

### Importing Libraries & Prerequisites

- The data is split into training and test sets using the `train_test_split` library .
- An `assert` statement verifies that the `sklearn` version is later than 0.20 for the colab to run .

### Pipeline Setup

- Model building makes use of a pipeline setup consisting of two stages: data preprocessing and estimator .
- **Preprocessing**: Uses the `MinMaxScaler` library .
- **Estimator**: Uses the `KNeighborsRegressor` estimator for the regression problem .

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3193c0da-9f3f-4fe1-b975-e0c45a3b3320" />

## 5.7 - Decision Trees for Regression

### Overview and Characteristics

- Decision trees are non-parametric supervised learning methods capable of finding complex non-linear relationships in data .
- They can perform both classification and regression tasks .
- Decision trees predict labels or values based on rules inferred from features in the training set .

### Scikit-Learn Implementation

- Scikit-learn uses an optimized version of the **CART** (Classification and Regression Tree) algorithm .
- Currently, scikit-learn decision trees do not support categorical variables .
- For regression tasks, scikit-learn provides the `sklearn.tree.DecisionTreeRegressor` API .
- Both classification (`DecisionTreeClassifier`) and regression (`DecisionTreeRegressor`) estimators share the same set of parameters except for the criterion used for tree splitting (e.g., `splitter`, `max_depth`, `min_samples_split`) .
- Notice the structural similarity in model fitting between Linear Regression and Decision Trees, as both are instances of the estimator class : ```python lin_reg.fit(train_features, train_labels) tree_reg.fit(train_features, train_labels) ```

<img width="800" height="1200" alt="image" src="https://github.com/user-attachments/assets/febd1677-191d-4c2d-8465-bf6408c28cf5" />

### Pipeline Setup & Cross-Validation

- A pipeline model can be set up using feature scaling (`StandardScaler`) as a pre-processing step followed by `DecisionTreeRegressor` : ```python dt_reg_pipeline = Pipeline([ ("feature_scaling", StandardScaler()), ("dt_reg", DecisionTreeRegressor(max_depth=3, random_state=42)) ]) ```
- Training and evaluation are performed using cross-validation (`cross_validate`) with `ShuffleSplit` cross-validation and `"neg_mean_absolute_error"` as the scoring mechanism .
- Cross-validation results show performance metrics such as mean absolute error on the train and test sets (e.g., train MAE of $0.590 \pm 0.005$ and test MAE of $0.593 \pm 0.007$) .

### Visualizing the Tree

- Decision tree outputs are intuitive and can be visualized as a tree diagram using tree APIs (`plot_tree`) .
- The `.fit()` function must be called on the pipeline object before printing or plotting the tree .
- **Tree Structure Details**:
- Displays split criteria (such as median income less than a threshold like $0.62$ at the root node) .
- Subsequent splits at lower levels rely on features like average room size, average occupancy, or median income .
- Each node displays the squared error (or MSE), number of samples, and predicted value .

<img width="1376" height="768" alt="image" src="https://github.com/user-attachments/assets/38549053-6678-4abc-9fc1-c45092a7c88f" />

### How Regression Trees Work

- Unlike classification trees that predict a class, a regression tree predicts a numerical value in each leaf node .
- For example, making a prediction for a new instance involves traversing the tree from the root to a leaf node .
- The prediction value at a leaf node is the **average target value** of the training instances associated with that leaf node, accompanied by a calculated mean squared error over those instances .

### Tips for Practical Usage

- Decision trees tend to overfit data with a large number of features; ensure an appropriate ratio of samples to features .
- Perform dimensionality reduction (such as PCA or Feature Selection) on data before training trees to improve the chances of finding discriminative features .
- Visualize the trained tree using `max_depth=3` as an initial tree depth to evaluate model fitment before increasing the depth .

> [!NOTE]
> Regularization techniques (such as setting `min_samples_leaf` or restricting `max_depth`) are crucial to prevent decision trees from overfitting the training data  .
>
>
