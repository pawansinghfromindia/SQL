<!-------Chapter1. Introduction to SQL------------------->
# Chapter 1. Introduction to SQL


- What is SQL?
- Why Learn SQL?
- What is Database & Types
- SQL Commands
- SQL Components
- Setup Environment

## Data

Everything that generate data and Data is everywhere. <br/>
Your first Name, Mobile & everything inside it, Car generating data, Bank Finance Statements etc.

- Where do we store our data? <br/>
Personally we store our data in like Excels, spreadsheets, text files, etc.
So we store our data in different files. <br/>
Companies have a lot of things that generates a lot of data. Products they produces, customers, sales informations etc.
So, companies generate massive amount of data.

- How companies store and handle the data? <br/>
Of course, companies can't use simple file, They need something bigger, stronger and smarter.
Here, **Database** comes in the picture.

<!------------------------------------------------------------>

## Database 

It's like a container for storing the data. But instead of just dumping files into folders. 
The database organizes the data so it is easy to access, to manage and to search.

> Database is a container that stores data.

- Why we are using database? Can't we just use files like what we do personally. <br/>

Imagine someone asked that question, Go & find the total spending in your data.
So in order to find the total spending and costs, you will be opening each of those files one by one, searching for the cost, trying to combine the data.
It is very long and messy process.

On the other side if your data is in database and you want to ask question, It's going to be very easy.
All what you have to do is to talk to the database to ask a question and database will answer your question with a result.

<!------------------------------------------------------------>

## Why Learn SQL?

- How do we talk to a database? <br/>
We use **SQL**. ```SQL = Structured Query Language``` <br/>
SQL is the language that we use in order to talk to the database. <br/>
Some people call it **SeQueL** <br/>
Another reason why we use database is that Database can handle huge amount of data like in millions which can't be handle in spreadsheets. <br/>
Another reason is Database is just Secured, Saviour to store important and critical data and you can control who is accessing what. So It is just
more professional to store the data inside a database. <br/>

Most of the companies store their data in container called Database. In order to ask question and talk to database we have to speak the language of SQL.

<!------------------------------------------------------------>

## 1.1 What is DBMS & SQL Server?

In companies, We have our data inside database, then there are multiple people with multiple roles that they're writing different SQLs in order to 
talk to database to get data. But not only employees and people interact with the database. <br/>

We could build a website and Application that as well interact with the database by sending different SQLs.
Depends on how many people are interaction with application and website, It might really massive amount of SQL that sent to the database. <br/>

Not only that we might have tools in order to do Data Visualizations like Dashboard, Reports etc created using PowerBI or Tableau used by stakeholders and
managers to make decisions. Those tools are also connected to the database and creating SQLs. <br/>

We can say, we have a lot of interactions with the database from Employees, Applications, Websites, Tools etc.
All of them are generating SQLs in order to interact with database. <br/>

But database is just a container/storage. So, We need something a software that manage all those requests that's why we have something called **DBMS**
```DBMS = Database Management System```. It is a software that is going to manage all those different requests to database and give priority which SQL must be 
executed first.

> **DBMS is the software that manages the database.**

Now we have **Data**, we have **Software (DBMS)** but what is missing here is the Hardware. <br/>
So, in real world companies, we can't run that on PC bcuz our PC is not capable enough and also it goes offline in case of powercut.
That's why we need a **Server**.

Server is like a very powerful PC and lives 24x7, always available. <br/>

We can decide whether we can have a Server inside a company or we can use Cloud Services in order to run our database.

- Summary 
   - Database : IT is container to store the data.
   - SQL : It is the language in order to talk to the database.
   - DBMS : It is the manager(Software), manages the database.
   - Server : It is a physical machine(Hardware) where the database lives.

<!------------------------------------------------------------>

## 1.2 Database Types

There are different types of databases

1. Relational Database
  - It is very simple like spreadsheets called **Table** where we have **Columns** and **Rows**
  - There is like a realtionship between those tables to describe how they related to each other that's why we call it Relational Database.
  - Generally when you say people think about Relational Database.

2. Key-Value Database
   - In this type of database, data is organized completely different where we have pairs of **Keys** and **Values**.
   - It is like a big dictionary where you have  words like keys and definitions of words like Values.

3. Column-Based Database
   - Instead of data by rows, this type of databases group the data into **Columns** that's why called as Column-Based.
   - This is very advanced database in order to handle huge amount of data where the main purpose is to **Search for Data**. 

4. Graph Database
  - The main focus in Graph database is **relationship between Objects**.
  - The main idea here is **How to connect data points**.

5. Document Database
  - The data is stored as entire documents where the structure of the data is not that important.
  - here, what is more important is to fit everything in one page in one document.

| Database Types | Category | Example |
|-----------------------|-------------|------------------------------|
| 1. Relational Database   |   SQL    | Mircosoft SQL Server, MySQL, PostgresSQL |
| 2. Key-Value Database    |   NoSQL  | Redis, Amazon Dynamo DB |
| 3. Column-Based Database |   NoSQL  | Apache Cassandra, Amazon Redshift |
| 4. Graph Database        |   NoSQL  | Neo4j |
| 5. Document Database     |   NoSQL  | MongoDB |

