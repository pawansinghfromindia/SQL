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

**Sub-Query in ```FROM``` Clause**
- We typically use Sub-Queries in the ```FROM``` clause in order to create temporary result sets. </br>
That acts as a table for the Main-Query.

- In some scenario, we can't use the table directly from the database. So, We have to prepare it somehow before our actual query.

</details>

<!--------------subqueries------------------->
## 9.2 CTE - Common Table Expression

<details>
  <summary> <b> </b> </summary>

</details>

<!-------------------CTE--------------------->
## 9.3 View - Views in Database

<details>
  <summary> <b> </b> </summary>

</details>

<!-------------------View--------------------->
## 9.4 Temp Tables - temporary Tables

<details>
  <summary> <b> </b> </summary>

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
