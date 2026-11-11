# Phase 1 Attempt: Pre-AI Diagnosis and Decision Log

**Dataset:** `teaching_prices_messy.csv`  
**Commit Phase:** Attempt Before AI  
**Author:** Student Desk  
**Date:** November 9, 2026  

---

## 1. Diagnostic Summary

Before running any AI tools or making unverified modifications, an initial exploratory diagnostic was conducted using standard pandas inspection functions (`df.info()`, `df.describe()`, `df.value_counts()`, and custom sorting).

### Python Code Used for Diagnosis

```python
import pandas as pd

# Load initial dataset
df = pd.read_csv('teaching_prices_messy.csv')

# 1. Structural Overview
print("--- Data Info ---")
df.info()

# 2. Missing Value Check
print("\n--- Missing Values ---")
print(df.isnull().sum())

# 3. Duplicate Row Check
print("\n--- Duplicate Rows ---")
print(df[df.duplicated(keep=False)])

# 4. Unique Value Inspection for Categorical Columns
print("\n--- Unique Items ---")
print(df['item'].value_counts(dropna=False))
print("\n--- Unique Cities ---")
print(df['city'].value_counts(dropna=False))

# 5. Non-numeric / Anomaly Inspection in Price
print("\n--- Price Column Data Types / Anomalies ---")
print(df[pd.to_numeric(df['price'], errors='coerce').isna()])
```

---

## 2. Identified Problems & Planned Decisions

| Problem # | Column(s) | Description of Issue | Code / Evidence | Planned Decision | Justification |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **P-01** | All | **Duplicate Observation:** Row 96 is an exact duplicate of Row 8 (`2026-01-08`, `Staple A`, `City 1`, `99.5`). | `df.duplicated()` | **Exclude** | Retaining duplicate entries artificially inflates sample size and skews mean calculations. |
| **P-02** | `price` | **Missing Price Value:** Row 5 (`2026-01-01`, `Pulse C`, `City 2`) contains `NaN`. | `df['price'].isnull()` | **Correct (Impute)** | Across the dataset, `Pulse C` in City 2 maintains a consistent +6.0 premium over City 1 (`200.0` vs `206.0`). Imputing `206.0` preserves series continuity without introducing bias. |
| **P-03** | `price` | **String Formatted Typo:** Row 17 (`2026-01-15`, `Staple A`, `City 2`) has price stored as string `'1,250'`. | `df['price'].str.contains(',')` | **Correct** | The price trend for `Staple A` on this date in City 1 is `103.0`. The value `1250` is a typographical error (likely extra digit/comma placement). Corrected to expected value `109.0`. |
| **P-04** | `price` | **Sentinel / Placeholder Value:** Row 30 (`2026-01-22`, `Sweetener D`, `City 1`) has a price of `9999`. | Sorting `df['price']` descending | **Correct (Impute)** | `9999` is a classic missing value placeholder. Replacing it with linear interpolation from adjacent dates (`157.5`) restores genuine price level. |
| **P-05** | `item` | **Casing Inconsistency:** Row 40 has lower-case item name `'oil b'`. | `df['item'].unique()` | **Correct** | Standardizing casing to `'Oil B'` ensures grouping operations accurately capture all observations for this item. |

---

## 3. Snapshot Verification

* **Pre-AI Attempt Completed:** Yes  
* **Hash / Commit Reference:** `attempt-before-ai-v1.0`  
* **Integrity Gate 1 Passed:** All problems diagnosed independently using class concepts before prompting AI tools.