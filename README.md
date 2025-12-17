# 📊 Indian Mutual Fund Analysis using MFtool

> *Mutual Fund investments are subject to market risks, read all scheme-related documents carefully.*

---

## 📖 Project Overview

This project focuses on analyzing the **Indian mutual fund ecosystem** using historical **Net Asset Value (NAV)** data sourced from **AMFI (Association of Mutual Funds in India)** via the **MFtool** Python library.

The objective is to perform a **category-wise and fund-level performance analysis** using **SQL**, following metrics and evaluation methods commonly used in the **investment and asset management industry**.

The project emphasizes:
- Data accuracy and financial precision  
- Industry-standard performance metrics  
- Evidence-based insights derived directly from query results  

---

## 📂 Dataset Overview

- **Source:** AMFI (publicly available mutual fund data)  
- **Access Method:** MFtool (Python library)  
- **Data Type:** Daily historical NAV data  
- **Granularity:** One record per scheme per trading day  
- **Volume:** Millions of rows across thousands of schemes  
- **Storage & Analysis:** PostgreSQL (pgAdmin)  

The dataset includes both **equity-oriented and debt-oriented mutual funds**, covering a wide range of schemes launched across different time periods.

---

## 🧱 Core Fields Used

The analysis is primarily based on the following attributes:

- **Scheme Code** – Unique identifier for each mutual fund scheme  
- **Scheme Name** – Official scheme name used for classification  
- **NAV Date** – Date of NAV declaration  
- **NAV** – Net Asset Value per unit  
- **Day Change** – Day-on-day NAV movement  

These fields are sufficient to evaluate **returns, growth trends, volatility, and consistency** of mutual fund performance.

---
## 🔍 Mutual Fund Category Distribution

### Question
How many mutual fund schemes are available in each category such as Large Cap, Mid Cap, Small Cap, Debt, and other categories?

### Why this analysis is needed
Before analyzing performance, it is important to understand how mutual funds are distributed across categories. Funds from different categories follow different investment strategies and risk levels, so comparisons should always be made within the same category.

### What we expect to find
- Total number of schemes in each category  
- Categories with the highest and lowest number of funds  
- Presence of niche categories such as Gold or Silver funds  

This analysis forms the foundation for further category-wise performance analysis.

## 🔍 Mutual Fund Category and Scheme Count

### Objective
The objective of this analysis is to identify the types of mutual fund categories present in the dataset and determine how many schemes belong to each category.

### Key Observations
- The dataset contains a broad mix of Equity, Debt, Hybrid, Solution-Oriented, and Other mutual fund schemes.
- A significant number of schemes fall under **Other – Unclassified**, indicating variability in scheme naming conventions across fund houses.
- **Index / ETF schemes** have strong representation, reflecting the increasing adoption of passive investment products.
- **Debt funds** such as Liquid, Banking & PSU, Overnight, Ultra Short Duration, and Gilt funds dominate in terms of scheme count.
- **Equity funds** including Large Cap, Mid Cap, Small Cap, Flexi Cap, ELSS, and Value funds are well represented.
- **Hybrid schemes**, particularly Arbitrage and Aggressive Hybrid funds, form a meaningful portion of the dataset.
- **Solution-oriented schemes** like Retirement and Children’s funds exist but have relatively limited representation.

### Conclusion
The category-wise distribution highlights the diversity of the Indian mutual fund market. Understanding this distribution is critical before performing any category-wise performance or risk analysis, as meaningful comparisons should always be made within the same fund category.


---

