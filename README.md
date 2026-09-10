# Credit Card Financial Analytics Dashboard 💳📊

An end-to-end Power BI dashboard built on a SQL data pipeline to analyze credit card customer behavior, weekly transaction trends, and revenue drivers — enabling stakeholders to monitor key financial and operational KPIs in real time.

---

## 📌 Project Objective

To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

## 🗂️ Dataset

- **10,108 unique customers** with weekly transaction records across FY2023 (Week 1 – Week 53)
- Two source tables:
  - `customer` — demographics: age, gender, income, education, marital status, job, state, satisfaction score
  - `credit_card` — transaction-level data: card category, credit limit, revolving balance, transaction amount/count, utilization ratio, interest earned, delinquency flag
- Data loaded into a **PostgreSQL** database via SQL scripts (`SQL_Query_-_Financial_Dashboard_Data.sql`) before being connected to Power BI

## 🛠️ Tools & Tech Stack

| Tool | Purpose |
|---|---|
| SQL (PostgreSQL) | Data staging, table creation, bulk import |
| Power BI | Data modeling, DAX measures, dashboard/report design |
| DAX | Calculated columns & measures for segmentation and WoW analysis |
| Excel/CSV | Raw source files |

## ⚙️ Data Modeling & Key DAX Measures

```dax
AgeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[customer_age] < 30, "20-30",
    'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40",
    'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50",
    'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60",
    'public cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)

IncomeGroup = SWITCH(
    TRUE(),
    'public cust_detail'[income] < 35000, "Low",
    'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Med",
    'public cust_detail'[income] >= 70000, "High",
    "unknown"
)

Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]

Current_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(ALL('public cc_detail'), 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]))
)

Previous_week_Revenue = CALCULATE(
    SUM('public cc_detail'[Revenue]),
    FILTER(ALL('public cc_detail'), 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2]) - 1)
)
```

These power the **week-over-week (WoW) revenue tracking** and **customer segmentation** views across the report.

## 📊 Dashboard Pages

1. **Credit Card Customer Report** — revenue by age group, gender, marital status, education, salary/income group, dependent count, and top 5 states
2. **Credit Card Transaction Report** — quarterly revenue & transaction count trend, revenue by card category, expenditure type (bills, fuel, grocery, travel, etc.), revenue by chip usage (swipe/chip/online), and customer acquisition cost by card tier
3. **Weekly Status Report** — WoW KPI snapshot for stakeholder reporting

## 🔍 Key Insights (Week 53, YTD)

- Overall YTD revenue: **₹57M**, total interest earned: **₹8M**, total transaction amount: **₹46M**
- Revenue grew **28.8% WoW** in the final week of the year
- Male customers contributed more to revenue (**₹31M**) than female customers (**₹26M**)
- **Blue & Silver** cards drove **93%** of total transaction volume
- **TX, NY & CA** together contributed **68%** of revenue
- Overall card **activation rate: 57.5%**; **delinquency rate: 6.06%**

## 🚀 How to Use

1. Clone this repo
2. Run `SQL_Query_-_Financial_Dashboard_Data.sql` against a PostgreSQL instance to create and populate the tables
3. Open `Credit_Card_Financial_Dashboard.pbix` in Power BI Desktop
4. Update the data source connection to point to your SQL instance
5. Refresh to explore the live dashboard

## 📁 Repository Structure

```
├── data/
│   ├── customer.csv
│   ├── credit_card.csv
│   ├── cust_add.csv
│   └── cc_add.csv
├── sql/
│   └── SQL_Query_-_Financial_Dashboard_Data.sql
├── dashboard/
│   └── Credit_Card_Financial_Dashboard.pbix
├── reports/
│   ├── Credit_Card_Financial_Dashboard-Customer.pdf
│   ├── Credit_Card_Financial_Dashboard-Transaction.pdf
│   └── Credit_Card_Financial_Weekly_Dashboard_Report.pdf
└── README.md
```

---

**Author:** Anshikha Chaurasiya
[GitHub](https://github.com/anshikhachaurasiya) · [LinkedIn](https://linkedin.com/in/anshikha-chaurasiya-24681328a)
