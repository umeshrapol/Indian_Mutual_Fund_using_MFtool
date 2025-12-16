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
