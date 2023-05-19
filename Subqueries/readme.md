### 1- Employees Whose Manager Left the Company
<hr>

#### Write an SQL query to report the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.

```MySQL
select employee_id
from Employees
where manager_id not in (select employee_id from Employees)
and salary < 30000
order by employee_id
```
<br>

### 2- Exchange Seats
<hr>

#### Write an SQL query to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

```MySQL
select
case when id%2 !=0 and id =(select count(*) from seat) then id
when id%2 = 0 then id-1
else id+1
end as id, student
from seat
order by id
```
<br>

### 3- Movie Rating
<hr>

#### Write an SQL query to:

#### Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.

#### Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.

```MySQL
(select name as results
from Users left join MovieRating using(user_id)
group by user_id
order by count(*) desc , name asc
limit 1)
UNION
(select title as results
from Movies left join MovieRating using(movie_id)
where month(created_at) = 2
group by movie_id
order by avg(rating) desc , title asc
limit 1);
```
<br>

### 4- Restaurant Growth
<hr>

#### Write an SQL query to compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). average_amount should be rounded to two decimal places.

```MySQL
SELECT a.visited_on AS visited_on, SUM(b.day_sum) AS amount,
       ROUND(AVG(b.day_sum), 2) AS average_amount
FROM
  (SELECT visited_on, SUM(amount) AS day_sum FROM Customer GROUP BY visited_on ) a,
  (SELECT visited_on, SUM(amount) AS day_sum FROM Customer GROUP BY visited_on ) b
WHERE DATEDIFF(a.visited_on, b.visited_on) BETWEEN 0 AND 6
GROUP BY a.visited_on
HAVING COUNT(b.visited_on) = 7
```
<br>

### 5- Friend Requests II: Who Has the Most Friends
<hr>

#### Write an SQL query to find the people who have the most friends and the most friends number.

```MySQL
select t.id , sum(t.num) as num
from(
select requester_id as id , count(*) as num
from RequestAccepted
group by 1
union all
select accepter_id as id , count(*) as num
from RequestAccepted
group by 1) as t
group by 1
order by 2 desc
limit 1
```
<br>

### 6- Investments in 2016
<hr>

#### Write an SQL query to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:

#### have the same tiv_2015 value as one or more other policyholders, and

#### are not located in the same city like any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.



```MySQL
SELECT ROUND(SUM(TIV_2016), 2) AS TIV_2016 FROM insurance 
WHERE PID IN 
(SELECT PID FROM insurance GROUP BY LAT, LON HAVING COUNT(*) = 1) 
AND PID NOT IN
(SELECT PID FROM insurance GROUP BY TIV_2015 HAVING COUNT(*) = 1)
```
<br>

### 7- Department Top Three Salaries
<hr>

#### Write an SQL query to find the employees who are high earners in each of the departments.

```MySQL
SELECT Department, employee, salary FROM (
    SELECT d.name AS Department
        , e.name AS employee
        , e.salary
        , DENSE_RANK() OVER (PARTITION BY d.name ORDER BY e.salary DESC) AS drk
    FROM Employee e JOIN Department d ON e.DepartmentId= d.Id
) t WHERE t.drk <= 3
```
<br>

