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

Motivations:

1. We can not always have a psuedo-RCT setting. For instance, we may not be able to treat half of a city and not the other (a marketing campaign).
2. On many platforms, one user’s action influence other users’ actions. For example, if I use certain products, then the people who are connected to me are more likely to use them. In the case of AB testing, if I am the one who is treated and some of my friends are not, then the effect is underestimated since both the purchase rate of the treatment group and that of the control group increase. This is coined the **spillover effect.**

Snythetic Controls compare the treatment unit of interest against a weighted average of units unaffected by the treatent. The weights are assigned to units in the donor pool (the group of potential control units) in order to construct a synthetic control unit that closely resembles the treated unit in terms of pre-treatment characteristics. In essence, based on the eucledian distance of the underlying metrics in the control groups to the treatment, we can assign weights that creates a "frankenstein" version of the treatment group so we can determine the counterfactual without the treatment.

![Screenshot 2024-12-21 at 10 07 09 AM](https://github.com/user-attachments/assets/14026a01-61a6-49e4-9f6f-6b24941f21f0)

The weights are chosen to minimize the distance between the treated unit’s pre-treatment characteristics (or outcomes) and the weighted average of the donor units' pre-treatment characteristics. This is typically done using an optimization procedure, such as minimizing the mean squared error (MSE) or Euclidean distance between the treated unit and the synthetic control.

![Screenshot 2024-12-21 at 10 04 07 AM](https://github.com/user-attachments/assets/05b16a72-9484-44ec-bfc4-9e8054210afe)

We then assess the statistical significance of the post treatment unit to the synethetic control unit by using a permuation test. For every unit in the synthetic control, we subtract the synthetic control number, and get a distribution of average treatment effect. Alternatively, just a do a pre vs post t test on the treatment unit.

![Screenshot 2024-12-21 at 10 14 28 AM](https://github.com/user-attachments/assets/ee17d926-a9de-41b8-bda2-fe06bbf2adbf)


### **Bayesian Stuctural TS**

Key points:

1. BSTS estimates trend, seasonality, and regression compinents, each with its own bayesian priors. The projection from the BSTS is then used as the counterfactual to measure the treatment effect.
2. BSTS is useful when the data is a time series, and there is no viable control group in the same time period as the treatment group. One benefit is there is no control group needed - and it is useful when the treatment affected everyone in the market. Question: How much did we gain from our superbowl ad?
3. It is important to try and have a large amount of data to built the BSTS on to ensure you capture all the historic trends.
4. We can calculate a posterior probability of a causal effect. The model will provide credible intervals for the treatment effect, which gives a range of plausible values.
5. One last point: the bayesian approach here allows you to specify the parameters governing the trend, seasonality, and the intervention effects. By specifying intervention effect of 0, our posterior credible interval will be akin to h-test m=0 for treatment effect.

   The CausalImpact R package provides an implementation of the approach.

![Screenshot 2024-12-21 at 10 20 44 AM](https://github.com/user-attachments/assets/3499c652-bd37-43d7-8744-761e4bbdb84c)


### **Meta Learners**

Meta learners are usueful in instances where we believe there may be a heterogenous treatment effect. We are using ML methods to estimate the treatment effect and then get the CATE or Conditional ATE.

1. Single learner: S
- Uses a single tree based model to estimate the effect of the treatment by comparing predictions with and without it. We run a model where T is a feature (0 or 1)
- We then can get predictions for each individual using both instances, one where the individual had treatment 0, and one where treatment was 1 (replicate an equal but opposite sample)
- We now have the counterfactual when we run on the opposite sample. We can estimate local and global CATE by comparing the delta in Y for T=0 and T=1.
- We can derive a 95% CI for cate using boostrapping. Remember that this CATE is global. We can plot it against your underlying features to see if the effect is heterogenous or not.

  <br>
  
![Screenshot 2024-12-21 at 11 09 53 AM](https://github.com/user-attachments/assets/25c4ea67-7dba-461a-bbbe-884781e80a4f)
![Screenshot 2024-12-21 at 11 12 55 AM](https://github.com/user-attachments/assets/8e3d29cb-dde8-4968-a6b8-bbf4392a021f)

2. Two learner: T
- Same idea as S learern, but instead we train two models, one on all the control and one on all the treatment. The treatment is no longer a feature - it will be latent.
- We then run the treatment group through both M1(T0) to get the counterfactual and M2(T1) to get the OOS Prediction.
- The difference between the two model outcomes is then consdiered the CATE.
- The advantage of the single learner is that the latent treatment effect can have more variation across different sub groups because we are not specifying a feature for it.

3. X learner - 2 stages
- Useful when there is a significant imbalance between treatment and control groups.
- We first make a propensity score of recieving treatment based on the features.
- We then make a t learner sperately and get the Cate0 and Cate1 from the t learner.
- We then get a final CATE which is the Propensity * Cate0 + 1- Propensity *Cate1
  
![Screenshot 2024-12-21 at 11 20 46 AM](https://github.com/user-attachments/assets/b79c3037-594a-4209-bed7-69f86ce93b0e)

This learner first crosses over estimates from treatment to control and vice versa to produce counterfactuals or each, then uses it to estimate the final cate once accounting for the propensity of recieving treatment. These are very useful in the original PSM scenario where we want to compare individuals against themselves rather than do a control and treatment group. This weights ensure that the CATE is not biased to the majority class. It is more adapted to estimate the heterogenous treatment effect where the presence in members of the treatment is low.
