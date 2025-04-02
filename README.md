# Customer Churn & Attrition: Analysis and Prediction
The Telco Churn Analysis and Modeling project focuses on understanding and predicting customer churn in the telecommunications industry. By integrating ETL, interactive dashboards, and machine learning, this end-to-end analytics system uncovers key customer insights, enabling data-driven retention strategies and proactive decision-making.

## 📌Introduction: 
Customers are the asserts for every business. Customer churn, or customer attrition, is a critical challenge for businesses across various industries, including telecommunications, banking, retail, and subscription-based services. Churn occurs when customers discontinue their association with a company, leading to revenue loss and increased customer acquisition costs. Analysing and predicting churn is essential to understand the reasons behind customer departures and implement strategies to retain them. Acquiring a new customer is economically expensive and critical than retaining existing old customers. In the telecom industry, where competition is intense, churn prediction helps identify at-risk customers by analysing factors such as usage patterns, billing issues, service quality, and customer interactions. By leveraging data analytics and machine learning, companies can proactively address customer concerns, offer personalized incentives, and improve overall satisfaction, ultimately enhancing customer retention and business sustainability.

## 🎯Project Goal: ETL, Power BI & Machine Learning for Customer Churn Analysis
The goal of this project is to develop a comprehensive ETL process within a database, create an interactive Power BI dashboard, and build a predictive machine learning model to analyze and forecast customer churn. This initiative aims to provide deeper insights into customer behavior across multiple dimensions, including demographics, geographic distribution, payment and account details, and service usage patterns.

By studying the characteristics of churned customers, we can identify key factors influencing customer attrition and pinpoint areas where targeted marketing campaigns can be implemented to improve retention. Additionally, leveraging predictive analytics, the project seeks to establish a reliable method for forecasting potential churners, enabling proactive intervention strategies to enhance customer loyalty and business growth.

## 📂Dataset Overview
> LINK:<a href="">Telecom Customer Dataset</a>

The dataset consists of customer records from a telecom company.It includes various attributes related to customer demographics, account details, service usage, and churn status.The target variable is "Customer_Status", which indicates whether a customer has left the service (Churned/Stayed).
### Key Features in the Dataset
- Customer Demographics
- customerID: Unique customer identifier
- gender: Male/Female
- Married: Whether the customer is a married or not (0/1)
- Age: Age of the customer
- State: Customers Geographic location.
### Account & Payment Details
- tenure: Number of months the customer has stayed with the company
- Contract: Type of contract (Month-to-month, One year, Two year)
- PaymentMethod: Payment method used (Credit card, Electronic check, etc.)
- MonthlyCharges: The amount charged per month
- TotalCharges: The total amount charged to the customer
etc..
### Service Usage & Subscription Details
- PhoneService: Whether the customer has phone service (Yes/No)
- MultipleLines: Whether the customer has multiple lines (Yes/No)
- InternetService: Type of internet service (DSL, Fiber optic, No)
- OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport: Value-added services (Yes/No)
- StreamingTV, StreamingMovies: Whether the customer subscribes to streaming services (Yes/No)
### Churn Label
Customer_Status: The target variable, indicating if the customer has left the company (Churned/Stayed)

