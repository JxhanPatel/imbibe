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
