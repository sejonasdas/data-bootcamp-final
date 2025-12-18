# Forecasting U.S. Rental Affordability: A Time-Series Forecasting Analysis

**Author:** Sejona Sujit Das (sd5466) 
**Data Bootcamp Final Project**

## Project Overview

This project develops predictive models to forecast monthly rental prices across the top 20 U.S. metropolitan areas. Using Zillow Observed Rent Index data (2015-2025) combined with Federal Reserve economic indicators, we compare four forecasting approaches: Naïve baseline, Linear Regression, ARIMA, and XGBoost. The analysis demonstrates that Linear Regression with engineered lag features achieves the best performance (RMSE $15.03, representing 51% improvement over baseline), outperforming both traditional statistical methods and modern machine learning approaches.

### Research Question

Can we accurately forecast monthly rent levels across U.S. metros through 2026, and which modeling approaches best capture recent structural changes in the rental market?

### Key Findings

- Linear Regression with lag features achieved best test set performance with RMSE of $15.03
- Simple engineered features (3-month rolling average, 1-month lag) capture most predictive signal
- XGBoost underperformed Linear Regression (RMSE $23.45) due to overfitting on limited data
- ARIMA struggled with multi-step forecasting (validation RMSE $85.33)
- Small resort markets are 3-5 times harder to forecast than large diversified metros