## ETL SQL: Data Processing and Exploration
### Create Database and Load Dataset
```sql
-- The database and tables are created.
-- Load dataset into SQL in the tables section as staging data.
```
### Data Exploration
```sql
SELECT Gender, Count(Gender) as TotalCount,
       Count(Gender) * 1.0 / (Select Count(*) from stg_Churn) as Percentage
FROM stg_Churn
GROUP BY Gender;

SELECT Contract, Count(Contract) as TotalCount,
       Count(Contract) * 1.0 / (Select Count(*) from stg_Churn) as Percentage
FROM stg_Churn
GROUP BY Contract;

SELECT Customer_Status, Count(Customer_Status) as TotalCount, 
       Sum(Total_Revenue) as TotalRev,
       Sum(Total_Revenue) / (Select sum(Total_Revenue) from stg_Churn) * 100 as RevPercentage
FROM stg_Churn
GROUP BY Customer_Status;

SELECT State, Count(State) as TotalCount,
       Count(State) * 1.0 / (Select Count(*) from stg_Churn) as Percentage
FROM stg_Churn
GROUP BY State
ORDER BY Percentage DESC;
```
### Checking for NULLS
```sql
SELECT 
    SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count,
    SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS Gender_Null_Count,
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS Age_Null_Count,
    SUM(CASE WHEN Married IS NULL THEN 1 ELSE 0 END) AS Married_Null_Count,
    SUM(CASE WHEN State IS NULL THEN 1 ELSE 0 END) AS State_Null_Count,
    SUM(CASE WHEN Number_of_Referrals IS NULL THEN 1 ELSE 0 END) AS Number_of_Referrals_Null_Count,
    SUM(CASE WHEN Tenure_in_Months IS NULL THEN 1 ELSE 0 END) AS Tenure_in_Months_Null_Count,
    SUM(CASE WHEN Value_Deal IS NULL THEN 1 ELSE 0 END) AS Value_Deal_Null_Count,
    SUM(CASE WHEN Phone_Service IS NULL THEN 1 ELSE 0 END) AS Phone_Service_Null_Count,
    SUM(CASE WHEN Multiple_Lines IS NULL THEN 1 ELSE 0 END) AS Multiple_Lines_Null_Count,
    SUM(CASE WHEN Internet_Service IS NULL THEN 1 ELSE 0 END) AS Internet_Service_Null_Count,
    SUM(CASE WHEN Internet_Type IS NULL THEN 1 ELSE 0 END) AS Internet_Type_Null_Count,
    SUM(CASE WHEN Online_Security IS NULL THEN 1 ELSE 0 END) AS Online_Security_Null_Count,
    SUM(CASE WHEN Online_Backup IS NULL THEN 1 ELSE 0 END) AS Online_Backup_Null_Count,
    SUM(CASE WHEN Device_Protection_Plan IS NULL THEN 1 ELSE 0 END) AS Device_Protection_Plan_Null_Count,
    SUM(CASE WHEN Premium_Support IS NULL THEN 1 ELSE 0 END) AS Premium_Support_Null_Count,
    SUM(CASE WHEN Streaming_TV IS NULL THEN 1 ELSE 0 END) AS Streaming_TV_Null_Count,
    SUM(CASE WHEN Streaming_Movies IS NULL THEN 1 ELSE 0 END) AS Streaming_Movies_Null_Count,
    SUM(CASE WHEN Streaming_Music IS NULL THEN 1 ELSE 0 END) AS Streaming_Music_Null_Count,
    SUM(CASE WHEN Unlimited_Data IS NULL THEN 1 ELSE 0 END) AS Unlimited_Data_Null_Count,
    SUM(CASE WHEN Contract IS NULL THEN 1 ELSE 0 END) AS Contract_Null_Count,
    SUM(CASE WHEN Paperless_Billing IS NULL THEN 1 ELSE 0 END) AS Paperless_Billing_Null_Count,
    SUM(CASE WHEN Payment_Method IS NULL THEN 1 ELSE 0 END) AS Payment_Method_Null_Count,
    SUM(CASE WHEN Monthly_Charge IS NULL THEN 1 ELSE 0 END) AS Monthly_Charge_Null_Count,
    SUM(CASE WHEN Total_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Charges_Null_Count,
    SUM(CASE WHEN Total_Refunds IS NULL THEN 1 ELSE 0 END) AS Total_Refunds_Null_Count,
    SUM(CASE WHEN Total_Extra_Data_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Extra_Data_Charges_Null_Count,
    SUM(CASE WHEN Total_Long_Distance_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Long_Distance_Charges_Null_Count,
    SUM(CASE WHEN Total_Revenue IS NULL THEN 1 ELSE 0 END) AS Total_Revenue_Null_Count,
    SUM(CASE WHEN Customer_Status IS NULL THEN 1 ELSE 0 END) AS Customer_Status_Null_Count,
    SUM(CASE WHEN Churn_Category IS NULL THEN 1 ELSE 0 END) AS Churn_Category_Null_Count,
    SUM(CASE WHEN Churn_Reason IS NULL THEN 1 ELSE 0 END) AS Churn_Reason_Null_Count
FROM stg_Churn;
```
### Remove and fill Nulls and Insert Data into Production Table
```sql
SELECT 
    Customer_ID,
    Gender,
    Age,
    Married,
    State,
    Number_of_Referrals,
    Tenure_in_Months,
    ISNULL(Value_Deal, 'None') AS Value_Deal,
    Phone_Service,
    ISNULL(Multiple_Lines, 'No') AS Multiple_Lines,
    Internet_Service,
    ISNULL(Internet_Type, 'None') AS Internet_Type,
    ISNULL(Online_Security, 'No') AS Online_Security,
    ISNULL(Online_Backup, 'No') AS Online_Backup,
    ISNULL(Device_Protection_Plan, 'No') AS Device_Protection_Plan,
    ISNULL(Premium_Support, 'No') AS Premium_Support,
    ISNULL(Streaming_TV, 'No') AS Streaming_TV,
    ISNULL(Streaming_Movies, 'No') AS Streaming_Movies,
    ISNULL(Streaming_Music, 'No') AS Streaming_Music,
    ISNULL(Unlimited_Data, 'No') AS Unlimited_Data,
    Contract,
    Paperless_Billing,
    Payment_Method,
    Monthly_Charge,
    Total_Charges,
    Total_Refunds,
    Total_Extra_Data_Charges,
    Total_Long_Distance_Charges,
    Total_Revenue,
    Customer_Status,
    ISNULL(Churn_Category, 'Others') AS Churn_Category,
    ISNULL(Churn_Reason , 'Others') AS Churn_Reason
INTO [db_Churn].[dbo].[prod_Churn]
FROM [db_Churn].[dbo].[stg_Churn];
```
### Create Views
```sql
CREATE VIEW vw_ChurnData AS
SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned', 'Stayed');

CREATE VIEW vw_JoinData AS
SELECT * FROM prod_Churn WHERE Customer_Status = 'Joined';
```
## Power BI: Analysis and Insights
> Explore and enjoy the interactive Customer Churn Analysis Report.  
> LINK:<a href="https://github.com/LakshmiPriyaSivaraman0806/Customer-Churn-Analysis-and-Prediction/blob/main/Churn%20Analysis%20-%20Power%20BI%20Report.pdf">Customer Churn Analysis Report</a>
### 💡**Summary Analysis: INSIGHTS**
> LINK:<a href="">Summary Analysis Dashboard</a>
- The age group >50 had the highest number of total customers (2,838), which was 2,325. 64% higher than the <20 age group, which had the lowest number of total customers (117).
- There is a positive correlation between **Total Customers and Churn Rate**, suggesting the number of customers increases, the churn rate also tends to rise.
- The >50 age group made up 44.22% of the total customer base, indicating that nearly half of the customers belonged to this age category.
- The largest gap between Total Customers and Churn Rate was observed in the >= 24 Months tenure group, where the number of total customers exceeded the churn rate count by 2,087 customers.
- Across all five tenure groups, the Total Customers ranged from 980 to 2,087, while the Churn Rate varied between 26.1% and 27.5%.
- Total churn among female customers (1,111) was significantly higher than among male customers (621), indicating that a larger proportion of female customers discontinued the service.
- Competitor Impact: High churn due to better offers, devices, and service quality from competitors. Stronger retention strategies needed.
- Jammu & Kashmir has highest Churn rate (59%):Urgent need for service improvement and localized retention plans.
-  Contract Type & Churn: Month-to-Month → 47.5% churn (highest), One-Year → 11.7% churn, Two-Year → 2.7% churn (lowest). Longer contracts significantly reduce churn. Push incentives to convert short-term users into long-term commitments.
  
