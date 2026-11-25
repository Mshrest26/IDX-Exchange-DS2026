# California Real Estate Price Prediction

## Project Overview

This project develops a machine learning model to predict the closing prices of residential properties across California using historical real estate transaction data obtained from the California Regional Multiple Listing Service (CRMLS).

The model takes property characteristics, such as living area, number of bedrooms, number of bathrooms, and other relevant features, as input variables and generates an estimated sales price as its output. The model is intended to support residential property price estimation for any individual property in California, regardless of whether the property is currently listed for sale or not.

## Project Objectives

The primary objectives of this project are to:

1. Explore and understand historical California real estate data.
2. Perform exploratory data analysis (EDA) to identify pricing trends and patterns.
3. Clean and preprocess data for machine learning.
4. Select relevant property features for price prediction.
5. Develop and train multiple machine learning models, including Linear Regression, Decision Tree, Random Forest, XGBoost, and LightGBM.
6. Evaluate and compare model performance using regression metrics.
7. Identify the best-performing model for predicting residential property sales prices.

## Dataset Description

- **Data Source:** California Regional Multiple Listing Service (CRMLS)
- **Property Type:** Residential – Single Family Residence
- **Data Type:** Historical real estate transaction data
- **Input Features:**
  - **Property Characteristics:** `LivingArea`, `BedroomsTotal`, `BathroomsTotalInteger`, `LotSizeSquareFeet`, `YearBuilt`, `Stories`, `GarageSpaces`
  - **Location Features:** `Latitude`, `Longitude`, `PostalCode`, `CountyOrParish`, `City`
  - **Property Amenities:** `NewConstructionYN`, `PoolPrivateYN`, `ViewYN`, `FireplaceYN`
- **Target Variable:** `ClosePrice` – the final sales price of the property.

## Exploratory Data Analysis (EDA)
1. Examined dataset structure, dimensionss, and data types.
2. Analyzed missing, zero, and duplicate values.
3. Investigated geographical coordinates and property locations.
4. Explored feature distributions and relationships with closing prices.

## Data Preprocessing
1. Date Filtering: Converted CloseDate to datetime format and removed records with missing dates or dates after October 30, 2026.
2. Duplicate Removal: Checked for and removed duplicate records.
3. Geographical Filtering: Removed properties outside California coordinate boundaries (Latitude:32-42, Longitude: -124 to -114).
4. Data Cleaning: Removed recoreds with non-positive ClosePrice, LivingArea, BedroomsTotal, BathroomsTotalInteger, invalid YearBuild (before 1900 or after 2026).
5. Missing Value Handling: Removed records with missing essential features, filled missing GarageSpaces with zero, and treated zero LotSizeSquareFeet values as missing. 
6. Price Filtering: Restricted closing prices to $50,000,000.
7. Outlier Treatment: Applied percentile-based thresholds to ClosePrice, LivingArea, LotSizeSquareFeet, BedroomsTotal, and BathroomsTotalInteger.
8. Chronological Splitting: Used separate time-based training, validation, and test datasets to evaluate model performance on future transactions.

#### Dataset Split

| Dataset | Period | Records After Scope |
|---|---|---:|
| Training | Oct 2025 – Jul 2026 | 111,570 |
| Validation | Aug 2026 | 11,448 |
| Testing | Sep 2026 | 11,254 |

## Feature Engineering and Selection
New features were created to capture additional property characteristics and location-based pricing patterns.
| Feature | Description |
|---|---|
| `Bed_Bath_Ratio` | Ratio of bedrooms to bathrooms. |
| `PropertyAge` | Property age calculated from `YearBuilt`. |
| `city_median_price` | Historical median closing price by City. |
| `zipcode_median_ppsf` | Historical median price per square foot by ZIP code. |
| `LogLivingArea` | Log-transformed living area to reduce skewness. |
| `SchoolDistrict` | School district associated with the property location. |

## Machine Learning Models

