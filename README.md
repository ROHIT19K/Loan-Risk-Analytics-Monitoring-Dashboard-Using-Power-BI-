# 📊 Loan Risk Analytics & Monitoring Dashboard

Live Dashboard Link : 
(https://app.powerbi.com/groups/21a65e37-284d-4372-b71a-df8a0d23ba50/reports/b2114313-d728-4f8b-b146-474928be5c8e/8f51836f02cc36b28ed6?experience=power-bi)

End-to-End Power BI Business Intelligence Project leveraging SQL Server (SSMS), Power BI Dataflows, DAX, Incremental Refresh, and Scheduled Refresh to analyze loan performance, borrower demographics, and default risk patterns.

## 📌 Project Overview

The Enterprise Loan Risk Analytics & Monitoring Dashboard is an end-to-end Business Intelligence project designed to analyze loan performance, borrower behavior, credit risk, and default trends.

The project follows a modern BI architecture where raw loan data is first stored and managed in Microsoft SQL Server (SSMS). The data is then integrated into Power BI Service using Dataflows, transformed into an optimized analytical model, and connected to Power BI Desktop for advanced reporting and dashboard development.

This solution helps financial institutions monitor loan portfolios, identify default risks, analyze borrower demographics, and support data-driven lending decisions.

## 🎯 Business Problem

Financial institutions process thousands of loan applications and transactions daily. Managing loan portfolios efficiently requires continuous monitoring of:

* Loam Amount By Purpose 
* Average Income By Employment Type
* Default Rate(%) By Employment Type
* Average Loan By Age Group
* Default Rate(%) By Year
* YOY Loan Amount Change By Year
* YOY Default Loans Change By Year

## 🛠 Tools & Technologies

📌 Database Layer
Microsoft SQL Server (SSMS)
SQL Queries
Database Modeling

📌 Data Integration
Power BI Service
Power BI Dataflows
Power Query

📌 Data Visualization
Power BI Desktop
Power BI Service
DAX

📌 Enterprise Features
Scheduled Refresh
Incremental Refresh
Cloud Deployment
Data Gateway Integration



## 📂 Dataset Information

📌 Loan Default Dataset

📌 Key Features

* Applicant Information
* Age
* Marital Status
* Employment Type
* Education Level
* Income
* Loan Information
* Loan Amount
* Loan Purpose
* Loan Status
* Loan Year
* Risk Indicators
* Credit Score
* Credit Score Category
* Default Status
* Mortgage Status
* Dependents
* Financial Metrics
* Income Bracket
* Default Rate
* Loan Growth Rate

## 📌 DAX Measure And Calculation 

*       Average Income By Employment Type = 
        CALCULATE(AVERAGE('Loan_default'[Income]),ALLEXCEPT('Loan_default','Loan_default'[EmploymentType]))

*      Average Loan By Age Group =  AVERAGEX(VALUES(Loan_default[Age Groups]),
                                    AVERAGE('Loan_default'[LoanAmount]))


*       Default Rate By Employment Type = 

        Var Totalrecords = COUNTROWS(ALL('Loan_default'))

        Var DefaultCases = COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE()))

        RETURN
        CALCULATE(DIVIDE(DefaultCases, Totalrecords),ALLEXCEPT('Loan_default','Loan_default'[EmploymentType])) * 100


*           Default Rate By Year = 
            Var TotalLoans = CALCULATE(COUNTROWS('Loan_default'),ALLEXCEPT('Loan_default','Loan_default'[Year]))

            Var Default = CALCULATE(COUNTROWS(FILTER('Loan_default', 'Loan_default'[Default]=TRUE())),ALLEXCEPT('Loan_default',Loan_default[Year]))

            RETURN 
            DIVIDE(Default, TotalLoans) * 100 


*      Loan Amount By Purpose = 
       SUMX(FILTER('Loan_default',NOT(ISBLANK ('Loan_default'[LoanAmount]))),'Loan_default'[LoanAmount])


*        Average Loan Amount (High Credit Score) = 
         AVERAGEX(FILTER('Loan_default','Loan_default'[Credit Score Bins]="High"),'Loan_default'[LoanAmount])

*     Loan By Education Type = 
      COUNTROWS(FILTER('Loan_default',NOT(ISBLANK('Loan_default'[LoanID]))))


*     Median By Credit Score Bins = MEDIANX('Loan_default','Loan_default'[LoanAmount])


*     Total Loan (Credit Bins) = 
      CALCULATE(SUM('Loan_default'[LoanAmount]),'Loan_default'[Age Groups]="Adults", ALLEXCEPT('Loan_default','Loan_default'[Age Groups],'Loan_default'[Age],'Loan_default'[CreditScore],'Loan_default'[Credit Score Bins]))

