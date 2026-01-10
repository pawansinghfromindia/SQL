<!-----------Chapter 9. Advanced SQL Techniques--------------->

# Chapter 9. Advanced SQL Techniques
In SQL, There are different techniques in order to organize our complex project : ```CTE```,```Views```,```CTAS Table & Temp Tables```, 
Stored Procedure and Triggers

<details>
  <summary> Introduction </summary>
  
Normally in a project, We have Database and We have a person who is responsible for taking care of database structure, known as **DBA(Database Administrator)**
We also a users, who are writting queries in order to retrieve data from the database.

Query written by an user sent to Database and Database will execute the query and accordingly then return the results to user.

At the end User is going to see the result of the query that he wrote.

This is a very simplified scenario on How we use database.

But in real world the things are totally different!

In real projects things are really complicated.

For example, a Financial Data Analyst writing huge block of SQL query which is very complex and 
there is another user who has different role like a Risk Manager who is also writing another complex SQL query like that
from different department from different projects for different tasks, there are a lot of Data Analysts in a company who are writing many complex queries.
So, All those Data Analyst and Managers have a direct access to the Database and they will be executing complex analytical Queries in order to generate 
reports or analysis. <br/>
Not only those guys are doing analysis on the database but there are Data Engineers, who are building the Data Warehouse. So they're extracting the data.
So, they also writes SQL Extract queries in order to extract the data from database. then different script for Transformations in order to manipulate filters
clean up, aggregate and other stuffs to the data. and then third script to collect the results of transformations and load it in another database called
**Data Warehouse**.

> A Data Warehouse is a special database that collects and integrates data from different sources to enable analytics and support decision-making.

At the end this chain, there are Data Analyst who writes SQL queries in order to analyze the data in Data Warehouse.
Or different SQL queries in order to prepare the data before inserting it to a tool like **PowerBI** in order to generate Visualization & Reports.

This is what we call Data Warehouse System or Business Intelligence System that extract data, manipulate it and transform it for analysis.

So, Not only Data Engineers and Data Analyst accessing the database but Data Scientists also has direct access to the database. So Data Scientists also write
different SQL queries in order to extract the data and manipulate the data that are needed in order to develop a model and doing Machine Learning and AI.

In many scenarios the result of Data Analyst is going to used in another query in order to prepare the results for Data Visualization PowerBI or
in order to export like a Excel List.

So, We can see a lot of peoples from different roles who access the database and do his job in a company. 
Because everyone needs to answer the questions based on data.

Now, This is still a simplified version of How things work in the data projects of a company.
In real projects things are way more complicated.

Now, when you see this, you will find many challenges and problems. e.g.
All those people are not talking to each other, Each of them are creating their own query. If you see all those queries and compare them you will find the 
every script every query logics keep repeating. So, query of a Data Engineer, Data Analyst or Data Scientist might contains redundant logic.
**Redundancy**

If optimization is not there, the performance issue will occur everywhere.
So, Queries of Data Engineers, Analyst and Scientist take hours to run.
Everyone suffers from bad performance of the query.
So, If everyone writing big complex queries then don't expect everyone has good performance.
**Performance Issues**

Another challenges in data projects is complexity. Behind the original database, we might have Data Model i.e. Prepare and Optimize only for one application.
So, In data model, we have a lot of tables and those tables have different relationships between them. Now only the Developers and Experts of this database 
understand that physical Data model behind the database.
Now, If you give the access to all those Data Analysts they will have a lot of questions bcuz first they have to understand the Data Model before writing any
query. Means a lot of Data Workers are keep asking questions from experts of the database. like
How to connect Table A with Table B?
Where do we find the specific columns?
What does this Table means?
Getting bad results in query bcuz Data is corrupted?
So, Developers of the Database & Experts of the Database will get a lot of questions from Data Analyst & Developers & Experts have to explain over & over their
data model to users so that they are able to write the complex SQL queries.
**Complexity**

Here, We will find a lot of queries from different users to Database and this might cause a lot of stress on the Database. So, keep executing repeatedly big
complex queries might bring Database down.
**Hard to maintain**

If we give the direct access of database to the users, we're in problem bcuz giving direct access to some Data Engineers & Data Developers is okay 
but giving access Data Analyst a full access to the databases all tables then we have to protect the tables, columns & rows everything.
So, We can give everyone direct access to database all physical tables.
**Data Security**

Now What are solutions of all these problems and Issues?
Here, We are going to focus on **5 Techniques**
1. Subquery
2. CTE - Common Table Expression
3. View - Views in Database
4. Temp Tables - temporary Tables
5. CTAS - Create Tables AS SELECT

This is why we have to understand those 5 techniques in order to solve all those issue that we might face in Data Projects.

</details>

<details>
  <summary>Simplified <b>Database Architecture</b> </summary>

- What happens behind the scene? <br/>
- How the databse executes the queries from the 5 techniques? <br/>
- By understanding this Architecture, We will understand How things works? <br/>

In this strory, there are 2 sides : 
1. **Server** Side : Where Database lives, It has many components like **Database Engine**, **Storage**.
2. **Client** Side : Users writing SQL Queries for specific purpose.

> **Database Engine** : It is the brain of the database, executing multiple operations such as storing, retrieving and managing data within the database.

Each time we execute any query, the Database Engine takes care of it.

In the Database, we have very important component i.e. **Storage**
There are 2 main types of Storage in the database.
1. **Disk Storage** :
2. **Cache Storage** : 

> **Disk Storage** : Long-term memory, where data is stored permanently.
> - It Stores the data permanently even if we turn off the system, bcuz It is stored on Hard Disk.
> - It can store a large amount of data
> - But disadvantage of Disk Storage is that It is slow. means Slow to write and Read

> **Cache Storage** : fast short-term memory, where data is stored temporarily.
> - It stores the data temporarily as you turn off the system it goes off, buz It is stored on RAM
> - It is extremely fast to read and to write data compared to Disk Storage
> - But disadvantage of Cache Storage is that It can hold small amount of data.

|                             Server                                    | 
|------------------------------------------------------------------------|
| Database Engine : Brain of database executes multiple operations.      |
| Storage : 2 Types of Storage -> 1. Disk and 2. Cache                   |
| Disk Storage : Long-term Memory, where data is stored permanently.     |
| Disk Storage, + Capacity : can hold  a large amount of data.           |
| Disk Storage, - Speed : Slow to read and to write.                     |
| Cache Storage : Short-Term Memnory, where data is stored temporarily.  |
| Cache Storage, + Speed : extremely fast to read and to write.          |
| Cache Storage, - Capacity : can hold small amount of data.             |
| Disk Storage : 3 types 1. User data 2. System Catalog 3. Temprary Table|


</details>

<details>
  <summary> <b>Disk Storage</b> </summary>



**Disk Storage** : Long-term memory, where data is stored permanently.
- It Stores the data permanently even if we turn off the system, bcuz It is stored on Hard Disk.
- It can store a large amount of data
- But disadvantage of Disk Storage is that It is slow. means Slow to write and Read

**Disk Storage** : There are 3 types of area inside Disk Storage
1. **User Data**
2. **System Catalog**
3. **Temporary Data**
- Each storage type has different purpose.

### What is **User Data Storage**?
- It is the ***main content of the database***.
- This is where the ***actual data that users care about*** is stored.
- So, This is the ***storage where Users are interacting*** all the time.
- If you go inside the ***database***, there you will find all the ***tables***, those are the Users data.
```sql
      SELECT * FROM Sales.customers;
      SELECT * FROM Sales.employees;
      SELECT * FROM Sales.orders;
      SELECT * FROM Sales.products;
```  
Inside the databases all other stuffs are there, As a users we don't care about it.
But in database, we have many other information.

### What is **System Catalog Storage**?
- This is the ***internal storage of database for its own information***.
- It's like ***a blueprint that keeps track of everything about the database itself, not the user data***.
- The main purpose of System catalog is that ***to holds the Metadata information about the database***.

#### MetaData

> **MetaData** : Data about data

Suppose, we have created a table called Customers inside the database, where we defines multiple columns like customer_id, first_name, last_name,
country, score etc. <br/>
Then we inserted the data inside the customer table like 5 customers.

**Customers** Table contains ***Actual Data***
| customer_id | first_name | last_name | country | score |
|-------------|------------|-----------|---------|-------|
| 1           | John       | Snow      | France  |  900  |
| 2           | Kevin      | Brown     | USA     |  350  |
| 3           | Mary       |           | UK      |  750  |
| 4           | Mark       | Hasi      | USA     |  500  |
| 5           | Smith      | Adams     | Germany |  NULL |

So, These inofrmation are users data. User has created those information and stored it inside the database. That's why we call it as USER DATA.

What happens behind the scenes is The Database Server will not only stored the User Data that user provided but Server also stored a different type of
data inside the database and This data is ***MetaData*** of customer table. Metadata look like table_name, column_name, data_type, length, is_nullable etc.
In the metadata we can find a lot of information about not only the tables columns but also schema of the table in the database.
So, We can find a whole catalog about the structure of database.

**Metadata of Customers** Table contains ***Data about Data***
| Table Name | Column Name | Data Type | Length | Is Nullable |
|------------|-------------|-----------|--------|-------------|
| customers  | customer_id | int       |        | No          |
| customers  | first_name  | varchar   |  50    | Yes         |
| customers  | last_name   | varchar   |  50    | Yes         |
| customers  | country     | varchar   |  50    | Yes         |
| customers  | score       | int       |        | Yes         |

So, In the database each table that we are using in order to store the data has a table twin that describes structure of your data.
This is what we mean when we say System Catalog storage.

Where do we find all those System Catalogs & metadata? <br/>
- We can't navigate through System Catalog storage in Object Explorer like User Storage.
- But we can find the System Catalogs in special hidden schema called **Information Schema**

> **Information Schema** : A system-defined schema with built-in views that provide info about the database, like tables and columns.

```sql
     SELECT *
     FROM INFORMATION_SCHEMA.COLUMNS;

     SELECT *
     FROM INFORMATION_SCHEMA.CONSTRAINT;
```

This is exactly why a database maintain system catalogs. It helps database to quickly find the structure of each table & of each column. 
It also helps us to browse the different databases.
```sql
      SELECT
            DISTINCT TABLE_NAME
      FROM INFORMATION_SCHEMA.COLUMNS;
```  

### What is **Temporary Storage**?
- It is a ***temporary space used by the database for short-term tasks***,
  like processing queries or sorting data. Once these tasks are done, the ***storage is cleared***.

Where do we find the temporary tables that is using the temporary storage on the Disk storage? <br/>
- Go to the Object Explorer, You will not find inside the Databases like Sales which was created by users.
- You will find inside System Databases. Now since we are working locally on our PC we have full access to everything inside our server.
> But in real project, if you're just a user or a developer, you will not have access to the ***System databases***. 

- ***System databases*** is only accessible to ***Database Administrators***
So, inside the System Databases > TempDB > Temporary tables.
This is where you can find all the temporary tables that you're generating.

</details>

<details>
   <summary>How simple query works? </summary>  

As we know the main components of Database Architecture : 
- Server Side & Client Side
- Database Engine : Brain of database to executes multiple operations such as storing, retrieving and managing data.
- Storages : Disk & Cache
- Disk Storage : User Data, System Catalog and Temporary Data

Let's see an example : 
- We have a table called Orders that is stored inside USER Storage, and metadata of Orders is stored inside System catalog Storage. <br/>
- Now let's say if we as a **client side** and we write a simple **SELECT query** in order to select the data of orders tables. <br/>
- Now the query is sent to the **Server** in order to be executed and **Database Engine** will take this query in order to process it. <br/>
- So, First Database Engine will do is to check whether we have the data in the **Cache Storage**
  bcuz if the data is stored in the cache then things is going to be really fast and Database Engine solve the task quickly. <br/>
- But in the current scenario, we don't have orders table information in the cache that's why the Database Engine didn't find the orders in cache,
  So, Now Database Engine will check the **Disk Storage** for orders table information & here found, So Query will going to be executed. <br/>
