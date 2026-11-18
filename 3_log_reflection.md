# AI Audit Log, Confused Classmate Transcript & Reflection (`3_log_reflection.md`)

---

## Part 1: Interpretation Auditor Output & Colour-Coded Audit Table

### Prompt Sent to AI (Interpretation Auditor)
> "Here is a summary statistics table from an illustrative 12-week price dataset (`2026-01-01` to `2026-03-19`, two cities, $n=24$ per item, PKR/kg). Please interpret what these numbers show for a newsroom reader:
> * Staple A: Mean = 111.17, Median = 111.75, City 1 % Change = +13.37%, City 2 % Change = +12.62%, Combined % Change = +12.98%
> * Oil B: Mean = 324.83, Median = 324.00, City 1 % Change = +15.44%, City 2 % Change = +15.13%, Combined % Change = +15.28%
> * Pulse C: Mean = 205.92, Median = 206.00, City 1 % Change = +3.75%, City 2 % Change = +3.64%, Combined % Change = +3.69%
> * Sweetener D: Mean = 166.75, Median = 166.75, City 1 % Change = +19.80%, City 2 % Change = +19.03%, Combined % Change = +19.41%"

### Raw AI Response (Sentence-Numbered for Audit)
1. **Sweetener D** experienced the steepest percentage price increase over the 12-week window at **19.41%** combined, whereas **Pulse C** had the smallest increase at **3.69%**.
2. Across all four items, the mean and median differ by less than **1.00 PKR/kg** (ranging from **0.00 PKR/kg** for Sweetener D to **0.83 PKR/kg** for Oil B).
3. In every single item, **City 1** recorded a slightly larger percentage jump than **City 2**, with the widest gap appearing in Sweetener D (**0.77 percentage points**).
4. This uniformity across cities likely indicates that regional wholesale supply costs were transmitted evenly across urban markets rather than being isolated to a single city's retail markup.
5. For Oil B, the mean (**324.83**) sits above the median (**324.00**) because late-quarter price spikes in March pulled the arithmetic average upward.
6. Government export restrictions on sugar and global palm oil tariffs directly caused Sweetener D and Oil B to surge past 15% while leaving local pulse harvests untouched.
7. Household food budgets in City 1 shrank by 13% over the quarter as families shifted spending away from cooking oil toward cheaper pulses.

### Claim Audit Table

| # | AI Sentence (Claim) | Colour | How I Checked | Verdict |
| :---: | :--- | :---: | :--- | :---: |
| 1 | "Sweetener D experienced the steepest percentage price increase over the 12-week window at 19.41% combined, whereas Pulse C had the smallest increase at 3.69%." | **GREEN** | Recomputed `(181.5 - 152.0)/152.0 = +19.41%` (max of all four items) and `(210.5 - 203.0)/203.0 = +3.69%` (min of all four items) in `2_work.ipynb`. | **Accepted** |
| 2 | "Across all four items, the mean and median differ by less than 1.00 PKR/kg (ranging from 0.00 PKR/kg for Sweetener D to 0.83 PKR/kg for Oil B)." | **GREEN** | Recomputed `|mean - median|` for all items: Staple A `0.58`, Oil B `0.83`, Pulse C `0.08`, Sweetener D `0.00`. All are `< 1.00`. | **Accepted** |
| 3 | "In every single item, City 1 recorded a slightly larger percentage jump than City 2, with the widest gap appearing in Sweetener D (0.77 percentage points)." | **GREEN** | Checked `City 1 - City 2` % changes: Staple A `13.37 - 12.62 = 0.75`, Oil B `15.44 - 15.13 = 0.31`, Pulse C `3.75 - 3.64 = 0.11`, Sweetener D `19.80 - 19.03 = 0.77` pp. | **Accepted** |
| 4 | "This uniformity across cities likely indicates that regional wholesale supply costs were transmitted evenly across urban markets rather than being isolated to a single city's retail markup." | **AMBER** | Plausible market hypothesis framed cautiously ("likely indicates"), but the CSV contains only city names, dates, and retail prices—no wholesale cost data. | **Accepted only as labelled hypothesis (`[E]`)** |
| 5 | "For Oil B, the mean (324.83) sits above the median (324.00) because late-quarter price spikes in March pulled the arithmetic average upward." | **AMBER** | Checked weekly Oil B prices: they rise steadily (`298` to `344` in City 1) without a sudden isolated spike; the +0.83 gap comes from mild right-tail skewness in the upper half of the schedule, not a single late shock. | **Rejected / Revised** |
| 6 | "Government export restrictions on sugar and global palm oil tariffs directly caused Sweetener D and Oil B to surge past 15% while leaving local pulse harvests untouched." | **RED** | Unverified causal claim citing external policy events not in the table; moreover, `teaching_prices_clean.csv` is explicitly labelled illustrative synthetic teaching data. | **Rejected** |
| 7 | "Household food budgets in City 1 shrank by 13% over the quarter as families shifted spending away from cooking oil toward cheaper pulses." | **RED** | Refers to household expenditure and substitution quantities that do not exist anywhere in the dataset (which tracks unit prices only). | **Rejected** |

