# A/B Testing Overview

## Introduction

This is a project that demonstrates how A/B testing using ML can be performed to grow a business.

## Contents

- Product Sense: business goals, user funnels, metrics
- Hypothesis formation, experimental design
- Post analysis: Validity threats, inference

  
## Product Sense

The high level overview for the motivation of performing A/B tests is to drive business growth through innovation of a product which brings additional value to the users. To determine what features can be innovated, consider the product breakdown:

<img width="952" alt="Screenshot 2024-12-16 at 8 36 46 PM" src="https://github.com/user-attachments/assets/485d617b-1b88-45ea-a445-5d9f5d9edfdb" />

Each point in this continuum is an opportunity to pull a certain lever to improve the business.<br>

Metrics have three primary components to consider: Action, unit of analysis, and statistic. For example, we may want to measure client screen time, but we must consider the time horizon: will this be per day, per session, per month, etc. Finally, the statistic is usally something like mean, median, count, proportion, but can be anything (even an F-1 score) in the right situation.

AAARG metrics are useful to remember so we can add in additional KPIs to test for during the experiment:

<img width="749" alt="Screenshot 2024-12-16 at 8 43 20 PM" src="https://github.com/user-attachments/assets/8deb5fb5-56d6-4b30-b698-605a084d0fc8" />
<br>
1. Acquisition (A)
Definition: How users find and come to your product or service.<br>
Key Metrics: Website traffic, app downloads, sign-ups, user acquisition channels (social media, ads, search engine, etc.)<br>
2. Activation (A)<br>
Definition: The first meaningful experience users have with your product.<br>
Key Metrics: Users who complete a key action (e.g., making their first purchase, completing a tutorial, or using a core feature for the first time).<br>
3. Retention (R)<br>
Definition: How well you keep users engaged and returning to your product.<br>
Key Metrics: Daily/Monthly Active Users (DAU/MAU), churn rate, user engagement metrics (time spent, frequency of use), and cohort analysis.<br>
4. Referral (R)<br>
Definition: How users recommend your product to others.<br>
Key Metrics: Referral rates, viral coefficient (how many new users each existing user brings in), and word-of-mouth marketing effectiveness.<br>
5. Revenue (R)<br>
Definition: The money your product generates.<br>
Key Metrics: Average Revenue Per User (ARPU), lifetime value (LTV), conversion rate from free to paid, and total revenue.<br>
6. Growth (G)<br>
Definition: The overall scale and expansion of the user base and product.<br>
Key Metrics: Growth rate of users, revenue growth, market share increase, and virality of the product.<br>
<br>
Why These Metrics Matter:
<br>
<br>
Acquisition: Understanding how users first find your product helps to optimize marketing and advertising efforts.<br>
Activation: Measuring whether users quickly experience value can guide product development, such as improving onboarding or making the product easier to use.<br>
Retention: Retaining customers is critical for long-term growth, as it’s more cost-effective to keep existing users than acquire new ones.<br>
Referral: Word-of-mouth marketing can amplify growth with low cost, so tracking referrals helps identify how well your product "sells itself."<br>
Revenue: Financial metrics show if the product is sustainable and profitable, which is vital for long-term success.<br>
Growth: Growth metrics allow you to gauge overall success, identify bottlenecks, and understand scalability.<br>
<br>
<br>
With the above in mind, we can select the KPIs we want to keep track of during our experiments:
<br>
<br>
<img width="900" alt="Screenshot 2024-12-16 at 8 48 15 PM" src="https://github.com/user-attachments/assets/fe557886-7ec7-4116-a8c9-569643d98f44" />
<br>
<br>

