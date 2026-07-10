# Sales-Analysis-using-Postgre-SQL

perfoemed analysis on ecommerce data set and answere the business question using postgre sql.
## Beginner Level
--1. Find total sales.
SELECT SUM(total_amount) AS total_sales FROM ecommerce;

--2. Find total profit of delivered order.
SELECT SUM(profit_amount) AS total_profit FROM ecommerce 
WHERE order_status='Delivered';

--3. Find total number of orders.
SELECT COUNT(order_id) AS total_orders FROM ecommerce;

--4. Find total quantity sold.
SELECT SUM(quantity) AS total_quantity_sold FROM ecommerce 
WHERE order_status='Delivered';

--5. Calculate profit margin.
SELECT (SUM(profit_amount)/SUM(total_amount))*100 FROM ecommerce
WHERE order_status='Delivered';

## Monthly Analysis
--6. Monthly Sales
SELECT month, SUM(total_amount) AS sales FROM ecommerce
GROUP BY month
ORDER BY sales DESC;

--7. Monthly Profit
SELECT month, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status='Delivered'
GROUP BY month
ORDER BY profit DESC;

--8. Monthly Orders
SELECT month, COUNT(order_id)  FROM ecommerce
GROUP BY month;

--9. Highest Sales Month
SELECT month, SUM(total_amount) AS sales FROM ecommerce
GROUP BY month
ORDER BY sales DESC
LIMIT 1;

## Product Analysis
--10. Top 10 Products by sales
SELECT product_category, SUM(total_amount) AS sales FROM ecommerce
GROUP BY product_category
ORDER BY sales DESC
LIMIT 10;

--11. Top Selling Products by profit
SELECT product_category, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY product_category
ORDER BY profit DESC
LIMIT 1;

--12. Lowest Profit Products
SELECT product_category, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY product_category
ORDER BY profit ASC
LIMIT 1;

--13. Sales by Age Group
SELECT age_group, SUM(total_amount) AS total_sales FROM ecommerce
GROUP BY age_group
ORDER BY total_sales DESC;

--14. Profit by Customer Segment
SELECT customer_segment, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY customer_segment;

--15. Orders by Membership
SELECT membership_status, COUNT(order_id) FROM ecommerce
GROUP BY membership_status;

## Country Analysis

--16. Profit by Country
SELECT country, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY country;

--17. Top 5 Countries by Sales
SELECT country, SUM(total_amount) AS sales FROM ecommerce
GROUP BY country
ORDER BY sales DESC
LIMIT 5;

## Payment Analysis
--18. Sales by Payment Method
SELECT payment_method, SUM(total_amount) AS total_sales FROM ecommerce
GROUP BY payment_method
ORDER BY total_sales DESC;

--19. Profit by Payment Method
SELECT payment_method, SUM(profit_amount) AS profit FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY payment_method;

--20. Most Used Payment Method
SELECT payment_method, COUNT(order_id) AS total_orders FROM ecommerce
GROUP BY payment_method
ORDER BY total_orders DESC
LIMIT 1;

## Order Analysis

--21. Return Rate
SELECT (
			COUNT
				(CASE WHEN returned='Yes' THEN 1 END)*100	
		)/COUNT(*) AS return_rate 
FROM ecommerce;

--22. Cancel Rate
SELECT (
			COUNT
				(CASE WHEN order_status='Cancelled' THEN 1 END)*100	
		)/COUNT(*) AS cancelled_rate 
FROM ecommerce

--23. Orders by Status
SELECT order_status, COUNT(order_id) AS total_orders FROM ecommerce
GROUP BY order_status;

## Seasonal Analysis
--24. Profit by Season
SELECT season, SUM(profit_amount) AS profits_by_season FROM ecommerce
WHERE order_status = 'Delivered'
GROUP BY season;

--25. Sales on Holidays vs Non-Holidays
SELECT holiday_season, SUM(total_amount) AS sales FROM ecommerce
GROUP BY holiday_season;

--26. Rank Products by Profit
SELECT
product_category,
SUM(profit_amount) AS profit,
RANK() OVER(ORDER BY SUM(profit_amount) DESC) AS ranking
FROM ecommerce
GROUP BY product_category;

--27. Running Total of Monthly Sales
SELECT month,
SUM(total_amount) AS sales,
SUM(SUM(total_amount)) OVER(ORDER BY month)
AS running_total
FROM ecommerce
GROUP BY month;


--28. Average Order Value
SELECT
ROUND((SUM(order_amount)/COUNT(DISTINCT order_id)),2)
AS avg_order_value
FROM ecommerce
WHERE order_status = 'Delivered';

--29. Top 5 Customers by Sales
SELECT customer_id, SUM(total_amount) AS sales FROM ecommerce
GROUP BY customer_id
ORDER BY sales DESC
LIMIT 5;

--30. Which countries have the highest cancellation rates? 
SELECT country, 
ROUND(
ROUND(COUNT(
CASE 
WHEN order_status='Cancelled' THEN 1 END
)*100,2)/COUNT(order_status),2) AS cancelletation_rate  FROM ecommerce
GROUP BY country
ORDER BY cancelletation_rate DESC
LIMIT 1;

--31. Which payment method has the highest average transaction value?
SELECT payment_method,ROUND(AVG(order_amount),2) AS average_transaction_value FROM ecommerce
GROUP BY payment_method
ORDER BY average_transaction_value DESC
LIMIT 1;

--32. Which customer segment has the highest return rate? 
SELECT customer_segment, 
ROUND(
	ROUND(
		COUNT(
			CASE WHEN order_status LIKE 'Return%' THEN 1 END
			)*100
		,2)/COUNT(order_id)
	,2) AS return_rate FROM ecommerce
GROUP BY customer_segment
ORDER BY return_rate DESC
LIMIT 1;

--33. What is the average order value by customer segment?
SELECT customer_segment, ROUND((SUM(order_amount)/COUNT(DISTINCT order_id)),2) AS average_order_value FROM ecommerce
GROUP BY customer_segment;