*     Total Loan Amt (Middle Aged Adults) = 
      SUMX(FILTER('Loan_default','Loan_default'[Age Groups]="Middle Age Adults"),'Loan_default'[LoanAmount])

*        Year to Date Loan Amount = 
         CALCULATE(SUM('Loan_default'[LoanAmount]),DATESYTD('Loan_default'[Loan_Date_DD_MM_YYYY].[Date]),ALLEXCEPT('Loan_default',Loan_default[Credit Score Bins],Loan_default[MaritalStatus]))


*     YOY Default Loans Change = 
      DIVIDE(
      CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))) - 
      CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1)
      ,CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),0) * 100

*     YOY Loan Amt Change = 
      DIVIDE(
      CALCULATE(SUM('Loan_default'[LoanAmount]),'Loan_default'[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))) - 
      CALCULATE(SUM('Loan_default'[LoanAmount]),Loan_default[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1)
      ,CALCULATE(SUM('Loan_default'[LoanAmount]),Loan_default[Year]=YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))-1),0) * 100

## 📌 Calculated Columns 

*     Age Groups = 
            IF('Loan_default'[Age]<=19,"Teen",
                IF('Loan_default'[Age]<=39,"Adults",
                    IF('Loan_default'[Age]<=59,"Middle Age Adults","Senior Citizens")))


*      Credit Score Bins = IF('Loan_default'[CreditScore]<=400,"Very Low",
                                IF(Loan_default[CreditScore]<=450, "Low",
                                    IF(Loan_default[CreditScore]<=650,"Medium","High")))

*       Income Bracket = SWITCH(
            TRUE(),
            'Loan_default'[Income]<30000,"Low Income",
            'Loan_default'[Income]>=30000 && 'Loan_default'[Income]<60000,
            "Medium Income",
            'Loan_default'[Income]>=60000,"High")

*      Year = YEAR('Loan_default'[Loan_Date_DD_MM_YYYY].[Date])

## 📊 Loan Portfolio Overview

👥 Applicant Demographics Dashboard

⚠️ Financial Risk Dashboard

## 🔍 Key Insights

📌 Page 1: Loan Default & Overview

* Loan amount is highest for Home loans (≈6545M), followed by Business and Education loans.
* Full-time employees have the highest average income, while unemployed customers have the lowest.
* Default rate is highest among unemployed applicants (3.39%) and lowest among full-time employees (2.36%).
* Adults receive the highest average loan amount, whereas Teen applicants receive the lowest.
* The default rate remained relatively stable around 11–12%, peaking in 2016 (11.75%) before declining.


📌 Page 2: Applicant Demographics & Financial Profile

* Customers with Low credit score bins have the highest median loan amount, while High credit score bins show comparatively lower median values.
* High-credit customers across different age groups and marital statuses have similar average loan amounts (~127K–128K).
* Adult customers contribute the largest total loan amount, while Low credit score bins contribute the least.
* For middle-aged adults, the presence of mortgage dependents has minimal impact on total loan amount.
* Loan counts are highest among applicants with a Bachelor's degree, followed by High School education.

📌 Page 3: Financial Risk Metrics

* Year-over-Year loan amount experienced fluctuations, with strong positive growth in 2015 and 2018.
* Default loans also showed significant YoY volatility, indicating changing credit risk conditions.
* High-income customers contribute the largest share of total income (≈17.88bn), while * Low-income customers contribute comparatively less.
* Most low-income customers are distributed across part-time, unemployed, and self-employed employment categories.
* Medium and High credit score bins account for a substantial share of Year-to-Date loan amounts across different marital statuses.

## 🚀 Future Enhancements

Real-time loan monitoring

Predictive default risk modeling

Azure SQL Integration

Machine Learning risk scoring

Power BI Row-Level Security (RLS)

Automated alerting system


## ▶️ Project Workflow

Data Collection

SQL Server Data Storage

Power BI Dataflow Creation

Data Cleaning & Transformation

Data Modeling

DAX Development

Dashboard Development

Power BI Service Deployment

Scheduled Refresh Configuration

Incremental Refresh Implementation

Business Insights Generation


## ✅ Results & Outcomes

* Built an enterprise-grade Power BI solution
* Implemented SQL Server + Dataflow architecture
* Automated dataset refresh processes
* Created advanced DAX-based KPIs
* Delivered actionable loan risk insights
* Reduced manual reporting efforts

#📬 Contact

## 👤 Author

Your Name

LinkedIn: https://linkedin.com/in/yourprofile

GitHub: https://github.com/yourusername

Email: yourmail@gmail.com

⭐ If You Like This Project

If you found this project useful, please consider giving it a ⭐ on GitHub!                     
