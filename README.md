# Marketing A/B Testing & User Conversion Analytics

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Executive Summary
This project delivers an end-to-end statistical analysis of an A/B testing campaign comprising **582,149 user interaction records**. The primary objective is evaluating whether targeted digital ad campaigns generate a statistically significant lift in user conversion rates compared to a Public Service Announcement (PSA) control group, while determining optimal ad frequency caps and peak temporal engagement windows.

- **Interactive Tableau Dashboard:** [View Live Dashboard](https://public.tableau.com/views/MarketingABTestingAnalytics/Executive?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- **Primary Tech Stack:** Python (Pandas, NumPy, SciPy, Statsmodels, Seaborn, Matplotlib), Jupyter/Google Colab, Tableau Public

---

## Business Problem & Objectives
* **Core Hypothesis:** Exposure to targeted marketing ads increases user conversion rate significantly over baseline exposure (PSA).
* **Ad Fatigue Threshold:** Identify the frequency of ad exposures beyond which conversion yield diminishes or plateaus.
* **Resource Optimization:** Pinpoint peak days and hours of user engagement to optimize programmatic ad spend allocation.

---

## Dataset Architecture & Feature Engineering

### Dataset Overview
* **Total Sample Size:** 582,149 unique user records
* **Test Group (Ad):** 558,883 users (96.0%)
* **Control Group (PSA):** 23,266 users (4.0%)
* **Baseline Conversion:** 2.39% overall conversion rate (13,916 total conversions)

### Data Dictionary & Engineered Features
| Feature Name | Data Type | Description |
| :--- | :--- | :--- |
| `test_group` | Categorical | Group identifier (`ad` vs `psa`) |
| `converted` | Binary | Conversion status (`True` = Converted, `False` = Non-converted) |
| `total_ads` | Continuous | Total ad/PSA exposures per user |
| `ad_freq_bucket` | Categorical | Bucketed exposure levels: `Very Low` (1–5), `Low` (6–15), `Medium` (16–30), `High` (30+) |
| `most_ads_day` | Categorical | Day of peak user exposure (Monday–Sunday) |
| `most_ads_hour` | Discrete | Peak exposure hour (0–23) |

---

## Statistical Hypothesis Testing & Methodology

### 1. Two-Sample Z-Test for Proportions
To determine if the observed lift in conversion rate is statistically significant:

* **Null Hypothesis ($H_0$):** $p_{\text{ad}} - p_{\text{psa}} = 0$
* **Alternative Hypothesis ($H_1$):** $p_{\text{ad}} - p_{\text{psa}} > 0$
* **Statistical Formula:**
$$Z = \frac{\hat{p}_1 - \hat{p}_2}{\sqrt{\hat{p}(1 - \hat{p}) \left(\frac{1}{n_1} + \frac{1}{n_2}\right)}}$$

### Statistical Results Summary
| Metric | Ad Group | PSA Group (Control) | Variance / Significance |
| :--- | :--- | :--- | :--- |
| **Sample Size ($n$)** | 558,883 | 23,266 | — |
| **Conversions** | 14,423 | 420 | — |
| **Conversion Rate ($\hat{p}$)** | **2.55%** | **1.79%** | **+0.76% Absolute Lift** |
| **Relative Lift** | — | — | **+42.5% Lift** |
| **p-value** | — | — | **$< 0.0001$** (Significant at $\alpha = 0.05$) |

### 2. Chi-Square Test of Independence ($\chi^2$)
Evaluated the association between ad exposure intensity (`ad_freq_bucket`) and conversion outcome (`converted`) to confirm that ad volume directly impacts user conversion behavior ($\chi^2 = \text{Significant}$, $p < 0.001$).

---

## Key Insights & Data Discoveries

* **Ad Campaign Effectiveness:** Exposure to ads yielded a **42.5% relative increase** in conversions over the control group ($p < 0.0001$).
* **Ad Fatigue & Diminishing Returns:**
  * Conversion rates rise steadily between 1 and 15 ad exposures.
  * Performance plateaus past **15+ impressions**, showing sharp diminishing returns and potential ad fatigue.
* **Temporal Patterns:**
  * **Peak Engagement Days:** Friday (91,381 exposures) and Monday (86,029 exposures).
  * **Peak Conversion Window:** 11:00 AM – 6:00 PM local time.

---

## Interactive Tableau Dashboard

The interactive Tableau dashboard provides dynamic exploration of campaign metrics across demographics, temporal windows, and exposure buckets.

- 🔗 **[Access the Dashboard Here](YOUR_TABLEAU_PUBLIC_LINK)**

**Key Dashboard Views:**
1. **Executive KPI Header:** Conversion rates, user split, and total lift.
2. **Impression vs. Conversion Curve:** Visualizing the saturation threshold for ad frequency capping.
3. **Hourly Engagement Heatmap:** Visual breakdown of peak conversion times throughout the week.

---

## Strategic Business Recommendations

* **Capitalize on Mid-Week Incremental Lift Peaks:** While Monday yields the highest raw ad conversion rate (**3.15% CVR**), Tuesday and Wednesday deliver the highest **incremental lift** over the PSA baseline (**\+60.66% on Tuesday** and **\+56.02% on Monday/Wednesday**). Structure dynamic bid rules to prioritize impression share aggressively on Tuesday and Wednesday, where paid ads generate the maximum organic push over baseline behavior.
* **Optimize Programmatic Bidding Schedules:** Concentrated ad spend during peak conversion windows (**11:00 AM – 6:00 PM**), prioritizing Mondays and Fridays.
* **Scale Treatment Group Parameters:** Expand campaign parameters across wider demographic segments, given the statistically validated 42.5% lift.

