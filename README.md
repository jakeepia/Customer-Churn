# Customer-Churn

![](intro.JPG)


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
Credit Type = SWITCH(TRUE(), Bank_Churn[CreditScore] >= 800 && Bank_Churn[CreditScore] <= 850, "Excellent", Bank_Churn[CreditScore] >= 740 && Bank_Churn[CreditScore] <= 799, "Very Good", Bank_Churn[CreditScore] >= 670 && Bank_Churn[CreditScore] <= 739, "Good", Bank_Churn[CreditScore] >= 580 && Bank_Churn[CreditScore] <= 669, "Fair", Bank_Churn[CreditScore] >= 300 && Bank_Churn[CreditScore] <= 579, "Poor")
  </pre>

## Model

![](images/model.JPG)

## Dashboard

![](images/dashboard1.JPG)

![](images/dashboard2.JPG)
