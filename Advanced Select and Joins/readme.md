### 1- The Number of Employees Which Report to Each Employee
<hr>

#### Write an SQL query to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.

```MySQL
select e.employee_id , e.name , r.reports_count , r.average_age
from Employees as e right join ( 
select reports_to , count(*) as reports_count , round(avg(age)) as average_age
from Employees
where reports_to is not null
group by 1 
order by 1 ) as r on e.employee_id = r.reports_to
```
<br>

### 2- Primary Department for Each Employee
<hr>

#### Write an SQL query to report all the employees with their primary department. For employees who belong to one department, report their only department.


```MySQL
SELECT employee_id, department_id 
FROM Employee
WHERE primary_flag = 'Y'
UNION
SELECT employee_id, department_id 
FROM Employee 
GROUP BY employee_id
HAVING COUNT(employee_id) = 1
```
<br>

### 3- Triangle Judgement
<hr>

#### Write an SQL query to report for every three line segments whether they can form a triangle.

```MySQL
SELECT *, IF(x+y>z and x+z>y and y+z>x, 'Yes', 'No') as triangle 
FROM triangle
```
<br>

### 4- Consecutive Numbers
<hr>

#### Write an SQL query to find all numbers that appear at least three times consecutively.

```MySQL
select distinct(t1.num) as ConsecutiveNums 
from logs t1 , logs t2 , logs t3
where t1.id = t2.id + 1 and t2.id = t3.id + 1
and t1.num = t2.num and t3.num = t2.num 
```
<br>

### 5- Product Price at a Given Date
<hr>

#### Write an SQL query to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

```MySQL
select distinct t1.product_id  , ifNull(t2.new_price,10) as price
from Products as t1 left join 
(select product_id , new_price
from Products
where (product_id , change_date) in ( select product_id , max(change_date) as change_date
from Products
where change_date <= "2019-08-16"
group by 1)) as t2
on t1.product_id = t2.product_id
```
<br>

### 6- Last Person to Fit in the Bus
<hr>

#### Write an SQL query to find the person_name of the last person that can fit on the bus without exceeding the weight limit. The test cases are generated such that the first person does not exceed the weight limit.

```MySQL
SELECT q1.person_name
FROM Queue q1 JOIN Queue q2 ON q1.turn >= q2.turn
GROUP BY q1.turn
HAVING SUM(q2.weight) <= 1000
ORDER BY SUM(q2.weight) DESC
LIMIT 1
```
<br>

### 7- Count Salary Categories
<hr>

#### Write an SQL query to report the number of bank accounts of each salary category. The salary categories are:

#### ."Low Salary": All the salaries strictly less than $20000.
#### ."Average Salary": All the salaries in the inclusive range [$20000, $50000].
#### ."High Salary": All the salaries strictly greater than $50000.

```MySQL
select t1.category , count(t2.category) as accounts_count
from (select 'Low Salary' as category
 union
 select 'Average Salary' as category union select 'High Salary' as category) as t1
 left join
(select case when income < 20000 then "Low Salary"
when income between 20000 and 50000 then "Average Salary"
else "High Salary" 
end as category
from Accounts) as t2
on t1.category = t2.category
 group by 1
 order by 2 desc
```
<br>