- Then the result of this query will be going to sent back to the Client Side, where at the end in return, we will see in the output
  the result of the table orders.

This is How SQL database executes very simple SELECT Query.

</details>

## 9.1 Subqueries

**A Query inside another Query.**


<details>
  <summary> <b> What is Subqueries? </b> </summary>

- A Query inside another Query

Simple Query :
```sql
   SELECT *
   FROM Sales.orders
   WHERE order_id=1 
```

Query inside Query : Sub-Query
```sql
   SELECT *
   FROM (
          SELECT *
          FROM Sales.orders
          WHERE order_id = 1
        )
   WHERE order_date < '2026-01-01'
```
We call it Embedded-Query or a Sub-Query.

First SQL execute the Sub-Query then Main Query. <br/>
The result of Sub-Query doesn't send to user, the result will stay as an Intermediate Result and the Main Query interacts with this intermediate result from Sub-Query.

Now Main Query can have two sources of data one from original database table and another from result of Sub-Query.

> Sub-Query is a query inside the Main-Query, Sub-Query supports the Main-Query with data. <br/>
> The job of Main-Query is to get those data & at the end shows final result.

Once the execution of the sub-query is completely done, what SQL does is SQL destroys the intermediate result, you will not find it anywhere.

The intermediate result of the Sub-query is only locally known from the Main-Query itself, It can't access from outside externally in any other query globally.

> Sub-Query can be only used from the main Query.

</details>

<details>
  <summary> <b> Why do we use Sub-Queries? </b> </summary>


<br/> When to use Sub-Queries?  <br/>

In a complex task, we might have to do several stuffs in our query. <br/>
Like 

Step 1. ```JOINs Table``` - We have to JOIN tables in order to prepare the data. 

Step 2. ```FILTERING``` - Outcome of the JOIN should be filtered. 

Step 3. ```TRANSFORMATIONS``` - On top of that we have to do transformations like Handling NULLs or Creating New Columns or other stuffs. 

Step 4. ```AGGREGATIONS``` - At the end we have to do Data Aggregation like Summarizing the data or finding average. 

If just start writing a SQL query without having any plan, you will ended up having a long complex SQL query. <br/>
It is hard to write as well as to read. <br/>
Instead of that we can divide our task based on steps. <br/>
We should write one query section for each step. <br/>
One query for Joining another for filtering and another one for transformation and last one for Aggregation. <br/>
Here, each step is like preparation of next step. <br/>
So, We can say each of those queries is a sub-query. <br/>
All the Sub-squeries are doing preparation for the last step. <br/>
And We call the last step as Main-Query. <br/>
The whole things can exist in one single query. <br/>

> Sometime we call ***Main-Query*** as ***Outer-Query*** and ***Sub-Query*** as ***Inner-Query***.

We can have many Sub-Query inside Sub-Query and so on and form something called **Nested-Query**

Using Sub-Query reduces the complexity and easier to read and logical flow inside the query.

</details>

<details>
  <summary> <b>Sub-Query Category </b> </summary>

For some Queries, there are many different types and categories.

- Based on **Dependency** between Main-Query and Sub-Query <br/>
There are mainly 2 types of Sub-Queries :
1. **Non-Correlated Sub-Query** : Sub-Query that is independent from the Main-Query. 
2. **Correlated Sub-Query** : Sub-Query that is dependent on the Main-Query.

- Based on **Result type**, there is another group of Sub-Query. <br/>
1. **Scaler Sub-Query** : It returns only one single value.
2. **Row Sub-Query** : It returns multiple rows.
3. **Table Sub-Query** : It returns multiple rows as well as multiple columns.

- Based on **Location/Clauses** where the Sub-Query is going to be used within the Main-Query. <br/>
1. **```SELECT``` Clause Sub-Query** : used in ```SELECT``` most common sub-query
2. **```FROM``` Clause Sub-Query** :   used in  ```FROM``` most common sub-query
3. **```JOIN``` Clause Sub-Query** :   used before joining tables
4. **```WHERE``` Clause Sub-Query** :  used in order to filter data
   - In the ```WHERE``` clause we have 2 different sets of operators.
     - Comparison Operators ```<, <=, >, >=, =, !=``` 
     - Logical Operators ```IN, ANY, ALL, EXIST```   

</details>

<details>
  <summary> <b>Based on Result Types Sub-Query </b> : <code>Scaler Sub-Query </code>,<code>Row Sub-Query </code>,<code>Table Sub-Query</code></summary>

We have different types of Sub-Query based on Results. <br/>
This means based on the amount of data that the sub-query returns.

**1. Scaler Sub-Query** 
- It is a Sub-Query that returns only one single value.

| Average |
|---------|
|   15    |
```sql
    SELECT
       AVG(sales)
    FROM Sales.orders
```
- This is a Scaler Query, where we have only one single value (one row, one column)

**2. Row Sub-Query**
- It is a Sub-Query that returns multiple rows and a single column.

| Customer_id |
|-------------|
|   1         |
|   2         |
|   3         |
|   4         |
```sql
   SELECT
        customer_id
   FROM Sales.orders
```
- This is Row Sub-Query that return multiple rows & one column.

**3. Table Sub-Query**
- It is a sub-query that return multiple rows & multiple columns.

| order_id | order_date | Product |
|----------|------------|---------|
|   1      | 2026-01-01 | Gloves  |
|   2      | 2026-01-04 | Caps    |
|   3      | 2026-02-15 | Shoes   |
|   4      | 2026-03-21 | Bag     |
```sql
    SELECT *
    FROM Sales.order
```
- This is a table sub-query that return multiple rows & multiple columns.
</details>

<details>
  <summary> <b>Based on Location/Clauses Sub-Query</b>: <code>FROM</code>,<code>SELECT</code>,<code>JOINs</code>,<code>WHERE</code> </summary>

<br/> How to use Sub-Query in the different Clauses/locations?

**1. Sub-Query in ```FROM``` Clause**
> ```FROM Sub-Query``` used as temporary table for the main-query.
- We typically use Sub-Queries in the ```FROM``` clause in order to create temporary result sets. </br>
That acts as a table for the Main-Query.


- In some scenario, we can't use the table directly from the database. So, We have to prepare it somehow before we do our actual query.

Syntax of SUb-Query inside the ```FROM``` Clause
```sql
     SELECT
          col1, col2
     FROM (SELECT column FROM table1 WHERE <condition>) AS alias  --many databases alias is optional but in few it is required!
```
```sql
    -- Task : Find the products that have a price higher than the average price of all products.
    -- Step 1. Calculate the average price of each product
    -- Step 2. Filter the products where price > average price

    --Main-query
    SELECT
         *
    FROM 
       --Sub-query
       ( SELECT
                 product_id,
                 price,
                 AVG(price) OVER() AS avg_price
           FROM Sales.products ) t
   WHERE price > avg_price;
```
The Sub-query is here, only to support the main-query. So, here we're preparing all the data that we need in order to have the final result for the main-query.

So, This is how we use the Table Sub-Query inside the FROM Clause.

> Tip : To check the intermediate results of a sub-query, highlight it and execute. 

```sql
   -- Task : Rank the customers based on their total amount of sales.
   -- Step 1. Find the total amount of sales
   -- Step 2. Rank them based on their total sales

      SELECT
          *,
          RANK() OVER(ORDER BY total_sales DESC) as customerRank
      FROM (
            SELECT
                customer_id,
                sales,
                SUM(sales) OVER() total_sales
            FROM Sales.customers ) t
```
How SQL execute the sub-query?</br>
-The 1st step is SQL will identify the sub-query & then execute it. </br>
-Once the sub-query is executed, the next step is Result is going to be introduced as an Intermediate Result which is not visible in output, It is temporarily saved in memory. </br>
-Next step SQL will take is start executing Main-Query based on intermediate result. </br>
-Output of main-query is Final Result.


**2. Sub-Query in ```SELECT``` Clause**
> Used to aggregate data side by side with the main-query's data, allowing for direct comparison.

- We typically use the sub-query in the SELECT Clause to aggregate the data side by side with the columns of the main-query.

How to use sub-query in ```SELECT``` clause?

Syntax of the sub-query in ```SELECT``` clause
```sql
      SELECT
           column1,
           (SELECT column FROM table2 WHERE condition) AS column2
      FROM table1
```
> Note : The result of sub-query must be **Scaler Query**. Means, the result must be a single value.

```sql
    -- Task : Show the product ids, product names, prices and the total number of orders.

       SELECT
           product_id,
           product_name,
           price,
           ( SELECT COUNT(*) FROM Sales.orders ) AS TotalOrders
       FROM Sales.products
```
This is what we call scaler-query inside the ```SELECT``` clause.

How SQL execute the above query? </br>
-1st SQL will identify the sub-query & execute it. </br>
-Sub-query is targeting the orders table where we're simply counting the orders, so result of this sub-query acts as intermediate result which be stored in cache. </br>
-Now SQL will start executing Main-query by passing it intermediate result. </br>
-SO, SQL is targeting the table products so it will get all those columns from product and that one column from the intermediate result. </br>
-And return the final result of the main-query. </br>

- It's very simple SQL always start with sub-query and then pass the value to the main-query and at the end main-query will be executed & we will get the final result. 

**3. Sub-Query in ```JOIN``` Clause**
> Used to prepare the data(filtering or aggregation) before joining it with other tables.
- Sometime we have to prepare the data before doing the join to dynamically create a result set for joining with another table. 
- Here, We can't join table directly, we have to prepare data before doing joins.
 
How to use sub-query in ```JOIN``` clause?

```sql
    -- Task : Show all the customers details and find the total orders of each customer.
    -- Of Course, we have multiple solution for this.
    -- Here, We will solve this task using Sub-query.
    -- part1. Show all the customer details
    -- part2. find the total orders for each customer

   --Main-Query
   SELECT
         c.*,
         o.TotalOrders
   FROM Sales.customers c
   LEFT JOIN 
   --Sub-query
   (SELECT
        customer_id,
        COUNT(*) AS TotalOrders
   FROM Sales.orders
   GROUP BY customer_id) o
   ON c.customer_id = o.customer_id
```

This is how we use sub-query inside JOINs.

**4. Sub-Query in ```WHERE``` Clause**
> Used for complex filtering logic and make query more flexible and dynamic.

- In SQL, we can filter the tables using the ```WHERE``` clause by putting static value like ```SELECT * FROM Sales.orders WHERE sales > 100```
But in real Data Projects, we filter the data based on complex logic. So, In order to prepare this complex logic we use sub-query in ```WHERE``` clause to make filter more flexible and dynamic for main-query.

- In order to filter data using ```WHERE``` clause we have to use operators.
1. **Comparison Operators** : ```<, <=, >, >=, =, != or <> ```
2. **Logical Operators** or **Sub-query Operators**   : ```IN()```, ```ANY```, ```ALL```, ```EXIST```

How to use sub-query in the ```WHERE``` clause?

**Comparison Operators**
> **Used to filter data by comparing two values** in order to filtering the data based on specific condition

| Comparison Operators | Name                    | Syntax                                               | 
|----------------------|-------------------------|------------------------------------------------------|
| ```=```              | Equal to                | ```WHERE sales = (SELECT AVG(sales) FROM orders)```  |
| ```!=``` or ```<>``` | Not Equal to            | ```WHERE sales != (SELECT AVG(sales) FROM orders)``` |
| ```>```              | Greater than            | ```WHERE sales > (SELECT AVG(sales) FROM orders)```  |
| ```>=```             | Greater than or Equal to| ```WHERE sales >= (SELECT AVG(sales) FROM orders)``` |
| ```<```              | Less than               | ```WHERE sales < (SELECT AVG(sales) FROM orders)```  |
| ```><=```            | Less than or Equal to   | ```WHERE sales <= (SELECT AVG(sales) FROM orders)``` |

