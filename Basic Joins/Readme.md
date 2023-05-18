### 1- Replace Employee ID With The Unique Identifier
<hr>
Table: Employees                           

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
Table: EmployeeUNI
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
<br>
#### Write an SQL query to show the unique ID of each user, If a user does not have a unique ID replace just show null

```MySQL
select unique_id , name
from Employees left join EmployeeUNI using(id)
```
<br>
### 2- Product Sales Analysis I
<hr>
Table: Sales

+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
Table: Product

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
<br>
#### Write an SQL query that reports the product_name, year, and price for each sale_id in the Sales table.

```MySQL
select product_name , year , price
from Sales left join Product using(product_id)
```
<br>
### 3- Customer Who Visited but Did Not Make Any Transactions
<hr>
Table: Visits

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
Table: Transactions

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
<br>
#### Write a SQL query to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

```MySQL
select customer_id , count(*) as count_no_trans
from Visits
where visit_id not in  (select distinct visit_id
from Transactions)

group by 1
order by 2 desc
```
<br>
### 4- Rising Temperature
<hr>
Table: Weather

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
<br>
#### Write an SQL query to find all dates' Id with higher temperatures compared to its previous dates (yesterday).

```MySQL
select t1.id
from Weather t1 ,Weather t2
where t1.recordDate  = DATE_ADD(t2.recordDate, INTERVAL 1 DAY) and t1.temperature > t2.temperature
```
<br>

### 5- Average Time of Process per Machine
<hr>
Table: Activity

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
<br>
#### Write an SQL query to find the average time each machine takes to complete a process.

```MySQL
SELECT s.machine_id, ROUND(AVG(e.timestamp-s.timestamp), 3) AS processing_time
# select * 
FROM Activity s JOIN Activity e ON
    s.machine_id = e.machine_id AND s.process_id = e.process_id AND
    s.activity_type = 'start' AND e.activity_type = 'end'
GROUP BY s.machine_id
```
<br>
### 6- Employee Bonus
<hr>
Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
Table: Bonus

+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
<br>
#### Write an SQL query to report the name and bonus amount of each employee with a bonus less than 1000.

```MySQL
select name , bonus
from Employee as t1 
left join Bonus as t2
on t1.empId = t2.empId
where bonus < 1000 or bonus is null
```
<br>
### 7- Students and Examinations
<hr>
Table: Students

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
Table: Subjects

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
<br>
#### Write an SQL query to find the number of times each student attended each exam.

```MySQL
SELECT student.student_id,student.student_name,subject.subject_name,COUNT(exam.subject_name) as attended_exams
FROM Students as student
JOIN Subjects as subject
LEFT JOIN Examinations as exam
ON student.student_id=exam.student_id AND subject.subject_name=exam.subject_name
GROUP BY student.student_id,subject.subject_name
ORDER BY student_id,subject_name;
```
<br>
### 8- Managers with at Least 5 Direct Reports
<hr>
Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
<br>
#### Write an SQL query to report the managers with at least five direct reports.

```MySQL
select name
from Employee
where id in 
(select distinct managerId 
from Employee
group by managerId
having count(*) >= 5)
```
<br>
### 9- Confirmation Rate
<hr>
Table: Signups

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
Table: Confirmations

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
<br>
#### Write an SQL query to find the confirmation rate of each user.

```MySQL
select s.user_id ,round(ifnull(sum(c.action = "confirmed") / count(c.action),0),2) as 
confirmation_rate
from Signups as s left join Confirmations as c
on s.user_id = c.user_id
group by s.user_id
```
<br>
