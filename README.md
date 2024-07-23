/* Questions & Answers */

**Q.1 Find the day with the highest total revenue and the corresponding revenue amount?**


<span style="color: red;">SELECT</span>

    DATE_FORMAT(STR_TO_DATE(order_date, '%Y-%m-%d'),   '%W') AS week_day,  
    SUM(total_price) AS total_revenue  
FROM   
      PIZZA_SALES  
GROUP BY week_day  
ORDER BY total_revenue DESC  
LIMIT 1;  




**Q.2 Calculate the cumulative revenue for each month.**

with   
total_sales_revenue as  
( select   
	 date_format(str_to_date(order_date , '%Y-%m-%d'), '%m') as month_number ,  
     date_format(str_to_date(order_date , '%Y-%m-%d') ,'%M') as month_1,  
     sum(total_price) as total_revenue  
     FROM PIZZA_SALES  
     GROUP BY month_number , month_1 ),  
sales_rank as  
		( SELECT   
				month_number,  
				month_1,  
                total_revenue,  
                round((total_revenue - LAG(total_revenue) over ( order by month_number))/LAG(total_revenue) over (order by month_number) *100,2) as cumulative_revenue  
			  	from   
                total_sales_revenue  
                )
  	select   
		  	month_number,      
		    	month_1 ,   
			total_revenue ,  
            cumulative_revenue  
  	from   
  		sales_rank  
  	group by     
			month_number , month_1  
	order by   
			month_number  asc ;  





**Q3. Identify the top 3 most expensive pizzas (by unit price) and their total sales (quantity).**
  
   SELECT 
    pizza_name_id, unit_price, SUM(quantity) AS total_sales
FROM
    PIZZA_SALES
GROUP BY pizza_name_id , unit_price
ORDER BY unit_price DESC
LIMIT 3;



**Q4. Get the average order value (total_price) for each pizza category.**
SELECT 
    pizza_category,
    (total_reveue / COUNT(pizza_id)) AS average_revenue
FROM
    (SELECT 
        pizza_category, SUM(total_price) AS total_revenue
    FROM
        PIZZA_SALES)
GROUP BY pizza_category
ORDER BY average_revenue DESC
  ;




**Q.5 Determine the most popular pizza size for each day of the week.**

WITH PIZZA_SALES_REVENUE AS (
    SELECT 
        pizza_size,
        DATE_FORMAT(STR_TO_DATE(order_date, '%y-%m-%d'), '%W') AS week_day,
        DATE_FORMAT(STR_TO_DATE(order_date, '%y-%m-%d'), '%w') AS week_number,
        SUM(total_price) AS total_revenue
    FROM
        PIZZA_SALES
    GROUP BY 
        pizza_size, week_day, week_number
),
sales_rank AS (
    SELECT 
        pizza_size,
        week_day,
        week_number,
        total_revenue,
        ROW_NUMBER() OVER (PARTITION BY week_day, week_number ORDER BY total_revenue DESC) AS RN
    FROM
        PIZZA_SALES_REVENUE
)
SELECT 
    pizza_size,
    week_day,
    week_number,
    total_revenue
FROM 
    sales_rank
WHERE 
    RN = 1
ORDER BY 
    week_number ASC, week_day ASC;



**Q.6 Calculate the percentage contribution of each pizza category to the total revenue.**


select
	  pizza_category,
      (total_value / total_revenue ) *100 as percentile_revenue
      from
		( select
          pizza_category,
          sum(total_price) as total_value ,
          (select sum(total_price) from pizza_sales ) as total_revenue
FROM PIZZA_SALES
group by pizza_category) as total_category

order by 
percentile_revenue  desc ;


Q.7  


