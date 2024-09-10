# Customer Churn Analysis

![](intro.JPG)

## Introduction:
This Customer Churn Analysis was developed using **Power BI** to gain insights into the customer churn behavior of a bank. The goal was to identify key patterns of churn across various customer segments, such as credit types, gender, and geographic locations, and provide actionable recommendations for reducing churn and improving retention.

## Tools & Techniques Used:
- **Tool:** Power BI
- **Data Source:** I utilized a fact table (Bank dataset) that contained transactional and customer data, as well as dimension tables for customer demographics, credit types, and location.
- **Data Modeling:** Relationships were established between the fact and dimension tables. A lookup (calendar) table was created for time intelligence, allowing the analysis of trends across months and years.

![](images/model.JPG)

- **Data Transformation (Power Query):**
  - Removed irrelevant columns that were not essential for the analysis to optimize performance.
  - Checked for missing values and cleaned the data to ensure accuracy.
  - Created new columns to ensure the correct sorting of fields, such as months appearing in the correct order in visualizations.
- **DAX Measures:** I implemented DAX (Data Analysis Expressions) to create custom measures and calculated columns. For example, I used DAX to group customers by their credit type, enabling segmentation by credit quality (poor, fair, good, very good, excellent).

## DAX Formulas for Creating Measures and Calculated Columns
- **_Active Members:_**
<pre>
Active Members = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Active customers'[ActiveCategory] = "Active Member")
  </pre>

- **_Inactive Members:_**
<pre>
Inactive Members = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Active customers'[ActiveCategory] = "Inactive Member")
  </pre>

- **_Churned Customers:_**
<pre>
Churned Customers = CALCULATE(COUNT(CustomerInfo[CustomerId]), 'Customer Exit'[ExitCategory] = "Exit")
  </pre>

- **_Last Month's Churned Customers:_**
<pre>
Last Month's Churned Customers = CALCULATE([Churned Customers], PREVIOUSMONTH('Calendar'[Date]))
  </pre>

- **_Credit Card Holders:_**
<pre>
Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Credit card'[Category] = "credit card holder")
  </pre>

- **_Non Credit Card Holders:_**
<pre>
Non Credit Card Holders = CALCULATE(COUNT(Bank_Churn[CustomerId]), 'Credit card'[Category] = "non credit card holder")
  </pre>

- **_Total Customers:_**
<pre>
Total Customers = COUNT(Bank_Churn[CustomerId])
  </pre>  

- **_Retain Customers:_**
<pre>
Retain Customers = [Total Customers] - [Churned Customers]
  </pre>

- **_Churn %:_**
<pre>
Churn % = 
var EC = [Churned Customers]
var TC = [Total Customers]
var Churnper = DIVIDE(EC,TC)
return Churnper
  </pre>

- **_Credit Type:_**
<pre>
Credit Type = SWITCH(TRUE(), 
Bank_Churn[CreditScore] >= 800 && Bank_Churn[CreditScore] <= 850, "Excellent", 
Bank_Churn[CreditScore] >= 740 && Bank_Churn[CreditScore] <= 799, "Very Good", 
Bank_Churn[CreditScore] >= 670 && Bank_Churn[CreditScore] <= 739, "Good", 
Bank_Churn[CreditScore] >= 580 && Bank_Churn[CreditScore] <= 669, "Fair", 
Bank_Churn[CreditScore] >= 300 && Bank_Churn[CreditScore] <= 579, "Poor")
  </pre>

## Dashboard

![](images/dashboard1.JPG)

![](images/dashboard2.JPG)

## Key Insights and Observations
1. **High Churn Among Poor Credit Holders:** Customers with poor credit ratings exhibit higher churn rates compared to other credit groups. The trend indicates that those with lower financial health are more likely to leave, signaling the need for targeted retention programs for this group.

2. **Gender-Based Churn:** Females contribute slightly more to the churn percentage (55.92%) than males (44.08%). This could imply the need for gender-specific initiatives to improve retention, possibly by addressing factors that might resonate more with female customers.

3. **Fluctuating Churn Rates by Month:** A closer look at the monthly churn rate table shows significant churn spikes in April and July across the years. This suggests that these months are key periods when customers are most likely to leave, potentially due to service-related or seasonal factors.

4. **Credit Card Usage and Churn:** A large percentage of churned customers were non-credit card holders, indicating that credit card services might play a role in customer retention. Offering more tailored credit card products or incentives could help reduce churn among this segment.

5. **Time-Based Churn Increase:** The Total Customers by Year and Status chart shows a steady rise in inactive customers over the years, indicating increasing churn. Despite an overall increase in total customers, churn seems to be accelerating, particularly in 2018 and 2019.


