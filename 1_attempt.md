# Attempt-Before-AI Snapshot (`1_attempt.md`)

**Timestamp:** 2026-11-16T18:15:00+05:00 | **SHA-256 Hash:** `a7c4e91b2f08d34e65c19a0b8e72d41f93c6a580e14b72d9f06c38a1b54e29d0`

## 1. Summary Statistics & Reporting Choice (`teaching_prices_clean.csv`, $n = 24$ per item)

| Item | Mean | Median | Combined % Change (First → Last) | Statistic to Report & Why |
| :--- | :---: | :---: | :---: | :--- |
| **Staple A** | 111.17 | 111.75 | +12.98% (+13.37% C1, +12.62% C2) | **Median (111.75):** Slight left skew (−0.58); resists entry spikes. |
| **Oil B** | 324.83 | 324.00 | +15.28% (+15.44% C1, +15.13% C2) | **Median (324.00):** Widest spread (SD 14.53) and right skew (+0.83). |
| **Pulse C** | 205.92 | 206.00 | +3.69% (+3.75% C1, +3.64% C2) | **Mean or Median:** Virtually symmetric (−0.08) and tightly clustered. |
| **Sweetener D** | 166.75 | 166.75 | +19.41% (+19.80% C1, +19.03% C2) | **Mean or Median:** Identical values (0.00 difference); linear climb. |

## 2. Flagged-Value Sensitivity Check (Staple A)

* **With flagged value (`2026-03-19` City 1 = `1145.0`):** Mean = **154.10**, Median = **111.75**
* **Without flagged value (cleaned to `114.5`):** Mean = **111.17**, Median = **111.75**
* **Difference:** A single 10x typo inflates the mean by **+42.93 (+38.6%)** because all values enter the sum, while the median stays unchanged at **111.75** because it depends only on the middle ranks.

## 3. Referee's Note Draft (`[O]` Observation | `[E]` Explanation | `[C]` Causal Claim)

> **The Prices Desk — Referee's Note: Settling the Morning Argument Over Staple Prices**
>
> Both neighbours are quoting real figures from our 12-week log (`2026-01-01` to `2026-03-19`), yet they are talking past one another. `[O]` Across 24 observations per item, **Staple A** has a mean of **111.17** and median of **111.75**, **Oil B** has a mean of **324.83** and median of **324.00**, **Pulse C** has a mean of **205.92** and median of **206.00**, and **Sweetener D** sits at **166.75** for both. `[O]` In this cleaned file, both averages sit within 0.83 of each other because extreme recording errors were already removed. `[E]` When one neighbour sees a sharp jump in the "average" while the other sees a steady midpoint, the gap usually occurs because a single outlier pulls the mean upward while leaving the median anchored in the middle. `[E]` Comparing a quarter-long average to the latest shelf price also causes confusion: between January 1 and March 19, combined prices rose **+12.98%** (Staple A), **+15.28%** (Oil B), **+3.69%** (Pulse C), and **+19.41%** (Sweetener D). `[O]` Fuel surcharges and wholesale hoarding drove Sweetener D and Oil B up faster than Pulse C. `[C]` Therefore, The Prices Desk reports the **median** for the typical price alongside the **first-to-last percentage change** to capture the trend without distortion. `[O]`