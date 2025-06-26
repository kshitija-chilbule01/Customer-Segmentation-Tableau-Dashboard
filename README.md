# SpicyFood: Customer Segmentation Analysis with a Tableau Dashboard

## [Read full Medium blog](https://medium.com/@kshitijachilbule2/spicyfood-exploring-customer-segmentation-with-a-tableau-dashboard-1167eed99ca2)

## [View Live Tableau Dashboard](https://public.tableau.com/views/Kshitija_SpicyFood_Dashboard1_6/Dashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## Tableau Dashboard Preview

![Dashboard](https://github.com/user-attachments/assets/fe73d679-99b6-4fc5-bd52-b97bae93f4c2)

## MySQL Queries for Validation 
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
