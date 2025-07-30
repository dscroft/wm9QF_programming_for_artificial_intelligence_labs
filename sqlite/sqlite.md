<!--
module_id: sqlite
author: David Croft
email: david.croft@warwick.ac.uk
version: 0.0.1
current_version_description: a
module_type: standard
language: en
narrator: US English Male
mode: Textbook
title: SQLite

comment:  a

long_description: a

estimated_time_in_minutes: ?

@pre_reqs
Experience working with rectangular data (data in rows and columns) is required, as is some exposure to the idea of SQL and its use of tables with rows and columns.  No experience writing SQL code is expected or required for this module.  If you would like a code-free overview to SQL we recommend our module [Demystifying SQL](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/demystifying_sql/demystifying_sql.md#1).
@end

@learning_objectives  
- Use SELECT, FROM, and WHERE to do a basic query on a SQL table
- Use IS NULL and IS NOT NULL operators to work with empty values
- Explain the use of DISTINCT and how it can be useful
- Use AS and ORDER BY to change how query results appear
- Explain why the LIMIT keyword can be useful
@end

good_first_module: false
data_domain: ehr
data_task: data_wrangling
collection: learn_to_code
coding_required: true
coding_level: basic
coding_language: sql
sequence_name: sql

@version_history
Previous versions: 
@end

import: ../module_templates/macros.md
import: ../module_templates/macros_sql.md
import: https://raw.githubusercontent.com/LiaTemplates/Pyodide/master/README.md
import: https://github.com/LiaScript/CodeRunner/blob/master/README.md


@create_sqlite_db
```python
with open("sqlite.db", "w") as f:
    f.write("Wibble")
```
@Pyodide.exec
@end

@style
.flex-container {
    display: flex;
    flex-wrap: wrap; /* Allows the items to wrap as needed */
    align-items: stretch;
    gap: 20px; /* Adds both horizontal and vertical spacing between items */
}

.flex-child { 
    flex: 1;
    margin-right: 20px; /* Adds space between the columns */
}

@media (max-width: 600px) {
    .flex-child {
        flex: 100%; /* Makes the child divs take up the full width on slim devices */
        margin-right: 0; /* Removes the right margin */
    }
}
@end


-->

# SQLite

SQLite is a lightweight, serverless SQL database engine that is widely used for local data storage in applications. It is particularly useful for mobile and desktop applications, as well as for small to medium-sized web applications. 

SQLite databases are stored in a single file, making them easy to manage and deploy.
This distinguishes SQLite from client-server databases like MySQL or PostgreSQL, which require a server to run and manage the database and support the full range of functionality that is typically expected from a database, i.e. multi-user access, transactions, and so on.

However, if you are looking to store a moderate amount of data to be accessed by a single application, SQLite is a great choice. It is easy to set up, requires no configuration, and has a small footprint.

<div class = "important">
<b style="color: rgb(var(--color-highlight));">Important note</b><br>

There have been multiple versions of SQLite since its initial release in 2000. 
At time of writing the latest version is 3.50.3, released in July 2025.
As long as you are not working across major versions, i.e. SQLite 2.x vs SQLite 3.x, you should not have issues with compatibility.

</div>

## Python and database access

Python has significant benefits for database access over older languages such a Java, C or C++.
In those languages you would typically need to use an external library to access the database and the functionality of said library would vary from database to database.

In Python, Guido van Rossum, the creator of Python, has made it a priority to ensure that Python has a standard interface for database access. This means that you can use the same code to access different databases, such as SQLite, MySQL, PostgreSQL, and others.
This is done through the [Python Database API Specification](https://www.python.org/dev/peps/pep-0249/), which defines a standard interface for database access in Python. This means that you can use the same code to access different databases, and you can easily switch between databases without having to change your code.


## SQLite in Python

To use SQLite in Python, you use the `sqlite3` module. 
This module provides a simple and easy-to-use interface for working with SQLite databases.

The main difference between accessing a SQLite database and a client-server database such as MySQL or PostgreSQL is that you do not need to set up a server to run the database. Instead, you can create a SQLite database file and access it directly from your Python code.

<section class="flex-container">

<div class="flex-child" style="min-width: 300px;">

**Accessing a MySQL database in Python**

```python
import mysql.connector

details = {'user':     'username', 
           'password': 'password', 
           'host':     'url_or_ip_address', 
           'database': 'database_name'}

# create a connection to the database
con = mysql.connector.connect(**details)
cur = con.cursor()

# execute a query
cur.execute("SELECT * FROM table_name")

# fetch the results
for row in cur:
    print(row)

# close the connection
con.close()
```

</div>

<div class="flex-child" style="min-width: 300px;">

**Accessing a SQLite database in Python**

```python
import sqlite3






# open the database files
con = sqlite3.connect('sqlite.db')
cur = con.cursor()

# execute a query
cur.execute("SELECT * FROM table_name")

# fetch the results
for row in cur:
    print(row)

# close the file
con.close()
```

</div>

</section>



## Cursors

## Performance

Regardless of the database that you are using, the performance of your code will depend on how you write your queries and how you choose to access the results.

For example, if you are using a SQLite database and you are accessing a large number of rows, it is more efficient to use the `fetchall()` method to retrieve all the rows at once, rather than using a loop to retrieve each row one at a time.

<section class="flex-container">
<div class="flex-child" style="min-width: 300px;">

**fetchall**

Acquires the full set of results from the query in one go.
This is the most fastest way to retrieve the results from a query, but means that you are storing all of the data in memory at once.

It is entirely possible to run out of memory if working with a large dataset and so this approach should be used with caution.

</div>
<div class="flex-child" style="min-width: 300px;">
```python
cur.execute("SELECT * FROM table_name;")
rows = cur.fetchall()
for row in rows:
    print(row)
```
</div>
</section>

----------------------------

<section class="flex-container">
<div class="flex-child" style="min-width: 300px;">

**fetchmany**

Acquires a specified number of results from the query in one go.
This is a good compromise between performance and memory usage, as it allows you to retrieve multiple rows at once without using too much memory.

</div>
<div class="flex-child" style="min-width: 300px;">
```python
cur.execute("SELECT * FROM users;")
while rows:=cur.fetchmany(10):
    print( f"{len(rows)} rows fetched:")
    for row in rows:
        print(row)
```
</div>
</section>

----------------------------

<section class="flex-container">
<div class="flex-child" style="min-width: 300px;">

**fetchone**

Acquires a single row from the query at a time.
This is the slowest way to retrieve the results from a query, as it requires a round trip to the database for each row.
However, it is the most memory-efficient way to retrieve the results, as it only retrieves one row at a time.
It is also faster than the other methods in the sense that the time to acquire the first row is usually much less, even while the time to acquire all rows is longer.

This is also the method that is used by default when iterating over a cursor, i.e. `for row in cursor:`.

</div>
<div class="flex-child" style="min-width: 300px;">
```python
cur.execute("SELECT * FROM table_name;")
while row:=cur.fetchone():
    print(row)
```

```python
cur.execute("SELECT * FROM table_name;")
for row in cur:
    print(row)
```
</div>
</section>





## Demo 

Use SQLite in Pyodide with generated database files.


``` python @Pyodide.exec
import sqlite3

with sqlite3.connect("sqlite.db") as con:
    con.execute("DROP TABLE IF EXISTS users;")
    con.execute("CREATE TABLE users (id integer PRIMARY KEY AUTOINCREMENT, username text, password text, firstname text, lastname text, address text, city text, county text, postal text, phone text, email text);")
    con.execute("INSERT INTO users VALUES (1000,'Trout393','steal3','Aleshia','Tomkiewicz','14 Taylor St','St. Stephens Ward','Kent','CT2 7PP','01835-703597','atomkiewicz@hotmail.com');")
    con.execute("INSERT INTO users VALUES (1017,'Pigeon729','nose8','Evan','Zigomalas','5 Binney St','Abbey Ward','Buckinghamshire','HP11 2AX','01937-864715','evan.zigomalas@gmail.com');")
    con.execute("INSERT INTO users VALUES (1034,'Aardwolf536','party9','France','Andrade','8 Moor Place','East Southbourne and Tuckton W','Bournemouth','BH6 3BE','01347-368222','france.andrade@hotmail.com');")
    con.execute("INSERT INTO users VALUES (1051,'Stoat334','brown0','Ulysses','Mcwalters','505 Exeter Rd','Hawerby cum Beesby','Lincolnshire','DN36 5RP','01912-771311','ulysses@hotmail.com');")
    con.execute("INSERT INTO users VALUES (1068,'Toucan875','ought3','Tyisha','Veness','5396 Forth Street','Greets Green and Lyng Ward','West Midlands','B70 9DT','01547-429341','tyisha.veness@hotmail.com');")
```



``` python
import sqlite3

sqlite3.connect(":memory:").execute("select sqlite_version()").fetchall()
```
@Pyodide.eval




``` python
import sqlite3

with sqlite3.connect("sqlite.db") as con:
    cur = con.cursor()
    cur.execute("SELECT * FROM users;")
    while row:=cur.fetchone():
        print(row)
```
@Pyodide.eval