```sql
SELECT
    CASE
        /* =======================
           EQUITY SCHEMES
        ======================= */
        WHEN LOWER(scheme_name) LIKE '%multi cap%' THEN 'Equity - Multi Cap'
        WHEN LOWER(scheme_name) LIKE '%flexi cap%' THEN 'Equity - Flexi Cap'
        WHEN LOWER(scheme_name) LIKE '%large cap%' THEN 'Equity - Large Cap'
        WHEN LOWER(scheme_name) LIKE '%large & mid%' 
          OR LOWER(scheme_name) LIKE '%large and mid%' THEN 'Equity - Large & Mid Cap'
        WHEN LOWER(scheme_name) LIKE '%mid cap%' THEN 'Equity - Mid Cap'
        WHEN LOWER(scheme_name) LIKE '%small cap%' THEN 'Equity - Small Cap'
        WHEN LOWER(scheme_name) LIKE '%dividend yield%' THEN 'Equity - Dividend Yield'
        WHEN LOWER(scheme_name) LIKE '%value%' THEN 'Equity - Value'
        WHEN LOWER(scheme_name) LIKE '%contra%' THEN 'Equity - Contra'
        WHEN LOWER(scheme_name) LIKE '%focused%' THEN 'Equity - Focused'
        WHEN LOWER(scheme_name) LIKE '%sector%' 
          OR LOWER(scheme_name) LIKE '%thematic%' THEN 'Equity - Sectoral/Thematic'
        WHEN LOWER(scheme_name) LIKE '%elss%' THEN 'Equity - ELSS'

        /* =======================
           DEBT SCHEMES
        ======================= */
        WHEN LOWER(scheme_name) LIKE '%overnight%' THEN 'Debt - Overnight'
        WHEN LOWER(scheme_name) LIKE '%liquid%' THEN 'Debt - Liquid'
        WHEN LOWER(scheme_name) LIKE '%ultra short%' THEN 'Debt - Ultra Short Duration'
        WHEN LOWER(scheme_name) LIKE '%low duration%' THEN 'Debt - Low Duration'
        WHEN LOWER(scheme_name) LIKE '%money market%' THEN 'Debt - Money Market'
        WHEN LOWER(scheme_name) LIKE '%short duration%' THEN 'Debt - Short Duration'
        WHEN LOWER(scheme_name) LIKE '%medium duration%' THEN 'Debt - Medium Duration'
        WHEN LOWER(scheme_name) LIKE '%medium to long%' THEN 'Debt - Medium to Long Duration'
        WHEN LOWER(scheme_name) LIKE '%long duration%' THEN 'Debt - Long Duration'
        WHEN LOWER(scheme_name) LIKE '%dynamic bond%' THEN 'Debt - Dynamic Bond'
        WHEN LOWER(scheme_name) LIKE '%corporate bond%' THEN 'Debt - Corporate Bond'
        WHEN LOWER(scheme_name) LIKE '%credit risk%' THEN 'Debt - Credit Risk'
        WHEN LOWER(scheme_name) LIKE '%banking%' 
          OR LOWER(scheme_name) LIKE '%psu%' THEN 'Debt - Banking & PSU'
        WHEN LOWER(scheme_name) LIKE '%gilt%' 
          AND LOWER(scheme_name) LIKE '%10 year%' THEN 'Debt - Gilt (10Y Constant Duration)'
        WHEN LOWER(scheme_name) LIKE '%gilt%' THEN 'Debt - Gilt'
        WHEN LOWER(scheme_name) LIKE '%floater%' THEN 'Debt - Floater'

        /* =======================
           HYBRID SCHEMES
        ======================= */
        WHEN LOWER(scheme_name) LIKE '%conservative hybrid%' THEN 'Hybrid - Conservative'
        WHEN LOWER(scheme_name) LIKE '%balanced hybrid%' THEN 'Hybrid - Balanced'
        WHEN LOWER(scheme_name) LIKE '%aggressive hybrid%' THEN 'Hybrid - Aggressive'
        WHEN LOWER(scheme_name) LIKE '%balanced advantage%' 
          OR LOWER(scheme_name) LIKE '%dynamic asset allocation%' THEN 'Hybrid - Balanced Advantage'
        WHEN LOWER(scheme_name) LIKE '%multi asset%' THEN 'Hybrid - Multi Asset'
        WHEN LOWER(scheme_name) LIKE '%arbitrage%' THEN 'Hybrid - Arbitrage'
        WHEN LOWER(scheme_name) LIKE '%equity savings%' THEN 'Hybrid - Equity Savings'

        /* =======================
           SOLUTION ORIENTED
        ======================= */
        WHEN LOWER(scheme_name) LIKE '%retirement%' THEN 'Solution - Retirement'
        WHEN LOWER(scheme_name) LIKE '%children%' THEN 'Solution - Children'

        /* =======================
           OTHER SCHEMES
        ======================= */
        WHEN LOWER(scheme_name) LIKE '%index%' 
          OR LOWER(scheme_name) LIKE '%etf%' THEN 'Other - Index / ETF'
        WHEN LOWER(scheme_name) LIKE '%fund of fund%' 
          OR LOWER(scheme_name) LIKE '%fof%' THEN 'Other - Fund of Funds'

        ELSE 'Other - Unclassified'
    END AS mutual_fund_category,

    COUNT(DISTINCT scheme_code) AS total_schemes
FROM mutual_fund_nav
GROUP BY mutual_fund_category
ORDER BY total_schemes DESC;
```




---

<img width="378" height="569" alt="image" src="https://github.com/user-attachments/assets/26540d0f-0c21-4465-a45d-4c25274d93a9" />
<img width="381" height="568" alt="image" src="https://github.com/user-attachments/assets/4810c004-f6f3-42c1-b2dd-8395e5ff4d90" />


## 🔍 Average Daily Performance by Category

### Objective
To compare the average daily performance of different mutual fund categories using historical NAV day-on-day changes.

### Approach
- Used `day_change` as a proxy for daily return.
- Calculated average daily return for each mutual fund category.
- Categories were pre-classified and stored as a physical column to avoid repeated classification logic.

### SQL Query
```sql
SELECT
    mutual_fund_category,
    ROUND(AVG(day_change), 6) AS avg_daily_return
FROM mutual_fund_nav
GROUP BY mutual_fund_category
ORDER BY avg_daily_return DESC;
```
<img width="397" height="566" alt="image" src="https://github.com/user-attachments/assets/eb3c53b9-437b-4c5d-b6b4-196674f5363d" />
<img width="395" height="515" alt="image" src="https://github.com/user-attachments/assets/66e0de67-402b-4173-a16b-0e1eaa95c196" />



