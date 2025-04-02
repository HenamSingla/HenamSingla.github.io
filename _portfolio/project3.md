---
title: "Nifty50 and Midcap50 Regression Analysis"
excerpt: "Statistical analysis of index price relationships <br/><img src='/images/nifty50.png'>"
collection: portfolio
---

Link to the [GitHub repository](https://github.com/HenamSingla/nifty50)

## Nifty50 and Nifty Midcap50 Regression Analysis

This project explores the statistical relationship between India's Nifty50 and Nifty Midcap50 indices using regression analysis techniques.

### Project Overview

I analyzed the relationship between the Nifty50 and Nifty Midcap50 indices using Ordinary Least Squares (OLS) regression. This analysis helps understand how the broader market (represented by Nifty50) influences the midcap segment, which can provide valuable insights for investment strategies and market analysis.

### Methodology

1. **Data Collection**:
   - Gathered historical price data for both Nifty50 and Nifty Midcap50 indices
   - Focused on daily closing prices as the primary variables of interest

2. **Statistical Analysis**:
   - Implemented OLS linear regression using the statsmodels library
   - Used Nifty50 as the independent variable and Nifty Midcap50 as the dependent variable
   - Added constant term to account for the intercept

3. **Model Evaluation**:
   - Analyzed R-squared values to understand explanatory power
   - Assessed coefficient significance through p-values
   - Examined standard errors to determine estimate precision

### Results

The regression analysis revealed significant statistical relationships between the two indices, demonstrating how movements in the Nifty50 index explain variations in the Nifty Midcap50 index. The model provides insights into market dynamics and potential investment opportunities.

### Technologies Used

- Python (pandas, statsmodels)
- Statistical regression techniques
- Financial data analysis
- Time series processing

### Applications

This analysis can be applied to:
- Portfolio optimization strategies
- Market risk assessment
- Index-based trading approaches
- Financial market research
