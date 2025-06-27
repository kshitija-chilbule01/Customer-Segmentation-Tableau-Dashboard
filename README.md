# SpicyFood: Customer Segmentation Analysis with a Tableau Dashboard
## [View Live Tableau Dashboard](https://public.tableau.com/views/Kshitija_SpicyFood_Dashboard1_6/Dashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Table of Contents
- [Project Overview](#project-overview)
- [Data Collection](#data-collection)
- [Tableau Dashboard Preview](#tableau-dashboard-preview)
- [Summary of Insights](#summary-of-insights)

## Project Overview
This project focuses on developing an interactive **Customer Segmentation Dashboard** in Tableau for a fictional FoodTech company, **SpicyFood**. By leveraging **RFM (Recency, Frequency, and Monetary)** Analysis, the goal is to segment customers based on their purchasing behavior to better understand engagement patterns and inform strategic decision-making. The segmentation provides clarity on who the company’s most valuable customers are, who is at risk of churning, and how to tailor marketing efforts accordingly.

## Data Collection
**The dataset includes essential details like Customer ID, Purchase Frequency, Bill Amount, Recency, and other relevant attributes.**
The dataset used for this analysis includes the following fields:

1. **Customer ID:** Unique identifier for each customer.
2. **Purchase Frequency:** The number of purchases a customer makes.
3. **Bill Amount:** Total monetary value spent by the customer.
4. **Recent Purchase Date:** The customer’s latest purchase date.
5. **Rank_Recency:** Ranking based on the recency of purchases.
6. **Rank_Frequency:** Ranking based on purchase frequency.
7. **Rank_Spending:** Ranking based on the total spending amount.
8. **Average of Rank:** Average rank across recency, frequency, and spending.
9. **Overall Rank:** Combined rank for RFM segmentation.
10. **Customer Tiers:** Segmented customer groups based on RFM scores
11. **Gender:** Gender of the customer.
12. **Age:** Age of the customer.

## Tableau Dashboard Preview
The final Tableau dashboard provides an interactive and visually compelling representation of key insights, enabling SpicyFood management to:
- Identify and target high-value customers.
- Re-engage at-risk customers to minimize churn.
- Strengthen retention strategies for long-term customer loyalty.
- Optimize marketing campaigns for better engagement and ROI.
  
![Dashboard](https://github.com/user-attachments/assets/36c1c752-2322-4872-bc78-e24d41f157d2)

## Key Metrics Overview
1. **Total Revenue:** Customers have made purchases totaling Rs. 179,500.
2. **Total Orders:** The total number of orders placed is 107.
3. **Unique Customer Base:** SpicyFood serves 35 unique customers, highlighting its niche market.
4. **Average Purchase Value:** On average, each customer spends Rs. 5,129 per transaction.

## Summary of Insights
As we already know, customers are segmented into 5 different categories such as Champions, Loyal Customers, Potential Loyalists, Promising, and At Risk.

![image](https://github.com/user-attachments/assets/9ad40e9d-5d90-488a-88cd-923106be8c59)

#### 1. Champions

![image](https://github.com/user-attachments/assets/090dbd93-8a27-451d-b3eb-73e8b98b3f7e)

- These customers make up 22% of the customer base and contribute to the majority of revenue.
- They are frequent buyers with high spending and recent purchases, making them ideal for loyalty programs or exclusive offers.

#### 2. Loyal Customers
![image](https://github.com/user-attachments/assets/1ddeff75-32ad-47fd-9fe8-e428dd6f967f)

- This group consistently makes repeat purchases and represents a significant portion of long-term revenue.
- Retaining them through rewards or early access to products can strengthen their loyalty even further.

#### 3. Potential Loyalist
![image](https://github.com/user-attachments/assets/17e6ba15-a559-4826-af25-e0c9350511e3)

- Customers in this group show high potential to become Champions.
- Strategies such as personalized marketing or small incentives (e.g., free shipping or discounts) could encourage them to purchase more frequently.

#### 4. Promising 
![image](https://github.com/user-attachments/assets/a334a9cb-e364-4dec-9be2-cbd4c905c819)

- These are new or occasional buyers with medium spending levels.
- Engaging them through follow-ups, product recommendations, or introductory offers can drive higher retention.

#### 5. At Risk
![image](https://github.com/user-attachments/assets/8f3bc814-83b0-4da7-aed6-22fb29ac64fd)

- This segment includes customers with low recent activity but medium or high spending in the past.
- Re-engagement campaigns, such as email reminders, exclusive discounts, or personalized offers, could win them back.

### Spending Patterns
![image](https://github.com/user-attachments/assets/3fd0a29a-229f-45e3-aa8c-cf3811b1eeb2)

- Loyal Customers and Champions have the highest spending levels, and their purchase frequency shows consistent growth.

### Demographics Patterns
![image](https://github.com/user-attachments/assets/666ec7a9-622a-45d1-952e-2a51ca6f5d5f)

- Champions, Potential Loyal Customers, and Promising are predominantly Teenagers, indicating a strong purchasing power in this demographic.
- Male customers slightly dominate the Champion segment, while females are more prevalent in the Potential Loyalist and Promising groups.

## MySQL Queries for KPI's Validation 
```
`USE spicyfood;

SELECT * FROM spicyfood_data;

-- Total Unique Customers count
SELECT DISTINCT(COUNT(*)) AS Total_Customers FROM spicyfood_data;

-- Total bill amount
SELECT SUM(`Bill Amount`) AS Total_Purchase FROM spicyfood_data;

-- Total Number of Orders
SELECT SUM(`Purchase Frequency`) AS Number_of_Orders FROM spicyfood_data;

-- Average Purchase Amount
SELECT ROUND(AVG(`Bill Amount`)) AS Avg_Purchase_Amount FROM spicyfood_data;

-- Unique Customer Tiers
SELECT DISTINCT `Customer Tiers` FROM spicyfood_data;

-- Select Customer ID, Purchase Frequency, Bill Amount and Recent Purchase Date with a Customer Tier "Champions"
SELECT `Customer ID`, `Purchase Frequency`, `Bill Amount`, `Recent Purchase Date`
FROM spicyfood_data 
WHERE `Customer Tiers` = 'Champions';

-- Select Customer ID, Purchase Frequency, Bill Amount and Recent Purchase Date with a Customer Tier "Loyal Customer"
SELECT `Customer ID`, `Purchase Frequency`, `Bill Amount`, `Recent Purchase Date`
FROM spicyfood_data 
WHERE `Customer Tiers` = 'Loyal Customer';

-- find the percentage of customers in each Customer Tier
SELECT 
    `Customer Tiers`, 
    COUNT(*) AS Customer_Count,
    ROUND((COUNT(*) * 100.0 / (SELECT COUNT(DISTINCT `Customer ID`) FROM spicyfood_data)), 2) AS Percentage
FROM spicyfood_data
GROUP BY `Customer Tiers`
ORDER BY Percentage DESC;

--  retrieve customers sorted by their overall rank
SELECT `Customer ID`, `Overall Rank`
FROM spicyfood_data
ORDER BY `Overall Rank`;

-- Select customers who have recently purchased and their total spend
SELECT `Customer ID`, SUM(`Rank_Spending`) AS Total_Spend
FROM spicyfood_data
WHERE `Recent Purchase Date` = (SELECT MAX(`Recent Purchase Date`) FROM spicyfood_data)
GROUP BY `Customer ID`;


-- Gender distribution by Customer Tiers
SELECT 
    `Customer Tiers`, 
    Gender, 
    COUNT(*) AS Customer_Count, 
    ROUND((COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY `Customer Tiers`)), 2) AS Percentage
FROM spicyfood_data
GROUP BY `Customer Tiers`, Gender
ORDER BY `Customer Tiers`, Percentage DESC;
```