Syntax of Sub-query in ```WHERE``` clause
```sql
    SELECT
          columns1,
          columns2
    FROM table1
    WHERE column = (SELECT column FROM table2 WHERE condition)
```
> Rule : Sub-query must be scaler sub-query which means must return only one single value.

```sql
   -- Task : Find the products that have a price higher than the average price of all products.
    --Main-query
    SELECT
          *
    FROM Sales.products
    WHERE price > (SELECT AVG(price) FROM Sales.products) --Sub-query must have single row
```
How SQL will execute the above query? <br/>
-1st SQL will identify the SUb-query & execute it. <br/>
-The result of sub-query is considered as intermediate result and will be stored in Cache memory. So, we will not see this in output. <br/>
-Next step is SQL will start executing Main-query where intermediate result will be passed to this main-query. <br/>
-Now, SQL will select only product where the price is higher than average value. <br/>
-In the output we will get the final result.


In some scenario we have to filter the data based on multiple values, In this case we use logical operator with ```WHERE``` clause.

**Logical Operators** or **Sub-query Operators**

| Logical Operators    | Definition                                           n    | Syntax                                | 
|----------------------|-----------------------------------------------------------|---------------------------------------|
| ```IN``` Operator    | Checks whether a value matches any value from a list      | ```WHERE sales IN (1,2,3,4)```        |
| ```NOT IN``` Operator| Checks whether a value not matches any value from a list  | ```WHERE sales NOT IN (1,2,3,4)```    |
| ```ANY``` Operator   | Checks if a value matches ANY value within a list         | ```WHERE sales < ANY (1,2,3,4)```     |
| ```ALL``` Operator   | checks if a values matches ALL value within a list        | ```WHERE sales < ALL (1,2,3,4)```     |
| ```EXISTS``` Operator    | Check if a subquery returns any rows         | ```WHERE EXISTS (SELECT 1 FROM table1 WHERE table1.ID = table2.ID```|
| ```NOT EXISTS``` Operator| Check if a subquery doesn't return any rows  | ```WHERE NOT EXISTS (SELECT 1 FROM table1 WHERE table1.ID = table2.ID```|


The big difference between comparison operator and logical operator using with ```WHERE``` clause is that ***Sub-query with logical operators allow to have multiple rows*** unlike ***Sub-query with comparison operators allow only one single row(scaler)***.

**```IN``` Operator**
>  Checks whether a value matches any value from a list.

**```NOT IN``` Operator**
>  Checks whether a value doesn't match any value from a list.

Syntax of a sub-query using ```IN``` Operator :
```sql
     SELECT
          column1,
          column2
     FROM table1
     WHERE column IN (SELECT column FROM table2 WHERE condition) --Sub-query can have multiple rows
```
> ```IN()``` is Non-correlated Sub-query : Independent of the main-query & can be executed on its own.

**```NOT IN```**
```sql
    -- Task : Show the details of orders made by customers in Germany.

    SELECT
         *
    FROM Sales.orders
    WHERE customer_id IN (SELECT customer_id FROM Sales.customers WHERE country = 'Germany' )

  -- Task : Show the details of orders made by customers who are not from Germany.
  SELECT
         *
    FROM Sales.orders
    WHERE customer_id IN (SELECT customer_id FROM Sales.customers WHERE country != 'Germany' )
  --or
   SELECT
         *
    FROM Sales.orders
    WHERE customer_id NOT IN (SELECT customer_id FROM Sales.customers WHERE country = 'Germany' )

```
How SQL execute the query? <br/>
-1st thing SQL does is identify the sub-query & execute it. <br/>
-Now, the result of the sub-query acts as intermediate result for the main-query which is stored in cache memory. <br/>
-Next thing SQL does is Start executing main-query by considering the intermediate result as support for main-query with the filter. <br/>
-Now, SQL return the final result of main-query.

**```ANY``` Operator**
> Checks if a value matches ANY value within a list. <br/>
> Used to check if a value is true for AT LEAST one of the values in a list.

**Note : ```ANY``` and ```ALL``` operators always used with comparison operators.**
 
Syntax of a sub-query using ```< ANY``` Operator :
```sql
     SELECT
           column1,
           column2
     FROM table1
     WHERE column < ANY (SELECT column FROM table2 WHERE condition)
```
```sql
    -- Task : Find female employees whose salaries are greater than the salaries of any male employees.

    SELECT
         employee_id,
         employee_name,
         gender,
         salary
    FROM Sales.employees
    WHERE gender = 'Female' AND salary > ANY(SELECT salary FROM Sales.employees WHERE gender = 'Male')
```

**```ALL``` Operator**
> Checks if a value matches ALL values within a list. <br/>
> If we need to check whether a condition is true for every value in a list.

**Note : ```ANY``` and ```ALL``` operators always used with comparison operators.**

Syntax of a sub-query using ```< ALL``` Operator :
```sql
     SELECT
           column1,
           column2
     FROM table1
     WHERE column < ALL (SELECT column FROM table2 WHERE condition)
```
```sql
   -- Task : Find female employees whose salaries are greater than the salaries of all male employees.

   SELECT
        employee_id,
        employee_name,
        gender,
        salary
   FROM Sales.employees
   WHERE gender = 'Female' AND salary > ALL(SELECT salary FROM Sales.employees WHERE gender = 'Male')
```

Till here, we have learned almost everything about How to use the Sub-queries in different loactions/clauses.
But didn't talk about ```EXISTS``` operator bcuz before that we have to understand a very important concept of Sub-query that is based on dependency COrrelated & Non-correlated Sub-query. 

 **```EXISTS``` operator in Sub-query** (Correlated Sub-query)
 > Check if a subquery returns any rows.

- In some scenario, As we're querying the data from one table, and you want to check whether the rows of this table does exist in another table.
  Means, You're checking existence of rows of one table in another table. In this scenario, we use the Sub-query with ```EXIST``` operator. 

Syntax of ```EXISTS``` operator in a ```WHERE``` clause sub-query :
```sql
      SELECT
           column1,
           column2
      FROM table2
      WHERE EXISTS (SELECT 1 FROM table1 WHERE table1.ID = table2.ID)
```
> We don't specify any column before ```EXISTS``` like we do for comparison operators or other logical operators like ```IN, NOT IN, ANY, ALL```. bcuz we're not filtering based on a value or values, we're filtering based on logic. That's why we're using ```EXISTS``` keyword immediately. 

How ```EXISTS``` works? <br/>
<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/49137319-ffeb-469b-9bd7-88a61d15ecad" />

- For each row in Main-query, It is going to execute the Sub-query, where Sub-query is going to evaluate that row. <br/>
- If Sub-query doesn't return anything, then the Row that we're evaluating from the main-query will be excluded from the final results. <br/>
- On the other hand, if Sub-query returns a value, then the row that we're evaluating from the main-query will be included in the final results. <br/>
- So, Sub-query is used in order to do a test. like Do we have a result or don't and based on that SQL either include or exclude the row from the final result.

This is the logic behind the ```EXISTS```!

```sql
   -- Task : Show the details of orders made by customers in Germany.

     SELECT
           *
      FROM Sales.orders o
      WHERE EXISTS (SELECT 1 FROM Sales.customers c WHERE c.country='Germany' AND c.customer_id = o.customer_id )

     -- Why the value 1?
     --It doesn't matter what you're selecting in the Sub-query, so you can go with * or a single column, multiple columns or a single static value.
     -- We should go with 1 a static value which gives you better performance compared to * or any column.
     -- Best practice is to use a static value 1 or 2 or anything. That's why 1.
```

**```NOT EXISTS```**
```sql
       -- Task : Show the details of orders which are not made by customers in Germany.

     SELECT
           *
      FROM Sales.orders o
      WHERE NOT EXISTS (SELECT 1 FROM Sales.customers c WHERE c.country='Germany' AND c.customer_id = o.customer_id )
```
How SQL executes Correlated Sub-query? <br/>
- This time, SQL will not start with the sub-query, this time SQL starts with Main-query, first It will identify the main-query and then start executing it.
- But It's going to be executed row-by-row.
- So, SQL is going to test row 1, so first row is customer_id 1, so Customer_id = 1 row will be going  to put under the test.
- So, SQL will pass the customer_id from the main-query to sub-query.
- So, SQL now will prepare the sub-query with following information, so SQL will execute sub-query for the customer_id = 1.
- Once the sub-query is executed it will either return something or not. So in this case it will return 1 as customer_id is exist in table2.
- So, Row1 from main-query passed the test and now this row will be included into the final result.
- Similarly now SQL will start testing for row2 of the main-query if it pass the test again it will be included in the final result, if not so then it will be excluded from the final result.
- And this process will repeat until we are done with all the rows of the main-query.
- At the end of this SQL will present the final result as output.

</details>

<details>
  <summary> <b>Based on dependency Sub-query : </b> <code>Correlated Sub-query</code> and <code>Non-Correlated Sub-query</code></summary>

Here, we're going to learn about Dependency between the Sub-query & Main-query like Sub-queries that depends on Main-query or that do not depends on Main-query.

So, far what we have learned all of the Sub-queries are **Non-Correlated Sub-queries**.
Now, We're going to see the **Correlated Sub-queriues**.

**Non-Correlated Sub-query**
> A Sub-query that run independently from the Main-query. <br/>
> Means, These are Stand Alone Sub-query.

**Correlated Sub-query**
> A Sub-query that relies(depends) on values from the Main-query for each row it process. <br/>
> Meaning, here Sub-query is totally dependent on the Main-query.

In this scenario, We have data in a table inside the Database.
Here, this time, SQL executes Main-query first.
So, Main-query will query the database in order to get result and SQL will going to process results row by row.
So what will happen is Main-query pass the 1st Row information to Sub-query.
So, after Sub-query get the data from Main-query, Now SQL will execute the Sub-query and Sub-query will return a value.
Now, SQL Main-query will check if there is a result from the Sub-query if yes then SQL will consider the row in Final Result.
This happens only for the 1st Row.
Similarly we again process whole thing for 2nd Row, 3rd Row and so on.
Now after executing the Sub-query, there is no result in that case SQL Main-query doesn't consider this row in the final Result.
And It will move on to the next row.
So, This will keep happening as long as we have rows in Main-query.
Once, we have processed all the rows of Main-query the final results will be presented in the Final Output.

- Summary : Correlated Sub-query is always depending on the Main-query. And Sub-query will be going to executed for each row that we get from the Main-query.

This is How Correlated Sub-query works. It is little bit more complicated than Non-Correlated Sub-query.!!
Non-Correlated Sub-query is straight-forward, 1st Sub-query will be executed & output of it will act as intermediate result for the Main-query, Main-query will be executed now & we will get the final result.

<img width="300" height="150" alt="image" src="https://github.com/user-attachments/assets/c3d4fdbf-2cea-4b52-b3ce-ccfd56654dfc" />

```sql
     -- Task : Show all customer details and find the total orders for each customer.
     -- We have already solved this task using LEFT JOIN & Sub-query, But in SQL we have multiple ways to do same task.
     -- Now, Let's solve it using Sub-Query & SELECT clause + Correlated Subquery

    -- Main-query
    SELECT
          *,
          (SELECT COUNT(*) FROM Sales.orders o WHERE o.customer_id = c.customer_id) AS totalOrders  --Sub-query
    FROM Sales.customers c

   SELECT customer_id, COUNT(*) FROM Sales.order GROUP BY customer_id
   -- we can't use this as Sub-query bcuz this is table query which is not allowed in SELECT clause,
   -- SELECT only accepts Scalar sub-query.
   -- So, We can solve it using Correlated Query, which will depends on the Main-query.
   -- In order to make it depends on the main-query we have to connect it through the aliasing and finding the relationship between 2 tables.
   -- Here in this case we will make our sub-query dependent on the main-query.   
```
| customer_id | customer_name | country | score | totalOrders |
|-------------|---------------|---------|-------|-------------|
| 1           | Kevin         | USA     | 350   |  3          |
| 2           | Rohit         | India   | 900   |  3          |
| 3           | Mark          | Germany | 750   |  3          |
| 4           | Marry         | USA     | 500   |  1          |
| 5           | Steve         | USA     | NULL  |  0          |