Key Insights

Debt-oriented categories such as Money Market, Overnight, and Liquid funds show stable average daily returns.

Equity categories including Mid Cap, Flexi Cap, and Large & Mid Cap demonstrate higher growth-oriented daily returns.

Hybrid funds reflect balanced performance with relatively lower daily volatility.

Long-duration debt and gilt-based funds show comparatively lower average daily returns.

The analysis confirms that return behavior varies significantly across fund categories, reinforcing the importance of category-wise comparison.


## 📊 Volatility Analysis by Mutual Fund Category

###   **Which mutual fund categories are the most volatile?**

To measure **risk and instability**, we calculated the **standard deviation of daily NAV changes (`day_change`)** for each mutual fund category.

---

### 📐 Why Daily Volatility?

- Daily NAV movement captures **true market fluctuations**
- Risk in finance is measured at the **most granular level**
- Monthly aggregation can **hide short-term risk spikes**
- Industry-standard risk metrics (**Volatility, Sharpe Ratio, VaR**) are **daily-based**

---

### 🔍 Key Insights from the Output

#### 🟡 Other – Unclassified
- Extremely high volatility  
- Indicates **mixed or poorly classified schemes**  
- Not suitable for **reliable risk analysis**

#### 🔵 Debt Categories (Surprisingly Volatile)
- **Liquid, Overnight, Money Market** funds show higher daily volatility than expected  
- Reason: very small NAV changes but **high frequency**, which increases standard deviation  
- **Key insight:** Low return does **not** always mean low volatility

#### 🟢 Equity Categories
- Observed volatility pattern:  
  **Mid Cap > Flexi Cap > Large Cap**
- Matches real-world risk hierarchy:
  - Mid & Small Caps are **more sensitive** to market movements
  - Large Caps are **relatively stable**

#### 🟣 Hybrid & Arbitrage Funds
- Lowest volatility across categories  
- Suitable for **low-risk investors**  
- Confirms **industry positioning** of these products

---

### 🎯 Takeaway
Daily volatility provides a clearer and more accurate view of **short-term risk** across mutual fund categories.  
Debt funds can still exhibit noticeable daily volatility, while hybrid and arbitrage funds remain the most stable options.

```sql

SELECT
    mutual_fund_category,
    ROUND(STDDEV(day_change), 6) AS daily_volatility
FROM mutual_fund_nav
GROUP BY mutual_fund_category
ORDER BY daily_volatility DESC;
```
<img width="381" height="568" alt="image" src="https://github.com/user-attachments/assets/5e44a16a-e5d2-4584-982e-ba4667c4b212" />
<img width="377" height="514" alt="image" src="https://github.com/user-attachments/assets/909a894e-600a-4012-99cb-66c90eee6965" />


# How do mutual fund categories perform on a monthly basis in terms of returns and volatility?

After analyzing daily movements, we shifted to **monthly aggregation** to reduce noise and better reflect how investors typically evaluate mutual fund performance.

## 🧮 Methodology

- Daily NAV data was aggregated to **month-end NAV**
- Monthly returns were calculated using **month-over-month NAV change**
- Two key metrics were derived:
    - **Average Monthly Return**
    - **Monthly Volatility (Standard Deviation of Monthly Returns)**

## 🔍 Key Observations

### 🔵 Debt Categories

- **Money Market, Liquid, Overnight, Short Duration** funds show:
    - Higher average monthly returns
    - Higher monthly volatility compared to other debt categories
- This reflects **interest rate sensitivity** and short-term rate movements.
- Despite being considered “safe”, these funds are **not volatility-free** in the short term.

### 🟢 Equity Categories

- Equity categories (Large Cap, Mid Cap, Small Cap, Flexi Cap):
    - Show **moderate monthly returns**
    - Much **lower volatility than daily analysis suggested**
- Monthly aggregation smooths out daily market noise and reflects **true trend-based performance**.

### 🟣 Hybrid & Arbitrage Funds

- **Arbitrage and Conservative Hybrid** funds exhibit:
    - Very low monthly volatility
    - Stable but lower monthly returns
- These categories are suitable for **capital preservation-focused investors**.

### ⚠️ Other – Unclassified

- Still shows irregular behavior
- Indicates mixed or inconsistent schemes
- Less reliable for structured long-term analysis

## 🎯 Key Takeaway

Monthly analysis provides a **more realistic performance view** than daily analysis.

Debt funds may appear volatile daily, but monthly results confirm their **relative stability**, while equity funds demonstrate **controlled risk with consistent growth patterns**.

This step bridges short-term volatility analysis with long-term investment decision-making.


