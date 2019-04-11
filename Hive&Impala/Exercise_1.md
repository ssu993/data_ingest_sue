# Exercise 1

## 1. Which customer did your query identify as the winner of the $5,000 prize?
<pre><code>select * from customers where city like "Kansas City" and fname like "Bridg%"</pre></code>
![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Hive&Impala/0번.PNG?raw=true)

## 2. Displays the three most expensive products.
<pre><code>select * from products order by price desc limit 3;</pre></code>
![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Hive&Impala/1번.PNG?raw=true)

## 3. Did bridget order the advertised tablet in May?
<pre><code>SELECT o.order_id, fname, lname, o.order_date
FROM customers c
JOIN orders o
   ON (c.cust_id = o.cust_id)
JOIN order_details d
   ON (o.order_id = d.order_id)
WHERE d.prod_id=1274348
  AND c.cust_id=1139477;</pre></code>
![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Hive&Impala/2번.PNG?raw=true)

## 4. Find all the Gigabux brand products whose price is less than 1000.
<pre><code>select * from products where brand like "Gigabux" and price < 1000;</pre></code>
![screenshot_20171221-151714](https://github.com/ssu993/data_ingest_sue/blob/master/Hive&Impala/3번.PNG?raw=true)