### 🚀Key Recommendations (Crisp & Actionable)
1) **Counter Competitor Influence –** Improve pricing, offer exclusive plans, and enhance customer service to reduce competitor-driven churn.
2) **Address Jammu & Kashmir Churn (59%) –** Investigate network/service issues and introduce region-specific loyalty programs.
3) **Encourage Long-Term Contracts –** Convert month-to-month users (47.5% churn) into long-term contracts with discounts, loyalty rewards, or bundled services.
4) **Enhance Retention for Age 50+ Customers –** Since they form 44.22% of the base, create personalized engagement plans to improve retention.
5) **Improve Female Customer Retention –** Address key pain points leading to higher female churn (1,111 vs. 621 for males) through targeted initiatives.

## 🔄Predictive Modeling
> LINK:<a href="">Jupyter Notebook</a>
> Model Selection: The Random Forest Classifier was chosen for churn prediction due to its ability to handle large datasets, mitigate overfitting, and capture complex patterns through ensemble learning. Its robustness to imbalanced data and high interpretability make it ideal for customer churn analysis

### Prediction Results and Model Performance
**Test Data Analysis:**

**Confusion Matrix:**

- True Negatives (TN): 3065 (Correctly predicted class 0)
- False Positives (FP): 198 (Incorrectly predicted class 1 when it was actually 0)
- False Negatives (FN): 354 (Incorrectly predicted class 0 when it was actually 1)
- True Positives (TP): 1188 (Correctly predicted class 1)

