# Forecasting Cabai Rawit Merah Price

A time series forecasting project for analyzing and predicting **Red Cayenne Pepper (Cabai Rawit Merah)** prices in Indonesia using statistical and machine learning-based forecasting approaches in R.

The project compares multiple forecasting model families, evaluates their performance using RMSE, MAE, and MAPE, and identifies the best-performing model based on forecasting accuracy.

---

## Overview

Agricultural commodity prices in Indonesia can fluctuate considerably over time. **Cabai Rawit Merah (Red Cayenne Pepper)** is one of the commodities that experiences notable price movements, making it an interesting case for time series analysis and forecasting.

This project applies a systematic time series forecasting workflow to historical Cabai Rawit Merah price data.

The analysis covers:

- Data preprocessing and transformation
- Exploratory Data Analysis (EDA)
- Time series characteristics analysis
- Stationarity analysis
- Box-Cox transformation
- Train-test splitting
- Multiple forecasting model families
- Forecast evaluation
- Model comparison
- Forecast vs. Actual visualization

The main objective is to compare different forecasting approaches and evaluate their predictive performance on the testing period.

---

## Objectives

This project aims to:

1. Analyze the historical price movement of Cabai Rawit Merah.
2. Explore the distribution and characteristics of the price series.
3. Examine the stationarity and time series structure of the data.
4. Apply appropriate transformations before modeling.
5. Compare different forecasting approaches.
6. Evaluate forecasting performance using RMSE, MAE, and MAPE.
7. Identify the best-performing model within each model family based on MAPE.
8. Visualize the relationship between actual and forecasted prices.

---

## Dataset

The dataset contains historical price observations for **Cabai Rawit Merah**.

The original dataset is processed from a regional commodity price table and filtered specifically for the commodity:

> **Cabai Rawit Merah**

The analysis covers the historical period shown in the project, from **2021 to 2026**.

### Main Variables

| Variable | Description |
|---|---|
| `Date` | Observation date |
| `Cabai_Rawit_Merah` | Price of Red Cayenne Pepper in IDR |

The raw dataset used in the repository is:

```text
raw_dataset.xlsx
```

---

## Methodology

The project follows the following workflow:

```text
Raw Dataset
     │
     ▼
Data Preprocessing
     │
     ▼
Exploratory Data Analysis
     │
     ▼
Time Series Characteristics
     │
     ├── Decomposition
     ├── Variance Stationarity
     ├── Box-Cox Transformation
     └── ADF Test
     │
     ▼
Train-Test Split
     │
     ├── 80% Training
     └── 20% Testing
     │
     ▼
Forecasting Models
     │
     ├── Naive
     ├── Exponential Smoothing
     ├── ARIMA
     ├── SARIMA
     ├── Time Series Regression
     └── Neural Network (NNAR)
     │
     ▼
Model Evaluation
     │
     ├── RMSE
     ├── MAE
     └── MAPE
     │
     ▼
Model Comparison
     │
     ▼
Forecast vs. Actual Visualization
```

---

# 1. Data Preprocessing

The raw dataset is first prepared for time series analysis.

### Steps

1. Load the required R packages.
2. Import the raw Excel dataset.
3. Select the Cabai Rawit Merah commodity.
4. Restructure the dataset into a time series format.
5. Convert the date variable into a proper date format.
6. Convert price values into numeric format.
7. Handle missing values using linear interpolation.
8. Validate the final dataset.
9. Construct the time series object.

The resulting time series is constructed with a frequency of **5**.

---

# 2. Exploratory Data Analysis

Exploratory Data Analysis (EDA) is performed to understand the behavior and distribution of Cabai Rawit Merah prices before modeling.

## 2.1 Time Series Plot

The historical price series is visualized to observe:

- Price movements
- Trends
- Fluctuations
- Potential seasonal patterns
- Periods of relatively high or low prices

## 2.2 Price Distribution

A histogram is used to examine the distribution of Cabai Rawit Merah prices.

## 2.3 Outlier Detection

A boxplot is used to identify potential extreme observations in the price series.

---

# 3. Time Series Characteristics