```SQL
WITH monthly_nav AS (
    SELECT scheme_code, mutual_fund_category, DATE_TRUNC('month', nav_date) AS month,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code, DATE_TRUNC('month', nav_date) ORDER BY nav_date DESC) AS month_end_nav
    FROM mutual_fund_nav
),
monthly_returns AS (
    SELECT scheme_code, mutual_fund_category, month,
        (month_end_nav - LAG(month_end_nav) OVER (
            PARTITION BY scheme_code ORDER BY month
        )) / NULLIF(
            LAG(month_end_nav) OVER (
                PARTITION BY scheme_code ORDER BY month
            ), 0
        ) AS monthly_return
    FROM monthly_nav
)
SELECT mutual_fund_category, ROUND(AVG(monthly_return), 6) AS avg_monthly_return, 
ROUND(STDDEV(monthly_return), 6) AS monthly_volatility
FROM monthly_returns
WHERE monthly_return IS NOT NULL
GROUP BY mutual_fund_category
ORDER BY monthly_volatility DESC;
```
<img width="541" height="567" alt="image" src="https://github.com/user-attachments/assets/22bdbc63-6011-4164-89e2-7ad280669138" />
<img width="543" height="518" alt="image" src="https://github.com/user-attachments/assets/919e2015-c3b7-42b3-ae54-a582d7a9edd6" />

# **Which Equity Large Cap mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Large Cap mutual funds over a **5-year investment horizon**, helping investors understand long-term performance dispersion within the same category.

## 🧮 Methodology

- Filtered data to **Equity – Large Cap** category only
- Calculated **5-year return (%)** using NAV growth over a 5-year period
- Ranked schemes based on returns
- Selected:
    - **Top 5** schemes with highest 5-year returns
    - **Bottom 5** schemes with lowest 5-year returns
- Labeled each scheme using a **performance bucket** (`Top 5`, `Bottom 5`)

## 🔍 Key Insights

- There is a **huge performance gap** within the same category
- Top performers delivered **150%+ returns**, while bottom performers showed **very low or negative returns**
- Category-level averages can hide **fund-level underperformance**
- Reinforces why **fund selection matters**, not just category selection

## 🧠 Industry Relevance

- Mirrors how **AMC analysts and portfolio managers** evaluate funds
- Common approach used in **long-term wealth creation analysis**
- Useful for:
    - Investment screening
    - Risk-adjusted decision making
    - Comparing active fund management effectiveness

```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Large Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="1084" height="310" alt="image" src="https://github.com/user-attachments/assets/58d63a1d-5762-4e82-9c98-20ef0a743acf" />

#  **Which Equity Mid Cap mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Mid Cap mutual funds over a **5 year investment horizon**, highlighting the performance spread within the Mid Cap category.

## 🧮 Methodology

- Filtered data to **Equity  Mid Cap** category only
- Calculated **5 year return (%)** using NAV growth over the last 5 years
- Ranked schemes based on their 5 year returns
- Selected:
    - **Top 5** funds with the highest 5 year returns
    - **Bottom 5** funds with the lowest 5 year returns
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for clear comparison

## 🔍 Key Insights

- Even within the **Mid Cap category**, performance varies significantly
- Top performers delivered **200%+ returns** over 5 years
- Bottom performers showed **negative or very low returns**
- This proves that **category selection alone is not sufficient**
- Individual **fund selection plays a critical role** in long-term wealth creation

## 🧠 Industry Relevance

- Reflects how **fund houses and analysts** evaluate mid-cap schemes
- Common approach in **long-term equity performance analysis**
- Helps investors:
    - Identify consistent performers
    - Avoid long-term underperformers
    - Make data-backed investment decisions
```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Mid Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="1075" height="311" alt="image" src="https://github.com/user-attachments/assets/c62a9fbe-ca51-421b-8b05-8d06a9d37228" />

#  **Which Equity Small Cap mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Small Cap mutual funds over a **5 year investment horizon**, highlighting the wide performance spread within the Small Cap category.

## 🧮 Methodology

- Filtered data to **Equity – Small Cap** category only  
- Calculated **5 year return (%)** using NAV growth over the last 5 years  
- Ranked schemes based on their 5 year returns  
- Selected:
  - **Top 5** funds with the highest 5 year returns  
  - **Bottom 5** funds with the lowest 5 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for easy comparison

## 🔍 Key Insights

- Equity Small Cap funds show **extreme performance dispersion**
- Top performers delivered **250%–280%+ returns** over 5 years
- Bottom performers posted **negative returns**, despite being in the same category
- Highlights the **high-risk, high-reward nature** of Small Cap investing
- Strongly reinforces that **fund selection is far more important than category selection**

## 🧠 Industry Relevance

- Reflects how **equity analysts and fund managers** assess Small Cap schemes
- Commonly used in **long-term alpha generation analysis**
- Demonstrates why Small Cap investments require:
  - Longer holding periods
  - Higher risk tolerance
  - Careful fund screening
