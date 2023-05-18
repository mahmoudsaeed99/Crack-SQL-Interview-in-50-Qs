### 1- Number of Unique Subjects Taught by Each Teacher
<hr>

#### Write an SQL query to report the number of unique subjects each teacher teaches in the university.

```MySQL
select teacher_id , count(distinct subject_id) as cnt
from Teacher
group by teacher_id
```
<br>

### 2- User Activity for the Past 30 Days I
<hr>

#### Write an SQL query to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on someday if they made at least one activity on that day.

```MySQL
select activity_date as day , count(distinct user_id) as active_users
from Activity
where activity_type in ('open_session', 'end_session', 'scroll_down', 'send_message')
and activity_date <= "2019-07-27" and activity_date > "2019-06-27"
group by day
```
<br>

### 3- Product Sales Analysis III
<hr>

#### Write an SQL query that selects the product id, year, quantity, and price for the first year of every product sold.

```MySQL
select product_id , year as first_year , quantity , price 
from Sales
where (product_id , year) in (select product_id , min(year) 
from Sales
group by 1)
```
<br>

### 4- Classes More Than 5 Students
<hr>

#### Write an SQL query to report all the classes that have at least five students.

```MySQL
select class
from Courses
group by class
having count(student) >= 5
```
<br>

### 5- Find Followers Count
<hr>

#### Write an SQL query that will, for each user, return the number of followers.

```MySQL
select user_id , count(follower_id) as followers_count
from Followers
group by 1
order by 1
```
<br>

### 6- Biggest Single Number
<hr>

#### Write an SQL query to report the largest single number. If there is no single number, report null.

```MySQL
select COALESCE( (select num 
from MyNumbers
group by num
having count(*) = 1
order by num desc
limit 1)
, null) as num
```
<br>

### 1- Customers Who Bought All Products
<hr>

#### Write an SQL query to report the customer ids from the Customer table that bought all the products in the Product table.

```MySQL
select customer_id
from Customer
group by 1
having sum(distinct product_key) = (select sum(distinct product_key) from Product)
```
<br>