**1. North Star Metric**
<br>
A North Star Metric (NSM) is a single, key metric that serves as the primary indicator of a company's long-term success and growth. It focuses on capturing the value your product delivers to customers, aligning teams around a common goal, and guiding decisions that drive growth. **Think what the guys on wall street would ultimately care about. Subscriptions, revenue, etc.** Provides value, contributes to profit, and gauges long term growth. 
<br>
<br>
**2. Driver Metric**
<br>
<br>
A Driver Metric is a key metric that directly influences or drives the performance of a North Star Metric (NSM). While a North Star Metric reflects the overall value or success of a business, driver metrics are the specific factors or actions that have a direct impact on achieving that success. Think what would lead to helping the north star: screen time, click through rate, average order value, push notification open rate, credit card sign ups, etc.
<br>
<br>
**3. Guardrail Metrics**
<br>
<br>
Guardrail Metrics are secondary metrics that help ensure the business does not negatively affect its growth or customer experience while focusing on its North Star Metric (NSM) and Driver Metrics. These metrics act as safeguards to keep the business on track and prevent unintended consequences as teams optimize for their primary goals. Think: statisfcation rate, churn rate, app deletion rate, un-sub rate, card default rate.
<br>
<br>
The remainder two are less important but always consider checking your results by as many segments as possible to ensure you are not missing sub-population effects. As a last note on this section, remember that the data you have access to, the cadence it comes in at, and the baseline performance all can have an impact on what metrics can be viable as KPIs in the first place. **Remember to always consider the sources of potential variance that can get us to or away from our desired result, and youll always find your way to the appropriate metrics to measure.**

## Experimental

This is the world that most statisticians should feel comfortable in: hypothesis testing, power analysis, statistical tests, practical and significant differences. 

### Hypothesis formation
1. Always state your feature launch criteria prior to testing (the necessary lift), along with your h0 rejection criteria, necessary power, etc.
2. Remember that you don't always want to be testing for improvements, sometimes you just want to make sure nothing has changed!
3. Alpha level (controlls type one error) meaning incorrectly rejecting a null hypothesis when it is true. They will not always be arbitrary: the alpha level here should be chosen considering how much risk there is in reject h0 and rolling out the new feature - a 95% chance that it improves by 10M but 5% chance it worsens by 20M may not be worth it! Rejecting the null hypothesis means rejecting the notion that both sets of data came from the same underlying distribution.
4. Statistical power: the probability that we correctly reject the null hypothesis. This can be a very confusing concept so I will expand on it here:<br>
<br>
Power has to do with the distribution of h0 outcomes vs ha outcomes and their overlap. The more data we collect, the smaller the standard deviations become, the more power we have in our result. That way, the distributions of the two hypothesis will have very little overlap if in fact they are different. If the overlap is smaller, the lower the type 2 error (B), and the higher the power (1-B). If the above criteria is met, the area of the distribution under Ha which overlaps with the distribution of h0 will be smaller.
<br>
<br>
Think of it this way: if I told you I surveyed 30,000 people from the general public and found that, contrary to sales figures, people tend to like Pepsi more than Coke, that would be very compelling. I found a result after studying 1% of a population (i.e. the US general public). It is likely to generalize to the larger population. If I surveyed 7 people and found the same thing, even if it was statistically significant, I wouldn't convince anyone. You can argue a lot of reasons for that (you can't get a representative sample, ANOVA/regression assumptions may not be met etc.), but what's important is that high power means highly persuasive (and you should be as critical or more of your results as those you are trying to convince). **You need both a signficiant result and a high power result to detect difference with conviction.**
<br>
<br>

