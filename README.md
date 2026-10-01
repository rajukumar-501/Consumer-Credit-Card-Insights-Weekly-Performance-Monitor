# 📊 Credit Card Analytics | End-to-End Data Analytics Project

This project demonstrates an **end-to-end data analytics workflow** built to solve real-world **credit card business problems** using SQL, DAX, and Power BI.

The focus of this project is **business decision support**, not just dashboard creation.

---

## 🎯 Business Objective

To develop a **weekly credit card performance dashboard** that enables stakeholders to:
- Monitor revenue and growth trends
- Understand customer behavior and segmentation
- Identify early risk signals
- Support data-driven operational and strategic decisions

---

## 🗂 Data Overview

The project uses structured credit card and customer data containing:
- Transaction amount, interest, fees, utilization
- Card category, transaction type, activation status
- Customer demographics, income, education, occupation
- Geographic and behavioral attributes

Raw data was provided in CSV format.

---

## 🛠 Tools & Technologies

- **Excel** – Initial data review and validation  
- **SQL** – Database creation, table design, data loading  
- **DAX** – Business logic, segmentation, time-based calculations  
- **Power BI** – Interactive dashboards and analytics  
- **PowerPoint** – Insight presentation and storytelling  

---

## 🗄 Data Engineering (SQL)

SQL was used to convert raw CSV files into a **structured, analytics-ready database**.

### Key SQL Work:
- Created a dedicated database for the project
- Designed separate tables for:
  - Credit card transaction data
  - Customer demographic and profile data
- Defined appropriate data types for financial, date, and categorical fields
- Loaded CSV files into SQL tables using bulk load operations
- Validated data using row counts after loading

### Example SQL Operations Used:
```sql
CREATE DATABASE ccdb;
USE ccdb;

CREATE TABLE cc_detail (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);

CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,

## 🧠 DAX Measures – Business Logic Explained

This section documents the **actual DAX measures used in the project**, as implemented in Power BI.  
These measures form the analytical backbone of the dashboards and enable revenue tracking, time-based comparison, and growth analysis.

---

### 1️⃣ Week Number (Calculated Column)

```DAX
week_num2 = WEEKNUM('ccdb cc detail'[Week_Start_Date])


## DAX QUERY

AgeGroup =
SWITCH(
    TRUE(),
    'public cust detail'[customer_age] < 30, "20-30",
    'public cust detail'[customer_age] >= 30 && 'public cust detail'[customer_age] < 40, "30-40",
    'public cust detail'[customer_age] >= 40 && 'public cust detail'[customer_age] < 50, "40-50",
    'public cust detail'[customer_age] >= 50 && 'public cust detail'[customer_age] < 60, "50-60",
    'public cust detail'[customer_age] >= 60, "60+",
    "Unknown"
)

## DAX QUERY

IncomeGroup =
SWITCH(
    TRUE(),
    'public cust detail'[income] < 35000, "Low",
    'public cust detail'[income] >= 35000 && 'public cust detail'[income] < 70000, "Medium",
    'public cust detail'[income] >= 70000, "High",
    "Unknown"
)

## DAX QUERY REVENUE

Revenue =
'ccdb cc detail'[Annual_Fees] +
'ccdb cc detail'[Total_Trans_Amt] +
'ccdb cc detail'[Interest_Earned]

##DAX QUERY CURRENT WEEK MEASURE

Current_week_revenue =
CALCULATE(
    SUM('ccdb cc detail'[Revenue]),
    FILTER(
        ALL('ccdb cc detail'),
        'ccdb cc detail'[week_num2] =
        MAX('ccdb cc detail'[week_num2])
    )
)

## DAX QUERY PREVIOUS WEEK MEASURE

Previous_week_revenue =
CALCULATE(
    SUM('ccdb cc detail'[Revenue]),
    FILTER(
        ALL('ccdb cc detail'),
        'ccdb cc detail'[week_num2] =
        MAX('ccdb cc detail'[week_num2]) - 1
    )
)


## DAX QUERY WOW(week of week) revenue

wow_revenue =
DIVIDE(
    ([Current_week_revenue] - [Previous_week_revenue]),
    [Previous_week_revenue]
)


isfaction_Score INT
);
