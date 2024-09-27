---
title: 'LeetCode Crack SQL Interview in 50 Qs'
date: 2024-09-18
permalink: /posts/2012/08/blog-post-1/
tags:
  - SQL
  - LeetCode
  - Data Engineer
---
## Q-1075. Project Employees I
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



## Q-1934. Confirmation Rate
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

## Q-1193. Monthly Transactions I
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


## Q-197. Rising Temperature
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

## Question:1661. Average Time of Process per Machine
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


## Question:1045. Customers Who Bought All Products
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


## Question: 196. Delete Duplicate Emails
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


## Question:176. Second Highest Salary
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

## Question-185. Department Top Three Salaries
     Hard
     Table: Employee

| Column Name  | Type    |
|--------------|---------|
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
|--------------|---------|

id is the primary key (column with unique values) for this table.
departmentId is a foreign key (reference column) of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.


Table: Department

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
|-------------|---------|

id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of a department and its name.


A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.

Write a solution to find the employees who are high earners in each of the departments.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Employee table:

| id | name  | salary | departmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
|----|-------|--------|--------------|

Department table:

| id | name  |
|----|-------|
| 1  | IT    |
| 2  | Sales |
|----|-------|

Output:

| Department | Employee | Salary |
|------------|----------|--------|
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
|------------|----------|--------|

Explanation:
In the IT department:
- Max earns the highest unique salary
- Both Randy and Joe earn the second-highest unique salary
- Will earns the third-highest unique salary

In the Sales department:
- Henry earns the highest salary
- Sam earns the second-highest salary
- There is no third-highest salary as there are only two employees


```SQL

WITH CTE AS (
    SELECT 
        Employee.id, 
        Employee.name as Employee ,
        Employee.Salary,
        Employee.departmentId,
        Department.name as Department ,
        DENSE_RANK() OVER (PARTITION BY Employee.departmentId ORDER BY Employee.Salary DESC) AS s_rank
    FROM 
        Employee 
    JOIN 
        Department ON Employee.departmentId = Department.id 
)
SELECT 
    Department,
    Employee , 
    Salary
FROM 
    CTE 
WHERE 
    s_rank <4; 

```

## Question-577. Employee Bonus
     Easy
     Table: Employee

| Column Name | Type    |
|-------------|---------|
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
|-------------|---------|

empId is the column with unique values for this table.
Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.


Table: Bonus

| Column Name | Type |
|-------------|------|
| empId       | int  |
| bonus       | int  |
|-------------|------|

empId is the column of unique values for this table.
empId is a foreign key (reference column) to empId from the Employee table.
Each row of this table contains the id of an employee and their respective bonus.


Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:

Employee table:

| empId | name   | supervisor | salary |
|-------|--------|------------|--------|
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
|-------|--------|------------|--------|

Bonus table:

| empId | bonus |
|-------|-------|
| 2     | 500   |
| 4     | 2000  |
|-------|-------|

Output:

| name | bonus |
|------|-------|
| Brad | null  |
| John | null  |
| Dan  | 500   |
|------|-------|


```SQL

# Write your MySQL query statement below
select employee.name,bonus.bonus from Employee left join  bonus on employee.empId = Bonus.empId
where Bonus.bonus<1000 
or Bonus.bonus is null;

```


## Question:1280. Students and Examinations
      Easy
      Table: Students

| Column Name   | Type    |
|---------------|---------|
| student_id    | int     |
| student_name  | varchar |
|---------------|---------|

student_id is the primary key (column with unique values) for this table.
Each row of this table contains the ID and the name of one student in the school.


Table: Subjects

| Column Name  | Type    |
|--------------|---------|
| subject_name | varchar |
|--------------|---------|

subject_name is the primary key (column with unique values) for this table.
Each row of this table contains the name of one subject in the school.


Table: Examinations

| Column Name  | Type    |
|--------------|---------|
| student_id   | int     |
| subject_name | varchar |
|--------------|---------|

There is no primary key (column with unique values) for this table. It may contain duplicates.
Each student from the Students table takes every course from the Subjects table.
Each row of this table indicates that a student with ID student_id attended the exam of subject_name.


Write a solution to find the number of times each student attended each exam.

Return the result table ordered by student_id and subject_name.

The result format is in the following example.



Example 1:

Input:

Students table:

| student_id | student_name |
|------------|--------------|
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
|------------|--------------|

Subjects table:

| subject_name |
|--------------|
| Math         |
| Physics      |
| Programming  |
|--------------|

Examinations table:

| student_id | subject_name |
|------------|--------------|
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
|------------|--------------|

Output:

| student_id | student_name | subject_name | attended_exams |
|------------|--------------|--------------|----------------|
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
|------------|--------------|--------------|----------------|

Explanation:
The result table should contain all students and all subjects.
Alice attended the Math exam 3 times, the Physics exam 2 times, and the Programming exam 1 time.
Bob attended the Math exam 1 time, the Programming exam 1 time, and did not attend the Physics exam.
Alex did not attend any exams.
John attended the Math exam 1 time, the Physics exam 1 time, and the Programming exam 1 time.

```SQL

SELECT Students.student_id, student_name, Subjects.subject_name, COUNT(Examinations.student_id) AS attended_exams
FROM Students JOIN Subjects
LEFT JOIN Examinations
ON Students.student_id = Examinations.student_id AND Subjects.subject_name = Examinations.subject_name
GROUP BY Students.student_id, subject_name
order by  Students.student_id, Subjects.subject_name

```


