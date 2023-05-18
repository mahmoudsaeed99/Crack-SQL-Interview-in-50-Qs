### 1- Not Boring Movies
<hr>

#### Write an SQL query to report the movies with an odd-numbered ID and a description that is not "boring".

#### Return the result table ordered by rating in descending order.

```MySQL
select id , movie , description , rating
from Cinema
where description != "boring" and id%2 != 0
order by 4 desc
```
<br>

### 2-  Average Selling Price
<hr>

#### Write an SQL query to find the average selling price for each product. average_price should be rounded to 2 decimal places.

```MySQL
select p.product_id , round(sum(u.units * p.price) / sum(u.units) , 2) as average_price
from Prices as p
left join
unitsSold as u
on p.product_id = u.product_id and (u.purchase_date between p.start_date and p.end_date)
group by 1
```
<br>

### 3- Project Employees I
<hr>

#### Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits

```MySQL
select project_id , round(avg(experience_years),2) as average_years
from Project
left join Employee using(employee_id)
group by 1
```
<br>

### 4- Percentage of Users Attended a Contest
<hr>

#### Write an SQL query to find the percentage of the users registered in each contest rounded to two decimals.

#### Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.

```MySQL
select contest_id , round(count(*)/ (select count(*) from Users) * 100 , 2) as percentage
from Register
group by contest_id
order by percentage desc, contest_id asc
```
<br>

### 5- Queries Quality and Percentage
<hr>

#### Write an SQL query to find each query_name, the quality and poor_query_percentage.

#### Both quality and poor_query_percentage should be rounded to 2 decimal places.

```MySQL
select query_name , round(avg(rating/position) , 2) as quality
, round(avg(rating < 3 )*100 , 2) as poor_query_percentage
from Queries
group by query_name
```
<br>

### 6- Monthly Transactions I
<hr>

#### Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

```MySQL
SELECT 
LEFT(trans_date, 7) AS month, country, 
COUNT(id) AS trans_count, 
SUM(state = 'approved') AS approved_count, 
SUM(amount) AS trans_total_amount, 
SUM(CASE 
    WHEN state = 'approved' THEN amount 
    ELSE 0 
    END) AS approved_total_amount
FROM Transactions
GROUP BY month, country
```
<br>

### 7- Immediate Food Delivery II
<hr>

#### Write an SQL query to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

```MySQL
select ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) as immediate_percentage
from delivery where (customer_id, order_date) IN
(select customer_id, min(order_date) as first_order
from delivery
group by customer_id)
```
<br>

### 8- Game Play Analysis IV
<hr>

#### Write an SQL query to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

```MySQL
select round(count(t2.player_id) / count(t1.player_id) , 2) as fraction
from(
select player_id , min(event_date) as date
from Activity
group by player_id) as t1
left join Activity as t2
on t1.player_id = t2.player_id and t1.date = t2.event_date - 1 
```
<br>