The following models were trained and compared:

- Linear Regression
- Decision Tree Regressor
- Random Forest Regressor
- XGBoost Regressor
- LightGBM Regressor

## Model Evaluation

#### Test Set Performance
The trained models were evaluated using the September 2026 test dataset. The following table summarizes the performance of each model based on $R^2$, Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), Median Absolute Error, and Median Absolute Percentage Error (MDAPE). 

| Model | Test R² (%) | MAE (\$) | RMSE (\$) | Median Absolute Error (\$) | MDAPE (%) |
|---|---:|---:|---:|---:|---:|
| LightGBM | 82.895 | 201,292.396 | 574,334.366 | 81,477.493 | 9.102 |
| XGBoost | 83.095 | 205,958.532 | 570,966.273 | 85,868.344 | 9.728 |
| Random Forest | 82.749 | 210,760.658 | 576,790.503 | 87,293.725 | 9.573 |
| Decision Tree | 75.158 | 271,340.494 | 692,158.168 | 124,610.865 | 14.033 |
| Linear Regression | 65.763 | 360,478.098 | 812,557.519 | 222,463.721 | 22.770 |

## Price Band Analsyis

| Price Band | Best Performing Model | MDAPE (%) |
|---|---|---:|
| Under \$300K | XGBoost | 26.10 |
| \$300K - \$750K | LightGBM | 7.90 |
| \$750K - \$2M | LightGBM | 8.83 |
| \$2M - \$5M | XGBoost | 12.63 |
| \$5M - \$10M | XGBoost | 19.60 |
| Over \$10M | Random Forest | 21.09 |

## Key Findings and Insights

LightGBM achieved its strongest performance in the \$300K - \$750K price band, with an MDAPE of 7.90%, followed by the \$750K - \$2M segment at 8.83%. XGBoost performed best in three price bands, while Random Forest performed best for properties valued over \$10M.

These results demonstrate that predictive performance varies across property price ranges, highlighting the importance of evaluating models at different price levels rather than relying solely on overall performance metrics.

Based on the results, LightGBM was selected as the overall best-performing model, with its strongest price-band performance in the \$300K-\$2M range. 

## Limitations

- **Price Range Variability:** Model performance varied across property price bands, with lower-priced properties (under \$300K) and high-value properties (over \$10M) presenting greater prediction challenges.

- **Model Generalization:** Although chronological data splitting was used to evaluate future transactions, changes in market conditions over time may affect the model's predictive accuracy.

- **Feature Limitations:** Model performance depends on the availability and quality of property features. Missing or incomplete information may limit predictive accuracy.

## Conclusion

This project explored the use of machine learning to predict real estate transaction prices using historical transaction data. Multiple models were developed and evaluated using a chronological data split to assess their predictive performance on future transactions.

LightGBM was selected as the overall best-performing model based on its balance of prediction accuracy and error metrics. However, the price band analysis showed that no single model performed best across all property price ranges.

The findings highlight the importance of feature engineering, appropriate model evaluation, and price-specific performance analysis when developing real estate price prediction models.

## GitHub Repository Structure

```text
IDX-Exchange-DS2026/
│
├── README.md
│
├── notebooks/
│   ├── 01_exploratory.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_baseline_model.ipynb
│   ├── 04_model_comparison.ipynb
│   ├── 04_model_comparision_Feature_engineering.ipynb
│   ├── 05_advanced_models.ipynb
│   └── 06_evaluation.ipynb
│
├── outputs/
│
└── requirements.txt
```

## Run The Project

#### Install Dependencies

```bash
pip install -r requirements.txt
```

#### Re-run the Notebooks

Run the notebooks in order:
```bash
01_exploratory.ipynb
02_preprocessing.ipynb
03_baseline_model.ipynb
04_model_comparison.ipynb
04_model_comparison_Feature_engineering.ipynb
05_advanced_models.ipynb
06_evaluation.ipynb
```