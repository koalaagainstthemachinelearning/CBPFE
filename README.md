# CBPFE: Cross-Basis Projection Feature Encoding

This repository contains the official R implementation for the methodology described in the manuscript: **"CBPFE: A DLNM-Informed XGBoost Framework for Nonlinear Climate Feature Engineering in Dengue Forecasting with Multi-Model Benchmarking"** by Abrar Islam and Syed Shafkat Raiyan.

## Overview

The CBPFE framework bridges classical epidemiological modeling with modern machine learning to accurately forecast dengue outbreaks. By extracting nonlinear exposure-lag-response surfaces from climate data and engineering them into isolated predictive features, this hybrid pipeline captures both complex environmental dependencies and high-dimensional interactions.

The proposed framework employs a two-stage analytical pipeline:

1. **Epidemiological Characterization (Stage 1):** Four univariate Negative Binomial Distributed Lag Non-linear Models (NB-DLNMs) are constructed to capture the non-linear, delayed effects of mean temperature, precipitation, relative humidity, and wind speed across a biologically relevant 6-month lag window.
2. **Feature Projection and Modeling (Stage 2):** To ensure rigorous out-of-sample evaluation, the initial DLNMs are fitted exclusively on historical training data. The derived cross-basis partial effects are deterministically projected onto the holdout set ($f_t = B_t \times \hat{\beta}$). These isolated environmental representations are subsequently fed into an XGBoost framework to generate the final incidence forecasts.

## Repository Structure

```text
├── dataset/
│   ├── dengue_forecasting_dataset.xlsx  # Bangladesh clinical and climate data
│   ├── pop_dataset.csv                  # Annual population estimates
│   └── brazil_monthly_dataset.csv       # External validation dataset
├── CBPFE.Rmd                            # Main execution pipeline and analysis
├── CITATION.cff                         # GitHub citation metadata
└── README.md                            # Project documentation
```

## Prerequisites

The analysis is conducted in R. Ensure you have the following dependencies installed before executing the pipeline:

```R
install.packages(c("dlnm", "splines", "MASS", "readxl", "dplyr", "lubridate", 
                   "lmtest", "zoo", "car", "xgboost", "ggplot2", "tidyr", 
                   "forecast", "SHAPforxgboost", "moments", "scales"))
```

## Pipeline Execution

The complete methodology is contained within the `CBPFE.Rmd` file. The pipeline executes the following analytical steps sequentially:

1. **Cross-Basis Construction:** Defines the 5th–95th percentile prediction ranges and builds the cross-basis matrices for all meteorological variables.
2. **Autocorrelation Correction:** Implements a Durbin-Watson grid search to dynamically select the optimal number of lagged deviance residuals for the DLNMs.
3. **Feature Assembly:** Refits the DLNMs strictly on the training partition and assembles the engineered CBPFE variables alongside autoregressive (AR) terms and seasonal cyclical encodings.
4. **Hyperparameter Tuning:** Conducts a random grid search utilizing custom time-series cross-validation (comparing expanding vs. rolling windows).
5. **Model Evaluation:** Evaluates three XGBoost configurations against classical ARIMA-family baselines over a 24-month prospective holdout (2024–2025).
6. **Explainability & External Validation:** Generates out-of-sample SHAP importance rankings and validates the framework's temporal transferability on a secondary dataset from Brazil using z-score mapping.

## Citation

If you utilize this code, methodology, or framework in your research, please cite our manuscript:

```bibtex
@unpublished{islam_raiyan_cbpfe_2026,
  title = {CBPFE: A DLNM-Informed XGBoost Framework for Nonlinear Climate Feature Engineering in Dengue Forecasting with Multi-Model Benchmarking},
  author = {Islam, Abrar and Raiyan, Syed Shafkat},
  note = {Unpublished manuscript},
  year = {2026},
  url = {[https://github.com/koalaagainstthemachinelearning/CBPFE](https://github.com/koalaagainstthemachinelearning/CBPFE)}
}
```