Several analyses are conducted to investigate the underlying characteristics of the time series.

## 3.1 Time Series Visualization

The time series is visualized using the `forecast` package to provide an overview of the series structure.

## 3.2 Time Series Decomposition

The series is decomposed into its components to investigate its underlying structure.

The project uses:

- STL decomposition
- Classical multiplicative decomposition
- Classical additive decomposition

This helps examine the contribution of:

- Trend
- Seasonal component
- Remainder / irregular components

## 3.3 Variance Stationarity

A power transformation analysis is performed using `powerTransform()` to investigate whether a transformation is required to stabilize the variance.

## 3.4 Box-Cox Transformation

A Box-Cox transformation is applied using:

```r
lambda <- -0.3426
```

The transformed series is then used for the ARIMA and SARIMA modeling procedures.

The inverse Box-Cox transformation is applied to the forecasts before comparing them with the original-scale test data.

## 3.5 Stationarity Analysis

The project applies the **Augmented Dickey-Fuller (ADF) test** to examine stationarity.

Additionally, ACF and PACF plots are generated to investigate the autocorrelation structure of the transformed series.

---

# 4. Helper Functions

Several helper functions are created to make the model evaluation and comparison process more consistent.

## 4.1 Forecast Evaluation

The `eval_forecast()` function calculates:

- RMSE
- MAE
- MAPE

for each forecasting model.

## 4.2 Best Model Selection

The `get_best_model()` function identifies the model with the lowest value of the selected evaluation metric.

The default metric used for model selection is **MAPE**.

## 4.3 Family Result Summary

The `print_family_table()` function displays the forecasting performance of all models within a model family and identifies the best-performing model.

---

# 5. Train-Test Split

The dataset is divided chronologically into:

- **80% training data**
- **20% testing data**

The training data is used to estimate the forecasting models, while the testing data is reserved for evaluating out-of-sample forecasting performance.

No random shuffling is applied because the temporal ordering of observations must be preserved in time series forecasting.

The same train-test approach is also applied to the transformed series used for ARIMA and SARIMA.

---

# 6. Forecasting Model Families

The project compares six major forecasting model families:

1. **Naive**
2. **Exponential Smoothing**
3. **ARIMA**
4. **SARIMA**
5. **Time Series Regression**
6. **Neural Network (NNAR)**

Each family contains multiple model specifications.

---

## 6.1 Family A: Naive

Two baseline forecasting methods are evaluated.

### Naive

The forecast for future observations is based on the most recent observed value.

### Seasonal Naive

The forecast uses the corresponding value from the previous seasonal cycle.

Both models are evaluated using the same testing period.

---

## 6.2 Family B: Exponential Smoothing

The project evaluates several exponential smoothing specifications.

### Holt-Winters

A Holt-Winters model with additive seasonality is fitted to the training data.

### ETS Models

Several ETS configurations are evaluated using different combinations of:

- Alpha
- Beta
- Gamma

The tested parameter configurations include:

```text
ETS(0.2,0.1,0.1)
ETS(0.4,0.1,0.1)
ETS(0.6,0.1,0.1)
ETS(0.6,0.3,0.1)
ETS(0.6,0.5,0.1)
ETS(0.6,0.5,0.2)
ETS(0.6,0.5,0.3)
ETS(0.5,0.5,0.4)
ETS(0.5,0.5,0.5)
```

An automatically selected ETS model is also evaluated using:

```r
ets(train_merah)
```

The performance of all exponential smoothing models is compared using RMSE, MAE, and MAPE.

---

## 6.3 Family C: ARIMA

ARIMA models are fitted using the **Box-Cox transformed time series**.

The evaluated model specifications include:

```text
ARIMA(1,0,0)
ARIMA(2,0,0)
ARIMA(0,0,1)
ARIMA(0,0,2)
ARIMA(1,0,1)
ARIMA(2,0,1)
ARIMA(0,1,1)
ARIMA(1,1,0)
ARIMA(1,1,1)
```

Forecasts are transformed back to the original price scale using the inverse Box-Cox transformation before evaluation.

### ARIMA Diagnostics

The project also performs:

