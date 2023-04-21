# SQL_queries

#Amazon want to know count of customer whether they are new customer or old customer on daily basis.

![image](https://user-images.githubusercontent.com/71668492/233616914-37ecec5a-9ab0-44f1-926f-42dbfc51a8aa.png)


select * from customer_orders;

--calculate count of customer whether it is  new customer or old customer

with first_visit as(
select customer_id,min(order_date) as first_visit_date
from customer_orders
group by customer_id)

select co.order_date,sum(case when order_date=first_visit_date then 1 else 0 end) as first_visit_flag,
sum(case when order_date!=first_visit_date then 1 else 0 end) as repeat_visit_flag
from customer_orders co
inner join first_visit fv on co.customer_id=fv.customer_id
group by order_date