|                  |  Non-correlated Sub-query                                  |  Correlated Sub-query                                      |
|------------------|------------------------------------------------------------|------------------------------------------------------------|
| ```Definition``` | Sub-query is **independent** of the main-query             | Sub-query is **dependent** of the main-query               |
| ```Execution```  | Executed **once** and its result is used by the main-query | Executed for **each row** processed by the main-query      |
|                  | **Can be** executed on its own                             | **Can't be** executed on its own                           |
|```Easy to use``` | **Easier** to read and simple                              | **Harder** to read and more complex                        |
|```Performance``` | Executed **only once** leads to **better performance**     | Executed **multiple times** leads to **better performance**|
|```Usage```       | Static Comparison, **filtering with Constants**            | Row-by-Row Comparison, **Dynamic filtering**               | 

Now, Let's see What is ```EXISTS``` operator in Sub-query?

</details>

<details>
  <summary> <b>How Database execute Sub-queries?</b> </summary>

Let's understand How database executes Sub-queries behind the scene?

- Suppose you're a Data Analyst & you're writing a query on client side where you have a sub-query inside the main-query.
- So, Once you execute your query, ***Database Engine will first Identify the Sub-query & execute the Sub-query first***.
- Sub-query is like selecting & retrieving data from the table, means Database has to ***retrieve the data from the Disk Store where the table is stored***.
- Once the Sub-query is executed, the result of Sub-query act as an ***Intermediate Result which is stored in Cache Memory***. Means, result of the Sub-query is temporary as well as very fast to retrieve.
- Once the Database is done with Sub-query, Database ***starts executing the main-query***.
- Main-query will go and interact with Cache storage, where data can be retrieve very fast from the result of the sub-query stored in cache.
- Once, It's done, It is going to forward the result to the Database Engine which ***return the final result to the client side***.
- Once everything is executed, the ***Database Engine will clean up the Cache***. So, the result of sub-query will completely destroyed from cache in order to have free space for other queries.

This is how Database Server execute the query behind the scene!!

</details>

<details>
  <summary> <b>Summary of Sub-query</b> </summary>

**Sub-query** 
- A query inside another query. <br/>
- We use sub-query in order to break down a complex query into smaller, simpler and managable pieces, which makes easier to develop and read.

**Use cases of Sub-query**
- In order to create **temporary Result set** to used by another query.
- In order to **Prepared the data before Joining the Tables**
- In order to filter data **Dynamic & Complex filtering**
- In order to check data existence in Correlated Query **Check the existence of Rows from another Table using ```EXISTS```**
- In order to do **Row-by-Row Caomparison - Correlated Sub-query**

</details>

Here, We covered an important technique How to next query in order to solve complex problems.
Next, we will learn another important twchnique on How to do multi-steps in SQL using CTE(Common Table Expression)
<!--------------subqueries------------------->
## 9.2 CTE - Common Table Expression

**CTE(Common Table Expression)** is temporary named result set like a virtual table that could be used multiple times within query to simplify and organize complex query.

<details>
  <summary> <b> What is CTE (Common Table Expression) ?</b> </summary>

> Temporary, named result set(virtual table), that can be used multiple times within the query to simplify and organize complex query.

Let's understand CTE,
Suppose we have data in a table inside the database.
In a simple scenario, we write a simple SQL Query in order to retrieve the data from database & in the output we gets the result of the query.
Now things get complicated in the project and we have to write query inside another query.
Like one query is independent to another query.
```sql
    --CTE QUERY
    SELECT *
    FROM table1
    WHERE condition

    --MAIN-QUERY
    SELECT *
    FROM table2
    WHERE condition
```
Now, what we can do is we can give the new query inside the main-query a name CTE and we can call this new query as CTE inside the main-query

What SQL does in case of CTE Query?
- 1st SQL executes the CTE-query, and we can get few information from database tables.
- The output is going to available only in the query (output is in the shape of table, which is intermediate virtual table)
- Now, in the Main-query we can start querying on that result from CTE(virtual Table) as like any normal table.
- So, Main-query can retrieve information, manipulate it on top of that virtual table(CTE Query result)
- Now, Main-query have two sources of tables, one is It can use tables from database as well as second It can access the CTE virtual table.
- Once Main-Query executed, Everything is done the final result of the main-query is going to be presented for the user as a final result.
> So, CTE query has one task where it generates a table that lives inside our query(virtual table), and we can use it as we want in our query.

- This intermediate virtual table created from CTE Query has 2 features :
1. Once the query ends, this intermediate virtual table will be destroyed by SQL in clean up process.
2. The intermediate table will be only locally available in the same query, It not globally available like Database tables.

### Difference between Sub-Query and CTE

The story of this CTE query and Sub-Query seems like identical.

Q : **What is Difference between Sub-Query and CTE ?** <br/>
Well, Yes, both seems exactly same but still there are differences between them.
 
- The way of writing Sub-Query is from Bottom to Top, where first Main-query is there and inside that we use Sub-query. on the otther hand CTE is written in Top to Bottom approach where we first write CTE and then use it anywhere in the main-query as amy places as many times we want.

- Output of Sub-query can be used only once on the other hand out of CTE is a temporary intermediate virtual table so we can use it as many times as we want.
  
- The main difference between the Sub-query and CTE is from the name itself, common table expression.
We think about the result of CTE as a table so, we can use it like SELECT, JOin with another table. So, Basically it is a hidden like virtual table lives within the query but Sub-query is totally different Its result is only for one position in the main-query. It is used only once. If we want to use the sub-query result at 2 or 3 different places, we have to write the sub-query at 2 or 3 places and times.
That's why we have CTE apart from Sub-query.

</details>

<details>
  <summary> <b>When to use & Why CTE ?</b> </summary>

Why do we need CTE? <br/>
What is the main purpose of CTE ?

Let's say in a complex SQL task, we have to do following steps :
Step 1. Joins the tables together in order to prepare all the data that we need.
Step 2. We have to Aggregate the data like Summarization
Step 3. Again JOIN the tables in order to perform different types of Aggregations again
Step 4. Aggregation like Average

- We have learned that we can use Sub-query in order to make this logic flow of a complex query.
  like Step1. -> Sub-query1, Step2. -> Sub-query2, Step3. -> Sub-query3 and so on at last , Main-Query.

Now, We keep doing this we will have problems, like repeating the same steps more than once. <br/>
like We're joining the table twice in step1 and step3 for different purposes which have 2 different Sub-queries that cause **REDUNDANCY** <br/>
So, Sub-query alone will be not enough to solve complex task. <br/>
So, we have different techniques in order to solve our complex task. <br/>

We can have only one step in order to join the table <br/>
Step 1. JOIN <br/>
Step 2. Aggregations (SUM) <br/>
Step 3. Aggregations (AVG), here we don't need to join again, we can re-use the step1 <br/>
We can do this with the help of **CTE** <br/>
That reduces the number of steps which can reduce the size of query and increase performance.

There are a lot of benefits of using CTE instead of using Sub-queries. We're bereaking down complex query into smaller pieces that are easier to write, manage, understand and logical flow.
With all of those, we reduce RUNDENCIES of our code which lead to better performances.

```sql
   CTE_Top_Customers
   SELECT *
   FROM Sales.customers
   WHERE conditions

   CTE_Top_Products
   SELECT *
   FROM Sales.products
   JOIN table

   CTE_Daily_Revenue
   SELECT *
   FROM Sales.customers
   JOIN table

    --At th end, we can put everything together in the main-query

   SELECT *
    FROM CTE_1,CTE2,CTE3
    WHERE conditions
```
### Advantages of using CTE inside the Query.

CTE reduces the **REDUNDANCY** of code.
- Means, Number of code will be reduce, which improves the performances

CTE improves the **READABILITY** of query.
- Means, code is divided into clear sections and making it easier to understand what each part does..

CTE introduces the **MODULARITY**
- Means, Instead of writting a huge query, It breaks code into smaller, managable parts.

CTE provides the **REUSABILITY**
- Means, We can have a result set that is used multiple times inside our query. Write the logic once and use it as many time you needs.

</details>

<details>
  <summary> <b>How Database Execute CTE ?</b> </summary>

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/6434aca4-7a05-4dc2-8490-ed3cbbba8db8" />

- Again, as a user(Data Analyst) at client side, writing a query where you defining a CTE called Details inside that you have some logic and inside the main-query we're selecting the data from a database table orders and joining it with Details CTE multiple times using multiple conditions. <br/>
Once we execute this query, the Database Engine is going to read the query and identify the CTE with priority and It will execute the - CTE first. and let's say in CTE, you're retrieving data from a table which is stored in Disk Storage inside USER Data.
Once CTE executed successfully, the Database Engine will place the result of CTE in Cache Memory & named it as Details like a table name.
- Next step Database Engine will do is start executing Main-Query step by step. So in order to get the data from table orders, since orders table exists in Disk Storage inside User Data so, DB Engine retrieve it from there.
Now DB Engine will check Details which is mentioned inside Main-query, as Details which is a virtual table lives in Cache memory. So, DB Engine start retrieving data from Details with high speed and then Join the Data with data of table of main-query.
Again as many time we have joined in the main-query with details It will do the same.
So, basically in the main-query we are using CTE result multiple times in different places and retrieval of all those information is happening with high speed this is the benefits of CTE is to utilize the high speed memory of the cache.
- Once the main-query is completely executed, the result is going to return to the database engine then DB Engine sent it back to the client side & we will see the final result it in output.

This is how Database Server execute CTE behind the scene.
</details>

<details>
  <summary> <b>Types of CTE </b> <code>Non-Recursive CTE</code> and <code>Recursive CTE</code> </summary>

We have different types of CTE.
1. **Non-Recursive CTE**
      - **Standalone CTE** : Single, Multi Standalone CTE
      - **Nested CTE**
2. **Recursive CTE**

```
Types of CTEs
│    
│
└───Non-Recursive CTE
│       │   
│       │
│       └───Standalone CTE
│       │      │   
│       │      └───Single Standalone CTE   
│       │      │
│       │      └───Multi-Standalone CTEs 
│       │
│       └───Nested CTE
│
│  
│  
└───Recursive CTE
```
    
</details>

**A. Non-Recursive CTEs** : ```Standalone CTE``` and ```Nested CTE```
> Non-Recursive CTE is executed only once without any repetition.

<details>
  <summary> <b> Standalone CTE </b> </summary>

> Defined and used independently. <br/>
> Runs independently as it's self-contained and doesn't rely on other CTEs or queries.

- It is a CTE query that is defined and used independently in the query.
- Means, It is self-contained and it doesn't depend on any other CTE or queries.

- We can run the Standalone query independently from anything inside our query.

``` Database <-----> CTE ------> Intermediate Result <-------> Main-Query ------> Final Result```

So, Simply CTE query the database and has one output. Since this CTE is independent from anything else we call it as standalone CTE.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/27a3ef31-397d-4273-9f34-827938733605" />
Standalone CTE

Here, If we compare the Stondalone CTE with Main-Query, Main-query can't be executed alone here, bcuz It needs the result from the CTE query.

**Syntax of Standalone CTE**:
```python
    # This is how we define CTE using WITH Clause
    WITH CTE_Name AS
    (
      SELECT .....
      FROM .....
      WHERE ....
    )

    -- This is how we use CTE in the Main-Query
    SELECT ....
    FROM CTE_Name
    WHERE ....
```
So, We use query inside the ```WITH``` clause, we call it as a **CTE Query** where we define a CTE inside the ```WITH``` and Of course we don't want only to define CTE, so we can use CTE outside of definition.

