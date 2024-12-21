# Causal Inference Overview

## Introduction

This is a project that demonstrates how causal inference can be used in a business setting.

## Contents

- Causal Inference: High level terminology
- Propensity Score Matching
- Regression Discontinuity Design
- Synthetic Controls
- Bayesian Structural Time Series
- Meta Learners

<br>

### **High-Level Overview of Terminology in Causal Inference**

Causal inference is the process of determining whether a relationship between two variables is causal (i.e., whether one variable *causes* changes in another). Below is a high-level overview of key terms and concepts commonly used in causal inference:

---

### **1. Treatment and Effect**

- **Treatment**: The variable or intervention that is hypothesized to affect the outcome. This could be any intervention, policy, or exposure you are studying (e.g., a new drug, marketing campaign, policy change).
  - **Treatment Group**: The group that receives the treatment or intervention.
  - **Control Group**: The group that does not receive the treatment or receives a baseline condition.

- **Outcome**: The variable that is affected by the treatment. This is the result you're interested in measuring (e.g., sales, health status, performance).

- **Causal Effect**: The change in the outcome variable that occurs when the treatment is applied to a unit, compared to when the treatment is not applied.
  - **Average Treatment Effect (ATE)**: The average effect of the treatment across all individuals in the population.
  - **Individual Treatment Effect (ITE)**: The effect of the treatment on an individual unit or person. This is usually unobservable, and ATE is often used as an estimate.
  - **Average Treatment Effect on the Treated (ATT)**: The average effect of the treatment on those who actually receive the treatment (the treated group).

---

### **2. Potential Outcomes and Counterfactuals**

- **Potential Outcomes**: The set of outcomes that could potentially occur for a unit (individual, group, etc.), depending on whether they receive the treatment or not. For any individual:
  - **Y(1)**: The potential outcome if the individual receives the treatment.
  - **Y(0)**: The potential outcome if the individual does not receive the treatment.

- **Counterfactuals**: The outcomes that would have occurred for a unit under an alternate treatment condition. Counterfactuals are **unobservable** because we can only observe one potential outcome (either the treated or untreated state, but not both). **Counterfactual is what would have happened theortically, if not for the treatment.**
  - The **fundamental problem of causal inference** is that we can never observe both the treated and untreated outcomes for the same individual.

---
### **3. Assumptions**

- **Assumption of No Confounding**: The treatment assignment is assumed to be independent of the potential outcomes, after conditioning on observable covariates. This is often the core assumption in estimating causal effects from observational data.
  
- **Stable Unit Treatment Value Assumption (SUTVA)**: The assumption that:
  1. There is no interference between units (i.e., the treatment of one unit does not affect the outcome of another unit).
  2. The treatment has a single, consistent effect for each unit (i.e., no different versions of the treatment).
  
- **Exchangeability**: The assumption that, after conditioning on relevant covariates, the treated and control groups are comparable and can be treated as if they were randomly assigned.

- **Unconfoundedness (or Ignorability)**: The assumption that, given the observed covariates, the treatment assignment is independent of the potential outcomes (i.e., no unmeasured confounders).

---
### **Propensity Score Matching**

The motivation for propensity score matching is to be able to create psuedo-random treatment and control groups when we are unable to have a RCT. They are particularly useful in business cases where people opt in for something. In helps mitigate both selection bias and confounding bias to a certain extent where we would want to control for underlying covariates. The main idea of a propensity score is that we want to "encode" user characteristics into a single score using a logistic regression or tree.

Steps:

1. Calculate the Propensity Score: 

- The propensity score is the probability that a unit (individual) receives the treatment, given their observed characteristics (covariates).
- Typically, this is estimated using logistic regression or other classification models where the treatment (binary variable: 1 for treated, 0 for control) is the dependent variable, and the covariates are the independent variables.
- Example: Suppose you're studying the effect of a new drug on health improvement. You calculate the propensity score for each person based on covariates like age, gender, and medical history.
  
2. Matching Process (Nearest Neighbor Matching):