```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Small Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="1027" height="307" alt="image" src="https://github.com/user-attachments/assets/844a1e76-a311-4617-bf33-a25e1558d96e" />


#  **Which Equity Flexi Cap mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Flexi Cap mutual funds over a **5 year investment horizon**, highlighting performance variation within a flexible equity strategy.

## 🧮 Methodology

- Filtered data to **Equity – Flexi Cap** category only  
- Calculated **5 year return (%)** using NAV growth over the last 5 years  
- Ranked schemes based on their 5 year returns  
- Selected:
  - **Top 5** funds with the highest 5 year returns  
  - **Bottom 5** funds with the lowest 5 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for easy comparison

## 🔍 Key Insights

- Flexi Cap funds show **wide performance dispersion**, despite flexible mandates  
- Top-performing Flexi Cap funds delivered **180%–200%+ returns** over 5 years  
- Bottom performers generated **very low or near-flat returns**  
- Flexibility in allocation does **not automatically guarantee superior performance**  
- Fund manager strategy and timing play a **crucial role in long-term outcomes**

## 🧠 Industry Relevance

- Reflects how **actively managed Flexi Cap funds** are evaluated by analysts  
- Commonly used in **long-term equity fund benchmarking**  
- Highlights the importance of:
  - Manager selection  
  - Consistency across market cycles  
  - Risk-aware allocation decisions  


```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Flexi Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="962" height="314" alt="image" src="https://github.com/user-attachments/assets/a836b4af-ecbe-422e-9e91-12520bb39562" />

#  **Which Debt Liquid mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Debt Liquid mutual funds over a **5 year investment horizon**, and to understand long-term return behavior in a category generally considered low-risk.

## 🧮 Methodology

- Filtered data to **Debt – Liquid** category only  
- Calculated **5 year return (%)** using NAV growth over the last 5 years  
- Ranked schemes based on their 5 year returns  
- Selected:
  - **Top 5** funds with the highest 5 year returns  
  - **Bottom 5** funds with the lowest 5 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for direct comparison

## 🔍 Key Insights

- Liquid funds show **limited upside potential** even over long periods  
- Top-performing Liquid funds delivered **~30%–34% returns** over 5 years  
- Bottom performers show **extreme negative values**, largely due to:
  - IDCW (Dividend payout) structure
  - NAV resets after dividend distributions
- This highlights that **return metrics must be interpreted carefully** for income-oriented funds

## 🧠 Industry Relevance

- Demonstrates why Liquid funds are used primarily for:
  - Short-term parking of funds
  - Liquidity management
  - Capital preservation rather than wealth creation
- Reinforces the importance of:
  - Understanding fund structure (Growth vs IDCW)
  - Using correct performance measures for debt instruments
- Aligns with real-world practices followed by **treasury teams and fixed-income analysts**

```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Liquid'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="1086" height="310" alt="image" src="https://github.com/user-attachments/assets/fbc0bb9a-7db0-4a40-980c-079deaf5eef5" />

# Q10: **Which Debt Corporate Bond mutual funds performed the best and worst over the last 5 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Debt Corporate Bond mutual funds over a **5 year investment horizon**, and to evaluate long-term return consistency within the corporate bond category.

## 🧮 Methodology

- Filtered data to **Debt – Corporate Bond** category only  
- Calculated **5 year return (%)** using NAV growth over the last 5 years  
- Ranked schemes based on their 5 year returns  
- Selected:
  - **Top 5** funds with the highest 5 year returns  
  - **Bottom 5** funds with the lowest 5 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for comparison

## 🔍 Key Insights

- Corporate Bond funds show **moderate return potential** over long periods  
- Top-performing funds delivered **~38%–40% returns** over 5 years  
- Bottom performers reported **negative returns**, mainly due to:
  - IDCW payout structures
  - Credit events and interest rate cycles
- Highlights that even within debt categories, **returns are not uniform**

## 🧠 Industry Relevance

- Reflects how **fixed-income analysts** assess credit-oriented debt funds  
- Common approach in **bond portfolio performance evaluation**  
- Reinforces the need to:
  - Understand credit quality exposure  
  - Differentiate between Growth and IDCW plans  
  - Avoid relying only on category-level averages  

```SQL
WITH fund_5yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Corporate Bond'
      AND nav_date >= CURRENT_DATE - INTERVAL '5 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category, ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_5yr_percent
    FROM fund_5yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_5yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_5yr_percent DESC;
```
<img width="1064" height="309" alt="image" src="https://github.com/user-attachments/assets/08e5122f-c297-4c8a-8fad-8823dd274829" />

#  **Which Equity Large Cap mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Large Cap mutual funds over a **10 year investment horizon**, helping evaluate long-term wealth creation and consistency among large-cap schemes.

## 🧮 Methodology

- Filtered data to **Equity – Large Cap** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for comparison

## 🔍 Key Insights

- Large Cap funds demonstrate **strong long-term compounding potential**
- Top performers delivered **300%–350%+ returns** over 10 years
- Bottom performers significantly underperformed despite being in the same category
- Highlights that **long-term investing reduces risk**, but **fund choice still matters**
- Confirms that Large Cap funds can generate **stable wealth over extended periods**

## 🧠 Industry Relevance

- Mirrors how **long-term investors and wealth managers** evaluate Large Cap funds  
- Commonly used in **retirement planning and portfolio core allocation**
- Reinforces:
  - Importance of holding quality Large Cap funds long-term  
  - Need for periodic fund review and performance benchmarking  


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Large Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```
<img width="1091" height="313" alt="image" src="https://github.com/user-attachments/assets/3a85af07-f51b-4abf-8173-f4e7e2aebdbd" />

