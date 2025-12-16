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
