### 1- Fix Names in a Table
<hr>

#### Write an SQL query to fix the names so that only the first character is uppercase and the rest are lowercase.

```MySQL
SELECT user_id , CONCAT(UPPER(SUBSTR(name,1,1)),LOWER(SUBSTR(name,2))) AS name 
FROM Users
ORDER BY user_id ASC
```
<br>

### 2- Patients With a Condition
<hr>

#### Write an SQL query to report the patient_id, patient_name and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with DIAB1 prefix.

```MySQL
select *
from Patients
where conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%'
```
<br>

### 3- Delete Duplicate Emails
<hr>

#### Write an SQL query to delete all the duplicate emails, keeping only one unique email with the smallest id. Note that you are supposed to write a DELETE statement and not a SELECT one.

```MySQL
delete p1 from person as p1 join person as p2
where p1.email = p2.email and p1.id > p2.id
```
<br>

### 4- Second Highest Salary
<hr>

#### Write an SQL query to report the second highest salary from the Employee table. If there is no second highest salary, the query should report null.

```MySQL
select IFNULL((select distinct salary 
from Employee
order by salary desc
limit 1 offset 1),null)as SecondHighestSalary
```
<br>

### 5- Group Sold Products By The Date
<hr>

#### Write an SQL query to find for each date the number of different products sold and their names.

#### The sold products names for each date should be sorted lexicographically.

```MySQL
select sell_date, count( DISTINCT product ) as num_sold ,
GROUP_CONCAT( DISTINCT product order by product ASC separator ',' ) as products
FROM Activities 
GROUP BY sell_date 
order by sell_date ASC;
```
<br>

### 6- List the Products Ordered in a Period
<hr>

#### Write an SQL query to get the names of products that have at least 100 units ordered in February 2020 and their amount.

```MySQL
select product_name, sum(unit) as unit
from Orders left join Products using(product_id)
where month(order_date) = 2 
group by product_id
having sum(unit) >= 100
```
<br>

### 7- Find Users With Valid E-Mails
<hr>

#### Write an SQL query to find the users who have valid emails.

. A valid e-mail has a prefix name and a domain where:

. The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.
The domain is '@leetcode.com'.

```MySQL
SELECT *
FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9._-]*@leetcode\\.com'
```
<br>

