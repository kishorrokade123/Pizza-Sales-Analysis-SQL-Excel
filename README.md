# Pizza-Sales-Analysis-SQL-Excel
Pizza Sales analyzed by firing SQL queries and EXCEL used for visualisation
Below are the queries fired to cross check data correctness-
use Pizza_DB
--Total Revenue
select sum(total_price) as Total_Revenue
from [dbo].[pizza_sales]

select
*
from [dbo].[pizza_sales]
--Avg Order Value
select
sum(total_price)/count(distinct(order_id)) as Avg_Order_Value
from [dbo].[pizza_sales]

--Total Pizzas sold
select
SUM(quantity) as Total_Pizzas
from [dbo].[pizza_sales]

--Total Orders

select
count(distinct (order_id)) as Total_orders
from [dbo].[pizza_sales]

--Average Pizzas per order
select
cast(cast(sum(quantity) as decimal (10,2))/ cast(COUNT(distinct (order_id)) as decimal (10,2)) as decimal (10,2)) as Avg_Pizza_Orders
from [dbo].[pizza_sales]

--Chart requirement 
--Daily Trend
select
DATENAME(DW,order_date) as order_day ,count(distinct order_id) as Total_orders
from [dbo].[pizza_sales]
group by DATENAME(DW,order_date)

--Hourly Trend
select
datepart(hour,order_time) as order_hours ,count(distinct order_id) as Total_orders
from [dbo].[pizza_sales]
group by datepart(hour,order_time)
order by datepart(hour,order_time)

--% Sales by Pizza Category
select
pizza_category,sum(total_price) as Total_sales ,sum(total_price) * 100 / (select sum(total_price) from [dbo].[pizza_sales] where month(order_date) = 1) as Total_Sales_By_Category
from [dbo].[pizza_sales] 
where month(order_date) = 1
group by pizza_category

--%Sales by Oizza Size
select
pizza_size,sum(total_price) as Total_sales ,sum(total_price) * 100 / (select sum(total_price) from [dbo].[pizza_sales] ) as Total_Sales_By_Category
from [dbo].[pizza_sales] 
group by pizza_size

--Total Pizzas sold by Category
select 
pizza_category,sum(quantity) as Total_Pizzas_Sold
from [dbo].[pizza_sales]
group by pizza_category

--Top 5 Pizzas

select
top 5 pizza_name,sum(quantity) as Total_Price
from [dbo].[pizza_sales]
group by pizza_name
order by sum(total_price)  desc

--Worst 5 Pizzas
select
top 5 pizza_name,sum(quantity) as Total_Price
from [dbo].[pizza_sales]
group by pizza_name
order by sum(quantity)  asc
