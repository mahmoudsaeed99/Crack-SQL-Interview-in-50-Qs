### 1- Recyclable and Low Fat Products
<hr>

#### Write an SQL query to find the ids of products that are both low fat and recyclable.

```MySQL
select product_id
from Products
where low_fats = "Y" 
and recyclable = "Y"
```
<br>

### 2- Find Customer Referee
<hr>

#### Write an SQL query to report the names of the customer that are not referred by the customer with id = 2.

```MySQL
select product_id
from Products
where low_fats = "Y" 
and recyclable = "Y"
```
<br>


### 3- Big Countries
<hr>

#### Write an SQL query to report the name, population, and area of the big countries.

```MySQL
select name , population , area
from World
where area >= 3000000 or population >= 25000000  
```
<br>

### 4- Article Views I
<hr>

#### Write an SQL query to find all the authors that viewed at least one of their own articles.
#### Return the result table sorted by id in ascending order.

```MySQL
select distinct author_id as id
from Views
where author_id = viewer_id
order by 1 asc
```
<br>

### 5- Invalid Tweets
<hr>

#### Write an SQL query to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.

```MySQL
select tweet_id
from Tweets
where length(content) > 15
```
<br>