<br/>

Relational Databases
  - Microsoft SQL sever
  - MySQL
  - PostgresSQL

Key-Value Databases
  - Redis
  - Amazon Dynamo DB

Column-based Databases
  - Apache Cassandra
  - Amazon Redshift

Graph Databases
  - Neo4j

Document Databases
  - MongoDB

    
Here, We're going to focus on **Relational Database** which is most famous and most used in companies.

<!------------------------------------------------------------>

## 1.3 Database Structure

Databases are very structured and organized. <br/>
It has following Hierarchy :
 - Starting point is **Server** (It is powerful PC where database lives)
 - Inside Server Multiple databases like Database for Sales, another database for HR. So Server can host multiple databases.
 - **Database** is a Container for data.
 - In each database we have multiple Schemas.
 - **Schema**, It is like a category or we can call it a logical container that we can use it in order to group up related objects.
   like we have 100s of tables, So we can splits all the tables that has to do with orders in one schema and then another group of tables where schema customers and so on.
 - So, Schema helps us to organise your tables and objects in the database.
 - Inside each **Schema**, we can have multiple objects like Tables.
 - What is a **Table**? : It is like a spreadsheet. It organize your data into Columns.
 - **Columns** : The Columns define the data that you store inside it.
   e.g. One column about Customer_ID, another Customer_Name, Score, Birthday etc.
 - Each Column is about one type of data.
 - Sometime we call the **Columns** as **Fields**
 - Other things that we hace in tables is Rows.
 - **Rows** : It is where actually the data is stored.
 - Sometime we call the **Rows** as **Records**
 - Each record represent one Customer or one Person. Like one record for John, one is for Peter, one is for Maria

Server > Database > Schema > Table > Columns and Rows

| ID  | Name | Score | Birthdate |
|-----|----|--------|----------|
| 1 | John | 450 | '01-01-2005' |
| 2 | Maris | 480 | '01-01-2008' |
| 3 | Peter | 350 | '01-01-2010' |

In each table there is one very important columns called **Primary Key**
It is always very important to have like one unique identifier for each Customer for each row.
We use it for different purposes in order to combine it with another table in other to identify quickly one customer.
So Primary key is unique and It's like fingerprint, There is no two customer having the same id.

The overlapping between the Columns and Rows, We have a single value is a **cell**. Each value stores a specific **datatype**

**Datatype** : It is like what kind of data we are storing.

like an Integer (1,2,3,...10) or a Decimal (1.25, 3.14)

1. Numeric Datatype
   - **INT** : 1, 2, 3, 100
   - **DECIMAL** : 3.14, 100.50

If you want to store Characters we have different datatypes for that like Name or Description 'John', 'E5A1' etc

2. Text / String Datatype
   - **CHAR** : CHAR is always the fixed one. If you define 5 char, It will reserve 5 characters from the space.
   - **VARCHAR** : But if you want things more dynamic they we use VARCHAR

Another datatype is date and Time. If you want to store a date like Birthdate or Time information

3. Date and Time Datatype
   - **DATE** : store a date like Birthdate
   - **TIME** : store a time information

So, INT, DECIMAL, CHAR, VARCHAR, DATE, TIME. They are datatypes.

<!------------------------------------------------------------>

## 1.4 Types of SQL COMMANDS

In SQL, We have different types of commands. <br/>

Let's say we have a database, It is empty. So we have nothing inside database. <br/>

Of course, the first thing that we have to do is to write an SQL Command ```CREATE``` in order to create brand New Table in the Database. <br/>

Once you execute your create command, database is going to create New Table but this table is empty. So, we have nothing inside it. <br/>

So, here we have done here is defined something new and we called this type of commands **Data Definition Langugage (DDL)** like ```CREATE``` Something new or ```ALTER``` in order to edit something already exists and ```DROP``` in order to delete something e.g. drop table. <br/>

So this is the first family of SQL Commands. <br/>

If we look at our Table, It is empty. What do we need? We need data. <br/>

Let's say we have an application. This application generating a lot of data. Now in order for this application to move the data inside our new table. 
It must use the SQL Command ```INSERT```. If you execute INSERT, you add new data inside the table. <br/>

This type of commands we call it **Data Manipulation Language (DML)**. 

```INSERT``` in order to insert new data, ```UPDATE``` in order to update an already existing data, ```DELETE``` in order to go and delete data from your table that's why we call it Data Manipulation Language bcuz we're manipulating our data. <br/>

Now We have Server, We have Database, We have Table, We have Data inside the table. Now we can start asking questions. <br/>

Let's say you have analytical questions about your data. <br/>

All you have to do is write something called SQL Query, inside it you use the ```SELECT``` command.

So we send a query(Question) to the database and database can return for you the result, the data answering your query. <br/>

We call this types of activities using SQL is **Data Query Language**, We have only one that is very famous ```SELECT```, We use it in order to query your data.

