Got it! Here's a more GitHub-friendly version of the README, with the correct formatting for code and LaTeX-style math equations using Markdown.

---

# RCT with Precision Adjustment Analysis

This repository contains an analysis of a **Randomized Controlled Trial (RCT)** designed to assess the incentive effects of an unemployment program on the duration of unemployment. Specifically, the experiment explores how much the treatment (the unemployment program) impacts the timeline of reemployment.

## Model Overview

The core of the analysis is based on the following regression model:

```
Y = D * β₁ + W' * β₂ + ε, where E[ε(D, W')] = 0
```

Where:
- `Y` is the outcome variable (length of unemployment).
- `D` is the treatment indicator (0 for control group, 1 for treatment group).
- `W` is a vector of covariates, including demographic factors (age, gender, race, number of dependents, etc.), as well as experimental variables like location and type of occupation.
- `β₁` represents the **Average Treatment Effect (ATE)**, which estimates the causal impact of the treatment assuming the RCT assumptions hold.

### Regression Analysis

The primary goal is to estimate the ATE of the treatment on the unemployment duration. While the ATE can be identified by running a simple linear regression of `Y` on `D` alone, we include the covariates `W` to increase the precision of our estimate.

The more complex regression model we estimate is:

```
Y = D * α₁ + D * W' * α₂ + W' * β₂ + ε, where E[ε(D, W', D * W')] = 0
```

Here:
- `W` are the covariates that are demeaned (except for the intercept).
- The interaction term `D * W'` is included to capture any heterogeneity in the treatment effect across different subgroups.
- `α₁` represents the ATE, similar to the simpler model, but with improved precision due to the interaction terms.

## Key Features and Functions

This analysis includes several important features and checks to ensure the robustness of the results:

### 1. **Covariate Balance Check**
   - A crucial step in RCT analysis is verifying that the treatment and control groups are balanced in terms of their covariates. This ensures that the differences in outcomes can be attributed to the treatment itself, not pre-existing differences.
   - The code includes a function to fit a regression model between the treatment indicator (`D`) and the covariates (`W`) to assess balance.

### 2. **Eicker-Huber-White Standard Errors**
   - Standard errors are estimated using the **Eicker-Huber-White** robust estimator, which is robust to heteroskedasticity and multicollinearity. This adjustment ensures valid standard errors even when residuals are not homoscedastic or predictors are correlated.

### 3. **QR Decomposition**
   - To avoid issues from multicollinearity (when predictor variables are highly correlated), **QR decomposition** is used to remove perfectly collinear columns before fitting the regression model.

### 4. **Familywise Error Rate Correction**
   - After fitting the regression, p-values are adjusted using the **Holm-Bonferroni** method to control the **familywise error rate**. This correction is important when performing multiple hypothesis tests to reduce the risk of false positives.

### 5. **Model Comparison**
   - The analysis compares several models:
     1. **CL**: The basic regression model without interaction terms.
     2. **CRA**: The model with covariates included but without interactions.
     3. **IRA**: The model with interactions between the treatment and covariates.
     4. **IRA w/ Lasso**: The interaction model with **Lasso** regularization to remove unnecessary interaction terms.
   
   This process evaluates the precision and efficiency of different modeling approaches.

## Results

The final results of the analysis are summarized in the table below:

| Model            | Estimate  | Standard Error |
|------------------|-----------|----------------|
| CL               | -0.085455 | 0.035856       |
| CRA              | -0.079680 | 0.035591       |
| IRA              | -0.075218 | 0.035604       |
| IRA w/ Lasso     | -0.071767 | 0.035316       |

The analysis shows that the treatment group (Group 4) experiences an average decrease of about **7.8%** in the length of their unemployment spell.

### Key Observations:
- Regression estimators (CRA, IRA, IRA w/ Lasso) deliver slightly more efficient estimates (lower standard errors) compared to the simple two-mean estimator.
- All models produce similar standard errors, suggesting that the choice of model does not dramatically affect the precision of the estimates.
- The IRA model results show no statistically significant heterogeneity in the treatment effect, indicating that the treatment effect is consistent across subgroups.
- The regression models show slightly lower estimates compared to the simple CL model, likely due to minor imbalances in treatment allocation that the regression tries to correct.

## Conclusion

While the Average Treatment Effect (ATE) of the unemployment program is estimated to be around **-0.075**, the statistical analysis reveals that the treatment effect is not necessarily consistent across all subgroups. Further modeling refinements (such as using Lasso) help identify and remove unnecessary complexity in the model. Despite some imbalance in treatment allocation, the regression models correct for this and provide slightly more precise estimates of the treatment's effect.
