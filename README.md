# Pizzapoint-sales-analysis-SQL
<br>
•	Conducted detailed data analysis of PizzaPoint sales, utilizing SQL Server to track pizza sales across categories, 
<br>
revealing Classic Pizza as the top seller with 14,888 units, followed by Supreme (11,987), Veggie (11,649), and Chicken (11,050).
<br>
•	Analyzed pizza order distribution, finding large pizzas lead with 18,525 orders,
<br>
while Medium (15,385), Small (14,137), and Extra-Large (544) were also significant, and XXL orders accounted for 28.
<br>
•	Calculated an average of 138 pizzas sold per day, providing actionable insights to improve sales strategies.
<br>
<br>
•••QUERY
<br>
<br>
• LIST THE TOP 5 MOST ORDERED PIZZA TYPES ALONG WITH THEIR QUANTITIES.
<br>
SELECT
<br>
PIZZUYPES.NAME, SUM(ORDERS_DETAILS.QUANTITY)AS QUANTITY FROM
<br>
PIZZUYPES JOIN
<br>
PIZZAS ON PIZZUYPES.PIZZUYPUD = PIZZAS.PIZZUYPUD JOIN
<br>
ORDERS_DETAILS ON ORDERS_DETAILS.PIZZAJD = PIZZAS.PIZZAJD GROUP BY PIZZUYPES.NAME
<br>
ORDER BY QUANTITY DESC LIMIT 5;
<br>
<br>
• SJoin the necessary tables to find the total quantity of each pizza ordered.
<br>
SELECT
<br>
pizza_types.category, SUM(orders_details.quantity) AS quantity
<br>
FROM
<br>
pizza_types JOIN
<br>
pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
<br>
JOIN
<br>
orders_details ON orders_details.pizza_id = pizzas.pizza_id GROUP BY pizza_types.category
<br>
ORDER BY quantity DESC;
<BR>
<br>
• Identify the most common pizza size ordered.
<br>
SELECT
<br>
pizzas.size,
<br>
COUNT(orders_details.order_details_id) AS order_count
<br>
FROM
<br>
pizzas
<br>
JOIN
<br>
orders_details ON pizzas.pizza_id = orders_details.pizza_id
<br>
GROUP BY pizzas.size
<br>
ORDER BY order count DESC;
<br>
<br>
 • Determine the distribution of orders by hour of the day.
<br>
SELECT	hour	order_count	
<br>
HOUR(ordeUime) AS hour, COUNT(order _id) AS order_count	
<br>
FROM
<br>
orders	
<br>
GROUP BY HOUR(ordeUime);	
<br>
<br>
• Join relevant tables to find the category-wise distribution of pizzas.
<br>
SELECT
<br>
category, COUNT(name) FROM
<br>
pizza_types
<br>
GROUP BY category;
<br>
<br>
• Group the orders by date and calculate the average number of pizzas ordered per day.
<br>
SELECT
<br>
RDUND(AVG( quantity), D) AS avg_pizza_per _day FROM
<br>
(SELECT
<br>
orders.order _date, SUM(orders_details.quantity) AS quantity FROM
<br>
orders
<br>
JOIN	orders_details	ON	orders.order _id orders_details.order_id
<br>
GROUP BY orders.order _date) AS order_quantity;
<br>
<br>
• Determine the top 3 most ordered pizza types based on revenue.
<br>
SELECT
<br>
pizza_types.name,
<br>
SUM(orders_details.quantity * pizzas.price) AS revenue
<br>
FROM
<br>
pizza_types JOIN
<br>
pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id JOIN
<br>
orders_details ON orders_details.pizza_id = pizzas.pizza_id	
<br>
group by pizza_types.name order by revenue desc limit 3 ;
<br>
<br>
• Calculate the percentage contribution of each pizza type to total revenue.
<br>
SELECT pizza_types.category,
<br>
ROUNO(SUM(orders_details.quantity *pizzas.price)/ (SELECT ROUNO(SUM(orders_details.quantity * pizzas.price), 2) 
<br>
AS totaLsales
<br>
FROM orders_details
<br>
JOIN pizzas ON pizzas.pizza_id = orders_details.pizza_id} * 100, 2)
<br>
as revenue
<br>
FROM pizza_types JOIN pizzas
<br>
ON pizza_types.pizza_type_id = pizzas.pizza_type_id JOIN orders_details
<br>
ON orders_details.pizza_id = pizzas.pizza_id GROUP BY pizza_types.category
<br>
ORDER BY revenue DESC;
<br>
<br>
• Analyze the cumulative revenue generated over time.	
<br>
SELECT order_date,
<br>
SUM(revenue) OVER(ORDER BY order_date) as cum_revenue
<br>
FROM (
<br>
SELECT orders.order _date, SUM(orders_details.quantity * pizzas.price)
<br>
as revenue
<br>
FROM orders_details JOIN pizzas
<br>
ON orders_details.pizza_id = pizzas.pizza_id JOIN orders
<br>
ON orders.order _id= orders_details.order_id GROUP BY orders.order_date
<br>
) as sales;
<br>
<br>
• Determine the top 3 most ordered pizza types based on revenue for each pizza category.
<br>
select name, revenue from (select category, name, revenue,
<br>
rank() overtpartition by category order by revenue desc) as rn from
<br>
(select pizza_types.category, pizza_types.name, sum((orders_details.quantity) * pizzas.price)
<br>
as revenue
<br>
from pizza_types join pizzas
<br>
on pizza_types.pizza_type_id= pizzas.pizza_type_id join orders_details
<br>
on orders_details.pizza_id = pizzas.pizza_id
<br>
group by pizza_types.category, pizza_types.name) as a) as b
