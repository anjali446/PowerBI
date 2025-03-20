# Adventure Works - Customer Retention & Acquisition Analysis

## Project Overview
This project focuses on analyzing **customer retention, first purchase trends, and future customer acquisition** using **Microsoft Power BI** for **Adventure Works**. The company has been successful in acquiring customers but struggles with **retaining them beyond the first purchase**. Using **cohort analysis, decomposition trees, and forecasting**, I have provided actionable insights to help **improve retention strategies and optimize marketing efforts**.

---

## Approach & Methodology

### 1. Setting Up the Power BI Report
- Loaded the **Adventure Works dataset** and ensured correct data formatting:
  - **Purchase Date** → Month-Year format (e.g., *March 2021*).
  - **Sales Amount & Income** → Currency format (2 decimal places).
  - **Region** → Categorized as **Continent**.

---

### 2. Cohort Analysis for Customer Retention
**Goal:** Understand customer retention trends based on the first purchase month.

#### Key Steps:
 - **Determine each customer's First Purchase Date**
```DAX
First Purchase Date = 
VAR CurrentCustomer = Sales[Customer ID]
RETURN 
    CALCULATE(
        EOMONTH(MIN(Sales[Purchase Date]), 0),
        FILTER(Sales, Sales[Customer ID] = CurrentCustomer)
    )
```
- **Create a table to track Months After Purchase**
```DAX
Months After Purchase = GENERATESERIES(0, 12, 1)
```
- **Calculate Customer Retention**
```DAX
Customer Retention = 
VAR CurrentMonthAfter = SELECTEDVALUE('Months After Purchase'[Value])
VAR CurrentFirstPurchaseMonth = SELECTEDVALUE(Sales[First Purchase Date])
RETURN
    CALCULATE(
        DISTINCTCOUNT(Sales[Customer ID]),
        FILTER(
            Sales,
            EOMONTH(Sales[Purchase Date], 0) = EOMONTH(CurrentFirstPurchaseMonth, CurrentMonthAfter)
        )
    )
```

- **Visualized cohort data using a Matrix in Power BI**

####  Findings:
 - **December 2021 had the highest number of first-time purchases (36.68%)**.
 - **From Dec 2021 – Dec 2022, 584,336 customers made their first purchase**.
 - **Retention drops significantly after the first three months**, highlighting engagement issues.

---

### 3. Customer Retention Rate Analysis
**Goal:** Measure retention performance over time across different cohorts.

- **Calculate Customer Retention Rate (%)**
```DAX
Customer Retention Rate % = 
VAR CurrentMonthAfter = SELECTEDVALUE('Months After Purchase'[Value])
VAR CurrentFirstPurchaseMonth = SELECTEDVALUE(Sales[First Purchase Date])
RETURN
    DIVIDE(
        CALCULATE(
            DISTINCTCOUNT(Sales[Customer ID]),
            FILTER(Sales, EOMONTH(Sales[Purchase Date], 0) = EOMONTH(CurrentFirstPurchaseMonth, CurrentMonthAfter))
        ),
        DISTINCTCOUNT(Sales[Customer ID])
    )
```

####  Findings:
 - **The December 2021 cohort had the highest retention rate in the first six months (65.43%)**.
 - **January 2022 had lower retention rates compared to December 2021**, indicating a **seasonal effect**.
 - **Retention rates drop significantly after three months**, highlighting **engagement issues**.

---

### 4. First Purchase Analysis with a Decomposition Tree
**Goal:** Identify key factors influencing first purchases.

- **Analyzed First Purchase trends by:**
  - **Region**
  - **Gender**
  - **Product**
  - **Customer Segment**

####  Findings:
 - **Asia had the highest number of first purchases (195,901 customers)**.
 - **Road Bikes were the most popular first purchase product (73,101 sales)**.
 - **Female customers had more first purchases (54,127) than males (18,974)**.
 - **Retail Stores drove more first purchases (42,464) than Individual Customers (11,663)**.

---

### 5. Forecasting Future Customer Acquisitions
**Goal:** Predict customer acquisition trends for the next 12 months.

- **Created a Line Chart to visualize historical First Purchase trends**
- **Applied Power BI Forecasting settings:**
  - **Forecast Length:** 12 months
  - **Confidence Interval:** 95%
  - **Seasonality:** 10 points

#### Findings:
 - **Expected First Purchases for April 2023** → Slight increase compared to previous months.
 - **Expected First Purchases for December 2023** → Forecast suggests a **peak in customer acquisition**.
 - **Confidence Intervals show uncertainty**, meaning **external factors (marketing campaigns, economic trends) may impact acquisition rates**.

---

##  Key Business Insights
 - **Customer retention is low after the first 3 months** → **Need better post-purchase engagement strategies**.
 - **December 2021 had the highest first-time purchases** → **Seasonal marketing campaigns are effective**.
 - **Asia is the strongest market** → **Investment in localized marketing & promotions is crucial**.
 - **Retail Stores drive more first purchases than Individual Customers** → **B2B partnerships should be expanded**.
 - **Road Bikes are the most popular product** → **Opportunity to bundle accessories & offer discounts**.

---

##  Conclusion & Recommendations
This **Power BI report** provides **critical insights** for **Adventure Works** to enhance **customer retention, optimize marketing strategies, and drive long-term growth**. By leveraging **cohort analysis, segmentation, and forecasting**, I have identified key areas for improvement, including **post-purchase engagement, regional marketing strategies, and retail partnerships**.

---