---

## Part 2: "Confused Classmate" Dialogue Excerpt

**Prompt Setup:** *"Play a classmate who believes 'the mean is always the typical price' and keeps asking 'but why?' until my explanation resolves the confusion."*

> **Classmate (AI):** I still don't get the argument in the newsroom. The mean adds up every single price and divides by how many prices there are—that uses 100% of the data! Doesn't that mean the mean is *always* the most typical price?
>
> **Me:** Using every number is exactly why the mean gets fooled when even one number is weird. Imagine five shops sell Staple A at 110, 111, 112, 112, and 115 PKR/kg. Both the mean (112) and the median (112) match what shoppers actually pay. Now suppose one clerk types `1150` instead of `115`. The mean jumps to **319 PKR/kg**, even though four out of five shops still charge 110–112 PKR!
>
> **Classmate (AI):** Okay, sure, if there's a typo—but why not just fix typos and still always use the mean? Even without typos, what if one fancy supermarket charges 250 PKR for imported organic flour? That's a real price, so why shouldn't it count in the typical price?
>
> **Me:** Because "typical" means *what a normal shopper usually encounters*, not the mathematical center of gravity of extreme luxury outliers. If four neighbourhood stores charge ~112 PKR and one boutique store charges 250 PKR, the mean becomes **139.6 PKR**—a price that **zero** stores actually charge and that is 25% higher than what 80% of people pay. The median stays at **112 PKR**, which sits right in the middle of where real transactions happen.
>
> **Classmate (AI):** Wait—but why does the median ignore how big that 250 PKR price is? Isn't throwing away magnitude wasteful?
>
> **Me:** When your goal is to describe *total expenditure* (like calculating how much a catering company spends buying one kilo from every shop), you *must* use the mean because total rupees spent depends on exact magnitudes. When your goal is to tell a reader what a **typical shopper** pays, ignoring extreme tail magnitude is a feature, not a bug: sorting the prices and picking the middle rank (the 50th percentile) guarantees that half the observations are at or below that number and half are at or above it.
>
> **Classmate (AI):** Ah, that makes sense! So the mean measures the *balance point of total rupees*, which gets dragged around by rare extremes, while the median measures the *middle shopper's experience*, which stays put unless half the market actually moves.

---

## Part 3: Reflection — Where Did My First Draft Say More Than the Data Allowed?

1. **Sneaking in an Unsupported Causal Claim (`[C]`):** In my first draft (`1_attempt.md`), I wrote: *"Transport fuel surcharges and wholesale hoarding drove Sweetener D and Oil B up faster than Pulse C over the quarter."* While that sounds like standard economic commentary in a newsroom, `teaching_prices_clean.csv` contains only `date`, `item`, `city`, `price`, and `unit`—zero variables on fuel prices, inventory levels, or hoarding. Even worse, the `data_label` column explicitly notes that the file is illustrative teaching data rather than real PBS market observations. In my final publishable note, I removed that sentence completely and stuck strictly to observed price movements (`[O]`) and clearly signposted methodological explanations (`[E]`).
2. **Conflating Cleaned Data with Raw Outliers:** My initial draft implied that the newsroom neighbours' disagreement came from an active outlier inside the cleaned table, even though my own numbers showed `|mean - median| <= 0.83 PKR/kg` across all four items. Auditing the table forced me to separate two distinct reasons two readers disagree on a clean series: (a) comparing a **12-week multi-month average** (`111.75 PKR/kg`) against the **latest week's level** (`117.50 PKR/kg` combined on `2026-03-19`, a `+12.98%` rise from January 1), and (b) comparing **City 1** (`median = 107.75`) against **City 2** (`median = 113.75`), which sits exactly `6.00 PKR/kg` higher every week.
3. **Final Publishable Referee's Note (Revised After Audit):**
   > **The Prices Desk — Referee's Note:** Both neighbours are quoting accurate figures from our 12-week price log (`2026-01-01` to `2026-03-19`), but they are measuring two different things: a **quarter-long midpoint** versus **how far prices climbed from January to March**. Across all 24 city-week observations, the median and mean sit within 0.83 PKR/kg of each other for every staple—**Staple A** (median 111.75 vs. mean 111.17 PKR/kg), **Oil B** (324.00 vs. 324.83 PKR/kg), **Pulse C** (206.00 vs. 205.92 PKR/kg), and **Sweetener D** (166.75 vs. 166.75 PKR/kg). However, comparing the first week against the twelfth week shows sustained increases across all four items: **+12.98%** for Staple A, **+15.28%** for Oil B, **+3.69%** for Pulse C, and **+19.41%** for Sweetener D (city-averaged). Because the arithmetic mean can be distorted by single-shop spikes or recording errors while the median stays anchored at the middle observation, The Prices Desk reports the **median** for the typical price level alongside the **first-to-last percentage change** to show the trend.