![Screenshot 2024-12-19 at 3 01 57 PM](https://github.com/user-attachments/assets/f217ca22-017d-4b63-987d-5a2d7b79e689)

<br>
5. MDE: Minimum detectable effect. Can be done using absolute difference, relative difference (% increase in mean), or cohens D (mean difference over pooled S.D). The choice of the MDE should be considered based on the amount of impact that can drive (1% at scale is worth a lot!)
<br>
<br>

### Experimental Design
The three big portions are sample formation (random or psuedo-random), sample size calculation for power, and experiment duration.

1. Randomization: In an ideal world we can just set a seed and assign customerIds to one of two groups. However there are multiple shortcomings with this method:
  - How can we assure that the underlying demopgrahics and characterstics between these groups are still random? **Solution: Do multivariate tests for differences between group A and B profiles MANOVA.**
  - There may be practical issues like non-compliance or skewed allocation due to factors such as self-selection, non-random drops, or systematic behavior differences between groups. Loyalty programs or credit card sign-ups are a good example of where this issue may arise. **Solution: Propensity score matching.** is a statistical technique used to reduce selection bias and confounding in observational studies where randomization is not possible. The primary goal of PSM is to estimate the treatment effect (e.g., the effect of a treatment or intervention) by creating a matched sample that mimics a randomized controlled trial (RCT) as closely as possible.<br>
  - Randomization works well when the treatment effect is expected to be homogeneous across the population, but it can mask significant effects in specific subgroups of users. **Solution: stratified randomization.**
  - Attrition: Randomization does not guarantee that all participants will complete the study or remain in the experiment for its duration. **Solution: intent-to-treat analysis, multiple imputation, sensitivity analysis, or weighting, all after the conclusion of the study.**
<br>
<br>

2. Sample size calculation and experiment duration.
   - Effect Size: Make sure the effect size you want to detect is realistic. Too small an effect size might require an impractically large sample.
   - Conversion Rate Variability: If you are dealing with proportions, make sure the baseline conversion rate is well estimated; large variability in the baseline can increase the required sample size.Power: While 80% power is common, in some situations, a higher power (e.g., 90%) may be preferred, but this will increase the required sample size.
   - Multiple Tests: If you are running multiple A/B tests or multiple comparisons (e.g., multiple versions of B), you will need to adjust the significance level to account for multiple testing (e.g., using a Bonferroni correction).
  
<br>
The delta here is the difference between the sample statistics. The variance here is pooled. 
<br>

![Screenshot 2024-12-19 at 7 07 42 PM](https://github.com/user-attachments/assets/4d9c6aa2-cdeb-4e5c-93fa-e14b3a9a02df)


<br>
This formula can be used to understand the minimum duration for a simple version of A/B testing. Other consideration for time horizons should be things like seasonality, rare events or confounders, and time for maturity of the subject (think in terms of how long it might take to get the measurements you need in a loyalty program for rare events like pet visits).
<br>

![Screenshot 2024-12-19 at 3 45 49 PM](https://github.com/user-attachments/assets/5bad96b0-6324-4185-8506-e6c3102571ec)


<br>
This kind of plot should always be used to determine what sample size is feasible to have the desired power and effective size for your study.
<br>
<img width="616" alt="Screenshot 2024-12-19 at 7 19 00 PM" src="https://github.com/user-attachments/assets/23de53e6-4328-4ca9-a945-6ce0e5e82084" />

<br>
<br>
## Post Analysis

### Validity threats

1. SUTVA: Stable unit treament value assumption
- Typically violated if there network effects. Imagine two algos getting a/b tests on tiktok, but group A gets reccomended videos on their algo and sends to friend in group B. This drives up group Bs screentime. **Solution: Community based randomization, causal inference**. You can check and control for the effect by seeing if the treatment of one user affects their friends' outcomes.
2. Survivorship bias: attrition/drop-out: use survival analysis, intention to treat.
3. Sample ratio mis-match: Randomization did not go as expected. Use x2 test to check for significance of observed group size vs expected after the test.
4. Primacy effect: Change eversion. **Solution: Segment users based on new vs returning, run experiment longer to see if time horizon shows more change**.
5. Novelty effect: Opposite of primacy effect. Same solution.
6. AA testing: Ensure that the control and treatment groups are starting in the same position before the AB test. This would only be looking at the baseline for the metric prior to the test.
7. Hold out set: even after a decision has been made to launch, keep 10% of users without the new feature to ensure that the long term effect of the new feature is still doing what you expected it to do, and you haven't unintentional captured a spurious change.
  
![Screenshot 2024-12-19 at 7 44 48 PM](https://github.com/user-attachments/assets/0492ea6f-db06-44a1-a9a8-cd8cac900b38)

<br>
<br>

### Inference

There are a wide variety of tests that can be applied based on the situation, including: x2 test, z test for proportions, t test for means (students(pooled) and welch (unpooled/unequal)), and non-parametric methods like rank or sign tests (for smaller sample sizes). Remember that Non-parametric tests may be less powerful compared to parametric tests when the assumptions of the parametric test are met because they don't utilize all the information available in the data (such as means and variances).

