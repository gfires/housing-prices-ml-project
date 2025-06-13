# House Price Prediction Models (Ames, IA)

This repository contains machine learning models developed to predict housing sale prices in Ames, Iowa, based on a dataset spanning 2006-2010. The project explores various algorithms, including Linear Regression (and its regularized variants, Ridge and Lasso), Neural Networks, and Random Forests, to determine the most accurate prediction model.

## Table of Contents

* [Introduction](#introduction)
* [Dataset](#dataset)
* [Preprocessing & Transformation](#preprocessing--transformation)
* [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
* [Modeling](#modeling)
    * [Linear Regression](#linear-regression)
    * [Ridge Regression (L2)](#ridge-regression-l2)
    * [Lasso Regression (L1)](#lasso-regression-l1)
    * [Neural Network](#neural-network)
    * [Random Forest](#random-forest)
* [Results](#results)
* [Limitations & Future Work](#limitations--future-work)
* [Installation & Usage](#installation--usage)

## Introduction

The primary goal of this project is to create a model that can accurately predict a house's sale price using various explanatory variables. By comparing the performance of different algorithms, the project aims to help consumers better understand housing pricing dynamics.

## Dataset

The dataset used for this project consists of 1460 entries of housing prices in Ames, IA, recorded between 2006 and 2010. Each entry includes 79 explanatory variables, with the target variable being the 'Sale price'.

## Preprocessing & Transformation

Data preprocessing and transformation steps were crucial to prepare the dataset for modeling.

### Data Cleaning
* Houses with missing 'Electrical' system data were dropped (only one instance).
* The 'Utilities' column was removed after dropping a single house without all public utilities.
* Houses with pools were eliminated, along with their corresponding 'PoolArea' and 'PoolQC' columns.
* 'MasVnrArea' (Masonry veneer area) was set to 0 for houses without masonry veneer.
* 'LotFrontage' was assigned 0 for houses not connected to the street.
* 'GarageArea' and 'GarageCars' were set to 0 for houses without a garage.
* Missing 'GarageYrBlt' (Garage Year Built) values were filled with the mean of the existing values.

### Data Transformation
* **One-Hot Encoding:** Applied to categorical variables like 'Foundation' and 'CentralAir'.
    * 'Foundation' with 6 values was transformed into 5 columns.
    * 'CentralAir' (binary Y/N) was transformed into one column.
    * 'Foundation\_PConc' showed a correlation of 0.503785 with SalePrice.
    * 'Foundation\_CBlock' showed a correlation of -0.347495 with SalePrice.
    * 'CentralAir' showed a correlation of 0.254504 with SalePrice.
* **Label Encoding:** Applied to ordinal categorical variables by mapping them to numerical scales (e.g., 1-5).
    * 'ExterQual' (Exterior Quality) and 'KitchenQual' (Kitchen Quality) were encoded from 1 (poor) to 5 (excellent).
    * 'ExterQual' showed a correlation of 0.692655 with SalePrice.
    * 'KitchenQual' showed a correlation of 0.663369 with SalePrice.
    * 'BsmtQual' (Basement Quality) was encoded from 0 (no basement) to 5.
    * 'BsmtQual' showed a correlation of 0.588895 with SalePrice.

### Data Split and Scaling
* The dataset was split into a training group (Size: 1160) and a testing group (Size: 291).
* Numerical variables were standardized using `StandardScaler` to prepare them for modeling.

## Exploratory Data Analysis (EDA)

### Sale Price Distribution
* The 'SalePrice' distribution is right-skewed.
* Mean SalePrice: \$180,443.1.
* Median SalePrice: \$162,900.
* Minimum SalePrice: \$34,900.
* Maximum SalePrice: \$755,000.
* Standard Deviation: \$78,213.9.

### Correlation Analysis
* Pearson correlation coefficients were calculated for numerical values.
* Variables with the highest positive correlation to 'SalePrice' include: 'Overall Qual' (0.790982), 'GrLivArea' (0.708624), 'GarageCars' (0.640409), 'GarageArea' (0.623431), and 'TotalBsmtSF' (0.613581).
* 'YearBuilt' (0.522897) and 'YearRemodAdd' (0.507101) also show significant positive correlations.

### Categorical Variables
* Analysis of 'Sale Price in Different Neighborhoods' shows that a higher median sale price is often associated with a wider range of sale prices. The interquartile range suggests that the neighborhood is probably associated with the sale price distribution.

### Quantitative Variables
* Visualizations of 'Sale Price vs YearBuilt' and 'Sale Price vs Overall Quality' demonstrate trends where higher overall quality and newer construction generally correlate with higher sale prices.

## Modeling

The project establishes an efficient model to accurately predict housing prices, focusing on numerical variables after cleaning and encoding. The models were standardized and then split into training and testing sets.

### Linear Regression
* **Results:**
    * RMSE: 26318.1
    * MAE: 19200.1
    * $R^2$: 0.8861
    * Training Time: 0.005s
* The "Normal Distribution" and "Randomized" nature of residuals suggest low bias.

### Ridge Regression (L2)
* **Results ($\alpha=100$):**
    * RMSE: 26139.9
    * MAE: 18805.9
    * $R^2$: 0.8877
    * Training Time: 0.126s
* Further increasing alpha decreases $R^2$.

### Lasso Regression (L1)
* **Results ($\alpha=100$):**
    * RMSE: 26221.5
    * MAE: 19034.3
    * $R^2$: 0.8870
    * Training Time: 0.099s

### Neural Network
* **Model Setup:**
    * ReLU Activation Function.
    * Two dense layers with 256 and 128 nodes, respectively.
    * Adam optimizer with Mean Squared Error (MSE) loss function.
* **Takeaways:**
    * $R^2$ value of 0.89 indicates strong fit.
    * Cheaper houses showed the best prediction performance.
    * Large positive residuals indicate that some expensive houses were underpredicted.

### Random Forest
* **Model Setup:**
    * 1750 decision trees.
    * 16 maximum depth of trees.
* **Takeaways:**
    * $R^2$ value of 0.76 indicates good fit.
    * Long training time, inconsistent results because of the random decision tree sampling.
    * Cheaper houses predicted well, overall a poor predictor.

## Results

The Neural Network model demonstrated the best performance for the training data. It effectively observes nonlinear patterns and learns how different features interact due to its multiple layers of computation. Additionally, the Neural Network scales better for larger datasets compared to regression models.

| Model             | Training Time (s) | $R^2$ Score |
| :---------------- | :--------------------------- | :--------------------- |
| Linear Regression | 0.258                        | 0.8825                 |
| LASSO (L1)        | 0.177                        | 0.8826                 |
| Ridge (L2)        | 0.115                        | 0.8826                 |
| Neural Network    | 3.124                        | 0.8957                 |
| Random Forest     | 45.881                       | 0.7644                 |

## Limitations & Future Work

### Limitations
* The data between 2006 and 2010 spans the housing market crash, but 'year sold' is only considered as an explanatory variable, not as a time-series element.
* The use of many parameters, particularly ones with minimal correlations with the sale price, can lead to overfitting.

### Future Work
* Incorporating time-dependency into the models.
* Modeling housing prices in different cities.
* Capturing a greater range of prices for improved prediction accuracy.

## Installation & Usage

To run the models and reproduce the results, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd <repository-name>
    ```

2.  **Install dependencies:**
    ```bash
    pip install pandas numpy seaborn matplotlib scikit-learn tensorflow
    ```

3.  **Prepare the dataset:**
    Ensure `train.csv` is in the root directory of the project.

4.  **Run the Python script:**
    ```bash
    python housing_workshop.py
    ```
    The script will perform data cleaning, encoding, train-test splitting, scaling, model building, training, and evaluation for all discussed models, printing metrics and generating plots.