# Assignment 2: Cleaning Log & Reflection

**Course:** ICA-F26 The Prices Desk  
**File:** `3_log_reflection.md`  
**Date:** November 9, 2026  

---

## 1. Prompt Sent to AI Tool

> **Prompt:** "I have a CSV dataset of price records with columns `date`, `item`, `city`, and `price`. The dataset contains text casing inconsistencies in `item`, string formatted numbers with commas, missing values (`NaN`), sentinel outlier placeholders (`9999`), and duplicate rows. Please generate python pandas cleaning code to address these issues."

---

## 2. Cleaning Code Court Verdicts

Every line of AI-suggested code was evaluated for correctness, context preservation, and unintended side effects prior to adoption.

| Line # | Proposed AI Code Snippet | Verdict | Evidence / Reason |
| :--- | :--- | :--- | :--- |
| 1 | `df = df.drop_duplicates()` | **Keep** | Removes exact duplicate Row 96 (`2026-01-08`, `Staple A`, `City 1`). Row count drops from 101 to 100 with zero loss of valid unique data. |
| 2 | `df['item'] = df['item'].str.title()` | **Keep** | Standardizes `'oil b'` to `'Oil B'`, unifying categories under consistent casing across all rows. |
| 3 | `df['price'] = df['price'].astype(str).str.replace(',', '')` | **Keep** | Removes thousand-separator comma in `'1,250'`, making the string safely convertible to numeric types. |
| 4 | `df['price'] = pd.to_numeric(df['price'], errors='coerce')` | **Keep** | Safely converts price column to float64, converting unparseable entries to `NaN` for explicit handling. |
| 5 | `df['price'] = df['price'].replace(9999, np.nan)` | **Modify** | AI blindly replaced `9999` with `NaN` and dropped it. **Modification:** Imputed value `157.5` based on temporal interpolation of `Sweetener D` in `City 1`. |
| 6 | `df['price'] = df.groupby('item')['price'].transform(lambda x: x.fillna(x.mean()))` | **Reject** | Blind mean substitution ignores cross-city price deltas and temporal trends. Replaced with domain-specific rule (`City 2` = `City 1 + 6.0`). |

---

## 3. Comprehensive Audit Log

| Row Index | Date | Item | City | Original Value | Cleaned Value | Action Taken | Rationale & Evidence |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **8 / 96** | 2026-01-08 | Staple A | City 1 | Duplicate Row | Excluded | **Exclude** | Row 96 is a exact duplicate of Row 8. Dropping it restores true row count to 100. |
| **5** | 2026-01-01 | Pulse C | City 2 | `NaN` | `206.0` | **Correct** | `Pulse C` maintains a fixed +6.0 premium in City 2 over City 1 (`200.0` vs `206.0`). |
| **17** | 2026-01-15 | Staple A | City 2 | `'1,250'` | `109.0` | **Correct** | Typographical error. Staple A in City 1 is 103.0; historic City 2 markup indicates expected price is `109.0`. |
| **30** | 2026-01-22 | Sweetener D | City 1 | `9999` | `157.5` | **Correct** | `9999` is an unannounced placeholder. Linearly interpolated between adjacent weekly readings (`155.0` and `160.0`). |
| **40** | 2026-02-05 | oil b | City 1 | `'oil b'` | `'Oil B'` | **Correct** | Inconsistent string casing. Standardized to match uniform title case convention. |

---

## 4. Reflection

### Human vs. AI Decision Discrepancies
The AI tool excelled at syntax generation for string cleaning (`.str.replace()`) and type casting (`pd.to_numeric()`). However, its domain logic was flawed:
1. **Blind Mean Imputation:** The AI suggested filling missing values with column means (`df.fillna(df.mean())`), which completely ignored city price differentials (+6.0 offset between City 1 and City 2).
2. **Aggressive Row Deletion:** The AI recommended dropping all `NaN` values and outlier rows containing `9999`. Deleting these observations would have destroyed valid panel structure.

### What Would Have Been Missed Alone?
Finding all the messy errors on our own would have taken much longer and required trying many different ways to spot them. On the other hand, trusting AI completely would have been a mistake because it likely would have thrown away good information and messed up our final numbers.

### Conclusion & Verification
After executing the verified cleaning log, each item-city pair contains exactly the expected number of periodic observations, ensuring full ledger integrity without undocumented alterations.