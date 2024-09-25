---
title: 'LeetCode Crack SQL Interview in 50 Qs'
date: 2024-09-18
permalink: /posts/2012/08/blog-post-1/
tags:
  - SQL
  - LeetCode
  - Data Engineer
---
Q-1075. Project Employees I
      Easy 

Table: Project

| Column Name | Type    |
|-------------|---------|
| project_id  | int     |
| employee_id | int     |
|-------------|---------|

(project_id, employee_id) is the primary key of this table.
employee_id is a foreign key to Employee table.
Each row of this table indicates that the employee with employee_id is working on the project with project_id.


Table: Employee

| Column Name      | Type    |
|------------------|---------|
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
|------------------|---------|

employee_id is the primary key of this table. It's guaranteed that experience_years is not NULL.
Each row of this table contains information about one employee.


Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

Return the result table in any order.

The query result format is in the following example.



Example 1:

Input:
Project table:

| project_id  | employee_id |
|-------------|-------------|
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |

Employee table:

| employee_id | name   | experience_years |
|-------------|--------|------------------|
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |

Output:

| project_id  | average_years |
|-------------|---------------|
| 1           | 2.00          |
| 2           | 2.50          |
|-------------|---------------|

Explanation: The average experience years for the first project is (3 | 2 | 1) / 3 = 2.00 and for the second project is (3 | 2) / 2 = 2.50

Solution:
------------------------------------------
------------------------------------------
```sql
select project_id,round(avg(experience_years/1.0),2) as 'average_years' from project join employee
on project.employee_id =employee.employee_id
group by project_id;

```



Q-1934. Confirmation Rate
      Medium



Table: Signups


| Column Name    | Type     |
|----------------|----------|
| user_id        | int      |
| time_stamp     | datetime |
|----------------|----------|

user_id is the column of unique values for this table.
Each row contains information about the signup time for the user with ID user_id.


Table: Confirmations

| Column Name    | Type     |
|----------------|----------|
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
|----------------|----------|

