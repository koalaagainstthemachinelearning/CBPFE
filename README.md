# Cross-Basis Projection Feature Encoding (CBPFE)

This repository contains the official R implementation for the methodology described in the upcoming manuscript: **"Cross-Basis Projection Feature Encoding (CBPFE): A DLNM-Driven XGBoost Framework for Dengue Forecasting"** by Abrar Islam and Syed Shafkat Raiyan.

## Overview

The CBPFE framework bridges classical epidemiological modeling with modern machine learning to accurately forecast dengue outbreaks. By extracting nonlinear exposure-lag-response surfaces from climate data and engineering them into leak-free predictive features, this hybrid pipeline captures both complex environmental dependencies and high-dimensional interactions.

The framework operates in two primary stages:
1. **Epidemiological Characterization (Stage 1):** Four univariate Negative Binomial Distributed Lag Non-linear Models (NB-DLNMs) are fitted to extract the partial effects of mean temperature, precipitation, relative humidity, and wind speed over a 6-month biologically relevant lag window. 
2. **Feature Extraction and Forecasting (Stage 2):** To prevent data leakage, the DLNMs are refitted strictly on the training partition. Cross-basis partial effects are then projected to the prospective test set via deterministic matrix multiplication ($f_t = B_t \times \hat{\beta}$). These engineered features are evaluated using an XGBoost framework.

## Repository Structure

```text
├── dataset/
│   ├── dengue_forecasting_dataset.xlsx  # Bangladesh clinical and climate data
│   ├── pop_dataset.csv                  # Annual population estimates
│   └── brazil_monthly_dataset.csv       # External validation dataset
├── CBPFE.Rmd                            # Main execution pipeline and analysis
├── CITATION.cff                         # GitHub citation metadata
└── README.md                            # Project documentation
