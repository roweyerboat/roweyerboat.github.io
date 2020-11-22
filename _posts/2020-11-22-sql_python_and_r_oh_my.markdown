---
layout: post
title:      "SQL, Python, and R Oh My!"
date:       2020-11-22 06:02:49 +0000
permalink:  sql_python_and_r_oh_my
---


![SQL logo](https://tse3.mm.bing.net/th?id=OIP.Ku0gGkdD0xDWs_pWugLk5AAAAA&pid=Api)


SQL or Structured Query Language is a foundational language in the Data Science realm. While there is big debates about R and Python, SQL stands on its own as a needed understanding for being able to pull data from relational databases. Both Python and R have ways to be able to use SQL queries within their languages.

### SQL and Python

![SQL and Python image](https://i1.wp.com/learn.onemonth.com/wp-content/uploads/2019/05/image3.png?resize=1000%2C297&ssl=1)

There are a few different libraries that allow you to connect to a database and use SQL queries to pull information from the databases.

Probably the most common library that is used to make the connection between python and SQL is SQLite. With SQLite, you are able to connect, store, and fetch data using an SQL server or simple SQL with Python.

Using SQLite and actually some of the other libraries that allow the connection between SQL and Python have some common methods: "connect()", "cursor()", "execute()", "fetchall()", etc. Here's the [SQLite documentation](https://sqlite.org/index.html). 

Here's an example of using the SQLite library to run a Select Statement:
```
# import the library
import sqlite3

# connect with the database titled myTable
connection = sqlite3.connect("myTable.db")

# initializing the cursor object
cursor = connection.cursor()

# execute the command to fetch all the data from the table emp in the database
cursor.execute("SELECT * FROM emp")

# store all the fetched date in the result variable
result = cursor.fetchall()

# loop to print all the data
for i in ans:
    print(i)
```

Notice the use of the cursor to be how you're directing where to pull the information. The SQL language used is the simple "SELECT * FROM emp" which means select all columns from the table with the title emp. You could use more complicated SQL commands. The key thing to remember is to execute and fetch. Execute is where the SQL language input goes and then the fetch commands brings out all that you queried. The fetch statement is like the commit statement. It's when the information is actually moved from one place to the other.

Another library is pyodbc. Here's the documentation for the [pyodbc library](https://github.com/mkleehammer/pyodbc/wiki). It is easy to install and once you've made the initial connection with a database, it is able to implement the SQL commands. It's called pyodbc is Oracle database is a highly used SQL database. Commands in Oracle are similar to MySQL, but not always the exact same.

Here are two examples of it in use with Select Statements:

```
# importing library that connects to the database
import pyodbc

# connecting to the server
conn = pyodbc.connect("Driver={SQL Server Native Client 11.0};"
                      "Server=PRASAD;"
                      "Database=SQL Tutorial;"
                      "Trusted_Connection=yes;")

# in python you use a "cursor" statement to navigate the database
cursor = conn.cursor()
cursor.execute('SELECT * FROM CustomerSale')

for row in cursor:
    print('row = %r' % (row,))
```
In this case, the fetch command didn't need to be used, but instead a for loop was used to pull the information from the cursor. 

The only line in that code that was SQL was 'SELECT * FROM CustomerSale' which selects all columns from the database titled "CustomerSale".

Another way to pull SQL queries in python is to use the "fetch" commands.

```
import pyodbc
conn = pyodbc.connect("Driver={SQL Server Native Client 11.0};"
                      "Server=PRASAD;"
                      "Database=SQL Tutorial;"
                      "Trusted_Connection=yes;")

cursor = conn.cursor()
cursor.execute('SELECT * FROM CustomerSale')
result = cursor.fetchone()
print(result)
```

This command will just fetch one, but you can also use "fetchall" to grab all. If you use limitations in your SQL query, then there isn't a need to limit with the fetch command. 

There are other libraries that make similar connections between Python and SQL. SQLite and pyodbc seem to be highly used.

### SQL and R

![SQL and R logos](https://tse1.mm.bing.net/th?id=OIP.kiIMUw3n7otk_5_5xYzWqwHaHa&pid=Api)

Similar to Python, R has libraries that can be used to connect to databases and handle the input of SQL queries. 

One library is DBI or R Database Interface. This library allows for connections to be made to databases within the R interface. Similar to the libraries for Python, the common methods are connecting, executing, and extracting results.
Here is the documentation for [DBI](https://www.rdocumentation.org/packages/DBI/versions/0.5-1)

An example of using DBI for queries:
```
library(DBI)
library(odbc)
con <- dbConnect(odbc::odbc(), "Oracle DB")

dbGetQuery(con,
    select "month_idx", "year", "month",
		sum(case when "term_deposit" = \'yes\' then 1.0 else 0.0 end) as subscribe,
		count(*) as total
		from  "bank"
		group by "month_idx", "year", "month"
')

```
You can see this is a more complicated Select Statement. To query with DBI, once you make the connection with the database, you can just copy paste an SQL command into the dbGetQuery() command with quotation marks wrapping it.

Another option with R is to write your code using dplyr syntax and then dplyr will do the translation into SQL. This is a great option for those less familiar with SQL.

```
q1 <- tbl(con, "bank") %>%
    group_by(month_idx, year, month) %>%
		summarise(
		    subscribe = sum(ifelse(term_deposit == "yes", 1, 0)),
				total = n())
show_query(q1)
```

This gets translated into SQL:
```
<SQL>
SELECT "month_idx", "year", "month", 
    SUM(CASE WHEN ("term_deposit" = 'yes') THEN (1.0) ELSE (0.0) END) AS "subscribe", COUNT(*) AS "total
FROM ("bank")
GROUP BY "month_idx", "year", "month"
```
 
However, probably the smoothest use of SQL in an R notebook is if you're using RStudio IDE, you can start a new code chunk with {sql} and then specify the connection you're making to the database and then you can input SQL commands directly:

```
{sql, connection = con, output.var = "mydataframe"}
SELECT "month_idx", "year", "month", 
    SUM(CASE WHEN ("term_deposit" = 'yes') THEN (1.0) ELSE (0.0) END) AS "subscribe", COUNT(*) AS "total
FROM ("bank")
GROUP BY "month_idx", "year", "month"
```

Then to change back to R start your next code chunk with {r}.


### Concluding Thoughts

All three of these languages show up on many job descriptions in the data world. SQL actually appears more often the both Python and R. SQL is also the language that connects many people across the work with databases. SQL is definitely necessary for anyone working in the data field, so it makes sense to be the first step in your data journey. The next step is then to learn either Python or R and how to integrate that language with SQL to be able to access data. Being able to "speak" multiple languages will only help you to in being able to make use of your data.

