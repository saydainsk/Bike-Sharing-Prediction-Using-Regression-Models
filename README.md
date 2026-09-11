# Bike Sharing Demand Prediction Using Regression Models

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![statsmodels](https://img.shields.io/badge/statsmodels-OLS-4051B5)](https://www.statsmodels.org/)

## Project Overview

This project builds an interpretable **multiple linear regression model** to explain and predict daily demand for a bike-sharing service. The analysis identifies the environmental, seasonal, and calendar-related factors that influence the total number of rentals (`cnt`) and translates the model findings into practical business insights.

The complete workflow is implemented in a Jupyter Notebook and includes data inspection, exploratory data analysis, categorical encoding, feature scaling, feature selection, multicollinearity checks, residual analysis, and model evaluation.

## Business Problem

A bike-sharing provider wants to better understand the factors that drive demand so it can plan fleet availability, improve operations, and prepare for changes in ridership.

This project addresses the following questions:

- Which variables significantly influence daily bike demand?
- How do weather, season, temperature, and calendar effects change rentals?
- Can daily rental demand be represented with an interpretable regression model?
- Which factors should receive the greatest attention during operational planning?

## Dataset

The repository contains `day.csv`, a daily bike-sharing dataset with **730 observations** and **16 original columns** covering two years, 2018 and 2019.

| Feature | Description |
|---|---|
| `instant` | Record index |
| `dteday` | Date |
| `season` | Season category |
| `yr` | Year indicator: 0 = 2018, 1 = 2019 |
| `mnth` | Month |
| `holiday` | Whether the day is a holiday |
| `weekday` | Day of the week |
| `workingday` | Whether the day is a working day |
| `weathersit` | Weather condition category |
| `temp` | Actual temperature |
| `atemp` | Feels-like temperature |
| `hum` | Humidity |
| `windspeed` | Wind speed |
| `casual` | Number of casual users |
| `registered` | Number of registered users |
| `cnt` | Total bike rentals and prediction target |

Because `cnt` is the sum of `casual` and `registered`, those two columns are removed before modeling to prevent target leakage. The identifier and date fields are also excluded, and `atemp` is removed because of its strong overlap with `temp`.

## Project Workflow

### 1. Data understanding and quality checks

- Loaded and inspected the dataset using Pandas.
- Reviewed column types, summary statistics, and dataset dimensions.
- Confirmed that the data contains no missing values, so imputation was not required.
- Mapped numeric codes for season, month, weekday, and weather to readable categories.

### 2. Exploratory data analysis

- Used pair plots, box plots, bar plots, scatter plots, and a correlation heatmap.
- Examined rental demand across seasons, weather conditions, months, years, holidays, and weekdays.
- Investigated the relationships among temperature, humidity, wind speed, and total demand.

### 3. Data preparation

- Created dummy variables for categorical features with `drop_first=True` to avoid the dummy-variable trap.
- Split the data into **70% training** and **30% test** sets using `random_state=1000`.
- Applied MinMax scaling to `temp`, `hum`, `windspeed`, and `cnt`.

### 4. Model development and feature selection

- Built an Ordinary Least Squares regression model with Statsmodels.
- Reviewed coefficient estimates and p-values to measure feature significance.
- Calculated Variance Inflation Factor (VIF) values to detect multicollinearity.
- Used Recursive Feature Elimination (RFE) and iterative feature removal to obtain a smaller, interpretable feature set.

### 5. Model diagnostics and evaluation

- Examined the distribution of residuals.
- Checked residual independence using the Durbin-Watson statistic.
- Compared actual and predicted demand visually.
- Reported R-squared and adjusted R-squared for the final models.

## Key Findings

- **Temperature** has the strongest positive relationship with bike demand.
- Demand was higher in **2019** than in 2018, suggesting overall growth in the service.
- **September** contributes positively to demand.
- **Spring**, higher **humidity**, and stronger **wind speed** are associated with lower demand in the final simplified model.
- Exploratory analysis showed the highest observed rentals during **fall** and under **clear weather** conditions.
- `temp` and `atemp` each had an approximate correlation of **0.63** with `cnt`, while `yr` had an approximate correlation of **0.57**.

## Model Results

The notebook reports the following final performance:

| Dataset | R-squared | Adjusted R-squared |
|---|---:|---:|
| Training set | 0.832 | 0.828 |
| Test set | 0.829 | 0.824 |

These results indicate that the fitted regression models explain approximately **83% of the variation** in daily bike-sharing demand.

The final simplified equation reported in the notebook is:

```text
Predicted demand = 0.3716
                 + 0.0645 × September
                 - 0.1730 × Spring
                 + 0.2827 × Year
                 + 0.4096 × Temperature
                 - 0.2391 × Humidity
                 - 0.2140 × Wind speed
```

> The coefficients above operate on the notebook's scaled variables. They describe direction and relative influence in the fitted model and should not be interpreted directly as changes in raw rental counts.

## Business Recommendations

- Increase fleet availability and rebalancing capacity on warmer days and during high-demand months.
- Use temperature forecasts as a primary input for short-term demand planning.
- Plan targeted promotions or alternative operational strategies for spring and adverse-weather periods.
- Account for the negative effects of high humidity and wind speed when forecasting daily demand.
- Investigate the year-over-year growth driver and incorporate a time trend into future forecasting models.

## Repository Structure

```text
Bike-Sharing-Prediction-Using-Regression-Models/
├── Bike Sharing Predictor Model .ipynb    # Analysis and model development
├── BIKE SHARE REGRESSION MODEL QUESTIONS.pdf
│                                           # Project interpretation and theory
├── day.csv                                 # Daily bike-sharing dataset
└── README.md                               # Project documentation
```

## Technologies Used

- Python
- Jupyter Notebook
- Pandas and NumPy
- Matplotlib and Seaborn
- scikit-learn
- Statsmodels

## How to Run the Project

1. Clone the repository:

   ```bash
   git clone https://github.com/saydainsk/Bike-Sharing-Prediction-Using-Regression-Models.git
   cd Bike-Sharing-Prediction-Using-Regression-Models
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   ```

   On Windows PowerShell:

   ```powershell
   .\.venv\Scripts\Activate.ps1
   ```

   On macOS or Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Install the required libraries:

   ```bash
   pip install jupyter numpy pandas matplotlib seaborn scikit-learn statsmodels
   ```

4. Start Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open `Bike Sharing Predictor Model .ipynb` and run the cells in order.

## Potential Improvements

- Fit preprocessing steps only on the training data and reuse the fitted scaler for the test data.
- Evaluate the training model directly on untouched test data without performing feature selection or refitting on the test set.
- Add MAE and RMSE alongside R-squared for a more complete evaluation.
- Compare linear regression with Ridge, Lasso, Random Forest, Gradient Boosting, and XGBoost.
- Add cross-validation and hyperparameter tuning.
- Build a reusable inference pipeline or deploy the model through a Streamlit application or REST API.
- Add automated tests and a pinned `requirements.txt` file for reproducibility.

## Author

**Saydain Sheikh**

- [GitHub](https://github.com/saydainsk)
- [LinkedIn](https://www.linkedin.com/in/saydain-sheikh/)

## Acknowledgements

This project was developed as a regression modeling case study focused on understanding bike-sharing demand through exploratory analysis and interpretable statistical modeling.
