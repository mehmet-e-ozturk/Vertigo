# Vertigo  
Data Scientist Case

---

## Question 1

From the given retention rates I have implemented the curves as:

S(t) = exp(-(t / η)^k)

**Variant A:**  
S_A(t) = exp(-(t / 2.116)^0.547)

**Variant B:**  
S_B(t) = exp(-(t / 1.796)^0.431)

These formulas created the retention rates as follows.

### Fitted parameters

| variant   | eta      | k        |
|----------|----------|----------|
| Variant A | 2.115654 | 0.547208 |
| Variant B | 1.795672 | 0.431498 |

### Curves table (head)

| day | Variant A | Variant B |
|----:|----------:|----------:|
| 0  | 1.000000 | 1.000000 |
| 1  | 0.514988 | 0.459882 |
| 2  | 0.379194 | 0.350779 |
| 3  | 0.298020 | 0.287108 |
| 4  | 0.242443 | 0.243454 |
| 5  | 0.201691 | 0.211057 |
| 7  | 0.145922 | 0.165513 |
| 10 | 0.096376 | 0.122705 |
| 15 | 0.053898 | 0.082159 |
| 30 | 0.014012 | 0.034377 |

---

## Key Results

![Curves](images/retention_model.png)

Then i have created imaginary cohorts for both curves to predict their DAUs.

---

### a)

The metrics for **day 15** are:

| scenario | event_date | day | variant | daily_users | daily_impressions | daily_buyers | daily_ad_rev | cum_buyers | cum_ad_rev |
|--------|------------|-----|---------|-------------|-------------------|--------------|--------------|------------|------------|
| sc1 | 2025-01-16 | 15 | A | 72533 | 166826 | 2212 | 1634.8948 | 27151 | 20066.2546 |
| sc1 | 2025-01-16 | 15 | B | 75830 | 121328 | 2389 | 1310.3424 | 28050 | 15386.8680 |

Variant B has more DAU at day 15.

---

### b)

**Day: 15**

- CumAdRev A, B: 20066.2546 , 15386.868  
- CumBuyers A, B: 27151 , 28050  
- v* (avg transaction revenue threshold): 5.205101890989993  

Rule:  
B wins if v > v* ; A wins if v < v*

If DARPU is smaller than 5.2 Variant A earns more money and vice versa.

---

### c)

**Day: 30**

- CumAdRev A, B: 46366.4362 , 37472.2308  
- CumBuyers A, B: 62741 , 68311  
- v* (avg transaction revenue threshold): 1.596805278276481  

Rule:  
B wins if v > v* ; A wins if v < v*

The DARPU threshold for Variant B to be better option is much smaller this time.

---

### d)

**Day: 30**

- CumAdRev A, B: 46366.4362 , 37472.2308  
- CumBuyers A, B: 70337 , 76478  
- v* (avg transaction revenue threshold): 1.448331770070021  

Rule:  
B wins if v > v* ; A wins if v < v*

In this case B is more likely to be the more feasible variant since it only need 1.44 as DARPU.

---

### e)

**Day: 30**

- CumAdRev A, B: 47663.6328 , 38349.7416  
- CumBuyers A, B: 64495 , 69909  
- v* (avg transaction revenue threshold): 1.7203345400812717  

Rule:  
B wins if v > v* ; A wins if v < v*

Still Variant B is more likely to be better option.

---

### f)

Even though 10 day sale is more feasible I would prefer permanent user channel as it will surpass the sale scenario in the long term.  
On the other hand even with a new scenario some sale actions must be done to keep the player base alive.

![Comparison1](images/sc2_v_sc3_total.png)  
![Comparison2](images/sc2_v_sc3_daily.png)

---
## Question 2

In this task I've made several analyses, with a strong focus on segmentation and churn understanding.  
The goal was not only to describe player behaviour, but also to make the results actionable for product, marketing, and monetization decisions.

---

### Segmentation Framework

Each player is assigned the following segmentation dimensions:

- win_lose_segment  
- spending_segment  
- weekly_behaviour_segment  
- frequency_segment  
- churn_segment  

These segments are complementary and describe **skill**, **value**, **play timing**, **engagement intensity**, and **future risk**.

---

### win_lose_segment

This segment captures player skill and competitive performance.