(user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.
user_id is a foreign key (reference column) to the Signups table.
action is an ENUM (category) of the type ('confirmed', 'timeout')
Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').


The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.

Write a solution to find the confirmation rate of each user.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Signups table:

| user_id | time_stamp          |
|---------|---------------------|
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |
|---------|---------------------|

Confirmations table:

| user_id | time_stamp          | action    |
|---------|---------------------|-----------|
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |
|---------|---------------------|-----------|

Output:

| user_id | confirmation_rate |
|---------|-------------------|
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |
|---------|-------------------|

Explanation:
User 6 did not request any confirmation messages. The confirmation rate is 0.
User 3 made 2 requests and both timed out. The confirmation rate is 0.
User 7 made 3 requests and all were confirmed. The confirmation rate is 1.
User 2 made 2 requests where one was confirmed and the other timed out. The confirmation rate is 1 / 2 = 0.5.


Solution:
------------------------------------------
------------------------------------------

```sql

with CTE as(
select s.user_id,count() as conf from Signups as s left join Confirmations as c
on s.user_id        = c.user_id where action = 'confirmed' group by s.user_id  ),
CTE2 as(
select ss.user_id,count() as al from Signups as ss left join Confirmations as cc
on ss.user_id= cc.user_id group by ss.user_id
)

Select CTE2.user_Id as user_id ,IFNULL(CTE.conf / CTE2.al, 0) as  confirmation_rate from CTE right join CTE2 on
CTE.user_Id = CTE2.user_Id;

```

Q-1193. Monthly Transactions I
      Medium


Table: Transactions


| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| country       | varchar |
| state         | enum    |
| amount        | int     |
| trans_date    | date    |
|---------------|---------|

id is the primary key of this table.
The table has information about incoming transactions.
The state column is an enum of type ["approved", "declined"].


Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

Return the result table in any order.

The query result format is in the following example.



Example 1:

Input:
Transactions table:

| id   | country | state    | amount | trans_date |
|------|---------|----------|--------|------------|
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |
|------|---------|----------|--------|------------|

Output:

| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
|----------|---------|-------------|----------------|--------------------|-----------------------|
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
|----------|---------|-------------|----------------|--------------------|-----------------------|


```sql

# Write your MySQL query statement below

select
    DATE_FORMAT(trans_date , '%Y-%m')  as month,
country , count(*) as trans_count,
sum(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) as approved_count,
sum(amount) as trans_total_amount,
SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) as approved_total_amount

from Transactions group by DATE_FORMAT(trans_date , '%Y-%m'),country;

```


Q-197. Rising Temperature
Easy


Table: Weather

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
|---------------|---------|

id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.


Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Weather table:

| id | recordDate | temperature |
|----|------------|-------------|
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
|----|------------|-------------|

Output:

| id |
|----|
| 2  |
| 4  |
|----|

Explanation:
In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
In 2015-01-04, the temperature was higher than the previous day (20 -> 30).


```sql 
# Write your MySQL query statement below
with CTE as (
select id,lag(temperature,1) over() as temp1,lag(recordDate,1) over() as recordDate1,temperature,recordDate from weather
order by recordDate
)

Select id from CTE 
 where temperature-COALESCE(temp1,1110)>0 and TIMESTAMPDIFF(DAY, recordDate1, recordDate) =1  ;

```

Question:1661. Average Time of Process per Machine
      Easy
      SQL Schema
      Table: Activity

| Column Name    | Type    |
|---------------|--------|
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
|---------------|--------|

The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
It is guaranteed that each (machine_id, process_id) pair has a 'start' and 'end' timestamp.


There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Activity table:

| machine_id | process_id | activity_type | timestamp |
|-----------|-----------|--------------|----------|
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
|-----------|-----------|--------------|----------|

Output:

| machine_id | processing_time |
|-----------|----------------|
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
|-----------|----------------|

Explanation:
There are 3 machines running 2 processes each.
Machine 0's average time is ((1.520 - 0.712) |(4.120 - 3.140)) / 2 = 0.894
Machine 1's average time is ((1.550 - 0.550) |(1.420 - 0.430)) / 2 = 0.995
Machine 2's average time is ((4.512 - 4.100) |(5.000 - 2.500)) / 2 = 1.456


```sql

    WITH CTE AS (
    SELECT 
        machine_id, 
        activity_type, 
        AVG(timestamp) AS processing_time
    FROM Activity
    GROUP BY machine_id, activity_type
),
CTE2 AS (
    SELECT 
        machine_id, 
        activity_type, 
        processing_time,
        LEAD(processing_time) OVER (PARTITION BY machine_id ORDER BY processing_time) AS lead_processing_time
    FROM CTE
)
SELECT machine_id,round((lead_processing_time-processing_time ),3) as processing_time
FROM CTE2 where lead_processing_time is not null;

```


Question:1045. Customers Who Bought All Products
      Medium
      Table: Customer

| Column Name | Type    |
|-------------|---------|
| customer_id | int     |
| product_key | int     |
|-------------|---------|
This table may contain duplicates rows.
customer_id is not NULL.
product_key is a foreign key (reference column) to Product table.


Table: Product

| Column Name | Type    |
|-------------|---------|
| product_key | int     |
|-------------|---------|
product_key is the primary key (column with unique values) for this table.


Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Customer table:

| customer_id | product_key |
|-------------|-------------|
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
|-------------|-------------|
Product table:

| product_key |
|-------------|
| 5           |
| 6           |
|-------------|

Output:

| customer_id |
|-------------|
| 1           |
| 3           |
|-------------|
Explanation:
The customers who bought all the products (5 and 6) are customers with IDs 1 and 3.

```sql


SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);


```


Question: 196. Delete Duplicate Emails
     Easy
     Table: Person

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |
|-------------|---------|

id is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.


Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

For SQL users, please note that you are supposed to write a DELETE statement and not a SELECT one.

For Pandas users, please note that you are supposed to modify Person in place.

After running your script, the answer shown is the Person table. The driver will first compile and run your piece of code and then show the Person table. The final order of the Person table does not matter.

The result format is in the following example.



Example 1:

Input:
Person table:

| id | email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
|----|------------------|

Output:

| id | email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
|----|------------------|

Explanation: john@example.com is repeated two times. We keep the row with the smallest Id = 1.

```SQL

DELETE FROM Person
WHERE id NOT IN (
    SELECT id FROM (
        SELECT MIN(id) AS id
        FROM Person
        GROUP BY email
    ) AS subquery
);

```


Question:176. Second Highest Salary
Medium

Table: Employee


| Column Name | Type |
|-------------|------|
| id          | int  |
| salary      | int  |
|-------------|------|
id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.


Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return null (return None in Pandas).

The result format is in the following example.



Example 1:

Input:
Employee table:

| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
|----|--------|
Output:

| SecondHighestSalary |
|---------------------|
| 200                 |
|---------------------|
Example 2:

Input:
Employee table:

| id | salary |
|----|--------|
| 1  | 100    |
|----|--------|
Output:

| SecondHighestSalary |
|---------------------|
| null                |
|---------------------|


```SQL

WITH CTE AS (
    SELECT Salary, dense_RANK() OVER (ORDER BY Salary DESC) AS s_rank
    FROM Employee
)
SELECT 
    COALESCE((
        SELECT Salary 
        FROM CTE 
        WHERE s_rank = 2
        LIMIT 1
    ), NULL) AS SecondHighestSalary;

```