```python
     ## Project 
     # Step1. Find the total sales per customer
     ## Since we have only one step, It make no sense to use CTE.
     ## But we will use it since we know there will be other steps later

      WITH CTE_TotalSales AS
      (
          SELECT
               customer_id,
               SUM(sales) AS totalSales
          FROM Sales.orders
          GROUP BY customer_id
      )

      ## Main-Query
      SELECT
         c.customer_id,
         c.customer_name,
         cte.totalSales
      FROM Sales.customers c
      LEFT JOIN CTE_TotalSales cte
      ON cte.customer_id = c.customer_id
```
| customer_id | customer_name| totalSales |
|-------------|--------------|------------|
|  1          | Joseph Bibin | 110        |
|  2          | Kevin Brown  | 55         |
|  3          | Mark Wood    | 125        |
|  4          | Rohit Negi   | 90         |
|  5          | Pavan kalyan | NULL       |

Here, customer_id and customer_name come from customer table and totalSales is coming from CTE as we don't have employee_id 5 in CTE orders table that's why it is NULL in the output. as we're doing LEFT JOIN.

> **CTE Rule : We can't use ```ORDER BY``` directly within the CTE***. Rest everything we can use inside CTE definition.
```python
      WITH CTE_TotalSales AS
      (
          SELECT
               customer_id,
               SUM(sales) AS totalSales
          FROM Sales.orders
          GROUP BY customer_id
          ORDER BY customer_id  # Throw Error
      )

      ## Main-Query
      SELECT
         c.customer_id,
         c.customer_name,
         cte.totalSales,
      FROM Sales.customers c
      LEFT JOIN CTE_TotalSales cte
      ON cte.customer_id = c.customer_id
```
This was the simplest Standalone CTE.
But we can have have multiple CTEs.

**Multiple Standalone CTEs**

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/7ca75c6d-d4ef-4c3e-a836-7d190570829c" />

- Here, in this example we have not only one CTE but have multiple CTEs in our query.
- And Each CTE is standalone CTE, As SQL will execute all of those CTEs one by one from Top to Bottom.
- All of the CTEs are independent of everything, so all will prepared their own intermediate results.
- Then Main Query will retrieve all those informations and do prepare the final result for the end user.

**Syntax of Multiple Standalone CTEs**:
```python
   ## CTE1
   WITH CTE_Name1 AS
   (
      SELECT
           column1,
           column2,....
      FROM table1
      WHERE condition
   )

  , CTE_Name2 AS
   (
      SELECT
           column1,
           column2,....
      FROM table2
      WHERE condition
   )

  , CTE_Name3 AS
   (
      SELECT
           column1,
           column2,....
      FROM table3
      WHERE condition
   )

  , CTE_Name4 AS
   (
      SELECT
           column1,
           column2,....
      FROM table1
      WHERE condition
   )

  ## Main-Query
  SELECT
      column1,
      column2,...
  FROM CTE_Name1
  JOIN CTE_Name2
  JOIN CTE_Name3
  JOIN CTE_Name4
  WHERE condition
```
> Only the 1st CTE uses ```WITH``` clause in order to tell SQL about the definition of CTE and all the other CTEs will be defined separated using comma ```,``` intead of ```WITH```.

```python
     ## Project 
     # Step1. Find the total sales per customer
     # Step2. Find the last order date per customer

# Step1. Find the total sales per customer
      WITH CTE_TotalSales AS
      (
          SELECT
               customer_id,
               SUM(sales) AS totalSales
          FROM Sales.orders
          GROUP BY customer_id
      )
# Step2. Find the last order date per customer
      , CTE_LastOrder AS
      (
            SELECT
                 customer_id,
                 MAX(order_date) AS lastOrder
            FROM Sales.orders
            GROUP BY customer_id
      )  

# Main-Query
      SELECT
         c.customer_id,
         c.customer_name,
         cts.totalSales,
         clo.lastOrder
      FROM Sales.customers c
      LEFT JOIN CTE_TotalSales cts
      ON cts.customer_id = c.customer_id
      LEFT JOIN CTE_LastOrder clo
      ON clo.customer_id = c.customer_id
```
| customer_id | customer_name| totalSales | lastOrder |
|-------------|--------------|------------|-----------|
|  1          | Joseph Bibin | 110        | 2026-02-15|
|  2          | Kevin Brown  | 55         | 2026-03-10|
|  3          | Mark Wood    | 125        | 2026-03-21|
|  4          | Rohit Negi   | 90         | 2026-02-18|
|  5          | Pavan kalyan | NULL       | NULL      |
</details>

<details>
  <summary> <b>Nested CTE </b> </summary>
> CTE inside another CTE. <br/>
> A nested CTE uses the result of another CTE.
> So, It can't run independently.

- It is a CTE inside another CTE, kind of query inside another query.
- So, Not only the Main-query can use the result of a CTE But another CTE can also use the result from a CTE.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/86428922-21cc-4cb4-a2b1-41a023b1cc59" />

**Syntax of Nested CTE**
```python
     # STANDALONE CTE
     WITH CTE_Name1 AS
     (
         SELECT
               column1,
               column2,....
         FROM table1
         WHERE condition
      )
      # NESTED CTE
      , CTE_Name2 AS
      (
          SELECT
               column1,
               column2,....
            FROM CTE_Name1
            WHERE condition
       )

       # Main- Query
         SELECT
               column1,
               column2,....
         FROM CTE_Name2
         WHERE condition
```

```python
     ## Project 
     # Step1. Find the total sales per customer
     # Step2. Find the last order date per customer
     # Step3. Rank the customers based on the total Sales per customer

# Step1. Find the total sales per customer
      WITH CTE_TotalSales AS
      (
          SELECT
               customer_id,
               SUM(sales) AS totalSales
          FROM Sales.orders
          GROUP BY customer_id
      )
# Step2. Find the last order date per customer
      , CTE_LastOrder AS
      (
            SELECT
                 customer_id,
                 MAX(order_date) AS lastOrder
            FROM Sales.orders
            GROUP BY customer_id
      )
# Step3. Rank the customers based on the total Sales per customer (Nested CTE)
      , CTE_CustomerRank AS
      (
             SELECT
                   customer_id,
                   RANK() OVER(ORDER BY totalSales DESC) AS custRank
              FROM CTE_TotalSales
      )

## Main-Query
      SELECT
         c.customer_id,
         c.customer_name,
         cts.totalSales,
         clo.lastOrder,
         crs.custRank
      FROM Sales.customers c
      LEFT JOIN CTE_TotalSales cts
      ON cts.customer_id = c.customer_id
      LEFT JOIN CTE_LastOrder clo
      ON clo.customer_id = c.customer_id
      LEFT JOIN CTE_CustomerRank ccr
      ON crs.customer_id = c.customer_id    
```
| customer_id | customer_name| totalSales | lastOrder | custRank |
|-------------|--------------|------------|-----------|----------|
|  3          | Mark Wood    | 125        | 2026-03-21|  1       |
|  1          | Joseph Bibin | 110        | 2026-02-15|  2       |
|  4          | Rohit Negi   | 90         | 2026-02-18|  3       |
|  2          | Kevin Brown  | 55         | 2026-03-10|  4       |
|  5          | Pavan kalyan | NULL       | NULL      | NULL     |

```python
 ## Project 
     # Step1. Find the total sales per customer
     # Step2. Find the last order date per customer
     # Step3. Rank the customers based on the total Sales per customer
     # Step4. Segment customers based on their total sales.

# Step1. Find the total sales per customer (Standalone CTE)
      WITH CTE_TotalSales AS
      (
          SELECT
               customer_id,
               SUM(sales) AS totalSales
          FROM Sales.orders
          GROUP BY customer_id
      )
# Step2. Find the last order date per customer (Multi-Standalone CTE)
      , CTE_LastOrder AS
      (
            SELECT
                 customer_id,
                 MAX(order_date) AS lastOrder
            FROM Sales.orders
            GROUP BY customer_id
      )
# Step3. Rank the customers based on the total Sales per customer (Nested CTE)
      , CTE_CustomerRank AS
      (
             SELECT
                   customer_id,
                   RANK() OVER(ORDER BY totalSales DESC) AS custRank
              FROM CTE_TotalSales
      )
# Step4. Segment customers based on their total sales. (Nested CTE)
       , CTE_CustomerSegment AS
      (
            SELECT
                 customer_id,
                 CASE
                     WHEN totalSales > 100 THEN 'High'
                     WHEN totalSales > 80 THEN 'Medium'
                     ELSE 'Low'
                 END AS custSegment
            FROM CTE_TotalSales
      )

## Main-Query
      SELECT
         c.customer_id,
         c.customer_name,
         cts.totalSales,
         clo.lastOrder,
         crs.custRank,
         ccs.custSegment
      FROM Sales.customers c
      LEFT JOIN CTE_TotalSales cts
      ON cts.customer_id = c.customer_id
      LEFT JOIN CTE_LastOrder clo
      ON clo.customer_id = c.customer_id
      LEFT JOIN CTE_CustomerRank ccr
      ON crs.customer_id = c.customer_id
      LEFT JOIN CTE_CustomerSegment ccs
      ON ccs.customer_id = c.customer_id

```
| customer_id | customer_name| totalSales | lastOrder | custRank | custSegment |
|-------------|--------------|------------|-----------|----------|-------------|
|  3          | Mark Wood    | 125        | 2026-03-21|  1       |   High      |
|  1          | Joseph Bibin | 110        | 2026-02-15|  2       |   High      |
|  4          | Rohit Negi   | 90         | 2026-02-18|  3       |   Medium    |
|  2          | Kevin Brown  | 55         | 2026-03-10|  4       |   Low       |
|  5          | Pavan kalyan | NULL       | NULL      | NULL     |   NULL      |

So, Here we have done a kind of Mini-Project, where we have analyze the customer information based on different aspects of data. We have done it like step by step. Now, we know How to write a complex queries using the help of CTEs. If anyone go through this SQL script, It is easy to understand as It is divided into multiple steps and each block is responsible for one specific problem of the whole report. This is the power of CTE. It introduces modularity. This is an amazing way on how to organize the project using SQL and How to structure works in SQL.

</details>

<details>
  <summary>Best Practices of using CTEs </summary>

**Best Practice of CTEs**

> ***"With great Power comes great Responsibility."***

All the SQL Developers, across the different projects, all of them love using CTE eveywhere, each time. It's okay. 
But problem with CTE is Its overuse. Of course, CTE is powerful. But with power comes responsibility.
So,Advice is try to not add a new CTE each time, you're doing something new.

If you think using CTEs, everything is organized and easy to read but if you have a lot of CTEs especially if they're nested. It is impossible to understand what is going on. even if developer describe each CTE with the task.

So, It is impossible to read & understand as well as It will take a lot of memory which might get bad performance.

> 1. Rethink and refactor your CTEs before starting a new one.
> 2. Don't use more than 5 CTEs in one query; Otherwise your code will be hard to understand and maintain.
</details>


**B. Recursive CTE**
> It is Self-referencing query that repeatedly processes data until a specific condition is met.

<details>
  <summary> <b>Recursive CTE</b> </summary>

- We usually use Recursive CTE if we have like hierarchical structure & we want to navigate and travel through the hierarchy.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/89eab1d3-a89c-407b-8490-34302e51416f" />

**Syntax of Recursive CTE**
```python

# RECURSIVE-CTE
  WITH CTE_Name AS
  (
     #Anchor Query : which is executed only once, provide 1st step
     SELECT
            column1,
            column2,.....
      FROM table1
      WHERE condition

     UNION ALL # Connector : To connect the achor query & recursive query
     # Recursive Query : which is executed multiple times, keep repeating & add data to the intermediate result until the breaking point
     SELECT
           column1,
           column2,....
     FROM CTE_Name
     WHERE [Break Condition]
  )

#MAIN-QUERY
 SELECT
       column1,
       column2,.....
 FROM CTE_Name
 WHERE condition
```
- SQL will go and execute the Anchor-Query only once and there after SQL will execute Recursive-Query and keep executing reapeating it until a certain condition is met and then SQL will be out from CTE.

