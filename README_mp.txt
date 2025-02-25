# Mobile price prediction SQL Project

## Project Overview

**Project Title**: Mobile Price prediction

**Database**: `price_prediction`

## Objectives

1. **Set up a mbolie price prediction database**: Create and populate a with the provided price and features data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the given data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `price_prediction`
- **Table Creation**: A table named mobile_prices is created to store the given data. The table structure includes columns for ID,screen_size,ram,storage,battery_capacity,camera_quality,price.

CREATE TABLE mobile_prices (
    ID INT PRIMARY KEY,
    screen_size FLOAT,
    ram INT,
    storage INT,
    battery_capacity INT,
    camera_quality FLOAT,
    price DECIMAL(10, 2)
);

### 2. Data Exploration & Cleaning :

-- to display all data

SELECT * from mobile_prices;

-- to check and clear missing values

SELECT * from mobile_prices
where
id is NULL OR screen_size is null or ram is null or storage is null or battery_capacity is null or camera_quality is null or price is null;

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

--1. Select Mobile Phones With Screen Size Greater Than 6 Inches:
SELECT * FROM mobile_prices
WHERE screen_size > 6;

-- 	2. Find the Average Price of Phones with Specific RAM:
SELECT ram,round(avg(price),2) as average_price from mobile_prices group by 1 order by 1;

-- 3.List Mobile Phones Sorted by Price
select * from mobile_prices order by price;

--4.Find the Mobile Phones with the Best Price-to-Specs Ratio (Price per GB of RAM):
SELECT *,
       price / ram AS price_per_gb_ram
FROM mobile_prices
ORDER BY price_per_gb_ram DESC;

--5. Get the Top 3 Most Expensive Mobile Phones:
SELECT * 
FROM mobile_prices
ORDER BY price DESC
LIMIT 3;

--6. Find the Average Price of Phones Based on Screen Size and RAM:
SELECT screen_size, ram, AVG(price) AS average_price
FROM mobile_prices
GROUP BY screen_size, ram
ORDER BY screen_size, ram;

--7.Find the Phone with the Highest Camera Quality and its Price:
SELECT * from mobile_prices
ORDER BY camera_quality,price DESC LIMIT 1;

--8.Get the Number of Mobile Phones in Each Price Range (Price Categories)
SELECT CASE
           WHEN price <= 300 THEN 'Low'
           WHEN price BETWEEN 301 AND 600 THEN 'Medium'
           WHEN price BETWEEN 601 AND 900 THEN 'High'
           ELSE 'Premium'
       END AS price_category,
       COUNT(*) AS number_of_phones
FROM mobile_prices
GROUP BY price_category;

--9.Find the Phones with Battery Capacity Greater Than 4000 mAh and Camera Quality Higher Than 12 MP
SELECT * 
FROM mobile_prices
WHERE battery_capacity > 4000
  AND camera_quality >12;
  
-- 10. List Mobile Phones with Prices Greater Than the Average Price of All Phones:
SELECT * 
FROM mobile_prices
WHERE price > (SELECT AVG(price) from mobile_prices);

-- Get the Distribution of Battery Capacity Across Different Price Ranges:
SELECT 
    CASE 
        WHEN price <= 300 THEN 'Low'
        WHEN price BETWEEN 301 AND 600 THEN 'Medium'
        WHEN price BETWEEN 601 AND 900 THEN 'High'
        ELSE 'Premium'
    END AS price_range,
    battery_capacity, 
    COUNT(*) AS number_of_phones
FROM mobile_prices
GROUP BY price_range, battery_capacity
ORDER BY price_range, battery_capacity;

--11. Find the Mobile Phone with the Most Balanced Screen Size, RAM, and Storage:
SELECT *,
       ABS(screen_size - (SELECT AVG(screen_size) FROM mobile_prices)) +
       ABS(ram - (SELECT AVG(ram) FROM mobile_prices)) +
       ABS(storage - (SELECT AVG(storage) FROM mobile_prices)) AS spec_balance
FROM mobile_prices
ORDER BY spec_balance
LIMIT 1;

### Findings:
1. **Price vs. RAM**: Higher RAM is directly correlated with higher prices.
2. **Battery Capacity and Camera Quality**: Higher battery capacity often leads to higher prices, but camera quality doesn't always correlate directly with price.
3. **Storage and Price**: More storage, particularly 128GB, tends to increase price.
4. **Screen Size**: Larger screens usually mean higher prices, but RAM and storage also influence this.
5. **Best Price-to-Specs Ratio**: Phones with high RAM at reasonable prices offer the best value.

---

### Report:
- **Data Insights**: RAM and storage are the biggest price drivers, followed by battery capacity and camera quality.
- **Trends**: Larger screen sizes and high-camera quality phones tend to be more expensive. Battery capacity plays a significant role.

---

### Conclusion:
- **Price Drivers**: RAM, storage, and battery capacity primarily determine price. Camera quality has a secondary impact.
- **Recommendations**: Focus on optimizing RAM and storage for value. Balance camera and battery for better overall performance.
- **Potential Research**: Investigate brand, 5G, and other design features not included in the dataset for deeper pricing insights.







