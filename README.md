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
## 🔍 Analysis Question 1: Mutual Fund Category Distribution

### Question
How many mutual fund schemes are available in each category such as Large Cap, Mid Cap, Small Cap, Debt, and other categories?

### Why this analysis is needed
Before analyzing performance, it is important to understand how mutual funds are distributed across categories. Funds from different categories follow different investment strategies and risk levels, so comparisons should always be made within the same category.

### What we expect to find
- Total number of schemes in each category  
- Categories with the highest and lowest number of funds  
- Presence of niche categories such as Gold or Silver funds  

This analysis forms the foundation for further category-wise performance analysis.

## 🔍 Analysis Question 1: Mutual Fund Category and Scheme Count

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

## 🔹 STEP 3: Paste YOUR CODE (READY TO COPY)

Just copy **everything below** and paste it into `README.md`:

```markdown
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