```python
  # Generate a sequence of number from 1 to 20

  WITH CTE_Recursive AS
  (
    # Anchor Query
    SELECT
          1 AS myNumber
    UNION ALL
    # Recursive Query
    SELECT
         myNumber + 1
    FROM CTE_Recursive
    WHERE counting < 20
  )

# Main -Query

SELECT
     *
FROM CTE_Recursive
```
| myNumber |
|----------|
|  1       |
|  2       |
|  .       |
|  .       |
|  .       |
|  20      |

> We can control How many recursion we can have in our query by defining it in the Main-query ```OPTION(MAXRECURSION 10)```
> The Default recursion in SQL is 100.

```python
WITH Series AS
  (
    # Anchor Query
    SELECT
          1 AS myNumber
    UNION ALL
    # Recursive Query
    SELECT
         myNumber + 1
    FROM Series
    WHERE counting < 100
  )


# Main -Query
SELECT
     *
FROM Series
OPTION(MAXRECURSION 1000)
```

Let's see SQL execute the Recursive Query. 

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/ab254af3-dc01-4c1f-9ef1-ac0c4c845441" />

- First step is to run Anchor-query, And It will be executed only once.
- Next step, SQL will execute the Recursive-query, It will print myNumber + 1 which is 2  and then check for condition if myNumber < 20 again it will execute the recursive-query with this time myNumber become 2, so this time it will print 2 + 1 = 3 then check for condition where myNumber < 20 again myNumber is 2 only so It will again execute the recursive query and it will keep repeating this process until the condition is met,
- So at the end when the myNumber will become 20 so the condition of myNumber < 20 will not satisfied and will be stop.

```python
# Task : Show the employee hierarchy by displaying each employee's level within the organization.

WITH Emp_Hierarchy AS
(
   # Anchor Query : This is only for Top Level Manager
   SELECT
        employee_id,
        employee_name,
        manager_id,
        1 AS Level
   FROM Sales.employees
   WHERE manager_id IS NULL
   UNION ALL
   # Recursive Query : Connecting Manager with Employees
   SELECT
        employee_id,
        employee_name,
        manager_id,
        level+1 AS level
   FROM Sales.employees e
   INNER JOIN Emp_Hierarchy ceh 
   ON e.manager_id = ceh.employee_id
   # There's no need of WHERE as INNER JOIN is fiter here which shows only those rows which are common between 2 tables 
)

# Main Query
SELECT *
FROM Emp_Hierarchy
```
| Employee_id | Emp_name | Manager_id | Level |
|-------------|----------|------------|-------|
|   1         | Frank    | NULL       |  1    |
|   2         | Kevin    | 1          |  2    |
|   3         | Mary     | 1          |  2    |
|   4         | Carol    | 3          |  3    |
|   5         | Michael  | 2          |  3    |

- In we have hierarchy in our data, and we can see in one table things are referencing each others like Manager_Id is actually the employee_id. So, Here we're referencing those Ids to each other. This means there is a hierarchy & there is a structure in this table. And we can use the Recursive CTE in order to build those level and to navigate as well.

```
         Frank              Level-1
           │       
           │
 Kevin──────────────Mary    Level-2
  │                   │   
  │                   │
Michael             Carol   Level-3
```
</details>

<details>
  <summary> <b>Summary of CTE</b> </summary>

**CTE**
- CTE(Common Table Expression) is a temporary, named result set (virtual table) that can be used at multiple places within the query.
- 2 Types of CTE Non-Recursive CTE and Recursive CTE
- Non-Recursive CTE : Standalone CTE which can be single or Multi-standalone CTE another is Nested CTE
- Advantages of CTEs
  - Readability : Break down complex query into smaller pieces
  - Modularity : Smaller pieces are easy to manage, develop and self-contained
  - Reusability : Reduce redundancies in the query
  - Recursive : Helps in Iterations & Looping in SQL
- Result of CTE is like Table but It can't be used from external query.
- We can use the Result of CTE from Main-Query as well as another CTE can use the result of a CTE which leads to having Nested CTE.
- We can use the Result of a CTE within itself which makes it Recursive CTE which allows Looping & Iterations.
- **Tips** : Don't create more than 5 CTEs in one query.

</details>

Next thing we will learn is a new type of object that we can use in database. We don't have only table in the database, We have Views. Views are amazing in order to give dynamic & flexible features in SQL project.

<!-------------------CTE--------------------->
## 9.3 View - Views in Database

Views is not like a query that we can use it in SQL. <br/>
It is an object that we can find in the database. <br/>
Before jumping to Views let's see the whole structure of the database. <br/>

<details>
  <summary> <b>Structure of Database</b> </summary>

- on the top of the hierachy of database structure is SQL Server.
- It manages multiple databases.
- It is like a control centre that keep everything running and accessible.

> **Database Server** stores, manages and provide access to databases for users or applications.
> - Inside SQL Server we have multiple databases.

> **Database** is a collection of information that is stored in a structured way.
> - It is where all of data is stored & oraganized in different tables and objects.
> - Each database is separated from others & has its own data.
> - Inside each database we have multiple schemas.

> **Schema** : is a logical layer that groups related objects together.
> - a logical way on how to group up realted objects like tables and view together within a database.
> - Example : We have database Sales which contains multiple tables, multiple views and objects about the Customers that will be put under Customer Schema. Similar way we can have another schema Orders which contains objects(tables,views) related to orders inside sales database.
> - Inside each schema we have Tables
> - Inside schema we have another type of object we call it View

> **Table** : is a place where data is stored and organized into rows and columns.
> - It is where data physically live.
> - Inside Table we can define multiple stuffs like Columns, Keys

> **View** : is a virtual table that shows data without storing it physically.
> - It has structure and everything but inside it we don't have any data.
> - So, View does not store data, in order to see the data we have to execute the query behind the view only after that we can see data but it's not like table which stores data permanently.
> - Inside View also we can define multiple stuffs like Columns, Keys

> For each **Column** we have a name & datatype.

Here, We can see Databases are really organized & where in hierarchy Top node is SQL **Server** and Lowest node is **Columns & Rows**. This is what we called as **Database Structure**

Now, in order to build and manage this structure, we have set of commands, we call it **DDL (Data Definition Language)**

**DDL (Data Definition Language)** <br/>
A set of commands that allows us to define and manage the structure of a database.
- ```CREATE``` to create databases, schemas, tables, views
- ```ALTER``` to modify or change in databases, schemas, table, views
- ```DROP``` to remove databases, schemas, tables, views

So, With this, we can say, we can create views inside the Server > database > schemas > Views.

Note : Generally, We don't create Schema but when you navigate through Object explorer of your Server > Databases > Security > Schema > List of all schema will be there. How? we didn't created!

Well, As we create a database in our SQL Server, we get a lot of other system default schemas created automatically. One of them is INFORMATION_SCHEMA where it holds a lot of views about Catalog and the metadata where we can find a list of columns, tables, views and other stuffs.

```
Database Server
│ 
├── Database1 (Sales)
│      │
│      ├── Schema1 (Customers)
│      │      │
│      │      ├── Tables(Rows & Columns)
│      │      │     └── Columns (col2,col2,...)
│      │      │     │     └── Name
│      │      │     │     └── DataType
│      │      │     └── Keys (PK, FK)
│      │      │     └── Constraints
│      │      │     └── Triggers
│      │      │     └── Indexes etc etc..
│      │      └── Views
│      │            └── Columns (col2,col2,...)
│      │            └── Keys (PK, FK)
│      │
│      └── Schema2 (Orders)
│             │
│             ├── Tables      
│             │
│             └── Views
│
├── Database2 (HR)
│      ├── Schema1
│      └── Schema2
│
│
└── DatabaseN
    ├── Schema1
    ├── Schema2
    └── SchemaN
```

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/26d499df-eb7f-4966-bb62-2822036318b6" />

</details>

In order to understand View we also have to learn a fundamental concept **3-Level Architecture of the Database**

<details>
  <summary> Database <b>3 Levels Architecture :</b> <code>Physical layer</code> <code>Logical layer</code> <code>View layer</code> </summary>

This architecture describe the different level of Data Abstraction in a database.

The Architecture is divided into 3 levels :
1. **Physical Level**
2. **Logical Level**
3. **View Level**



**1. Physical Level** 
- It is lowest level of the database where the actual data is stored in a physical storage.
- Usually DBA has access to Physical Level bcuz DBAs are experts & they have to manage the access & security of this layers.
- As an expert **DBA**, they have to manage a lot of stuffs like **Optimizing the performance, making sure evrything is secureand, managing the backups & recovery and all the configurations** etc etc.
- At Physical Layer, we have to deal with lot of stuffs like **Data Files, Partitions, Logs, Catalogs, Blocks and caches** and other tuffs that database needs in order to store data.
- So, Physical Layer is very complicated and we need to be expert in order to manage Physical Layer.
- Sometime, Physical Layer is also known as **Internal Layer**.

**2. Logical Level**
- This is less complex than physical layer. Here, we have to deal with **How to organize you data?***
- Normally **Application Developers** or **Data Engineers** who access the Logical Layer in order **to define structure of data**. So, Developers & Data Engineers are only focus on How to structure data rather than How the data is exactly stored physically at Physical layer. That's why we need abstraction for the role of Developers & Data Engineers.
- What actually Developers & Data Engineers do at this logical layer is **Create tables & define the relationships between those tables**, **Create Views**, **Creates Indexes on table in order to optimize the performace of table**, **create Store procedures, functions and some codes in order to manage those tables**.
- > So, Basically Developers & Data Engineers builds the Data Model, they structure data but they don't care about where are those data stored physically in the database.
- So, Here things are little complicated in this Logical Layer and It is perfect abstraction for developers to build projects.
- Sometime we call Logical Layer as **Conceptual Layer**.

**3. View Level**
- View layer is the **highest level of abstraction** in the database.
- It is about **What the end users and applications can access and see?**
- Example :
  - We can have 1 View for Business Analyst, So Here, we prepare a customized Views that are suitable only for the Business Analyst.
  - Another set of Views that are suitable for Data Visualization & reporting (Connect it through PowerBI in order to create Dashboard)
- So, we can create multiple set of views that are suitable for specific purpose and use case.

- So, at this View Layer, we're exposing our data for multiple-users and multiple-applications.
- End Users and Applications have to only Views. They don't have to deal with the tables, indxes, Store Procedures, any files, logs, partitions or anything. This is the highest level of abstraction bcuz the focus of this layer is to make it friendly & easy to consume for the End users/applications.
- Sometime, we call View Layer as **External Layer**.

This is the 3-Level Architecture of the Databases or we call it 3 Abstraction level of the database!
That's why Views are very important concept in SQL

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/64f1483e-f928-4f45-9795-4f5ff35e0b08" />

</details>

Let's get back to the View

<details>
  <summary> <b>What is View?</b> </summary>

> View is a virtual table based on the result set of a query, without storing the data in database. <br/>
> Views are persisted SQL queries in the database.
- A view is a virtual table in SQL that is based on the result of a query, without actually storing the data in database.
- In short, Views are stored or persisted SQL queries in the database.

So, for we have seen a database table and we query our table in order to retrieve data from this table.Once we execute the query we will get the result. 

Now, We talk about Views. They are also like Table which have structures but without any data inside it. 

For each View there is a query attached to it. So, there is no data but have query in order to get data.

> We call **Normal table as Physical Table** and **View as Virtual Table**

How we get data in View?
- So, If we write a query by selecting data from View. What SQL does is It triggers the query that is attached to View and this query is responsible to query the physical table and 
- then the result of the query will fill the structure of the view and result is also loaded in the view.
- So, Here we are directly querying the View but actually we're querying the physical table.
- Basically Views are between us and data that means Real data is stored inside database table and **views are like abstraction layer between us and real data**
- Of course, the data is not stored inside the view, each time we're querying View, SQL will execute the query behind the view.It will retrieve the data in view and then we see the data in output.

