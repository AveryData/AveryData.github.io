---
title: SQL
except: SELECT * FROM X
---


## SQL Intro

In data science and analytics, data management is key. Data is only getting bigger and bigger, and being able to wrangle, slice, and understance extremely large data sets is only becoming more popular and important.
Severs and databases are a strong part of this domain. SQL is one of the most common ways to manage data sets in databases. 

SQL, pronounced 'Sequel' or 'S','Q','L', stands for Structure Query Language. It lets you access and manipulate databases. 

### SQL can:
- Get data from a database
- Add data to a database
- Edit data in a database
- Make databases
- Delete databases 

Semicolons seperate SQL statements to allow for more than one statment to be executed in the same call to the server. 

Really, the most common commands I typicially use are *SELECT* , *JOIN* , and *UNION*

### SELECT

### JOIN
A "JOIN" command is used to combine columns from seperate tables. 

#### INNER JOIN
Selects records that have matching values between both tables

#### LEFT JOIN
Only keeps records from the first table that match with the second table. 

#### RIGHT JOIN
Opposite of the LEFT JOIN and keeps records in the second table that match on the first. 

#### FULL JOIN
Sometimes also referenced to FULL OUTER JOIN, returns all records where there is any match in table 1 or table 2. 

#### SELF JOIN
A regular join, but with itself? Haha I'm not sure when you'd use this to be honest. 

### UNION
The UNION command combines the results of multiple SELECT statements. It stacks the results on top of eachother. 


## Comments in SQL
-- for a single line.
/* for mutliple lines */


## Keys

### Primary Keys
Primary keys are a field that can be used to uniquely identify each record in the table. They must contain unique values, and cannot contain NULL values.
A table can have only ONE primary key, and it can consist of mutliple columns. When multiple fields are used, it is called a **composite key**. 

### Foreign Keys
Foreign keys allow for linking between two tables. 



## SQL - More Advanced

### Views
SQL views are virtual tables. It is basically a saved SQL command to combine columns from multiple tables. The database **recreates** the data each time a user queries the view.


### Stored Procedures
Stored Procedures are SQL code that is saved to be used repeatidly. 



(See W3Schools for more infromation)[https://www.w3schools.com/sql]