- Match each treated unit to a control unit with the closest propensity score. The absolute difference in propensity score can have a threshold based on the rarity of the score.
- Once a control unit is matched, it cannot be used again for another treated unit.
- Stratification Matching is another method used in Propensity Score Matching (PSM) to adjust for confounding in observational studies. It involves dividing the entire sample into subgroups (strata) based on the estimated propensity score, and then comparing treated and control units within each stratum (subgroup).
- Kernel Matching is an advanced method for estimating causal treatment effects using propensity scores, where each treated unit is matched to a weighted average of control units results based on their similarity in propensity score.

<br>

At the conclusion of the study, we can then look at ATE (average treatment effect in treatment vs not overall) and ATT (average treatment effect of the treated) - the delta between the post PM treatment / control split.

Key assumption: Conditional independence
  - We assume that given the propensity score, the assignment to treatment vs control is as good as random. AKA: We have taken care of all confoudning factors.


---
### **Regression Discontinuity Design **

Key Idea: RDD estimates causal effects by exploiting a cutoff or threshold that determines treatment assignment. It compares units that are just above and just below the cutoff, assuming they are similar except for receiving the treatment.

The motivation for RDD is to estimate causal effects when random assignment to treatment is not feasible, but there is a clearly defined cutoff or threshold that determines who receives the treatment. RDD is particularly useful in situations where treatment is assigned based on a continuous variable, and only those above or below a certain cutoff (threshold) receive the treatment. This design allows researchers to estimate the causal effect of the treatment by comparing units just above and just below the threshold, assuming that the assignment around the threshold is as good as random.

Understanding where RDD is applicable is best illuminated by examples:

1. Researchers can compare students who scored just above 85 (and received the scholarship) to those who scored just below 85 (and did not receive the scholarship) to estimate the effect of the scholarship on later earnings.
2. Researchers could study the effects of receiving welfare benefits by comparing families just above and below the eligibility cutoff of 30k income on college matriculation rate.
3. Assess the impact of stricter environmental regulations on air quality or factory performance by comparing factories just above and just below the emissions cutoff.
4. A bank approves loans for individuals with credit scores above a certain threshold, while those below it are denied or offered different loan terms on default rates.

![Screenshot 2024-12-20 at 8 31 16 PM](https://github.com/user-attachments/assets/abcffb85-fc61-441c-8704-202b617d9af7)

![Screenshot 2024-12-20 at 8 32 47 PM](https://github.com/user-attachments/assets/d90cc249-bcdd-4e4c-a967-72768fd7b74a)

<br>

- The interaction term is only included in the model to ensure that the slope of the line pre and post cut off is the same. If this term is significant, the effect of the treatment increases with the increased value of the variable (credit score or test score or whatever else). Always be sure to check your residuals vs fitted, vs. leverage, etc, as you do with any regression model.
- The only downside the RDD us that you can not generalize your result to how the result would look like with a different cut-off point.
- However, you can use your result to argue a different cut-off point, or whether or not the program is having any effect at all.

### **Difference in Difference**

DID compares the changes over time in outcomes for treated and control groups before and after the treatment is applied. It estimates causal effects by observing the differential change in outcomes between the two groups. It is particularly useful when you are using a data set that is aggregated at a group level (city, market, age bucket) and tracked across some time horizon at a daily weekly or monthly granularity. It assumes that the treatment group and the control group must follow parallel trends in the outcome variable before the treatment is applied. This means that, in the absence of treatment, the treated and control groups would have followed the same trend over time.

![Screenshot 2024-12-20 at 8 50 37 PM](https://github.com/user-attachments/assets/422c9a39-1212-4374-9d21-ff56671d08b2)

Imagine that the blue line are a group of cities that are not recieving the new ride-pool feature, and the orange line is going to recieve the ride-pool feature at the intervention line. If the new feature has a causal effect, we would expect the lines to no longer be parallel after the treatment.

The key thing to consider for DID is the time horizon. As a general rule of thumb, you want the pre-intervention period to be >= the post-intervention measurement period.
The model will look like: Y = intercept + beta (delta between group 1 and 2 prior to treatment) + beta(trend global) + dummy (post intervention) + interaction treatment*intervention

If the dummy variable is significant, that B represents the amount both groups on average changed in the post intervention period (controls for global changes in trend)
The interaction coefficient represents the marginal change in Y due to the treatment. This is what will tell you how much of your change is due to the treatment.

If the parallel trends assumption is violated, we can use syntethic controls or bayesian structural time series.

### **Snythetic Controls**