</details>

<details>
  <summary> <b> Tables vs Views</b> </summary>

**Table**
- **persisted Data** : tables stores actual data physically inside database. So, tables is where **data is Persisted** physically.
- **Hard to maintain**. It takes a lot of efforts in order to change like adding/removing columns.
- **Fast Response** : tables are faster than views
- **Read/Write** : We can read & write on tables.

**View**
- **No persistance** : Views are virtual tables, they don't store any data inside database, but they present the data from underlying tables. So, In views **No persistance data** physically.
- **Easy to maintain**. It is very flexible to change, all what we have to do is change the query of the View.
- **Slow Response** : bcuz getting data from view means 2 query first on View and second is behind the scene querying on table
- **Only Read** : View are Read Only as the name itself says **View**. We can't write something to the database using the view

| Table            |  View            |
|------------------|------------------|
| Persisted Data   | No persistance   |
| Hard to Maintain | Easy to Maintain |
| Fast Response    | Slow Response    |
| Read & write     | Read Only        |

</details>

<details>
  <summary> <b>USECASE OF VIEWS </b> : Why do we need view? </summary>

### **Use case 1 : Central Logic in Complex Query**

> We use view is **to store central logic from a complex query in the database** so that everyone can access it and we improve reusability between multiple queries which reduces the project complexity.

In our project, we have 2 tables Orders and Customers inside Sales database. <br/>
We have learned that if we have a complex query, we can use **CTEs**. <br/>
So, in an example, we're joining tables and doing aggregations like SUM. So, CTE is storing the data in intermediate results and then we have a main-query, we're ranking the data. So, whole thing is in one query. 
- Let's say a financial Analyst is doing this analysis.
- We have another user Budget Analyst, He is doing exactly same first step but in last step instead of ranking, doing MAX & MIN in the main-query.
- We have another third user who is Risk Analyst, He is also doing same initial steps using CTE joining the tables & summarization But in last step He is just comparing the data in the main-query.

Problem : <br/>
If we look back to this, We can see all those 3 Data Workers are doing same initial first step like same CTE(Joining & Summarizing data).
It is a complete waste of time that each of them has to create first CTE from scratch in order to analysis.
It's redundancy and make no sense. 
This is exactly the dis-advantage of only using CTEs in the complex query of project.

Solution : <br/>
What we can do instead of that, those 3 data workers decide let's put the 1st step as a View in the database.
So, instead of using CTE each time, take this script and put it in database.
Now a central logic that is stored in the database where everyone can use it.
So, Now the Query logic that was repeated, is now only once execute & everyone can benefit from it.
So, Now financial Analyst, instead of directly going to physical tables, He can go to the view.
So Now he only needs to write one script Rank.
Same for Budget Analyst he only need to write MAX,MIN script
Similar way Risk Analyst also have to write only one script for comparison.

Now We can see all those query are reduced and here we only focus on Analysis.
This is exactly the magic of views in the Data Analytics.

The logic/script/knowledge can be centralized in the database. This is way faster & better than having writing logic/script each time when someone need to do analysis.

### **Use case 2 : Hide Complexity**

> Views can be use to hide the complexity of database tables and offers users more friendly and easy-to-consume objects.

We use view in order to hide complexity and to improve abstraction.
- In many scenario, we work with very large and complex databases, so we use view in order to reduce the complexity and make things easier for the users.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/9acc9348-3891-4ef8-89f6-5b11fe8b0e66" />


We get an access to the database, where you want to do analysis.
You will get a large database where the tables are very complex to understand. They have a lot of columns, column names will te technical & cryptical, How tables are connected to each other, relationships between tables, It's impossible to understand for everyone. To understand, we have to deeply involve with the data-model with documentations & with experts KT(Knowledge Transfer) to understand How to query the particular database.
This things are done by Developers & Data Engineers But from End-User perspective, It is going be nightmare where you're stuck with multiple joins in order to make even simple analysis.

Of course, from Database perspective this Data-model is good enough for one application but if you're opening your database for multiple Data Analysis Projects this can be nightmare bcuz we have to explain each user how to query data.

