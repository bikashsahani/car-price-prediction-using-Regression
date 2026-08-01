# Car Price prediction - Using Regression Model
---

## Overview
- This project is about predicting the car price based on the input features by Multiple Linear Regression.
-----------------------

## Problem Statement
A Chinese automobile company Geely Auto aspires to enter the US market by setting up their manufacturing unit there and producing cars locally to give competition to their US and European counterparts.

They have contracted an automobile consulting company to understand the factors on which the pricing of cars depends. Specifically, they want to understand the factors affecting the pricing of cars in the American market, since those may be very different from the Chinese market. The company wants to know:

Which variables are significant in predicting the price of a car
How well those variables describe the price of a car
Based on various market surveys, the consulting firm has gathered a large data set of different types of cars across the America market

----------------

## Business Goal
We are required to model the price of cars with the available independent variables. It will be used by the management to understand how exactly the prices vary with the independent variables. They can accordingly manipulate the design of the cars, the business strategy etc. to meet certain price levels. Further, the model will be a good way for management to understand the pricing dynamics of a new market.

-----

#### 1. Data Understanding / EDA
- No missing values in any columns and no duplicate rows.
- the distribution of price is heavily right-skewed (positively skewed). It is not normally distributed.
- min car price = 5118.00, max car price = 45400.00, avg. car price = 13276.710571
- Lower Outlier Threshold: $-5,284.50, Upper Outlier Threshold: $29,575.50 and Total Outliers: 15
- Uncleaned Unique Companies: 27, Cleaned Unique Companies: 23
- Unique Companies List: ['alfa-romero', 'audi', 'bmw', 'buick', 'chevrolet', 'dodge', 'honda', 'isuzu', 'jaguar', 'mazda', 'mercury', 'mitsubishi', 'nissan', 'peugeot', 'plymouth', 'porcshce', 'porsche', 'renault', 'saab', 'subaru', 'toyota', 'volkswagen', 'volvo']
- Car company 'toyota' appears most frequently in the dataset.
- the count of gas car are much higher then diesel one which indicates that gas car in high demands. 
- the average price of gas car = 12999.7982 which is less then average price of diesel car = 15838.1500. Fuel type affect the price of the car.
- carbody strongly impacts the price of the car.
- Drive wheel also impact the price of the car.
- The car with rear engine is more expencive then the car with front engine.
- Four cylinder is the most common in the car.
- Cylinder number in car affect the car price. As the number of cylinders increases, car prices increase substantially. But in the case of 2 cylinder engine, it is special sports car engine so that it cost more then 3 cylinder engine.
- ohc engine type is the most common engine type.
- Engine type feature has the strongest positive correlation with price i.e 0.874145.


-----------

#### 2. Data Preprocessing 
- CarName need to be split into the company and model.
- there are inconsistent/misspelled company names (e.g. "maxda" vs "mazda", "porcshce" vs "porsche", "toyouta" vs "toyota", "vw"/"vokswagen" vs "volkswagen").
- car_ID and CarName should be dropped.
- 'doornumber' and 'cylindernumber' columns have wrong data type. i.e string/object. we need to convert that into the numerical values.
- there are no any negative or impossible values that indicates data entry errors.

----
#### 3. Feature Engineering
- Categorical variables must be encoded into numeric formats using distinct techniques based on whether they have a natural order:
- In highly correlated featues, one of them should be dropped to reduce multicollinearlity.
- A derived feature like power_to_weight ratio ($\frac{horsepower}{curbweight}$) adds substantial predictive value.

-----

#### 4. Standardization.
- We use StandardScaler to bring all continuous measurements (curbweight, horsepower, highwaympg, ratios) onto the same standard scale ($\mu=0, \sigma=1$). This prevents large range numerical values from dominating the models and decrease our accuracy of the model

------

## 5. Regression
#### 5.1 Linear regression:
- Performance Matrix:
                    MSE: 7392389.787943535
                    MAE: 1922.758772016182
                    RMSE: 2718.894957136729
                    R2 score: 0.9063590921063864

#### 5.2 Ridge Regression
- Performance Matrix:
                    MSE: 7653957.828865168
                    MAE: 1939.3599740011723
                    RMSE: 2766.578722694362
                    R2 score: 0.9030457564286868

#### 5.3 Lasso Regression
- Performance Matrix:
                    MSE: 7421229.099375359
                    MAE: 1927.429453874768
                    RMSE: 2724.1932933210446
                    R2 score: 0.9059937786714932



#### 5.4 ElasticNet Regression
- Performance Matrix:
                    MSE: 7568681.269740359
                    MAE: 1931.6862659560297
                    RMSE: 2751.1236376688635
                    R2 score: 0.9041259719811072

#### 5.5 Polyminal Regression
- Performance Matrix:
                    MSE: 35932468.28555776
                    MAE: 3847.7261779455407
                    RMSE: 5994.369715454475
                    R2 score: 0.544836101769718

#### 5.6 Decision Tree Regressor
- Performance Matrix:
                    MSE: 6529338.746387537
                    MAE: 1750.6626097560975
                    RMSE: 2555.257080293006
                    R2 score: 0.9172915355256497


#### 5.7 Decision Tree Regressor (with Hyperparameter Tuning)
- Best parameters:
            {'ccp_alpha': 0.1,
            'criterion': 'poisson',
            'max_depth': 10,
            'max_features': 'log2',
            'min_samples_leaf': 1,
            'min_samples_split': 10,
            'splitter': 'best'}
            
- Performance Matrix:
                    MSE: 17539226.22658768
                    MAE: 2662.0378790166474
                    RMSE: 4187.985939158306
                    R2 score: 0.7778270471765748

----
### Model Performance Comparison

| Model | $R^2$ Score | MSE | RMSE | MAE |
| :--- | :---: | :---: | :---: | :---: |
| **Decision Tree (default)** | **0.9173** | **6.53e+06** | **2,555.26** | **1,750.66** |
| **Linear Regression** | 0.9064 | 7.39e+06 | 2,718.89 | 1,922.76 |
| **Lasso Regression** | 0.9060 | 7.42e+06 | 2,724.19 | 1,927.43 |
| **ElasticNet** | 0.9041 | 7.57e+06 | 2,751.12 | 1,931.69 |
| **Ridge Regression** | 0.9030 | 7.65e+06 | 2,766.58 | 1,939.36 |
| **Decision Tree (tuned)** | 0.7778 | 1.75e+07 | 4,187.99 | 2,662.04 |
| **Polynomial Regression** | 0.5448 | 3.59e+07 | 5,994.37 | 3,847.73 |

------

#### Conclusion:
- Among all the Regression models used for the car price perdiction. Decision Tree (default) performs the best.
- Among the multiple independant features, 'enginesize' and 'curbweight' features contains altogether about 91% importance and other rest features have very little importance. so the main goal of the business is to focus on the 'enginesize' and 'curbweight' for the car market

------
#### Files:
- car_price_prediction.ipynb -----> main file
- CarPrice_Assignment.csv   ------> dataset file
- Car_Price_Data_Dictionary.xlsx -------> features information
- regressor.pkl         ---------> model dataset 
---

#### Tools Used
- Python, Pandas, NumPy
- Matplotlib, Seaborn
- Jupyter Notebook
- sklearn
- Regression

----
## 👤 Author
- Bikash Sahani github.com/bikashsahani | linkedin.com/in/bikash-sahani
