PIZZA_SALES_USING_SQL

-- Retrieve the total number of orders placed.
SELECT COUNT(*) AS total_orders FROM orders;

-- Calculate the total revenue generated from pizza sales.
SELECT SUM(order_details.quantity * pizzas.price) AS total_revenue 
FROM order_details
JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id;

-- Identify the highest-priced pizza.
SELECT name, price 
FROM pizzas 
ORDER BY price DESC 
LIMIT 1;

-- Identify the most common pizza size ordered.
SELECT size, COUNT(*) AS order_count 
FROM pizzas 
GROUP BY size 
ORDER BY order_count DESC 
LIMIT 1;

-- List the top 5 most ordered pizza types along with their quantities.
SELECT pizza_types.name, SUM(order_details.quantity) AS total_quantity 
FROM pizza_types 
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name 
ORDER BY total_quantity DESC 
LIMIT 5;

-- Intermediate:
-- Join the necessary tables to find the total quantity of each pizza category ordered.
SELECT pizza_categories.name, SUM(order_details.quantity) AS total_quantity 
FROM pizza_categories 
JOIN pizza_types ON pizza_categories.category_id = pizza_types.category_id
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_categories.name;

-- Determine the distribution of orders by hour of the day.
SELECT HOUR(order_time) AS hour, COUNT(*) AS order_count 
FROM orders 
GROUP BY hour 
ORDER BY hour;

-- Join relevant tables to find the category-wise distribution of pizzas.
SELECT pizza_categories.name, COUNT(*) AS total_pizzas 
FROM pizza_categories 
JOIN pizza_types ON pizza_categories.category_id = pizza_types.category_id
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_categories.name;

-- Group the orders by date and calculate the average number of pizzas ordered per day.
SELECT order_date, AVG(total_pizzas) AS average_pizzas 
FROM (
    SELECT order_date, COUNT(*) AS total_pizzas 
    FROM orders 
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY order_date
) AS daily_orders 
GROUP BY order_date;

-- Determine the top 3 most ordered pizza types based on revenue.
SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue 
FROM pizza_types 
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order