#  **Which Equity Mid Cap mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Mid Cap mutual funds over a **10 year investment horizon**, highlighting long-term growth potential and downside risk within the Mid Cap segment.

## 🧮 Methodology

- Filtered data to **Equity – Mid Cap** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for clarity

## 🔍 Key Insights

- Mid Cap funds show **exceptional long-term growth potential**
- Top performers delivered **450%–480%+ returns** over 10 years
- Bottom performers delivered **very low or negative returns**, despite being in the same category
- Indicates **higher dispersion of returns** compared to Large Cap funds
- Confirms that Mid Cap investing offers **high reward with higher risk**

## 🧠 Industry Relevance

- Reflects how **fund managers and long-term equity investors** evaluate Mid Cap funds  
- Commonly used in **wealth creation and aggressive portfolio strategies**
- Highlights:
  - Importance of **long-term holding period**
  - Critical need for **careful fund selection** within Mid Cap category  


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Mid Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```

<img width="1070" height="310" alt="image" src="https://github.com/user-attachments/assets/9a63233a-432a-4a93-a3b3-90220201256e" />

#  **Which Equity Small Cap mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Small Cap mutual funds over a **10 year investment horizon**, highlighting the extreme return dispersion and high-risk, high-reward nature of Small Cap investing.

## 🧮 Methodology

- Filtered data to **Equity – Small Cap** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for comparison

## 🔍 Key Insights

- Small Cap funds show the **widest performance gap** among all equity categories  
- Top performers delivered **500%–575%+ returns** over 10 years  
- Bottom performers delivered **negative returns**, even over a decade  
- Confirms that Small Cap investing is **extremely outcome-dependent**
- High returns come with **significant fund selection risk**

## 🧠 Industry Relevance

- Reflects real-world **small-cap investing behavior**
- Used by:
  - Aggressive long-term investors
  - PMS and high-risk portfolio strategies
- Reinforces why **due diligence and long holding periods** are critical in Small Caps  


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Small Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```

<img width="1042" height="311" alt="image" src="https://github.com/user-attachments/assets/a8ed3166-06d8-4124-8abf-f8a409b7fdde" />

#  **Which Equity Flexi Cap mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Equity Flexi Cap mutual funds over a **10 year investment horizon**, showcasing how flexible allocation strategies impact long-term returns.

## 🧮 Methodology

- Filtered data to **Equity – Flexi Cap** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for clear comparison

## 🔍 Key Insights

- Flexi Cap funds show **wide dispersion in long-term performance**
- Top performers delivered **450%–530%+ returns** over 10 years
- Bottom performers delivered **flat or near-zero returns**, despite being in the same category
- Demonstrates that **manager allocation decisions matter significantly**
- Flexibility does not automatically guarantee superior returns

## 🧠 Industry Relevance

- Mirrors how **fund managers are evaluated** in Flexi Cap strategies
- Useful for assessing:
  - Asset allocation skill
  - Consistency across market cycles
- Reinforces that **flexibility + skill**, not flexibility alone, drives long-term success


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Flexi Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```

<img width="964" height="310" alt="image" src="https://github.com/user-attachments/assets/25773b0a-128d-4f53-b35c-f0c8210f810c" />


#  **Which Debt Liquid mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Debt Liquid mutual funds over a **10 year investment horizon**, highlighting how even low-risk categories can show large performance differences over long periods.

## 🧮 Methodology

- Filtered data to **Debt – Liquid** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for clear comparison

## 🔍 Key Insights

- Despite being considered **low-risk**, Liquid funds show **extreme return dispersion** over long periods
- Top performers delivered **very high cumulative returns**, driven by compounding and institutional growth plans
- Bottom performers showed **negative or near-zero returns**, mainly due to:
  - IDCW payout structures
  - Regular income distribution reducing NAV growth
- Highlights that **return structure matters as much as category**

## 🧠 Industry Relevance

- Reflects real-world analysis done by **treasury teams and institutional investors**
- Demonstrates why **growth vs dividend plans** must be analyzed separately
- Reinforces that **Liquid funds are not interchangeable** over long-term horizons


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Liquid'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```


<img width="973" height="310" alt="image" src="https://github.com/user-attachments/assets/0b3df444-3c2e-4494-9dad-6b3b1e07606d" />

#  **Which Debt Corporate Bond mutual funds performed the best and worst over the last 10 years?**

## 🎯 Objective

To identify **top-performing** and **underperforming** Debt Corporate Bond mutual funds over a **10 year investment horizon**, and understand long-term performance variation within corporate bond investments.

## 🧮 Methodology