**Classification Report:**
- High accuracy (0.89).
- High precision for both classes (0.94 for 0, 0.77 for 1).
- High recall for class 0 (0.90), good recall for class 1 (0.86).
- High F1-scores for both classes (0.92 for 0, 0.81 for 1).
** Model learned the training data very well.**
------------------------------------------------------------------------------------------------
**Test Data Analysis:**

**Confusion Matrix:**

- True Negatives (TN): 753 (Correctly predicted class 0)
- False Positives (FP): 73 (Incorrectly predicted class 1 when it was actually 0)
- False Negatives (FN): 103 (Incorrectly predicted class 0 when it was actually 1)
- True Positives (TP): 273 (Correctly predicted class 1)

**Classification Report:**

- Good accuracy (0.86), slightly lower than training.
- Reasonable precision for both classes (0.91 for 0, 0.73 for 1).
- Reasonable recall for both class 0 (0.88) and class 1 (0.79).
- F1-score for class 0 (0.90) is good, but lower for class 1 (0.76).
**Performance is slightly lower on unseen data, especially for correctly identifying class 1.**
--------------------------------------------------------------------------------------------
**Final Conclusion:**

> The model performs well overall, achieving a good accuracy on the unseen test data. However, there is a noticeable drop in performance compared to the training data, suggesting a slight degree of overfitting. The model is particularly better at identifying instances of class 0 compared to class 1, as indicated by the lower recall and F1-score for class 1 on the test set. This may be because the data is imbalanced {Customer_Status : Stayed > 2(churned)} This the class 0 is influencing the model performance since it is the majority class. To resolved use Oversampling technique like SMOTE to enhance performance of model in classifying class 0 better.

## 💡INSIGHTS From Predicted_Churn Data
> LINK:<a href="">Prediction Analysis Dashboard</a>
- The >50 age group had the highest count of predicted churners at 138, which was 1,050% heigher than the <20 age group with only 12 customers.
- The predicted churn distribution among age groups follows this order: **>50 (138)** > **35-50 (127)** > **20-35 (104)** > **<20 (12)**.
- Customers aged >50 make up 36.22% of all predicted churners, indicating a significant churn risk in this age category.
- The number of predicted churners varied across 22 states, with the highest count observed in **Uttar Pradesh (44 customers)** and the lowest count at 2 customers in the least affected states.
### 🚀Recommendations:
1) **Targeted Retention for Older Customers (>50) –** Offer personalized plans, better support, and loyalty benefits to reduce churn.
2) **Region-Specific Interventions –** Focus on high-churn states like Uttar Pradesh with localized service improvements and promotions.
3) **Engage At-Risk Age Groups –** Design retention campaigns for 35-50 age group as they also show high churn risk.
4) **Improve Customer Experience –** Address pain points leading to dissatisfaction, especially for older customers.
5) **Customized Offers for High-Risk Customers –** Provide discounts, extended contracts, or added benefits for month-to-month contract holders to encourage long-term commitment.

## CONCLUSION:
This project successfully integrates ETL, Power BI, and Machine Learning to analyze and predict customer churn in the telecom industry. The insights from data exploration, visualization, and predictive modeling highlight key churn drivers such as contract type, competitor influence, and customer demographics. The Random Forest model demonstrated strong predictive performance, though improvements like SMOTE for class balancing could enhance its ability to detect churners. Strategic recommendations, including targeted retention plans for high-risk segments and incentives for long-term contracts, can help reduce churn. By leveraging data-driven decision-making, telecom companies can enhance customer satisfaction, improve retention, and drive long-term business growth.