So, what we usually do, instead of giving the direct access of technical & hard to understand Data-Model, Developers & Data Engineers(as they're Exerts of Data-Model) simply create multiple views.
This new Views is an abstractions of the complexity that have in the technical & large database.
We just have to make sure those Views are providing objects that are friendly.
Like Normal plain English names of tables & columns, limited views which have lot of informations so that end-users don't have to joins things here & there for Analysis.

> Basically end users have access to something more friendly & easy-to-consume, They just have to write simple query in order to do analysis on top of views.

> This is what Developers & Data Engineers provides a Data Product from complex physical database.

 This is how Views are important in order to provide abstraction and easy to consume objects for the end users.
 Views Scripts are written only once by experts of Data-Model like Data Engineers & Developers.
 This is how huge complex Data Projects will become easier for end user.

 ```python
# Task : Provide a view that combines details from orders, products, customers and employees.
# orders table is central table that connects eveything.

CREATE VIEW Sales.V_Order_Details AS (
     SELECT
        o.order_id,
        o.order_date,
        p.product,
        p.category,
        COALESCE(c.first_name, '') + ' ' + COALESCE(c.last_name,'') AS customer_name,
        c.country AS customer_country,
        COALESCE(e.first_name, '') + ' ' + COALESCE(e.last_name,'') AS sales_name,
        e.department,
        o.sales,
        o.quantity
    FROM Sales.orders o
    LEFT JOIN Sales.products p
    ON p.product_id = o.product_id
    LEFT JOIN Sales.customer c
    ON c.customer_id = o.customer_id
    LEFT JOIN Sales.employees e
    ON c.employee_id = o.salesPerson_id
)

# This result relatively big but still we have all the information in one view
# & it is more friendly for the users to consume our data instead of joining 4 tables together



SELECT
      *
FROM Sales.V_Order_Details
```
| order_id | order_date | product | category | customer_name | cust_country | Sales_name | department | sales | quantity |
|----------|------------|---------|----------|---------------|--------------|------------|------------|-------|----------|
| 1        | 2026-01-01 | Bottle  | Accessories | Kevin Brown| USA          | Mary       | Sales      | 20    |    1     |
| 2        | 2026-01-05 | Tire    | Accessories | Mary       | Germany      | Carol      | Sales      | 120   |    1     |
| 3        | 2026-01-10 | Gloves  | Clothing    | Kevin Brown| USA          | Frank      | Sales      | 220   |    2     |
|..........| ...........|.........|..........|...............|..............|............|............|.......|..........|


### **Use case 3 : Data Security**

> Use vew to enforce security and protect sensitive data, by hiding columns or rows from tables.

We use Views in order to implement Security & to protect our data in the database.

In many scenario, we have sensitive information in our data and we can't share it with everyone. So, one of the best practice is to create views in order to protect your data before sharing it with users.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/aa85407e-77b2-4eb7-bf37-2fcf7b527cd8" />

Suppose, We have a table orders, in our projects we have multiple people like Manager, Data Analyst other users which can be access the data.
We can see here, we have different people with different roles but having same rights of accessing directly the table. All of them are seeing whole tables all rows all columns.
In real project this is a big problem, because sometime data are sensitive & we can't give access to everyone.
Of course if you're using only tables, It is going to nightmare, we can't create multiple tables bcuz it's hard to make all table sync.
But Instead of that we have views, So we can remove all access to the physical table rather we can create multiple views for each role.
Like create view order_managers, create another view orders_analyst which contains all the columns except the sensitive one, so here we protects sensitive information we call it ***Column-level Security***.
Also create another views orders_users for other users, here we will not only protecting columns which are sensitive but also few rows bcuz we don't want to share few rows which are sensitive. So here we are protecting columns as well as rows ***Column-level + row-level security***

- Creating views are really easy and It provides us a perfect tool in order to manage the security of our data.

```python
# Task : Provide a view for EU Sales Team that combines details from all tables and execludes data related to the USA.

CREATE VIEW Sales.V_Order_Details_EU AS (
       SELECT
            o.order_id,
            o.order_date,
            p.product,
            p.category,
            COALESCE(c.first_name, '') + ' ' + COALESCE(c.last_name,'') AS customer_name,
            c.country AS customer_country,
            COALESCE(e.first_name, '') + ' ' + COALESCE(e.last_name,'') AS sales_name,
            e.department,
            o.sales,
            o.quantity
      FROM Sales.orders o
      LEFT JOIN Sales.products p
      ON p.product_id = o.product_id
      LEFT JOIN Sales.customer c
      ON c.customer_id = o.customer_id 
      LEFT JOIN Sales.employees e
      ON c.employee_id = o.salesPerson_id
      WHERE c.country != 'USA'
)


SELECT
      *
FROM Sales.V_Order_Details_EU
```
| order_id | order_date | product | category | customer_name | cust_country | Sales_name | department | sales | quantity |
|----------|------------|---------|----------|---------------|--------------|------------|------------|-------|----------|
| 1        | 2026-01-01 | Bottle  | Accessories | Smith Brown| France       | Mary       | Sales      | 20    |    1     |
| 2        | 2026-01-05 | Tire    | Accessories | Mary       | Germany      | Carol      | Sales      | 120   |    1     |
|..........| ...........|.........|..........|...............|..............|............|............|.......|..........|

So, We can say - Views are really great in order to provide security to our data whether we're protecting the columns or rows.

### **Use case 4 : Flexibility & Dynamic**

> We use Views in order to have more dynamic & flexibility in our projects.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/5d4a7a30-7324-408e-9c83-27734e0037cd" />


If we have a table & multiple users accessing this table.
Now as a requirement you have to change the Data-Model, like instead of one table split it into two tables or rename the table or rename few columns or add a new column & remove old column and so on. Basically making changes in your physical data-model & stuffs like rows in table.

The problem with this is all those users who are accessing the table will scream bcuz all of them have a complex SQL queries & a small changes in table will break everything in their query, This means **Escalations** and We don't have freedom to change anything in our database without discussing with 100 people team.

So, Instead of that, create a view and tell users to consume the view. Now you have freedom to do any changes you want.
So, Now go to table and do splitting, renaming, adding, removing everything you want as long as you're updating the query between the table and view to make sure users are not noticing any change.

For example 
- If we want to split the table into 2 tables then we have to put join or union in View Query in order to re-construct the same structure that users are used to.
- If you're renaming a column in your table, you can simply make the changes in your View Query.

So, No-one is going to notice that you're doing changes in the physical table.

Using Views & offering it to users is a game-changer for Data-Engineers & Developers bcuz giving users views will give more freedom, It is flexible & dynamic to change anything in your data-model without getting any headache.


### **Use case 5 : Multiple Languages**

> We can use Views in order to introduce a second version of our data-model in another language. So, We could offere multiple languages to users.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/fb4fbbec-2db5-48d5-8481-c1c81b8d729f" />

Let's understand this through a scenario, We have a table orders data is persisted & everything is in English.
Sometimes we have international team who are accessing your data. Like you have a team in Germany or in India who are end users who want to access your data.
Of course, It depends on the number of users that are using your database. But if you have a lot of users  that comes from India as well as from Germany.
It might make sense that you translate your data and table structure into another language.

For example :
- Instead of giving access to table orders, we can create another view called आदेश  That's in Hindi.
- Same thing we can do for German Users.

As we can see, we are using views in order to provide a translation for our database by just giving a new name for the views as well as for columns.


### **Use case 6 : Virtual Data Marts**

> Views can be used as a Data Marts in Data Warehouse System, because they provide a flexible & efficient way to present data.

We can use views as a virtual Data Mart in a data warehouse. why is this favourite for every data engineer, bcuz this topic is very important decision in each project.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/13a5750a-85c4-4d13-8569-5e10daca2786" />


A classical Data Warehouse architecture is based on the approach of M1 look like Source System - ERP System | CRM System | LOGS
We have multiple source systems where our data are spreaded and would like to extract all our data from these multiple sources and put it in one big database called **Data WareHouse**. There will be a lot of operations on this central database(Data Warehouse) like 1st **Data Cleansing** and then may be **Data Integration**  together and may be building some historical data **Data Loading**. So basically we're doing multiple steps in order to prepare data for complex reporting and analysis.

What we usually do at Data Warehouse is we store all those information as a physical table.
Once We built the Data Warehouse, we will have multiple use cases that would like to access the data warehouse in order to do different reporting.
It is going to be very complex if we connect immediately a reporting engine (PowerBI) directly to the data warehouse.
But instead of this we try to split data warehouse into multiple subsets like Topic or Domain or Departments and we call those subset as **Data Marts**

> A Data Mart is always specific for a usecase that focus on one topic. e.g.
> - a dedicated Data Mart for Sales
> - another Data Mart which is dedicated only for finance topic
> Both of them comes from our Data Warehouse.

The last layer is Reporting & Dashboarding where we have something tools like PowerBI to create a dashboard for one data mart sales and few stuffs from other data marts.

Question is How should we store the data in Data Mart? <br/>
Should we store the data in tables or in views.
The best practice says if you're building Data Marts then use Views and we call this **Virtual Data Marts**

There are many reasons why using views at a Data Mart is way better than using tables like views are more dynamic & quicker to change them bcuz usually at Data mart we're building a lot of business logic which requires flexibility  speed and maintainance effort is very simplified no need to build any ETLs or Data Loads from the Data warehouse to Data marts that makes the Data Warehouse as a real single point of truth for your data.
Once you start copying the data from one layer to another layer, It is going to really hard to maintain, It becomes chaotic anf you have to have really restrict monitoring and data quality.
That's why using views is always going to reflect the status of the data warehouse. This will help in Data Consistency which is a critical point in each Data Warehouse project.

As we can see how views are playing a very important role in building a Data-Warehouse.
</details>

<details>
  <summary> <b>View vs CTE</b> </summary>

**CTE**
- CTE is used to **reduce redundancy within 1 single query**
- Improve **Resuability in 1 single query**
- **Temporary Logic** Here, logic is not persisted, it is temporary and calculated only on the fly within the scope of 1 single query. So, this logic is important only in the single query. So, It doesn't make sense to persist this logic using the view.
- **No Maintainance**, Database do **auto clean up** once the query is done. So no extra effort to drop a CTE

**View**
- View is used to **reduce redundancies from multiple queries**
- Improve **Resuability in multiple-queries**
- **Persisted Logic** : We use Views in order to persist a logic in the database. So, the logic is so important that we want to persist it in the database.
- Creating Views always need extra steps, so **Need to Maintain**, CREATE/DROP views

|                CTE                  |                View                   |
|-------------------------------------|---------------------------------------|
| Reduce Redundancy in single-query   | Reduce Redundancy in multiple-queries |
| Improve Resuability in single-query | Improve Resuability in single-query   |
| No Persisted Logic, only Temporary Logic, on the fly | Persisted Logic      |
| No Maintainance, Auto Clean-up      | Need to Maintain, CREATE/DROP Views   |

</details>

<details>
  <summary> <b> SQL Views</b> <code>CREATE</code>, UPDATE<code>REPLACE</code> <code>DROP</code> </summary>

**Creating Views** using ```CREATE```

We create views using the DDL Command ```CREATE VIEW view_name AS (SELECT * FROM table1 WHERE condition)```

> Note : If a table or view is created without specifying a **Schema**, it defaults to the **DBO**
- In order to put our view in correct schema instead of default schema. we have to specify the Schema Name. <br/>
```CREATE VIEW schema_name.view_name AS (SELECT * FROM table1 WHERE condition)```

**Syntax of View** :
```python
   CREATE VIEW view_name AS
   (
       #Query 
       SELECT
            col1,
            col2,.....
       FROM table1
       WHERE condition
   )
```

```python
# Find the running total of sales for each month.
# We can solve any task in SQL in different ways.
# Here, Let's do it by using View as well as with CTE and then see the difference


# Solving through CTE
WITH CTE_Monthly_Summary AS
(
     SELECT
          DATETRUNC(month, order_date) AS orderMonth,
          SUM(sales) AS totalSales,
          COUNT(order_id) AS totalOrders,
          SUM(quantity) AS totalQuantity
     FROM Sales.orders
     GROUP BY DATETRUNC(month, order_date)
)
# Using CTE in main-query
 SELECT
      orderMonth,
      totalSales,
      SUM(totalSales) OVER(ORDER BY orderMonth) AS RunningTotal
 FROM CTE_Monthly_Summary
  


# Solving through VIEW
CREATE VIEW V_Monthly_Summary AS
(
     SELECT
          DATETRUNC(month, order_date) AS orderMonth,
          SUM(sales) AS totalSales,
          COUNT(order_id) AS totalOrders,
          SUM(quantity) AS totalQuantity
    FROM Sales.orders
    GROUP BY DATETRUNC(month, order_date)
)
# This will create a View named V_Monthly_Summary inside database
# Now we can query the view

SELECT *
FROM V_Monthly_Summary

```
| Order_month | totalSales | totalOrders | totalQuantity |
|-------------|------------|-------------|---------------|
| 2025-01-01  | 105        |  4          |  6            |
| 2025-02-01  | 195        |  4          |  8            |
| 2025-03-01  | 80         |  2          |  2            |

```python
# Now we can query directly from the View
# No need to write CTE Query

SELECT
    Order_month,
    totalSales,
    SUM(totalSales) OVER(ORDER BY Order_month) AS runningTotal
FROM V_Monthly_Summary
```
| Order_month | totalSales | runningTotal |
|-------------|------------|--------------|
| 2025-01-01  | 105        |  105         |
| 2025-02-01  | 195        |  300         |
| 2025-03-01  | 80         |  380         |

Note : If a table or view is created without specifying a **Schema**, it defaults to the **DBO**. <br/>
So, In order to put our view in correct schema, bcuz we don't want our view to be default schema, we want our view in Sales Schema.
So, Let's specify the Schema name while creating the view.

```python
CREATE VIEW Sales.V_Monthly_Summary AS
(
     SELECT
          DATETRUNC(month, order_date) AS orderMonth,
          SUM(sales) AS totalSales,
          COUNT(order_id) AS totalOrders,
          SUM(quantity) AS totalQuantity
    FROM Sales.orders
    GROUP BY DATETRUNC(month, order_date)
)

SELECT
    Order_month,
    totalSales,
    SUM(totalSales) OVER(ORDER BY Order_month) AS runningTotal
FROM Sales.V_Monthly_Summary
```

**Droping Views** using ```DROP```

If we don't need the view you created in your database, basically you want to clean up your view. 
We can do this by using the command ```DROP```

```DROP VIEW view_name``` in case of default schema

```DROP VIEW schema_name.view_name``` in specified schema

```python
DROP VIEW V_Monthly_Summary


DROP VIEW Sales.V_Monthly_Summary
```

**Updating Views** using ```REPLACE``` or ```DROP```+```CREATE```

If we want to change the logic of our view.
How can we do that?
We have 2 ways :
 - We can do this using ```REPLACE```
```sql
CREATE OR REPLACE VIEW view_name AS (SELECT * FROM table1 WHERE condition)
```
 - First ```DROP``` the view and then ```CREATE``` view
```sql
DROP VIEW view_name;
CREATE OR REPLACE VIEW view_name AS (SELECT * FROM table1 WHERE condition)
```

```python

CREATE OR REPLACE VIEW V_Monthly_Summary AS
(
     SELECT
          DATETRUNC(month, order_date) AS orderMonth,
          SUM(sales) AS totalSales
    FROM Sales.orders
    GROUP BY DATETRUNC(month, order_date)
)

# OR

DROP VIEW V_Monthly_Summary;

CREATE OR REPLACE VIEW V_Monthly_Summary AS
(
     SELECT
          DATETRUNC(month, order_date) AS orderMonth,
          SUM(sales) AS totalSales
    FROM Sales.orders
    GROUP BY DATETRUNC(month, order_date)
)

```

**T-SQL** <br/>
If you want everthing in one command in SQL Server. Then use T-SQL, which is Transact-SQL. <br/>
Transact-SQL is an extension of SQL that adds programming features like variables or checks(if)

```python
   IF OBJECT_ID ('Sales.V_Monthly_Summary', 'V') IS NOT NULL //if this object exists
         DROP VIEW Sales.V_Monthly_Summary;
   GO
   CREATE  VIEW V_Monthly_Summary AS
    (
         SELECT
              DATETRUNC(month, order_date) AS orderMonth,
              SUM(sales) AS totalSales
         FROM Sales.orders
         GROUP BY DATETRUNC(month, order_date)
   )

```

</details>

<details>
  <summary> <b>How database execute Views? </b> </summary>

Let's say a Data Engineer creating a view called TopN.

So, the query is going to sent to the Database Engine. Once DB Engine identify it as a View. Then DB Engine will go to Disk storage > System Catalog
Catalog stores not only the metadata about the view but also the SQL Query that is responsible for view.

So, DB Engine will take Query that is defined in create View ans place it as well in the System catalog.
> So if your compare table, in System catalog memoey only table metadata is there, but for View its metadata + responsible create view query is also inside the Catalog.

Here, Database Engine will not create table in Users Data memory of Disk Storage.
So, Physical data will be not stored anywhere, We're storing only metadata & query of View inside System Catalog memory. 

Now, SQL will tell Data Analyst, okay we have a new View and now Data Analyst can write a query in order to retrieve the data from the view.
The DB Engine will take it and understand it that we're taking about view. So DB Engine first has to retrieve not the data but the query of View from System Catalog in order to understand what do we have to execute now. Then DB Engine will execute the query of the view first and then data for this query cames from a physical table called orders. Now DB Engine will querying the order to retrieve the data so that we have data for End User. Then It will execute the query and result will be send back to Data Analyst.

As we can see, there are 2 queries. SQL Engine first execute the query from the view and only after that it will execute the next query which comes from the user.
So, Actually the data always comes from the physical table but we're not providing the Data Analyst an access to the table, we are just providing the access of View.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/64fed585-0be5-4874-a3f8-9f835c5d58b7" />

Now if Data Engineer wants to drop the view. So, he write a query in order to drop the view. The Database Engine will go to the System Catalog Memory delete both metadata & the query of the view. So, Here there will no user data lost at all. only Query and metadata of the view will be deleted.
So, dropping view is not bad like dropping a table which cause lost of user data.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/7b920165-c666-4e5f-8cc0-dd926a5cc7b4" />

</details>

<details>
  <summary> <b>View Summary</b> </summary>

**Views**
- View is virtual table that is based on the result of a query, without actually storing any data inside database.
- We use views in order to persist a complex SQL Query logic in the database.
- Views are better than CTEs in some scenario bcuz It improves resuability in multiple queries
- Views are better than Tables in some scenario bcuz Views are flexible & easy to maintain
**UseCases of Views**
  - Store Central Logic in Complex Query to be resused
  - Hide complexity by offering friendly views to users
  - Data Security by hiding sensitive rows & columns
  - Flexibility & Dynamic
  - Offer your object in Multiple Language
  - Virtual Layer (Data Mart) in Data Warehouse System.

</details>

Next we will learn about How to create table based on query? using Temporary table.

<!-------------------View--------------------->
## 9.4 Temp Tables - temporary Tables

<details>
  <summary> <b>CTAS & TEMP </b> </summary>

</details>

<!-------------------Temp Tables--------------------->
## 9.5 CTAS - Create Tables AS SELECT

<details>
  <summary> <b> </b> </summary>

</details>

<!-----------------------CTAS----------------------->

<!-----------Chapter 9. Advanced SQL Techniques--------------->

[< Aggregation & Analytical Function](https://github.com/pawansinghfromindia/SQL/blob/main/08_Aggregation_and_Analytical__Functions/aggregation_and_analytical_functions.md) |
Advanced SQL Techniques | [Performance Optimization >]()