- Filtered data to **Debt – Corporate Bond** category only  
- Calculated **10 year return (%)** using NAV growth over the last 10 years  
- Ranked schemes based on their 10 year returns  
- Selected:
  - **Top 5** funds with the highest 10 year returns  
  - **Bottom 5** funds with the lowest 10 year returns  
- Assigned a **performance bucket** (`Top 5`, `Bottom 5`) for comparison

## 🔍 Key Insights

- Corporate Bond funds show **clear separation** between growth-oriented and income-oriented plans
- Top performers delivered **100%+ returns** over 10 years
- Bottom performers recorded **negative or very low returns**, mainly due to:
  - Monthly / Quarterly dividend (IDCW) payout structures
  - Lower reinvestment compounding
- Indicates that **plan structure significantly impacts long-term outcomes**

## 🧠 Industry Relevance

- Mirrors analysis done by **fixed income portfolio managers**
- Highlights importance of choosing **Growth plans** for long-term investors
- Demonstrates why Corporate Bond funds should be evaluated beyond “low risk” labels


```SQL
WITH fund_10yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Corporate Bond'
      AND nav_date >= CURRENT_DATE - INTERVAL '10 years'
),
returns AS (
    SELECT scheme_code, scheme_name,mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_10yr_percent
    FROM fund_10yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_10yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_10yr_percent DESC;
```

<img width="1262" height="310" alt="image" src="https://github.com/user-attachments/assets/21c65070-0f43-403f-871e-ac1b08d910f6" />


##  Which Equity Large Cap mutual funds performed the best and worst over the last 15 years?

### Objective
Identify top and bottom performing Equity Large Cap mutual funds over a 15-year horizon to evaluate long-term wealth creation potential.

### Methodology
- Filtered data to Equity – Large Cap category
- Calculated 15-year return (%) using NAV growth
- Ranked schemes by cumulative return
- Selected Top 5 and Bottom 5 performers
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Massive performance divergence exists even within Large Cap funds
- Top performers delivered over 600% returns in 15 years
- Bottom performers generated very low or negative returns
- Highlights importance of fund-level selection, not just category exposure

### Industry Relevance
- Aligns with long-term investment and retirement planning analysis
- Common evaluation method used by asset managers and institutional investors
- Reinforces the role of active fund selection in Large Cap investing

```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Large Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```

<img width="1037" height="307" alt="image" src="https://github.com/user-attachments/assets/7ea6ac43-fb6e-46da-a500-e23bad19a7fc" />


##  Which Equity Mid Cap mutual funds performed the best and worst over the last 15 years?

### Objective
Evaluate long-term performance of Equity Mid Cap mutual funds by identifying top and bottom performers over a 15-year period.

### Methodology
- Filtered data to Equity – Mid Cap category
- Calculated 15-year return (%) using NAV growth
- Ranked schemes based on cumulative returns
- Selected Top 5 and Bottom 5 funds
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Equity Mid Cap funds exhibit extreme return dispersion
- Top funds delivered 800%–1100%+ returns over 15 years
- Bottom funds produced weak or negative returns
- Highlights higher risk–reward nature of Mid Cap investing
- Reinforces importance of fund-level selection

### Industry Relevance
- Aligns with long-term equity analysis practices
- Useful for SIP planning, retirement modeling, and portfolio construction
- Demonstrates why Mid Cap funds require careful monitoring and selection


```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Mid Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```

<img width="923" height="311" alt="image" src="https://github.com/user-attachments/assets/05b8c032-0aa1-480c-9e82-1a87b440c005" />


##  Which Equity Small Cap mutual funds performed the best and worst over the last 15 years?

### Objective
Analyze long-term performance extremes within the Equity – Small Cap category over a 15-year horizon.

### Methodology
- Filtered data to Equity – Small Cap funds
- Calculated 15-year return (%) using NAV growth
- Ranked schemes based on cumulative returns
- Selected Top 5 and Bottom 5 funds
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Equity Small Cap funds exhibit extreme long-term volatility in returns
- Top performers generated 1300%–1500%+ returns
- Bottom performers delivered negative returns even over 15 years
- Highlights high-risk, high-reward nature of Small Cap investing
- Strongly reinforces importance of fund-level selection

### Industry Relevance
- Aligns with long-term equity and small-cap investment analysis
- Useful for aggressive portfolio construction and SIP strategies
- Demonstrates downside risk of poor Small Cap fund selection

```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Small Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```
<img width="1040" height="310" alt="image" src="https://github.com/user-attachments/assets/14a8c599-7f59-4381-823a-67d8eb9aa682" />


##  Which Equity Flexi Cap mutual funds performed the best and worst over the last 15 years?

### Objective
Identify long-term top and bottom performers within the Equity – Flexi Cap category over a 15-year horizon.

### Methodology
- Filtered data to Equity – Flexi Cap funds
- Calculated 15-year return (%) using NAV growth
- Ranked schemes by cumulative returns
- Selected Top 5 and Bottom 5 funds
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Flexi Cap funds exhibit significant performance dispersion
- Top funds delivered 650%–850%+ returns over 15 years
- Bottom funds showed near-zero or negative returns
- Demonstrates that flexibility alone does not ensure performance
- Fund manager decisions play a critical role

