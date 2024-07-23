#  Questions & Answers 

# Q.1 Find the day with the highest total revenue and the corresponding revenue amount?

**SELECT**  
    DATE_FORMAT(STR_TO_DATE(order_date, '%Y-%m-%d'), '%W') **AS** week_day,  
    SUM(total_price) **AS** total_revenue  
**FROM**   
      PIZZA_SALES  
**GROUP BY** week_day  
**ORDER BY** total_revenue DESC  
**LIMIT** 1;  





# Q.2 Identify the top 3 most expensive pizzas (by unit price) and their total sales (quantity)?
  
   **SELECT**   
    pizza_name_id, unit_price, SUM(quantity) **AS** total_sales  
**FROM**  
    PIZZA_SALES  
**GROUP BY** pizza_name_id , unit_price  
**ORDER BY** unit_price DESC  
**LIMIT** 3;  




# Q.3 Get the average order value (total_price) for each pizza category ?

**SELECT**   
    pizza_category,  
    (total_reveue / COUNT(pizza_id)) **AS** average_revenue  
**FROM**  
    (**SELECT**   
        pizza_category, SUM(total_price) **AS** total_revenue  
    **FROM**  
        PIZZA_SALES)  
**GROUP BY** pizza_category  
**ORDER BY** average_revenue **DESC**  
  ;




# Q.4 Calculate the percentage contribution of each pizza category to the total revenue ?


**SELECT**  
	  pizza_category,    
      (total_value / total_revenue ) *100 **AS** percentile_revenue    
      **FROM**  
		( select  
          pizza_category,  
          sum(total_price) **AS** total_value ,  
          (**SELECT** sum(total_price) **FROM** pizza_sales ) **AS** total_revenue  
**FROM** PIZZA_SALES  
**GROUP BY** pizza_category) **AS** total_category  
**ORDER BY**   
percentile_revenue  **DESC** ;


# Q.5 Find the average quantity of pizzas ordered per order for each pizza size ?

**SELECT**   
    pizza_size,  
    (SUM(quantity) / COUNT(pizza_id)) **AS** AVERAGE  
**FROM**   
    PIZZA_SALES  
**GROUP BY**   
    pizza_size  
**ORDER BY**   
    AVERAGE **DESC**;  


# Q.6 Get the total revenue for each combination of pizza size and pizza category ?


**SELECT**  
    pizza_size,   
    pizza_category,  
    sum(total_price) **AS** revenue  
**FROM** PIZZA_SALES  
**GROUP BY**   
		pizza_size, pizza_category  
**ORDER BY** revenue **DESC** ; 


# Q.7 Identify the pizza that had the highest number of orders in the last month ?


**SELECT**   
date_format(str_to_date(order_date , ' %y-%m-%d'), '%Y-%m') **AS**  MONTH_number,  
date_format(str_to_date(order_date , ' %y-%m-%d'), '%Y-%M') **AS** MONTH_NAME,  
pizza_name_id,  
count(pizza_id) **AS** TOTAL_ORDERS  
**FROM**  
	PIZZA_SALES  
**GROUP BY** MONTH_NAME ,MONTH_number , pizza_name_id   
**ORDER BY**   
        MONTH_number **DESC** , TOTAL_ORDERS **DESC**  
        **LIMIT** 1 ;  


# Q.8 Determine the correlation between order time and total revenue ?


**SELECT**  
    order_time,  
    SUM(total_price) **AS** revenue  
**FROM**   
    PIZZA_SALES  
**GROUP BY**   
    order_time  
**ORDER BY**   
    order_time;  



# Q.9 Determine the most popular pizza size for each day of the week ?


**WITH** PIZZA_SALES_REVENUE **AS** (  
    **SELECT**   
        pizza_size,  
        DATE_FORMAT(STR_TO_DATE(order_date, '%y-%m-%d'), '%W') **AS** week_day,  
        DATE_FORMAT(STR_TO_DATE(order_date, '%y-%m-%d'), '%w') **AS** week_number,  
        SUM(total_price) **AS** total_revenue  
    **FROM**  
        PIZZA_SALES     
    **GROUP BY**    
        pizza_size, week_day, week_number  
),  
sales_rank **AS** (  
    **SELECT**   
        pizza_size,  
        week_day,  
        week_number,  
        total_revenue,  
        **ROW_NUMBER**() **OVER** (**PARTITION BY** week_day, week_number **ORDER BY** total_revenue **DESC**) **AS** RN   
    **FROM**  
        PIZZA_SALES_REVENUE  
)
**SELECT**   
    pizza_size,  
    week_day,  
    week_number,  
    total_revenue  
**FROM**  
    sales_rank   
**WHERE**  
    RN = 1  
**ORDER BY**   
    week_number **ASC**, week_day **ASC**; 


    


# Q.10 Calculate the cumulative revenue for each month ?


**WITH**   
total_sales_revenue **AS**  
( **SELECT**   
	 date_format(str_to_date(order_date , '%Y-%m-%d'), '%m') **AS** month_number ,  
     date_format(str_to_date(order_date , '%Y-%m-%d') ,'%M') **AS** month_1,  
     sum(total_price) **AS** total_revenue  
    **FROM** PIZZA_SALES  
     **GROUP BY** month_number , month_1 ),  
sales_rank **AS**  
		( **SELECT**     
				month_number,    
				month_1,    
                total_revenue,    
                round((total_revenue - **LAG**(total_revenue) **over** ( **order by** month_number))/**LAG**(total_revenue) **OVER** (**ORDER BY** month_number) *100,2) **AS**  
 cumulative_revenue        
			  	**FROM**     
                total_sales_revenue      
                )  
  	**SELECT**    
		  	month_number,          
		    	month_1 ,     
			total_revenue ,    
            cumulative_revenue    
  	**FROM**   
  		sales_rank    
  	**GROUP BY**       
			  month_number , month_1   
	**ORDER BY**       
			month_number  **ASC** ;