## Installation and Setup

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or Google Colab
- FRED API key (free registration at https://fred.stlouisfed.org/docs/api/api_key.html)

### Installation Steps
```bash
# Clone repository
git clone https://github.com/yourusername/rental-affordability-forecasting.git
cd rental-affordability-forecasting

# Install required packages
pip install -r requirements.txt

# Launch Jupyter Notebook
jupyter notebook rental_affordability_forecasting.ipynb
```

### Requirements
```
pandas==2.0.3
numpy==1.24.3
matplotlib==3.7.2
seaborn==0.12.2
scikit-learn==1.3.0
xgboost==1.7.6
pmdarima==2.0.3
fredapi==0.5.1
```

## Data Sources

### Zillow Observed Rent Index (ZORI)

- **Source:** Zillow Research Data
- **URL:** https://files.zillowstatic.com/research/public_csvs/zori/Metro_zori_uc_sfrcondomfr_sm_month.csv
- **Coverage:** January 2015 - November 2025 (131 months)
- **Description:** Monthly rent observations for U.S. metropolitan areas, capturing typical market rates for single-family homes, condos, and co-ops

### Federal Reserve Economic Data (FRED)

**Median Household Income (MEHOINUSA672N)**
- **Source:** U.S. Census Bureau via FRED
- **URL:** https://fred.stlouisfed.org/series/MEHOINUSA672N
- **Frequency:** Annual
- **Use:** Contextual economic indicator for affordability analysis

**Consumer Price Index (CPIAUCSL)**
- **Source:** Bureau of Labor Statistics via FRED
- **URL:** https://fred.stlouisfed.org/series/CPIAUCSL
- **Frequency:** Monthly
- **Use:** Inflation adjustment and macroeconomic context

### Geographic Scope

Analysis focuses on the top 20 U.S. metropolitan areas by average rent:

1. San Jose, CA - $2,986
2. Key West, FL - $2,901
3. Santa Cruz, CA - $2,796
4. Heber, UT - $2,785
5. San Francisco, CA - $2,739
6. New York, NY - $2,659
7. Napa, CA - $2,562
8. Santa Maria, CA - $2,520
9. Glenwood Springs, CO - $2,459
10. Boston, MA - $2,448
11. Los Angeles, CA - $2,411
12. Oxnard, CA - $2,386
13. San Diego, CA - $2,336
14. Barnstable Town, MA - $2,296
15. Urban Honolulu, HI - $2,282
16. Santa Rosa, CA - $2,251
17. San Luis Obispo, CA - $2,195
18. Salinas, CA - $2,187
19. Bridgeport, CT - $2,158
20. Naples, FL - $2,130

## Methodology

### Feature Engineering

Eleven temporal features were engineered across four categories:

**Time Features:** Month, Year_Num, Quarter  
**Lag Features:** Rent_Lag_1, Rent_Lag_3, Rent_Lag_12  
**Rolling Averages:** Rent_Roll_3, Rent_Roll_6, Rent_Roll_12  
**Growth Rates:** Rent_Growth_MoM, Rent_Growth_YoY

### Train/Validation/Test Split

- **Training:** January 2015 - December 2023 (1,751 observations)
- **Validation:** January 2024 - December 2024 (240 observations)
- **Test:** January 2025 - November 2025 (220 observations)

### Models Evaluated

1. **Naive Forecast** - Persistence model establishing baseline performance
2. **Linear Regression** - Multiple regression with time and lag features
3. **ARIMA** - Statistical time-series model with automatic parameter selection
4. **XGBoost** - Gradient boosting with hyperparameter tuning via RandomizedSearchCV

### Evaluation Metrics

- **RMSE (Root Mean Squared Error)** - Primary metric, in dollars
- **MAE (Mean Absolute Error)** - Average absolute error, in dollars
- **MAPE (Mean Absolute Percentage Error)** - Scale-independent percentage error

## Results

### Model Performance Comparison

| Model | Test RMSE ($) | Test MAE ($) | Test MAPE (%) | vs. Baseline |
|-------|---------------|--------------|---------------|--------------|
| Naive Forecast | 30.76 | 20.33 | 0.67 | - |
| Linear Regression | 15.03 | 9.42 | 0.31 | +51.1% |
| XGBoost | 23.45 | 13.00 | 0.41 | +23.8% |
| ARIMA | Not evaluated* | Not evaluated* | Not evaluated* | - |

*ARIMA not evaluated on test set due to poor validation performance (RMSE $85.33)

### Feature Importance

Analysis revealed that the 3-month rolling average (Rent_Roll_3) accounts for 72.6% of the predictive signal in XGBoost, followed by 1-month lag (Rent_Lag_1) at 24.2%. Time features, longer lags, and growth rates contributed minimally, suggesting that recent rent levels dominate forecasting accuracy.

### Regional Performance

Metro-level analysis revealed substantial heterogeneity in forecast difficulty:

**Most Difficult to Forecast:**
- Glenwood Springs, CO (RMSE: $42.17)
- Key West, FL (RMSE: $26.58)
- Heber, UT (RMSE: $22.18)

**Easiest to Forecast:**
- Napa, CA (RMSE: $7.13)
- Salinas, CA (RMSE: $7.75)
- San Luis Obispo, CA (RMSE: $7.82)

Small resort and amenity-driven markets exhibit significantly higher volatility than large diversified metropolitan areas.

## Key Insights

### Model Complexity vs. Performance

Linear Regression outperformed both ARIMA and XGBoost despite being the simplest approach. This demonstrates that model complexity should match data complexity. With limited observations (1,751 training samples) and strong linear autocorrelation in rent data, simple models prove more robust than complex alternatives that risk overfitting.

### Feature Engineering Over Algorithm Selection

The most important modeling decision was feature design rather than algorithm choice. The 3-month rolling average alone captured most predictive signal. This emphasizes that domain knowledge in feature engineering often matters more than algorithmic sophistication.

### Practical Applications

The Linear Regression model achieves forecast accuracy suitable for multiple applications:

- **Property managers** can set monthly rents within $15 confidence intervals
- **Renters** can budget for housing costs with 95% confidence intervals of approximately ±$30
- **Policymakers** can identify metros at risk of affordability decline
- **Investors** can evaluate acquisition opportunities with data-driven rent projections

## Limitations

1. **Limited geographic coverage** - Analysis restricted to top 20 metros, excluding mid-size and lower-cost markets
2. **Absence of external variables** - Model excludes housing supply, unemployment, wage growth, and policy variables
3. **Short test period** - 11-month test set insufficient for full business cycle assessment
4. **Distribution shift** - Higher test RMSE suggests 2025 dynamics differ from historical patterns
5. **No uncertainty quantification** - Point forecasts provided without confidence intervals

## Future Work

1. **Expand geographic coverage** to all U.S. metros with sufficient data
2. **Incorporate external economic indicators** including housing supply, labor market conditions, and policy variables
3. **Develop confidence intervals** through parametric methods, bootstrap resampling, or quantile regression
4. **Implement ensemble forecasting** combining predictions from multiple models
5. **Conduct walk-forward validation** across multiple test windows
6. **Develop metro-specific models** or hierarchical approaches to capture regional heterogeneity
7. **Build interactive dashboard** for stakeholder access
8. **Implement automated retraining pipeline** with drift detection

## References

### Data Sources

Federal Reserve Bank of St. Louis. (2025). Federal Reserve Economic Data (FRED). Retrieved from https://fred.stlouisfed.org/

Zillow Research. (2025). Zillow Observed Rent Index (ZORI). Retrieved from https://www.zillow.com/research/data/