- Coefficient significance analysis
- Lilliefors normality test on residuals
- AIC comparison between models

The AIC values are sorted to provide an additional model diagnostic for the ARIMA family.

---

## 6.4 Family C: SARIMA

Seasonal ARIMA models are also fitted using the Box-Cox transformed data.

The seasonal period is set to:

```text
5
```

Several combinations of non-seasonal and seasonal orders are evaluated.

The tested configurations include:

```text
SARIMA(1,0,0)(1,0,0)[5]
SARIMA(2,0,0)(1,0,0)[5]
SARIMA(0,0,1)(0,0,1)[5]
SARIMA(0,0,2)(0,0,1)[5]
SARIMA(1,0,1)(1,0,1)[5]
SARIMA(2,0,1)(1,0,1)[5]
SARIMA(0,1,1)(0,0,1)[5]
SARIMA(1,1,0)(1,0,0)[5]
SARIMA(1,1,1)(0,0,1)[5]
```

The models are evaluated based on their out-of-sample forecasting performance.

Coefficient diagnostics and AIC values are also examined.

---

## 6.5 Family D: Time Series Regression

A time series regression approach is implemented using lagged values of the price series as explanatory variables.

The following lag structures are evaluated:

```text
Regression(lag1+lag2+lag3)
Regression(lag1+lag3)
Regression(lag1)
Regression(lag3)
```

A recursive forecasting procedure is used to generate multi-step forecasts.

In recursive forecasting, previously generated forecasts are used as lagged inputs for subsequent predictions.

This allows the regression models to generate forecasts over the entire testing horizon.

---

## 6.6 Family E: Neural Network (NNAR)

The project also evaluates **Neural Network Autoregression (NNAR)** models using the `nnetar()` function from the `forecast` package.

The tested configurations include:

```text
NNAR(1,0,2)
NNAR(2,0,2)
NNAR(3,0,2)
NNAR(2,0,3)
NNAR(5,0,3)
NNAR(7,0,4)
NNAR(10,0,5)
NNAR(1,1,2)
NNAR(3,1,2)
NNAR(5,1,3)
NNAR(7,1,4)
NNAR(10,1,5)
```

An automatically configured NNAR model is also evaluated.

A fixed random seed is used:

```r
set.seed(123)
```

to improve reproducibility of the neural network results.

---

# 7. Model Evaluation

All forecasting models are evaluated using three performance metrics.

## RMSE

**Root Mean Squared Error**

RMSE measures the square root of the average squared difference between actual and predicted values.

```text
RMSE = sqrt(mean((Actual - Forecast)^2))
```

A lower RMSE indicates smaller forecasting errors.

## MAE

**Mean Absolute Error**

MAE measures the average absolute difference between actual and predicted values.

```text
MAE = mean(|Actual - Forecast|)
```

A lower MAE indicates smaller average forecasting errors.

## MAPE

**Mean Absolute Percentage Error**

MAPE measures the average absolute percentage error between actual and predicted values.

```text
MAPE = mean(|(Actual - Forecast) / Actual|) × 100%
```

A lower MAPE indicates better forecasting accuracy.

---

# 8. Model Selection

For each model family, the model with the lowest **MAPE** is selected as the best model within that family.

The selected models are then combined into a main comparison table containing:

| Family | Model | RMSE | MAE | MAPE |
|---|---|---:|---:|---:|
| Naive | Best model | — | — | — |
| Exponential Smoothing | Best model | — | — | — |
| ARIMA | Best model | — | — | — |
| SARIMA | Best model | — | — | — |
| Regression | Best model | — | — | — |
| Neural Network | Best model | — | — | — |

The comparison allows the forecasting performance of the different model families to be evaluated using the same testing dataset.

The exact model names and metric values are generated when the R code is executed.

---

# 9. Forecast vs. Actual Visualization

The project generates visual comparisons between:

- Actual prices
- Forecasted prices

for individual models and the best-performing model from each family.

The individual visualizations cover:

- Naive
- Seasonal Naive
- Exponential Smoothing
- ARIMA
- SARIMA
- Time Series Regression
- NNAR

