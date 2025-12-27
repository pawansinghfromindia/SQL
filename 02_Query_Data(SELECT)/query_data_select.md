<!-------Chapter 2. Query Data (SELECT)------------------>
# Chapter 2. Query Data (SELECT)

Here in this chapter, we will see - How to query our data?

Query Data (SELECT)
  - ```SELECT```
  - ```FROM```
  - ```WHERE```
  - ```ORDER BY```
  - ```GROUP BY```
  - ```HAVING```
  - ```DISTINCT```
  - ```TOP```
  - ```JOIN```
  - Query Order and Execution

<!--------------------------------->

## 2.1 Query Data (SELECT)

Understanding What is an SQL Query? 

Normally data is inside the table, and table is inside the database.

Now you have questions from the business like -
  - What is the total sales?
  - What is the total number of customers?

So, We ask for data, data is in the database and we have to retrieve data from the database, in order to do that we have to talk to the database using the language
that database understands that is SQL.

We will write query something called ```SELECT``` statement, with that we're asking our database for data. 
Once you execute SQL Query, the database is going process your query and fetch your data and then it prepares a result to send back to you.

So Here, we are like reading our data from the database.
The queries will not modify anything, no data is changed inside table or no structure is changes of the database.

> Use ```SELECT``` statement only in order to read something from the database.
You will retrieve data from the database.

This is what we mean with a Query!

<!--------------------------------->

## 2.2 SQL Query Clauses

Each SQL Query has usually different sections, different components. We call the **Clauses**.

Clauses are amazing tools to write a query that matches any question that you have about your data.

Let's start with two clauses that makes the simplest query in SQL : ```SELECT``` and ```FROM```.

- **```SELECT *```** : Retrieve all columns(Everything) from your table. We can also specify which column to select.
- **```FROM```** : Tells SQL where to find your data. So, specify your table.

**SELECTE *(ALL) data from table***

Syntax :
```sql
   SELECT * FROM Table_Name;
```
Once you execute the SQL Command, what's going to happen is First SQL execute ```FROM``` clause, then ```SELECT``` which columns to keep in results.

Important Note : Make sure you're connected to the correct database, otherwise your queries won't work. We also have a command for that is :

```sql
   USE database_Name;
   SELECT * FROM table_Name;
```

<!--------------------------------------->
**COMMENTS** :
  - Comments are like notes to explain code, and ignored by SQL Engine during the execution.
  - There are two ways to comment

  1. Inline Comments
  ```sql
        -- This is inline comments
  ```

  2. Multiple line Comments
  ```sql
            /* This
                 is a
                   multi-line comments.
            */
   ```
<!----------------------->

SQL Task : Retrieve All Customer Data (All Rows & Columns)
```sql
      SELECT *
      FROM table_name;
```

**SELECTE Few COlumns from table*** : Choose Columns

If we don't want to see all the columns, we want to see only specific column or columns.
```sql SELECT Column1, Column2 FROM table_name```

Syntax
```sql
     SELECT
           name,
           rollNumber,
           score
     FROM Students;
```
The result will be only those columns in which are mentioned in exactly same order.

Note : No comma (,) after the last column otherwise syntax error.

<!------------------------------------->

## ```WHERE``` Clause in order to filter the data













<!-------Chapter 2. Query Data (SELECT)------------------>
