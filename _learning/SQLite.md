---
title: SQLite
except: Storing data in the easiest, most effective way
---



Once you obtain your data, you have to store it.

## CSV
The easiest way is in a CSV (comma separated files). It is just a text file with values as comma-separated and line separated for rows. Storage in CSV can be hard for strings where there are special characters such as commas, quotes, ect.

## SQLite
SQLite is the most popular embedded database in the world. It is self-contained (only one file), serverless (on your very computer), and low configuration. It can also have multiple tables. It also has schemas that allow for relationships and variable types. It can also work very well cross platform (windows, mac, linux). It is also easier to search as well.


### How to use SQLite

Create a new database
'''
sqlite3 new_database_name.db
'''

This will open sqlite's command prompt where you can do things to the database. For example:

Create a table
'''
create table student(id integer, name text)
'''

Insert data
'''
insert into student values (111, "Smith")
insert into student values (255, "Johnson")
'''

Query that data
'''
select * from student
'''

Use joins to combine tables
'''
select name from student, grades
where
student.id = grades.id
and
grades.course_id = 101
'''

You can also summarize the data via aggregation:
'''
select id, avg(grade)
from grades
group by id
'''

You can filter too
'''
select id, avg(grade)
from grades
group by id
having avg(grade) > 90;
'''

## Indexes
Indexes are really important for the speed of your query. Make sure to have them!


## FTS
FTS is virtual table modules that allow for full-text searches. Basically it's what Google does a search engine. 