## Question-620. Not Boring Movies
     Easy
     Table: Cinema

| Column Name    | Type     |
|----------------|----------|
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
|----------------|----------|

id is the primary key (column with unique values) for this table.
Each row contains information about the name of a movie, its genre, and its rating.
rating is a 2 decimal places float in the range [0, 10]


Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

The result format is in the following example.



Example 1:

Input:
Cinema table:

| id | movie      | description | rating |
|----|------------|-------------|--------|
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
|----|------------|-------------|--------|

Output:

| id | movie      | description | rating |
|----|------------|-------------|--------|
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
|----|------------|-------------|--------|

Explanation:
We have three movies with odd-numbered IDs: 1, 3, and 5. The movie with ID = 3 is boring so we do not include it in the answer.


```SQL
# Write your MySQL query statement below
select * from Cinema
where id%2 =1 and description != 'boring'
order by rating  desc; 

```


## Questions-1251. Average Selling Price
      Easy
      Table: Prices


| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
|---------------|---------|

(product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the price of the product_id in the period from start_date to end_date.
For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.


Table: UnitsSold


| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
|---------------|---------|

This table may contain duplicate rows.
Each row of this table indicates the date, units, and product_id of each product sold.


Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places. If a product does not have any sold units, its average selling price is assumed to be 0.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Prices table:

| product_id | start_date | end_date   | price  |
|------------|------------|------------|--------|
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |
|------------|------------|------------|--------|

UnitsSold table:

| product_id | purchase_date | units |
|------------|---------------|-------|
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |
|------------|---------------|-------|

Output:

| product_id | average_price |
|------------|---------------|
| 1          | 6.96          |
| 2          | 16.96         |
|------------|---------------|

Explanation:
Average selling price = Total Price of Product / Number of products sold.
Average selling price for product 1 = ((100 * 5) | (15 * 20)) / 115 = 6.96
Average selling price for product 2 = ((200 * 15) | (30 * 30)) / 230 = 16.96



```sql

# Write your MySQL query statement below
select Prices.product_id, round(sum(coalesce(price,0)*coalesce(units,0))/sum(coalesce(units,0)),2) as average_price from prices left join UnitsSold

on Prices.product_id=UnitsSold.product_id
where purchase_date between start_date  and end_date   
group by Prices.product_id;

```

## Question-1211. Queries Quality and Percentage
    Easy
    Table: Queries

| Column Name | Type    |
|-------------|---------|
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
|-------------|---------|

This table may have duplicate rows.
This table contains information collected from some queries on a database.
The position column has a value from 1 to 500.
The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.


We define query quality as:

The average of the ratio between query rating and its position.

We also define poor query percentage as:

The percentage of all queries with rating less than 3.

Write a solution to find each query_name, the quality and poor_query_percentage.

Both quality and poor_query_percentage should be rounded to 2 decimal places.

Return the result table in any order.

The result format is in the following example.



Example 1:

Input:
Queries table:

| query_name | result            | position | rating |
|------------|-------------------|----------|--------|
| Dog        | Golden Retriever  | 1        | 5      |
| Dog        | German Shepherd   | 2        | 5      |
| Dog        | Mule              | 200      | 1      |
| Cat        | Shirazi           | 5        | 2      |
| Cat        | Siamese           | 3        | 3      |
| Cat        | Sphynx            | 7        | 4      |
|------------|-------------------|----------|--------|

Output:

| query_name | quality | poor_query_percentage |
|------------|---------|-----------------------|
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |
|------------|---------|-----------------------|

Explanation:
Dog queries quality is ((5 / 1) | (5 / 2) | (1 / 200)) / 3 = 2.50
Dog queries poor_ query_percentage is (1 / 3) * 100 = 33.33

Cat queries quality equals ((2 / 5) | (3 / 3) | (4 / 7)) / 3 = 0.66
Cat queries poor_ query_percentage is (1 / 3) * 100 = 33.33


```SQL

# Write your MySQL query statement below
select query_name ,round(avg(rating/position ),2)  as quality , round(avg(if( rating <3 , 1,0))*100,2)  as poor_query_percentage  from Queries where query_name is not null

Group by query_name

```




## Questions-1174. Immediate Food Delivery II
    Medium

Table: Delivery

| Column Name                 | Type    |
|-----------------------------|---------|
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
|-----------------------------|---------|

delivery_id is the column of unique values of this table.
The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).


If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.

The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

The result format is in the following example.



Example 1:

Input:
Delivery table:

| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|-----------------------------|
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |
|-------------|-------------|------------|-----------------------------|

Output:

| immediate_percentage |
|----------------------|
| 50.00                |
|----------------------|

Explanation:
The customer id 1 has a first order with delivery id 1 and it is scheduled.
The customer id 2 has a first order with delivery id 2 and it is immediate.
The customer id 3 has a first order with delivery id 5 and it is scheduled.
The customer id 4 has a first order with delivery id 7 and it is immediate.
Hence, half the customers have immediate first orders.






```SQL

WITH CTE AS (
    SELECT *,
        
        DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS `rank`
    FROM 
        Delivery
)

SELECT 
    ROUND(SUM(CASE WHEN order_date=customer_pref_delivery_date THEN 1 ELSE 0 END)/count(DISTINCT customer_id)*100, 2) as  immediate_percentage
    from 
    CTE
WHERE 
    cte.`rank` = 1;


```