|          SQL                  |                  COMMANDS                  |
|-------------------------------|--------------------------------------------|
| 1. Data Definition Language   |  ```CREATE```, ```ALTER```, ```DROP```     |
| 1. Data Manipulation Language | ```INSERT```, ```UPDATE```, ```DELETE```   |
| 1. Data Query Language        | ```SELECT```                               |

Those are the 3 different commands in SQL, We will learn How to write correct query for getting the result from the database.

<!------------------------------------------------------------>

## 1.5 WHY to learn SQL?

The top 3 reasons for learning SQL, why it is relevent.

1. In order to talk to the data, SQL is the language.
   - Most of the companies store the their data in databases and this is the standard way. If you works under the company in data field and you want to talk to the data then you have to use SQL. This is the most important reason to learn SQL.

2. High Demand of SQL
   - SQL is in high demand, Job Description of Software Development, Data Analyst, Data Engineer, Data Scientist.
   - SQL skills in almost each job description of Data related job.
  
3. SQL is Industry Standard
   - If you go to multiple modern data platforms and tools like PowerBI, Tableau, Kafka, Spark, Synaps etc there you need SQL code.
   - Most of those tools vendors adapt SQL bcuz It's standard and widely used across the tools. It's like selling points that their tools are easy to use. 

<!------------------------------------------------------------>

## 1.6 Setup SQL Environment

Here, We prepare our PC with data, databases and all the tools that we need in order to learn SQL

Setup SQL Environment

  - Download & Install SQL Server
  - Datasets and Create Databases
  - GIT repo 

Install SQL Server

Download SQL Server Express Edition free > Basic > Accept > Install, take little bit time 
Download SSMS SQL Server Management Studio Free > keep this as Default, Install 

Now from PC Searchbar you can open SSMS
Server type : Database Engine keep as it is.
Server name will be your PC and SQLExpress.
Authentication - How to access the database, list it as Windows Authentication
Username: yourpcname
Password:
If you're having issue connecting with database, make sure to check the encryption, It should be mandatory 
Encryption : Mandatory, Checkbox Trust server certificate.
Once you do that, you will be able to connect.

Now we have server and we have clients.
Next and last step is Go and create a database.
If you look to Object Explorer, and open the Databases, It is empty as we don't have any database.
Go to the datasets of respective database you're using like MySQL or Postgres, find the exact same data for the database that you're using.
So, Here we are using SQL Server, So we take the dataset of sql-server folder.
You will find the different files in my-sql folder of dataset, having different extensions .sql

In SQL Server, there are multiple ways for How to create databases :

Option 1. Create Database using SQL script
  - In the init.sql file, we have script for Database Creation and Table Setup
  - Script Purpose:
    This script creates a new SQL Server database named 'MyDatabase'. 
    If the database already exists, it is dropped to ensure a clean setup. 
    The script then creates three tables: 'customers', 'orders', and 'employees' 
    with their respective schemas, and populates them with sample data.
    
  - WARNING:
    Running this script will drop the entire 'MyDatabase' database if it exists, 
    permanently deleting all data within it. Proceed with caution and ensure you 
    have proper backups before executing this script.

  - Copy the code from init.sql file and go pack to SQL Studio and then go to menu, click on new query and paste the copied code.
    Now we have the code for our first database. Now execute it. Once it is executed, refresh it, you will see your MyDatabase inside that all the tables will be stored.

Now, We have Object Explorer, SQL Editor to run query and see results in our SQL Studio.

Similar ways you can create you different databases on your local PC(server).

If you want to practice another database. In microsoft there are a lot of databases, one of them is AdventureWorks. It is really amazing.
Let's see how to import it.

Import the database : 
   - Go to adventureworks on microsoft page.
   - You will see 3 different types of databases, OLTP, Data Warehouse and Lightweight.
   - **OLTP** : Most complicated on Online Transaction Processing where a lot of tables and transactions etc etc
   - **Data Warehouse** : It is like really nice one in order to data analysis and stuffs.
   - **Lightweight** : It is the simplest one.

Let's go and get Data Warehouse, click on that, you will see adventureworks.bak file, .bak is Backup File contains Database Structure & Data.
Go to your path where you have installed SQL Server
Go to C:\Programs\MSSS\SQLExpress\MSSQL\Backup\ Place adventuresworks.bak file, you will see all the file with extensions .bak

Option 2. Create database by restoring it using .bak file
If due to some reason if script didn't work for you.
   - Go back to SQL Studio > Right Click on Database > Restore Database > Source
   - In source you will have 2 option one is database (default) and another is device.
   - Switch to device bcuz we want to import it from file > three dot eye > Add > select AdventureWorks.bak > ok > ok > ok > Restored successfully.
   - Now you can see you new database AdventureWorks

We have SQL Server Express running on our local PC <br/>
We have also SQL Studio clients where we're going to use it in order to interact with the database. <br/>
We have also created 2 databases and imported one database from Microsoft website. </br>

<!------------------------------------------------------------>

<!-------Chapter1. Introduction to SQL------------------->

[<- Content](https://github.com/pawansinghfromindia/SQL/blob/main/00_SQLRoadMap/Course_Structure.md) [Query Data ->]()