### Industry Relevance
- Mirrors professional Flexi Cap fund evaluation practices
- Useful for balanced and long-term portfolio construction
- Highlights importance of fund-level selection over category-level choice


```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Equity - Flexi Cap'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```

<img width="973" height="315" alt="image" src="https://github.com/user-attachments/assets/b03be275-b792-451f-a93b-4fd12b979fb4" />



##  Which Debt Liquid mutual funds performed the best and worst over the last 15 years?

### Objective
Evaluate long-term performance variation within the Debt – Liquid category over a 15-year horizon.

### Methodology
- Filtered data to Debt – Liquid funds
- Calculated 15-year return (%) using NAV growth
- Ranked schemes by cumulative returns
- Selected Top 5 and Bottom 5 funds
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Liquid funds exhibit extreme return divergence over long horizons
- Top funds show 27,000%+ returns due to long-term compounding
- Bottom funds reflect fund closures, IDCW payouts, or unclaimed redemptions
- Absolute long-term returns are misleading for liquid funds

### Industry Relevance
- Confirms that liquid funds are not designed for long-term return comparison
- Used primarily for liquidity management and short-term parking
- Reinforces importance of context-aware performance evaluation

```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Liquid'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```

<img width="1090" height="310" alt="image" src="https://github.com/user-attachments/assets/75404302-c0ac-4bae-8687-d66ff547e048" />


##  Which Debt Corporate Bond mutual funds performed the best and worst over the last 15 years?

### Objective
Analyze long-term performance dispersion within the Debt – Corporate Bond category over a 15-year horizon.

### Methodology
- Filtered data to Debt – Corporate Bond funds
- Calculated 15-year return (%) using NAV growth
- Ranked schemes based on cumulative returns
- Selected Top 5 and Bottom 5 funds
- Assigned performance buckets (Top 5, Bottom 5)

### Key Insights
- Corporate bond funds show strong long-term compounding effects
- Top performers delivered 19,000%–34,000%+ returns
- Bottom performers largely reflect IDCW and payout-based plans
- Growth plan selection significantly impacts long-term outcomes

### Industry Relevance
- Mirrors long-term corporate bond fund evaluation methods
- Highlights importance of reinvestment in debt instruments
- Useful for fixed-income portfolio construction decisions

```SQL
WITH fund_15yr_returns AS (
    SELECT DISTINCT scheme_code, scheme_name, mutual_fund_category,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date) AS start_nav,
    FIRST_VALUE(nav) OVER (PARTITION BY scheme_code ORDER BY nav_date DESC) AS end_nav
    FROM mutual_fund_nav
    WHERE mutual_fund_category = 'Debt - Corporate Bond'
      AND nav_date >= CURRENT_DATE - INTERVAL '15 years'
),
returns AS (
    SELECT scheme_code, scheme_name, mutual_fund_category,
    ROUND(((end_nav / start_nav) - 1) * 100, 2) AS return_15yr_percent
    FROM fund_15yr_returns
    WHERE start_nav > 0
)
(
    SELECT *, 'Top 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent DESC
    LIMIT 5
)
UNION ALL
(
    SELECT *, 'Bottom 5' AS performance_bucket
    FROM returns
    ORDER BY return_15yr_percent ASC
    LIMIT 5
)
ORDER BY performance_bucket, return_15yr_percent DESC;
```

<img width="1180" height="310" alt="image" src="https://github.com/user-attachments/assets/1e806769-77ea-43c8-b501-fe48231555c7" />


# 🏁 Final Conclusion

**This project presents a structured, long-horizon analysis of Indian mutual fund performance, moving deliberately from short-term noise to long-term investment outcomes.**

We began with a daily-level analysis to understand short-term NAV fluctuations and volatility. While daily movements provided insight into market noise and category behavior, they were not used to judge performance, as daily data alone does not reflect real investment outcomes.

To address this, we transitioned to monthly aggregation, which reduced short-term volatility and provided a clearer, more practical view of fund performance. This step acted as a bridge between raw daily data and meaningful long-term evaluation.

The core of the analysis focused on **long-term performance across 5-year, 10-year, and 15-year investment horizons**.

Over a **5-year period**, equity-oriented categories such as Large Cap and Mid Cap showed steady growth, establishing a baseline for medium-term investing.

As the horizon expanded to **10 years**, returns increased noticeably across most equity categories, reflecting the benefits of staying invested through market cycles.

By the **15-year horizon**, the effect of compounding became clearly visible, offering a strong long-term picture of wealth creation and revealing which categories consistently delivered superior outcomes over time.

This phased approach demonstrates that while short-term volatility is unavoidable, long-term investment performance is driven by time, consistency, and compounding. By combining daily diagnostics with multi-year return analysis, this project mirrors how real-world investors and fund managers evaluate mutual funds in practice.