- **rookie** – too few matches to judge skill  
- **unlucky** – low win rate despite playing  
- **competing** – average, balanced performance  
- **winner** – consistently high win rate  

A minimum match threshold is applied to avoid noisy win-rate calculations.

---

### spending_segment

This segment captures player monetary value.

- **free rider** – plays without spending  
- **low spender** – occasional, small purchases  
- **high spender** – regular, meaningful spending  
- **golden goose** – top 20% revenue contributor  

Golden goose players are identified using the upper quantile of IAP revenue.

---

### weekly_behaviour_segment

This segment explains *when* players prefer to play.

- **white_collar** – mostly weekday play  
- **regular** – evenly spread across week  
- **weekender** – concentrated weekend play  
- **dormant** – no meaningful activity  

The segment is based on the ratio of weekend session duration to total session duration.

---

### frequency_segment

This segment captures engagement intensity and lifecycle stage.

- **new player** – installed during the period  
- **dormant** – no active days  
- **seldom** – plays occasionally  
- **frequent** – plays often  
- **addicted** – plays almost every day  

Active days are calculated as distinct days with at least one session.

---

### churn_segment

This segment predicts near-future activity risk.

- **churned** – already inactive  
- **alarming** – active but high churn risk  
- **safe** – active with low churn risk  

The churn segment is produced using a predictive model trained on early behaviour signals.

---

### Churn Modeling

Besides static segmentation, I've built a churn prediction model using **first-day player data**.  
With only D0 behaviour (sessions, duration, match activity, wins/losses, errors, and revenue), the model explains retention and churn risk significantly.

This allows early intervention before users actually churn.

---

### Seasonality Analysis

I performed a day-of-week seasonality analysis on seasoned players.

Key observations:
- Average session duration and total session duration show different weekday vs weekend patterns.
- This behaviour is likely influenced by **new installs**, who typically have shorter but more frequent early sessions.
- Seasoned players show more stable and predictable weekly patterns.

---

### Geography Analysis

I created a country-level aggregation to analyze:
- total players
- total IAP revenue
- total ad revenue  

This allows quick identification of high-value and high-volume regions and supports localization or regional pricing decisions.

---

### Supporting Visuals

Below are the key screenshots generated during the analysis:

**Daily Metrics**
![Average Session Duration](images/avg_session_duration.png)
![Daily Unique Players](images/daily_unique_players.png)
![Install Counts](images/install_counts.png)
![Total Session Count](images/total_session_count.png)
![Total Session Duration](images/total_session_duration.png)

**Seasoned Player Analysis**
![Seasoned Avg Session Duration](images/seasoned_avg_session_duration.png)
![Seasoned Total Session Count](images/seasoned_total_session_count.png)
![Seasoned Total Session Duration](images/seasoned_total_session_duration.png)
![Seasoned Unique Users](images/seasoned_unique_users.png)

**Retention & Modeling**
![Retention Model](images/retention_model.png)
![Survival of Newcomers](images/survival_of_newcomers.png)
![Elbow Method](images/elbow_method.png)

---

By combining descriptive analytics, segmentation, and predictive modeling, this task provides both **explainability** and **actionability** for user growth, monetization, and retention strategies.


---

## Executive Summary

In this case study I analyzed player behavior, retention, monetization, and churn using a full-funnel data science approach.

I first modeled retention using parametric survival curves and compared two variants under multiple monetization scenarios. While Variant A performs better at low DARPU levels due to stronger early retention, Variant B becomes more profitable as user lifetime value increases, making it the more scalable long-term option.

I then built a comprehensive player segmentation framework covering skill, spending, play-time preferences, engagement intensity, and churn risk. These segments are interpretable, statistically validated, and designed to be directly actionable for product, marketing, and CRM use cases.

To move beyond descriptive analysis, I developed churn prediction models using early behavioral signals. A short-term inactivity prediction model achieved strong performance and enabled classification of users into churned, alarming, and safe groups, allowing proactive intervention.

Finally, I supported the analysis with seasonality and geographic insights to highlight temporal usage patterns and regional value differences.

Overall, the work combines **explainable analytics**, **robust segmentation**, and **predictive modeling** to support data-driven decisions across acquisition, retention, and monetization.

