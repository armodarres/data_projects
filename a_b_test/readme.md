# A/B Testing Overview

## Introduction

This is a project that demonstrates how A/B testing using ML can be performed to grow a business.

## Contents

- Product Sense: business goals, user funnels, metrics
- Experimental Design
  
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
A North Star Metric (NSM) is a single, key metric that serves as the primary indicator of a company's long-term success and growth. It focuses on capturing the value your product delivers to customers, aligning teams around a common goal, and guiding decisions that drive growth. **Think what the guys on wall street would ultimately care about. Subscriptions, revenue, etc. ** Provides value, contributes to profit, and gauges long term growth. 
<br>
**2. Driver Metric**
<br>
A Driver Metric is a key metric that directly influences or drives the performance of a North Star Metric (NSM). While a North Star Metric reflects the overall value or success of a business, driver metrics are the specific factors or actions that have a direct impact on achieving that success. Think what would lead to helping the north star: screen time, click through rate, average order value, push notification open rate, credit card sign ups, etc.
<br>
**3. Guardrail Metrics**
<br>
Guardrail Metrics are secondary metrics that help ensure the business does not negatively affect its growth or customer experience while focusing on its North Star Metric (NSM) and Driver Metrics. These metrics act as safeguards to keep the business on track and prevent unintended consequences as teams optimize for their primary goals. Think: statisfcation rate, churn rate, app deletion rate, un-sub rate, card default rate.
<br>
<br>
The remainder two are less important but always consider checking your results by as many segments as possible to ensure you are not missing sub-population effects. As a last note on this section, remember that the data you have access to, the cadence it comes in at, and the baseline performance all can have an impact on what metrics can be viable as KPIs in the first place.

## Experimental

This is the world that most statisticians should feel comfortable in: hypothesis testing, power analysis, statistical tests, practical and significant differences. Always state your feature launch criteria a priori, along with your h0 rejection criteria, necessary power, etc.

