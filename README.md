# Customer Churn Analysis

![](intro.JPG)

## Introduction:
This Customer Churn Analysis was developed using **Power BI** to gain insights into the customer churn behavior of a bank. The goal was to identify key patterns of churn across various customer segments, such as credit types, gender, and geographic locations, and provide actionable recommendations for reducing churn and improving retention.

## Tools & Techniques Used:
- **Tool:** Power BI
- **Data Source:** I utilized a fact table (Bank dataset) that contained transactional and customer data, as well as dimension tables for customer demographics, credit types, and location.
- **Data Modeling:** Relationships were established between the fact and dimension tables. A lookup (calendar) table was created for time intelligence, allowing the analysis of trends across months and years.
- **Data Transformation (Power Query):**
  - Removed irrelevant columns that were not essential for the analysis to optimize performance.
  - Checked for missing values and cleaned the data to ensure accuracy.
  - Created new columns to ensure the correct sorting of fields, such as months appearing in the correct order in visualizations.
- **DAX Measures:** I implemented DAX (Data Analysis Expressions) to create custom measures and calculated columns. For example, I used DAX to group customers by their credit type, enabling segmentation by credit quality (poor, fair, good, very good, excellent).

## DAX Formulas Used in Creating Measures and Calculated Columns
- Active Members:
<pre>
Active Members = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Active customers'[ActiveCategory] = "Active Member")
  </pre>

- Inactive Members:
<pre>
Inactive Members = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Active customers'[ActiveCategory] = "Inactive Member")
  </pre>

- Churned Customers:
<pre>
Churned Customers = CALCULATE(COUNT(CustomerInfo[CustomerId]), 'Customer Exit'[ExitCategory] = "Exit")
  </pre>

- Last Month's Churned Customers:
<pre>
Last Month's Churned Customers = CALCULATE([Churned Customers], PREVIOUSMONTH('Calendar'[Date]))
  </pre>

- Credit Card Holders:
<pre>
Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Credit card'[Category] = "credit card holder")
  </pre>

- Non Credit Card Holders:
<pre>
Non Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Credit card'[Category] = "non credit card holder")
  </pre>

- Total Customers:
<pre>
Total Customers = COUNT(Bank_Churn[CustomerId])
  </pre>  

- Retain Customers:
<pre>
Retain Customers = [Total Customers] - [Churned Customers]
  </pre>

- Churn %:
<pre>
Churn % = 
var EC = [Churned Customers]
var TC = [Total Customers]
var Churnper = DIVIDE(EC,TC)
return Churnper
  </pre>

- Credit Type:
<pre>
Credit Type = SWITCH(TRUE(), 
Bank_Churn[CreditScore] >= 800 && Bank_Churn[CreditScore] <= 850, "Excellent", 
Bank_Churn[CreditScore] >= 740 && Bank_Churn[CreditScore] <= 799, "Very Good", 
Bank_Churn[CreditScore] >= 670 && Bank_Churn[CreditScore] <= 739, "Good", 
Bank_Churn[CreditScore] >= 580 && Bank_Churn[CreditScore] <= 669, "Fair", 
Bank_Churn[CreditScore] >= 300 && Bank_Churn[CreditScore] <= 579, "Poor")
  </pre>

## Model

![](images/model.JPG)

## Dashboard

![](images/dashboard1.JPG)

![](images/dashboard2.JPG)
