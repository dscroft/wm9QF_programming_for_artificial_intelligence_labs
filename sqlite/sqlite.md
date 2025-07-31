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

# Python and databases

Accessing databases from a programming language is essential for building dynamic, data-driven applications. Here are some key reasons why this is relevant and important:

- **Data Storage and Retrieval:** Most applications need to store, retrieve, and manipulate data efficiently. Databases provide a structured way to manage large volumes of data, while programming languages allow you to interact with this data programmatically.

- **Automation:** By accessing databases through code, you can automate repetitive tasks such as data entry, updates, reporting, and analytics, reducing manual effort and minimizing errors.

- **Integration:** Applications often need to combine data from multiple sources or systems. Programming languages enable you to connect to different databases, integrate data, and build unified solutions.

- **Scalability and Flexibility:** Code-based access allows you to scale your application, handle complex business logic, and adapt to changing requirements more easily than manual database management.

- **Security and Validation:** Programming languages provide mechanisms to validate user input, enforce business rules, and implement security controls before interacting with the database, helping to protect sensitive data.

- **User Interaction:** Most modern applications have user interfaces that require real-time interaction with data. Accessing databases from code enables features like search, filtering, and personalized content.

In summary, accessing databases from a programming language bridges the gap between raw data storage and practical, interactive applications, making it a fundamental skill for developers.

## Python

Python has significant benefits for database access over older languages such a Java, C or C++.
In those languages you would typically need to use an external library to access the database and the functionality of said library would vary from database to database.

In Python, Guido van Rossum, the creator of Python, has made it a priority to ensure that Python has a standard interface for database access. This means that you can use the same code to access different databases, such as SQLite, MySQL, PostgreSQL, and others.
This is done through the [Python Database API Specification](https://www.python.org/dev/peps/pep-0249/), which defines a standard interface for database access in Python. This means that you can use the same code to access different databases, and you can easily switch between databases without having to change your code.

## SQLite

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

### SQLite in Python

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


<div class = "important">
<b style="color: rgb(var(--color-highlight));">Important note</b><br>

In sqlite if you specify a file that does not exist it will be created for you.
The newly created database will be empty, i.e. it will not contain any tables or data.

Therefore if you want to ensure that you are working with an existing database, you should check that the file exists before trying to open it. E.g. using the `os.path.exists()` function.

</div>


### :memory: databases

One piece of functionality posessed by SQLite that is not typical of other databases is the ability to create an in-memory database.
This means that the database is created in RAM and does not persist to disk.

Scenarios where this might be useful include:

- Testing code that interacts with a database, without having to create a file on disk.
- Prototyping a database schema or queries without having to create a file on disk.
- Storing temporary data that is only needed for the duration of a program's execution, such as caching results or storing intermediate data.
- Running a database in a web application that does not require persistent storage, such as a simple web app that only needs to store data for the duration of a user's session.

To create an in-memory database, you can use the special database name `:memory:` when connecting to the database instead of a filename.

```python
con = sqlite3.connect(":memory:")
```



# Python Database API

As discussed previously, the standard interface for database access in Python means that although the examples given may use SQLite, identical code can be used to access other databases such as MySQL or PostgreSQL, with only minor changes to the connection details.

## Cursors

## Query gotchas

When writing SQL queries in Python, there are a few common gotchas that you should be aware of. These can lead to errors or unexpected behavior if not handled correctly.

### " and '

When writing SQL queries in Python, you will need to be careful about how you use quotes.
In SQL, you can use either single quotes (`'`) or double quotes (`"`) to define string literals.
However, in Python, you also can also use single and double quotes to define string literals.

The problem occurs when you are defining a string literal in python containing a SQL query, and that query contains also string literals.

For example:

```python 
query = "SELECT * FROM products WHERE product_type = "FRUIT";"

query = 'SELECT * FROM products WHERE product_type = 'FRUIT';'
```

There are a few solutions to this problem:

**Different quotes**

Use single quotes for the SQL query and double quotes for the string literal in Python, or vice versa.

```python
query = "SELECT * FROM products WHERE product_type = 'FRUIT';"

query = 'SELECT * FROM products WHERE product_type = "FRUIT";'
```

----------------------

**Escape quotes**

You can escape the quotes in the SQL query using a backslash (`\`).
This works for both single and double quotes.

```python
query = "SELECT * FROM products WHERE product_type = \"FRUIT\";"
```

-----------------------

**Triple quotes**

You can use triple quotes to define a multi-line string in Python, which allows you to use both single and double quotes without escaping them.

```python
query = """SELECT * FROM products WHERE product_type = "FRUIT";"""
```


### Multiline queries

When working with SQL queries, it is often useful to be able to write multiline queries for readability and maintainability.

<section class="flex-container">
<div class="flex-child" style="min-width: 300px;">
**This:**

```sql
SELECT
  price,
  best_by_date,
  sale_pct,
  quantity
FROM products
WHERE product_type = "FRUIT";
```
</div>
<div class="flex-child" style="min-width: 300px;">
**Not this:**

```sql
select price, best_by_date, sale_pct, quantity from products where product_type = "FRUIT";
```
</div>
</section>

To write multiline queries in Python, you will need to define a multi-line string. 
In Python this is done using triple quotes.

```python
query = """
SELECT
  price,
  best_by_date,
  sale_pct,
  quantity
FROM products
WHERE product_type = "FRUIT";"""

cur.execute(query)
```

### String concatenation

When writing SQL queries in Python, especially if you are writing queries that rely on user input, you may be tempted to assemble your SQL queries using string concatenation.

![](media/Season_18_Episode_13_GIF_by_The_Simpsons.gif)

Using string concatenation to build SQL queries is a terrible idea, as it leaves your code open to SQL injection attacks.
This concept will be explored in more detail in the [SQL Injection](https://liascript.github.io/course/?https://dscroft.github.io/liascript_sqli/lia.md) activity.

There are a *very* small number of cases where string concatenation could be acceptable, such as when you are writing a query that does not rely directly on user input. 
But for the most part string concatenation for SQL query creation should be viewed as an actively dangerous anti-pattern and is to be avoided.

----------------------

You should use instead use parameterized queries, these allow you to safely pass user input to your SQL queries without the risk of SQL injection attacks.

`?` is used as a placeholder for the parameter in the query and will be replaced with the provided value when the query is executed.

**This:**

```python
query = "SELECT * FROM products WHERE product_type = ?;"
cur.execute(query, ("FRUIT",))
```

**Not this:**

```python
search_term = "FRUIT"

query = "SELECT * FROM products WHERE product_type = '" + search_term + "';"
cur.execute(query)

# or

query = f"SELECT * FROM products WHERE product_type = '{search_term}';"
cur.execute(query)
```

**Never, *ever*, under *any* circumstances, this:**

```python
search_term = input("Enter a search term: ")

query = "SELECT * FROM products WHERE product_type = '" + search_term + "';"
cur.execute(query)
```

<div class = "important">
<b style="color: rgb(var(--color-highlight));">Important note</b><br>
This is one place where code may differ between databases.

The specific placeholder syntax is database-specific and not part of the Python Database API Specification.
You will need to check the documentation for the specific database you are using to see what placeholder syntax is supported.

For example, MySQL uses `%s` as a placeholder, while SQLite uses `?`.

However all databases should support the use of parameterized queries, so you should always use this approach when writing SQL queries in Python.
</div>



## Performance

Regardless of the database that you are using, the performance of your code will depend on how you write your queries and how you choose to access the results.

In python there are three main ways to retrieve the results of a query:

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
cur.execute("SELECT * FROM table_name;")
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