A final combined visualization compares the **best model from each family**.

The combined visualization includes:

- Naive
- Exponential Smoothing
- ARIMA
- SARIMA
- Time Series Regression
- NNAR

This allows the predicted price trajectory to be visually compared against the actual observations during the testing period.

---

# 10. Reproducibility

The project uses a fixed random seed for NNAR models:

```r
set.seed(123)
```

The data is split chronologically rather than randomly to preserve the temporal structure of the observations.

The complete workflow is provided in both R Script and R Markdown formats.

---

# Tools & Technologies

The project was developed using:

- **R**
- **R Markdown**
- **RStudio**
- **Excel**
- **Time Series Analysis**
- **Statistical Forecasting**
- **Machine Learning**

### R Packages

The following packages are used throughout the analysis:

```r
library(readxl)
library(dplyr)
library(lubridate)
library(forecast)
library(tseries)
library(ggplot2)
library(Metrics)
library(nortest)
library(lmtest)
library(car)
library(pracma)
library(tidyverse)
```

---

# Repository Structure

```text
Forecasting-Cabai-Rawit-Merah-Price/
│
├── README.md
│   └── Project documentation
│
├── code.R
│   └── Complete R analysis and forecasting code
│
├── code.Rmd
│   └── R Markdown version of the analysis
│
├── raw_dataset.xlsx
│   └── Raw dataset used for the analysis
│
└── paper.pdf
    └── Complete project report
```

---

# How to Run

## 1. Clone the Repository

```bash
git clone https://github.com/asyifaizza/Forecasting-Cabai-Rawit-Merah-Price.git
```

Navigate into the repository:

```bash
cd Forecasting-Cabai-Rawit-Merah-Price
```

## 2. Install Required Packages

Open R or RStudio and run:

```r
install.packages(c(
  "readxl",
  "dplyr",
  "lubridate",
  "forecast",
  "tseries",
  "ggplot2",
  "Metrics",
  "nortest",
  "lmtest",
  "car",
  "pracma",
  "tidyverse"
))
```

## 3. Run the Analysis

Open:

```text
code.R
```

in RStudio and run the script sequentially.

Alternatively, open:

```text
code.Rmd
```

and use:

```text
Knit → HTML
```

to generate the analysis report.

---

# Project Outputs

The analysis produces:

- Historical time series plots
- Price distribution plots
- Boxplots for outlier inspection
- Time series decomposition
- Stationarity analysis
- ACF and PACF plots
- Training and testing split visualization
- Forecast results for multiple models
- RMSE, MAE, and MAPE evaluation
- Best model selection for each model family
- Forecast vs. Actual plots
- Combined comparison of the best model from each family

The complete written analysis is available in:

```text
paper.pdf
```

---

# Key Takeaways

This project demonstrates an end-to-end time series forecasting workflow applied to an Indonesian agricultural commodity.

The analysis combines classical statistical forecasting methods with a neural network-based approach.

The main forecasting approaches evaluated are:

| Model Family | Approach |
|---|---|
| Naive | Naive and Seasonal Naive |
| Exponential Smoothing | Holt-Winters and ETS |
| ARIMA | Multiple ARIMA specifications |
| SARIMA | Multiple seasonal ARIMA specifications |
| Regression | Lag-based time series regression |
| Neural Network | NNAR |

Each model is evaluated on the same testing framework using RMSE, MAE, and MAPE.

The project provides a structured comparison of different forecasting approaches for Cabai Rawit Merah price prediction.

---

# Academic Context

This project was developed as part of a **Time Series Analysis** assignment.

It demonstrates the practical application of statistical forecasting techniques to real-world agricultural commodity data using the R programming language.

The complete academic report, methodology, results, and discussion are available in:

```text
paper.pdf
```

---

# Author

- **Asyifa Izzatil Isma**
- **M. Aufa Mumtaza Ibadillah**

Computer Science & Statistics  
BINUS University

GitHub: 
- [@asyifaizza](https://github.com/asyifaizza)
- [@mumtazaufaa](https://github.com/mumtazaufaa)

---

# License

This repository is intended for **educational, academic, and portfolio purposes**.
