<!-----------Chapter 10. Performance Optimization---------->
# Chapter 10. Performance Optimization
- Indexes
- Partitions
- Performance Tips
<!-----------Chapter 10. Performance Optimization---------->

This chapter is about performance, as we start writing complex and multiple queries on multiple tables, views in the database.
One thing to notice is the performance  of the query are really slow.

Here, in this chapter we will learn different techniques on how to increase the performence like twice by using
techniques Indexes and partitions.

## 10.1 Indexes

<details>
  <summary> <b>What is an Index ? </b> </summary>

> **An index is a data structure that provides a quick access to the row (data), to improve(optimizing) the speed of the queries.**

Index is like a guide for our database in order to speed up the process of searching for data especially if we have like big tables..

In order to understand indexes, imagine we have a book and we want to find a specific Topic inside a chapter.
So, instead of flipping every single page to search for the topic. We can use the index of the Book chapters in the initial page 
in order to jump straight to the right page. <br/>
That's exactly Index does for our data in database.

Another analogy to understand indexes is think about the indexes as a big hotel.
In hotel if we don't have any guide and we have to find the room number 101.
Now there you have to find the room floor by floor and checking each room until we find our room.
But instead of that, Thankfully hotels have a numbering system and we can ask for a map from a reception in order to understand 
in which building, on which floor, we can find our room.
There we have maps & signs to very quickly to locate & find the room in such big hotel. <br/>
That's exactly what each database needs. It needs an index in order to help the database in finding & locating the right data without
having to scan everything.

Let's say we have a big table. <br/>
Now we would like to speed up queries using indexes. <br/>
Question :
- What're we exactly going to do with the table?
- Are we using the table to search for text ?
- or Are we doing complex analysis with tables ?

Reason to ask these question is that We have different indexes in database for different purposes.
</details>

<details>
  <summary> <b>Indexes Types</b> </summary>

We divides the **Indexes** in database in 3 categories :
1. **Structure** : How the database is organizing and referencing the data? They're of 2 types :
  - **Clustered Index**
  - **Non-Clustered Index**

2. **Storage** : How the data is stored physically in the database?. They're of 2 types :
  - **RowStore Index**
  - **ColumnStore Index**

3. **Functions** : How the database function? They're of 2 types :
   - **Unique Index** 
   - **Filtered Index** 

<img width="350" height="150" alt="image" src="https://github.com/user-attachments/assets/59652c38-50ac-4b1f-8f58-371b62205ba5" />

Each index type has its own strings but it's always a trade off.
Some might improve their read(Select) performance other one might improve writing(Insert, update) performance.
So, It's all about choosing the right type of index for the job.

> Trade-off
> - Some Indexes are better for reading.
> - Others for writing performance.
> - Choose the right one for the job.

Let's deep dive into each of the type in order to understand :
How they work? and
How we can create them?

</details>

<details>
  <summary> <b> Heap Structure</b> : Page, Data Page(Header,Rows Spaces,Offset array), Full table Scan </summary>

Before we dive into How indexes work in database? <br/>

Let's understand first, What happens to database tables if we don't use any index?
- We create a new table in database. e.g. Customers table where we have 20 customers inside this table.
- What we see at client side is like spreadsheet (a table, rows & columns) but behind the scene the database stores it bit differently.
- Database stores the data in a data file on the disk and inside this file the data can be stored inside the blocks called **pages**.
- **Pages** are not like **rows & columns** 

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/1c5ded8c-9be2-4349-95e9-d1ccf6ce5536" />

**What is a Page ?**
> A page is the smallest unit of data storage in a database (8kb)
> - It is a fixed size of 8 kilobytes where SQL database can store anything inside it.
> - It stores anything (Data like table:rows & columns, Metadata, Indexes etc)
> - So, Everytime we interact with data, the SQL is reading and writing on those pages.

As we can see, SQL is not storing the data inside database like table : rows & columns. 
So, if we're running a query then SQL not like selecting a specific column. SQL always fetch a data page in order to read the rows 
inside the this page.

There are mainly 2 types of Pages :
1. **Data Page**
2. **Index Page**

How the **Data Page** looks like? </br?
It is divided into multiple sections.

**Header**
- 1st section is Page **Header**, where database stores key information about the metadata like Page ID.
- It has following format :
  - It starts with the File ID and then have a unique number for each page. like File Id is 1 and Page Number is 150```1 : 150```
- The page header is a fixed size of 96 bytes.

| Header   File ID : Page Number  |
|---------------------------------|
| Header         1 : 150          |

**Rows Section**
- This section have a variable size, this is where our data is stored.
- The actual data is going to be stored in this section.
- SQL try to fits as many rows as it can in one single page.
- Of course this depends on size of each row. So if we have a large table in which rows are really big then
  SQL can fit only few rows in one single page.

| Rows  | Columns Data  |
|-------|---------------|
| Row 1 | 1, David, USA |
| Row 2 | 1, Mike, UK   |
| Row 3 | 1, John,  CA  |
| ..... | ............. |
| Free  | Spaces        |
| Free  | Spaces        |

**Offset Array**
- The section of Data page is  Offset Array. This is like a quick index for the rows stored inside this page
- It keeps track of where each row begins so that the SQL can easily locate a specific rows
  without having scanning the entire page in order to find a row.

| Offset [address of rows] |
|--------------------------|
| 1, 2, 3 <br/> [row1, row2, row3]|

This is the Structure of a Data Page. This is exactly how SQL stores the data inside the databases.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/c3ef9f81-6bca-481d-852d-ae3c1be19765" />

Let's get back to example of our table Customers where we have 20 rows.
Let's see How SQL will creates those pages for the table customers.

- So, If we're not using any index on the table Customers what is going to happen is : SQL is going to insert the data inside those pages
as we're inserting the data inside the customers.

- like may be we are inserting the customers like 12, 5, 6, 7 and SQL is going to insert it to the Data pages exactly like that only.
So, SQL is just inserting the data as we insert it to the table.

- So Let's say each data page is fitting only five rows. So after inserting 5 customers, 
SQL will create another data page for that next rows. Once it again load 5 customers, It will create another data page and so on
until covered all the rows that are being inserted.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/849dc3f0-a2ee-4065-baaf-9140ffddb11e" />

- Now if we check customers inside those 4 pages. We can see that they're not sorted at all. That's bcuz in this scenario we're not
  using any index.
- We call this Structure as **Heap Structure**.

#### **Heap Structure**

> Heap table = Table without Clustered Index
> - that means the Rows are stored randomly without any particular order.
> - This is not very bad, bcuz It's going to be very quick to insert the data inside the table.
> - But of course the finding something from this table is going be slow.

This is the first trade-off. If you have a very fast writes but very slow reads.
It is like We're throwing all our papers in a drawer without organizing them. So, we can put a paper very quickly but if we search for
a specific paper later, It is going to very long time taking process until we find it. Bcuz things are no it order.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/60ce4214-f8c9-4f51-84fa-41ada666083e" />

Let's see How SQL handle this Heap Table while we read something from it? <br/>
-  We're searching for the Customer with the id = 14.
-  So, now SQL has no idea where to find this customer whose id = 14.
-  So SQL start fetching each data page and start scanning each rows.
-  SQL will start with 1st Data Page and start scanning the rows in it. If not find move to next data page i.e. data page 2 there again start scanning all the rows inside it, here also SQL doesn't find it Now It will move to the next data page i.e data page 3 here gain it start scanning all the rows and it doesn't find here and it will move to the next data page 4 there it will scan all the rows and at the row 4 of it SQL found the customer whose id = 14.

<img width="300" height="200" alt="image" src="https://github.com/user-attachments/assets/82f5748b-270e-4d57-baa0-0dfaa9b30f76" />


Finally SQL will find and return it to the client, if it doesn't find it will return no result to the client.

Here, As we can see in order to find one customer SQL did read 4 different pages & scanned around 15 rows in order to find the specific customer. This process we call it **Full table Scan**

**Full Table Scan**
> **Full Table Scan Means SQL scans the entire table page by page and row by row in order to find specific row (searching for data).

Here, in this customer table we have only 4 data pages & each datapage has 6 rows. There is no big deal for SQL. But If there is a big table containing 1000s of rows searching through Heap Structure will be very painful & slow in order to read one row.
That's exactly why we need Indexes in database. 

</details>

<details>
  <summary> <b>Clustered Index </b> </summary>
  
Let's understand what is going to happen if we create clustered index in our table.

We create a cluster index on the id column of the customer table.

- So, first thing SQL is going to do is to  physically sort all the data based on the column id. <br/>
So, the rows are going to re-arrange in each data page from the lowest to the highest. <br/>
So, in the first page, we will have first customer id = 1, then id = 2, the 3, 4, 5, 6,......and so on until we reach the last page & last customer id = 20.<br/>
So, the first page has the lowest customer id values and the last page has the highest customer id values. <br/>
<img width="300" height="200" alt="image" src="https://github.com/user-attachments/assets/9c25b57a-85cd-4dcd-b8a4-3b888c3247c7" />


- Next step is, SQL will start structuring and building the **B-Tree** (Balance Tree)

**What is a B-Tree ?**
> A B-tree(Balance Tree) is a hierarchical structure that is storing data at leaves, to help quickly locate data.
> - It starts with the root and then it keep branching out until we reach eventually the leaves.
> - Between the **leaf nodes** and **root nodes**, We call this section **intermediate nodes**.
> - It can be of one level or multiple level

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/32df51d8-02c5-4e7b-b445-fe2e254fb630" />

Once SQL construct the B-Tree, It's going to very easy for SQL to navigate through the B-Tree in order to specific information.

Let's see How SQL builds the B-Tree for the Clustered index? <br/>
It's very important to understand that the **Leaf Node** in the B-Tree for clustered index contains the actual data, the data pages 
So, all our nicely sorted data pages stored at the leaf level.
Then after that SQL starts building the **Intermediate Nodes** and here SQL use different type of pages, we have the **Index page**

**Index Page**
> It store key values(Pointers) to another page.
> - It doesn't store the actual data(rows).

- In the Index page, we can't find the actual data, entire rows but instead Index page stored a key value that contains a pointer to another index page or data page.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/e428e25c-f5f9-4992-900d-b19da8c89bfe" />

So, if we're searching for IDs between 1 and 5, we locate it at data page ID = 1 : 100 and then we can store in this index page another pointer where we can search IDs between 6 and 10, we can get it at data page ID = 1 : 101.
This is what the structure of an Index page which contains only pointers to another page.
Same thing SQL is going to create for another index page for 2 more data pages like if we're searhing the IDs between 11 to 15, we can get it at data page ID = 1 : 102 similar for Ids between 16 to 20, we will get it at data page ID = 1 : 103

As we can see inside those Index Pages, we have a pointer for each group of ids for each cluster. SO for the group of customers between 1 and 5 we have one pointer and for the second group between 6 and 10 we have another pointer. SO that means we don't have a pointer to a pointer for each row, we have a pointer to each group / for each cluster / each data page. That's why we call it **clustered Index**.

Now SQL is done building intermediate node. <br/>
Now SQL is will and build the last node the **Root Node** where it says if you're searching for customers between 1 1 and 10 then go to index page with ID = 1 : 200. That means Root Node here is pointing to another index page not directly to the data page. Similarly if we're looking for customers between 11 to 20 then it will go the index page with ID = 1 : 201.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/6cbba823-98ef-4b97-9e1c-53cb96a0bb30" />

This is what exactly happens if we create a clustered Index.
- First it will physically sort all the data in the data pages. So if the first time sorted randomly SQL has to arrange everything and sort the data from the scratch.
- Then it will build a structure where we have a root node and index page and intermediate nodes the index pages  but at the leaf level, we have actual data, the data pages.

Let's see what happenf if we query the table  where we search for the customer_id=14.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/c76e7f11-a1aa-4cb7-a9fd-925638d04f98" />


So, It is going to check pointer number 2, since 14, It is a group between 11 and 20. where Index Page ID = 1 : 201
Then it will open the Index page ID = 1 : 201 and Since 14 is between 11 and 15 so It will check the pointer to data page ID = 1 : 102
And with that SQL locate the correct data page.
Now SQL will open this data page and find the customer_id = 14.
So, It was very fast for SQL to locate the correct data page, with only 3 jumps from the root node to intermediate node, SQL is able to find the correct data page quickly. bcuz here SQL needs only to read one data page instead of all data pages as we saw in the Heap structure for different data pages.

Well we can say here still we are reading 3 pages but you know **Reading a index pages is very fast compared to a data page** bcuz at top level of Index pages only contains pointers to another index pages, as only leave nodes contains the data pages which holds the actual data.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/425b2ff6-0768-41e3-bc7e-aa7774165e2d" />

This is exactly How Clustered Index works in SQL database. 

</details>
<!---------------Clustered Indexes------------------>

<details>
  <summary> <b>Non-Clustered Index </b> </summary>

Let's see How SQL builds the Non-Clustered Index?

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/4d8566bd-1122-4de8-a509-c1847add915d" />

As we know in Heap structure where table don't have any index and data are stored randomly inside the data pages.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/577c1122-9693-4508-aae2-0c4f42bfb83f" />

Now, if we go and create a non-clustered Index on the customer_id what is going to happen is SQL will not touch or change anything on the physical actual data on the data pages, so the data pages is going to stay as it is and nothing is going to be change.

And SQL start building the B-Tree structures. So, It is immediately start building an index page. This index page is little bit different than the one which it builds in Clustered Index. 
Since It's an index page, it stores pointers but this time SQL will store here in this Index page key is Customer_Id and values will be not data pages ID.
So, It's going to start with File ID and page number because customer_id=1 is stored in the page 1:102. But SQL will going to add Offset Number of the row as well where exactly in the page we can find the this customer_id and the whole thing we call it an **R ID** the Row Identifier.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/2d865e6a-38e3-446f-a017-b923bea0c30a" />

Now Let's see how Index Page is exactly pointing to the row inside the data page?
So, The first part of ***Row Identifier*** RID [1:150] is mapping to the ***data page ID***
Then from the second part of ***Row Identifier*** RID [96] is mapped to the ***Offset number*** that holds exactly the location of the all rows so obviously row 1. That's exactly the place where we read information about row ID = 1
This is how Index Page is locating the exact pplace of the rows.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/dd3921cc-65a7-4aec-95f4-e171ece789ca" />

So, SQL will go and continue and assign for each customer_id a pointer to the exact location.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/55394979-6c6b-467e-bad3-77747e20d30b" />

As we can see in the index page we don't have a pointer for each group of customers like we have learned in Clustered index but now we have a pointer for each customer_id and this type of index page, we call it **ROW LOCATOR PAGE**.

So now SQL will continue to map a pointer for each customer_id that we have inside our table.
So, We will have multiple index pages pointing to our data page.

> We will have a lot of pointers and the data inside index pages is of course sorted but inside the data pages it is left as it is.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/820c19de-58ce-4ea7-8542-9378b93b6eb2" />


Those index pages that has the Row Identifier RID is going to be stored at leaf level of the B-Tree.
At the leaf level we don't have the actual data the data pages we have index pages where we have pointers to the actual data. Then SQL will go and start building the intermediate nodes, It's exactly like clustered Index where It is going to point another index page.
So, between 1 and 5 customers it's index page numeber 200 and so on.

<img width="400" height="150" alt="image" src="https://github.com/user-attachments/assets/936a968a-caab-4020-af26-c1644ef69c73" />

The next step SQL will do is builds Intermediate Node. It's exactly like the clustered Index, nothing is going to change. It's like same structure. So, It is an index page pointing to another index page but for this for a group of customers.

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/5bdef7e9-5933-4093-a98d-18b12b7c05ca" />

Then we are going to have Root node.
Again we call this structure as B-Tree structure where they points to another data pages but the data pages are not part of the B-Tree.

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/76bc3492-80b5-4ac4-bd9c-b86bf9cc7192" />

Now, Let's see we're searching for Customer_id = 14, What SQL does?
- SQL starts again from the Root node, Then find the pointer to the intermediate node
- Then jump to the next step to the intermediate node, then It finds the pointers to the index page between 11 and 15.
- Then SQL will can the index page and find for the customer_id = 14 , we have the following address there.
- So, It locates the exact data page and exact place of that row in the data page.
- So, Jump directly to the row without scanning anything else.
- So, here this time Non-Clsutered Index, the SQL read 3 different index pages and finally found the data in one data page.

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/eeaa0933-2988-44ea-b02f-8fbd4df49c80" />

This is how SQL Create the B-Tree for Non-clustered Index and how SQL scans it in order to find the information.

 So, If we compare the Non-Cluster Index to Clustered Index, So in non-clsutered index we have one extra  layer to be scanned in order to find the right place of the row.
 
</details>
<!---------------Non-Clustered Indexes------------------>

<details>
  <summary> <b>Clustered Index vs Non-Clustered Index </b> </summary>

We can think of Clustered Index as Book's Table of Content at the front of the Book.
So, Table of contents tells us where to find each chapter and chapters are exactly sorted in table of content.
This is waht exactly Clustered Index does.

<img width="200" height="400" alt="image" src="https://github.com/user-attachments/assets/e1b25386-f049-4583-8225-12d3a2bb7dcd" /> <img width="200" height="400" alt="image" src="https://github.com/user-attachments/assets/a190d1cf-2913-4f28-a744-910da36fd2ff" />

On the other hand, think about the Non-Clustered Index as the Index page that we can find at the end of the book.
The index of the book is a very detailed list of topic, terms & keywords where it points exactly to the loaction where we can find it in the book. And the content/topic of the book is not sorted like the index of book.
This is exactly what the Non-Clustered Index does. It is co-existing with the data, It is an extra list where it can point exactly where to find data inside our table. 



| Clustered Index vs Non-Clustered Index |
|-------------------------------------|
| <img width="300" height="200" alt="image" src="https://github.com/user-attachments/assets/71b2d641-5f7f-41cc-918d-32c8586ec62a" /> | 

The big difference between Clustered Index and Non-Clustered Index is that : <br/>
- In Non-Clustered Index, Data-pages are not part of the B-Tree.
- The B-Tree of the Non-Clustered Index is just a separate structure that doesn't involve any data.So we have only index pages and it just points to the data-pages without changing anything physically with our data.

But in reality what happens is we can have both indexes Clustered Index and Non-clustered Index in one table.
One can have leaf level of that Non-clustered index is going to point to data pages of the Clustered Index. Bcuz those index pages don't care whether those pages are sorted or not. It just going to point to the correct data page and correct row.

Here, we have two different B-Tree structure that are pointing to the data.

<img width="300" height="200" alt="image" src="https://github.com/user-attachments/assets/b3a5c754-394f-4fbd-a20a-eeb701f589c3" />

Basically we can create only one clustered index on a table. This make sense bcuz we can store the data only in one way in SQL. that's bcuz we can sort the data only once. That's why in SQL we are allowed to create only one clustered index. bcuz physically the data can be sorted only one way.

On the other hand in Non-Clustered index, we can create as many non-clustered index we need. So, we can create 3 or 4 etc and all of them are pointing to the same data-pages. bcuz in the B-Tree of Non-clustered index we don't store any data pages. We store only pointers to the data that's why we could have multiple pointers.

<img width="300" height="200" alt="image" src="https://github.com/user-attachments/assets/35cddfbb-b935-4215-acec-89e9e7824ac4" />

<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/57596788-006c-4aa7-a15e-91be990dd4c0" />

</details>

<details>
  <summary> <b>How to create Index ? </b> </summary>

Syntax of Index :

```python
CREATE [CLUSTERED | NONCLUSTERED] INDEX index_name ON table_name (col1, col2, ...);
```

The default Cluster is Non-Clustered, So if we do not mention cluster SQL will created NON-CLUSTERED
```sql
CREATE INDEX index_name ON table_name (col1, col2, ...)
```

```python
CREATE CLUSTERED INDEX IX_customer_id ON Sales.customer (customer_id);

CREATE NONCLUSTERED INDEX idx_customers_city ON Sales.customers (city);

CREATE INDEX idx_customer_name ON Sales.customers(las_name ASC, first_name DESC)
```

Que : Where do we find indexes in Database?
- We can to Object Exporer > Database > Table > Indexes > there you find index.

> Important INFO : **a Primary Key(PK) automatically creates a clustered index by default**
> - bcuz it always make sense to create index on primary key.

So, let's create a new table without any indexes. <br/>
```sql
CREATE TABLE Sales.NewCustomers USING
(
  SELECT *
  FROM Sales.customers
)

--OR

SELECT *
INTO Sales.NewCustomers
FROM Sales.customers
```

Here if we search for customer_id = 1, SQL will do fullscan of the table NewCustomers in order to find the customer_id = 1. So, our newCustomer table is a heap cluster.
```sql
SELECT *
FROM Sales.NewCustomers
```
But let's change that by create a new cluster index on our table.
```sql
CREATE CLUSTERED INDEX idx_newcustomers_id ON Sales.NewCustomers(customer_id)
```
Now let's check the indexes of our new table Object Explorer > Database > Table > Indexes > there idx_newcustomers_id.

> **Rule : Only one clustered index can be created per table**

Let's try to create one more clustered index and let's see what happen?
```sql
CREATE CLUSTERED INDEX idx_newcustomer_name ON Sales.NewCustomers(first_name DESC);
-- Error : Can't create more than one clustered index
```
If we created index on wrong table, then what to do? Drop the old index and create new index.
```sql
DROP INDEX index_name ON table_name;

DROP INDEX idx_newcustomer_id ON Sales.NewCustomers;
```

So, creating index on Customer_name column is not right bcuz Customer_name is not unique and update could happens in name that's why it's not primary key. <br/>
So, creating correct index is important that's why we should create index on unique column which is primary key.

Let's say we have table and we're seaching for the last_name.
```sql
SELECT *
FROM Sales.NewCustomers
WHERE last_name = 'Brown';
```
So, our table is having more and more new customers and table is getting bigger and bigger. And we frequently using the above query where last_name = ''. <br/>
So what we can do here is we can create a non-clustered index for the last_name in order to improve the performance of the query.

```sql
CREATE NONCLUSTERED INDEX idx_newCustomer_lastname ON Sales.NewCustomers(last_name);
```
Open object Explorer > Database > Table > Indexes > there idx_newCustomer_lastname.

We can created multiple non-clustered index on the same table.

So, Suppose we're also using another query by first_name, very frequently and it is slow. So we can create another non-clustered index on the same table. <br/>
Even if we don't specify the type of index it will create Non-Clustered Index.
```sql
CREATE NONCLUSTERED INDEX idx_newCustomer_firstname ON Sales.NewCustomers(first_name);
--OR
CREATE INDEX idx_newCustomer_firstname ON Sales.NewCustomers(first_name);
```
Open object Explorer > Database > Table > Indexes > there idx_newCustomer_lastname,idx_newCustomer_firstname

</details>

<details>
  <summary> <b>Composite Index </b> </summary>

> **Composite Index** is an index that has has multiple columns inside same index.

Why we need it?
- That's bcuz sometime our where conditions are complicated and based on multiple columns.
- e.g. searching for country = USA and Score > 100
```sql
SELECT *
FROM Sales.NewCustomers
WHERE country = 'USA' AND score > 100
```
As we are using two filters and we want to speed up this query. So How can we do that? : by creating idex.
```sql
CREATE INDEX idx_newCustomer_countryScore ON Sales.NewCountry (score, country); --❌

CREATE INDEX idx_newCustomer_countryScore ON Sales.NewCountry (country, score); -- ✅
```
> **Rule : The columns of index order must match the order in your query.**

Open object Explorer > Database > Table > Indexes > there idx_newCustomer_lastname, idx_newCustomer_firstname, idx_newCustomer_countryScore 

once we create such index, our table will always update this index. So we have to be committed and responsible. 
So, in our query if we want to filter the data using country and score, always start with country and score in order to be able to use the index optimizer.
So, If the order of filter is same as the index created order it's fine otherwise SQL will be not using the index we created. so, **column order is very important in indexing**. So, either adjust your query or re-create index based on column order.

What happens if we have created index using 2 columns and while querying we're filtering on only one column?
```sql
SELECT *
FROM Sales.NewCustomers
WHERE country = 'USA';
```
Yes, It will use index optimization bcuz it follows the leftmost prefix rule.

**Leftmost Prefix Rule**
> Index works only if your query filters start from the first column in the index and follow its order.

```sql
SELECT *
FROM Sales.NewCustomers
WHERE score > 100;
```
No, It will not use index optimization bcuz It does not follow leftmost prefix rule.

So, basically we have index based on for columns A, B, C, D and if target the columns A it will work, if we target A,B it will also work, if we target A,B,C it will also work and if we target A,B,C,D ofcourse it will work.
But if we target B, it will not work and if we target A,C it will not work. <br/>
So, It will only use index optimization if and only if it follows the leftmost prefix rule.

</details>

Till here, we have see the first category of index i.e. based on structure **Clustered Index** and **Non-Clustered Index**. <br/>
<!----------index i.e. based on structure------------------->
Now next we will see the second category of index i.e. based on Storage **RowStore Index** and **ColumnStore Index**

<details>
  <summary>What is <b>ColumnStore Index</b> ?</summary>

Let's say we have table in which we have multiple rows and multiple columns.

Now if we use a Row Store Index, this is the classical one. What is going to happen is It will splitted into multiple rows and each group of rows will be going to be stored inside a data-page that means we're orgnazing the data row by row which means all the for each row is going to be stored together. This is the traditional way on how the databases organize their data where the information are stored row by row.

On the other side if we use Column Store Index, the SQL will split the table into multiple separate columns and SQL stored values of one column together in data-page that means if we open the data-page we will find only values of one column, we will not find the entire row.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/ca8f3306-462b-46c6-8167-8e1064fdbdd4" />

> The **Row Store Index** stores the data **row by row** and **Column Store Index** stored the data **column by column**

This is a very high level representation on how the row store index and column store index stores the data.
We will see in details in order to understand exactly how SQL works with Row STore Index and Column Store Index.

</details>

<details>
  <summary>How SQL builds Column Store Index <b>The process</b> ?</summary>

Let's say we have a table for Customers in which 3 columns id, name and status. and we have around 2 million rows in the customer table.

| Id | Name | Status |
|----|------|--------|
|....|......|........|
|....|......|........|
|....|......|........|
|....|......|........|
|....|......|........|

As we know the table is by default built as Heap Structure where rows are stored row by row inside the data-pages.
But now we have to create a column store index on top of this table.

Once we do that, SQL will go through a process in order to build the column store index.

1. 1st step is SQL will divide the data, the rows into **Row Groups**. <br/>
Now in SQL server, each row group can contain like around 1 million rows. <br/>
So, in this example our table is going to be splitted into 2 groups.
   - the 1st group one million row in one group
   - the 2nd group one million row in second group
Now we might have a question, we're talking about columns still we're splitting the row? <br/>
Well, this is just a pre-step in order to just optimize the performance and do parallel processing. <br/>
Of course the data will not be stored like this bcuz we have the second step.

2. 2nd step is SQL will **Segment the Columns**
Now SQL will go and for each row group and start splitting the data by columns. That's why we call it Column Store because we're separating the columns from each others so that we have one segment for one column and another segment to second columns and so on. <br/>
Now SQL will move to the next step in this process.

3. 3rd Step is **Data Compression**.
This is the most important step in this process bcuz it is the reason why columns store is very fast compared to row store.
So in this process, there are like different techniques on how to do Data Compression and the most famous one is the _create a dictionary_ <br/>
e.g. column status whether it is active or inactive, so the word active and inactive is going to repeated like 2 millions time bcuz we have 2 million customers and since it is like string so it will take a lot of spacce and storage, But now instead of that we're going to compress the data.
- 1st SQL will create a dictionary by replacing the value Active and NonActive into smaller value like 1 and 0.
  so we have mapping between the long value to a small value.
- After that SQL will store the data stream where we have like only 2 values 0 and 1. So, we have a big stream of 2 million rows. So, It is going to do this for each column and with that the size of each column is going to change depends on how much different values we have in each column.

So , this step is very important in order to reduce the size of data and as well to increase the performance.
Now once everything is organized and compressed, SQL will start storing the results in data-pages. But here SQL will not use the standard Data-pages that we learned previously. But instead SQL will use a special data-page called **LOB = Large Object Page** 

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/ecd6a3b8-e7aa-4281-90f4-81f359a79ff8" />


Let's compare the Normal Data-Page (Row Store) and LOB Large Object-Page(Column Store).
- As usual each page has a **Header**, So similar to data-page, LOB page has also a header.
- Next segment in LOB has a **Segment Header** which is metadata information about column segment that is stored in this page like segment_id, row_group_id, dictionary_id.
> Dictionary page is also a type of page in SQL. It has a header and inside it we have mapping. It maps the original value i.e. long value to smaller value.

- Beneath the Segment header, We have a place where data is stored i.e. known as **Data Stream**. It is a sequence of Id from the dictionary that represents the values of the column side by side. <br/> Of course we can't fit the one million rows inside the one Data Stream, that's why we have multiple LOP (large object Pages) 

This is how SQL exactly store the data, if we go with the ColumnStore.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/3d4cc3f6-4944-4c2b-9472-c9508b7e6489" />


As we can see SQL stores data as LOP data storage, so this is the last step and with that SQL did convert our table into a columnStore. <br/>
Now we can't just create a column store without defining whether it is clustered index or non-clustered index.

So let's 1st start with Clustered ColumnStore Index.
- If we create such an index, SQL will not build a B-Tree structure. SQL is going to use the ColumnStore Structure.
- As we learned the clustered index is a complete makeover of our table. When we apply it the SQL will format everything column-wise and It is fully replacing the old Row based table structure that we have at the start.
- So, once we applied the ColumnStore Index, It will not leave anything behind and our table is going to completely structured as a column stored.
- One more thing which makes sense is all the columns from the original table is going to convert to a columnStore.

<img width="500" height="200" alt="image" src="https://github.com/user-attachments/assets/fcef485d-92e0-4b99-9e85-2e0f57e7a211" />


On the other hand if we're suing Non-Clustered ColumnStore Index, 
- As we learned It is like a companion to our existing table. So, It co-exist with the table and it will not replace anything.
- So, ColumnStore index can be an additional thing that is stored beside our table. That means the original table is not deleted at all like Clustered ColumnStore Index. The 1st one is in the old row based storage and the regular table, the first one and our data is going to stored in a separate structure  in the ColumnStore Index.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/19e69f17-6fc2-4ef4-b864-9d921a0411d4" />


- Of course in Non-clustered ColumnStore Index, Since we're creating an extra index outside of original table, we can define which column should be included in this process. It must not be all the columns, for example we can have only one column like status. that means we built a columnStore index only for one column which is status of the customer.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/eb837c3e-ad38-4f19-ad64-0407e927f62e" />

This is what we mean with the clustered columnStore Index and the non-clustered ColumnStore index.

</details>


<details>
  <summary>What is <b>RowStore Index vs ColumnStore Index</b> </summary>

Why would we split our data by the columns?
- It's all bcuz of Analytics, in analytics we have big complex query where we have a lot of data aggregations and stuffs on the big tables and the RowStore Index is perfectly designed in order to improve the performance of such big queries. That's why SQL databases like SQL Server and BI tools like PowerBI did adopt this method in order to offer fast platform for data analysis.

Let's understand why the ColumnStore Index is way faster for Data Analyser than the RowStore Index.
- we have a customer table, where we have 5 customers with columns Id, Name and status.

|Id  | Name | Status  |
|----|------|---------|
| 1  |Mahesh| Active  |
| 2  |Suresh| Inactive|
| 3  |Ganesh| Active  |
| 4  |Dinesh| Active  |
| 5  |Ramesh| Inactive|

- If we are using RowStore Index, the data can be store in multiple data-pages and in each data-page we're going to have the whole record (whole information about one customer) for this example we have 3 data-pages.

- But if we are using ColumnStore Index, the data is going to stored little bit differently.
  - Here first column id is stored in one data-page and here SQL will bot build a dictionaru bcuz Ids are already short. We're going to have one Data-Stream with all ids and
  - for the next column name is going to stored in separate data-page where we're going to have extra dictionary-page where each name is going to map to a small value so the data is going to be compressed save storage and in the Data-storage will have ids of the dictionary.
  - Now for the third column status, we will have one more data-page where there is another extra dictionary-page to map each status is map to a small value and in the Data-Stream we will be storing only ids of the dictionary. 

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/a328c269-4ae3-409c-807b-1383505ad897" />

Let's understand why the ColumnStore Index is faster. <br/>
Suppose we have a query to find the total number of customers that are active. <br/>
Query : 
```sql
SELECT
    COUNT(*)
FROM Sales.customers
WHERE status = 'Active'
```
If we query the table with RowStore Index what is going to happen is SQL first go and collect the data like it will go to 1st data-page and collects first 2 customers then second and then and so on.
As we can see here, SQL is reading everything the whole rows id,name,status even though for that query where we actually don't all those information, we just need to count how many customers we need with status active.
After that SQL has all the data then it will filter the data only for active rows. Then SQL will do the aggregate operation and with that we will get the final result 3 rows which are active.

But let's see how SQL will query the ColumnStore Index, here SQL first analyse which column it needs actually for this query. Well It only needs status. So, SQL will not go and open all three data-pages and read it rather It will target only one data-page, the data-page where we have column Status. So, SQL will take this very simple Data-Stream of column 3 and understand the dictionary of it and it will remove all the values where it's equal to 0 to filter out inactive status and return the final result which has only 3 rows that are active.
It is very quick.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/afc1d554-f281-45f6-affd-c5ae3a387d20" />


Now, if we compare the intermediate resultsets from the RowStore Index and COlumnStore Index, we can see that in the RowStore we have fetch and retrieve a lot of unnecessary information for this query which is even not required. This will make speed of the query very slow. <br/>
But in the columnStore, SQL reads exactly what it needs for the aggregation in query. We didn't read any extra information about the names, ids of the customers.
That's why the performance of the queries where we have aggregations and data analysis, It is going to very fast if we're using ColumnStore Index compared to the RowStore Index.

|                       | RowStore Index                     | ColumnStore Index                              |
|----------------|-------------------------------------------|------------------------------------------------|
| Definition :   | Organizes and stores data **row by row** | Organizes and stores data **column by column**  |
| Storage Efficiency : | **less efficient** in storage        | **highly efficient** bcuz of **Compression**  |
| Read/Write Operation :|Fair speed for read and write operations | **Fast** read performance **slow** write performance |
| I/O Efficiency : | **Lower** (retrieves all columns)        | **Higher** (retrieves only **specific** columns)|
| Best for :       |**OLTP** (Transactional) <br/> commerce, banking, financial systems, order processing | **OLAP** (Analytical) <br/> Data WareHouse, Business Intelligence, Reporting, Analytics |
| Use case : | High Frequency transaction application <br/> Quick access to complete records | Big Data analytics <br/> Scanning of large datasets <br/> Fast aggregation. |

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/e6896330-ce1a-4617-8a59-d9361cb3ca46" />

</details>

<details>
  <summary> How to create <b>ColumnStore Index</b> ? </summary>

Syntax of ColumnStore Index :
```python
CREATE [CLUSTERED | NONCLUSTERED] [COLUMNSTORE] INDEX index_name
ON table_name (col1, col2, ...)
```
- If we don't specify the ```COLUMNSTORE``` then it will by default Create ```ROWSTORE```.

```ROWSTORE``` 
```py
CREATE NONCLUSTERED INDEX idx_customers_country ON Sales.customers (country);

CREATE CLUSTERED INDEX idx_customers_id ON Sales.customers (id);
```
```COLUMNSTORE``` 
```py
CREATE NONCLUSTERED COLUMNSTORE INDEX idx_customers_country ON Sales.customers (country);

CREATE CLUSTERED COLUMNSTORE INDEX idx_customers_id ON Sales.customers (id); # ❌ Not allowes to use column in Clustered Index bcuz It make no sense once awe say Clustered ColumnStore then all the columns will included in the new structure.

CREATE CLUSTERED COLUMNSTORE INDEX idx_customers ON Sales.customers
```
> **Rule :** We can't specify Columns in **Clustered ColumnSTore Index**

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/27de9aa5-c718-43dc-b065-e4dd90b1dbff" />

Now if we check Object explorer > Database > Table > Indexes > there you will find all your indexes
```sql

CREATE CLUSTERED COLUMNSTORE INDEX idx_customers_id ON Sales.customers (id); //❌ Error not allowed

CREATE CLUSTERED COLUMNSTORE INDEX idx_customers  ON Sales.customers; // ✅ but again an error ❌

DROP INDEX idx_customers_id ON Sales.customers;

CREATE CLUSTERED COLUMNSTORE INDEX idx_customers  ON Sales.customers; // ✅
```
> **Note : We can't create more than one Clustered index on the same table!**

```sql
CREATE NONCLUSTERED COLUMNSTORE INDEX idx_customers_firstName  ON Sales.customers (first_name); //❌ Error

DROP  INDEX idx_Customers ON Sales.customers;
CREATE NONCLUSTERED COLUMNSTORE INDEX idx_customers_firstName  ON Sales.customers (first_name); //✅
```
> **Multiple ColumnStore indexes are not supported!**
> - We can't create multiple columnstore indexes, we can create only one columnstore index for each table.
> - We have to decide bewteen CLUSTERED or NONCLUSTERED either of them. unlike ROWSTORE Multiple Non-Clustered Index.
> - This limitation is only in SQL Server, in other databases like Azure SQL Server, It is allowed to have multiple columnStore indexes.


As we know that ColumnStore Index is going to compress the data and that makes it to take less storage for the entire table than the RowStore Index. <br/>
Let's check whether that's true or not? <br/>
To do this :
- Let's create 3 identical copies of a table and have different structures on each of the copy
  - 1st copy -> heap Structure
  - 2nd copy -> rowStore Structure
  - 3rd copy -> columnStore Structure
- Then compare the storage of all of these three.

```sql
USE Testing --switching the database Sales to Testing

--HEAP
SELECT *
INTO Testing.customers_heap
FROM Testing.customers;
--With that we have created a heap table Since we didn't define any clustered index. 

--ClUSTERED ROWSTORE INDEX
SELECT *
INTO Testing.customers_rowStore
FROM Testing.customers;
CREATE CLUSTERED ROWSTORE INDEX idx_customers_id ON Testing.customers_rowStore (id, order_id);


--COLUMNSTORE INDEX
SELECT *
INTO Testing.customers_columnStore
FROM Testing.customers;
CREATE CLUSTERED COLUMNSTORE INDEX idx_customer_status ON Testing.customers_columnStore (status);
```
Now we have created those 3 tables, let's check storages of each of them.
- 1st Heap table ```Testing.customers_heap``` > right click > property > storage > Data space | index space
- 2nd rowStore table ```Testing.customers_rowStore``` > right click > property > storage > Data space | index space
- 3rd columnStore table ```Testing.customers_columnStore``` right click > property > storage > Data space | index space

Here what we will notice is that the Data Space size of Heap table and rowStore table is same but the index space size is different.
Interesting thing is when we compare the data space of columnStore table with rowStore or heap. It is very than than these both bcuz of compression on data and no index spaces bcuz we don't have B-tree structure in ColumnStore Index

Now we want to them based on storage, the best will be rowStore table, then Heap structure table and then rowStore clustered table

</details>
<!---------------Clustered ColumnStore Indexes------------------>

<!----------index i.e. based on storage------------------->

**Unique Index**

<details>
  <summary> <b>Unique Index </b> </summary>

What is an **unique Index** ?
- Unique Index ensures no duplicate value exist in specific column.
- Benefits :
  - Enforce uniqueness
  - SLightly increase query performance

> Unique index is a special type of index that make sure no duplicates in our data.
> There are couple of reasons why it is important to have unique index.
> 1. To have **Data Integrity**. So, Unique Index enforce uniqueness in our data and that is very helpful. e.g. if we have a column like email_address or a product_id, having duplicate these columns can mess up data very badly.
> 2. To **improve performance**. e.g. if we're searching for specific email, the SQL start searching for the email value and once the SQL find the value, SQL will stop searching because we're sure that there is no duplicate data. So with that we're improving the performance of our queries. So, if you're creating an idex and you know this column is unique then make sure index as unique index.

If we have to look our clustered index again where we have the B-Tree structure, Here If we make the index as unique then we are giving the extra task for SQL that make sure all those ids of the customer table will going to be unique. So, Now SQL has the gurantee that there are no duplicates at all inside the data in the data-pages.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/b9abaf2a-172c-4aea-99f9-44fc0d39f5fd" />


Since we're giving SQL an extra task to prove the uniqueness of the data, building the clustered index will become little bit slower. That means INSERTING/WRITING new data is going to be slower as compared to normal clustered index. But READING performance will be optimize and little bit faster than normal clustered index.

Basically, in this trade off, we're making writing data slower but we're gaining more speed on the reading data. This is what we mean by Unique Index.

**Performance**
> - Writing to an unique index is slower than non-unique.
> - Reading from an unique index is faster than non-unique.

</details>

<details>
  <summary> Syntax of Unique Index </summary>

```python
CREATE [UNIQUE] [CLUSTERED | NONCLUSTERED] [COLUMNSTORE] INDEX index_name ON table_name (col1, col2, ...)
```
If we don't specify the ```UNIQUE```, the default is normal index which allows duplicates.
```sql
CREATE INDEX Idx_Customers_Email ON Sales.customers (email);
```
If we specify the ```UNIQUE```, the duplicates are not allowed.
```sql
CREATE UNIQUE INDEX Idx_Customers_Email ON Sales.customers (email);
```

```sql
SELECT * FROM Sales.products

CREATE UNIQUE NONCLUSTERED INDEX idx_product_category ON Sales.products (product_category);
--get an error bcuz category has duplicates. SQL can't create unique index for this table.
--We can create an unique index on an empty table, but SQL will not allow us to insert any duplicate in the column that table has.
-- Of course It doesn't make sense to have unique index on column category bcuz category itself says duplicates.

CREATE UNIQUE NONCLUSTERED INDEX idx_product_id ON Sales.products (product_id);
--It will work bcuz we don't have any duplicates inside the product_id
```
> **Rule : Duplicates in the column will prevent creating a unique index.**

Now we can go and check Object Explorer > Database > table > Indexes > unique non-clustered index

Let's check Data Integrity. Are we allowed to add any duplicates in this table based on product_id.
```sql
INSERT INTO Sales.products
VALUES (105, "Gloves", "Clothing", "30")
--Error we cvan't insert duplicate key bcuz we have unique index
-- As we can see Unique index is helping us and improving the quality of table.
```
| Product_id | product | category | price |
|------------|---------|----------|-------|
| 101        | Bottle  | Accessories | 10 |
| 102        | Tire    | Accessories | 15 |
| 103        | Socks   | Clothing    | 20 |
| 104        | Caps    | Clothing    | 25 |
| 105        | Gloves  | Clothing    | 30 |

</details>

<!----------Unique Indexes------------------->

**Filtered Index**

<details>
  <summary> <b> Filtered Index </b> </summary>

> **An index that includes only rows meeting the specified conditions**.

A filtered Index is a regular index but with a twist. It only includes the rows that meet specific condtions.

We have our non-clustered index and B-Tree structure. <br/>
Now at the leaf nodes we will het only the Ids the data that fulfill a specific condtions. <br/>
For example if we're saying we want only active customers, this is the condition.
So, that means on the leaf node we will have only the customer_ids that are active and any inactive customer will be not included at all at the data-page and at the nodes. <br/>
So, Our B-Tree structure is going to be little bit smaller as usual bcuz we have less data included in the structure. So, Index is going to be samller than the regular non-clustered index. <br/>

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/d05b0f99-d383-4dce-8f7b-3749faf643cd" />


Que : **Why is it important to have a filtered Index ?** <br/>
Ans : Well, bcuz filters index benefits is :
- **Targeted optimization** : e.g. if our analysis is always focus on the active users and inactive users are totally irrelevent. That means having only relevant subset of data in the dex makes the whole index much smaller which leads to faster performance. Here, We're doing targeted optimization and improving the query performance.
- **Reduce Storage : Less data in the index** : Since the size of B-Tree is going to be smaller that means we need less storage space in order to store the index which is a great thing if we have large tables in the database, and that's bcuz we're filtering the irrelevant data. 

Basically Filtered Index is going to make the structure of the index smaller which is going to improve the speed and performance and as well reduce the storage that is need for the index.

</details>

<details>
  <summary> Syntax of Filtered index </summary>

Filtered Index Syntax :
```python
CREATE [UNIQUE] [NONCLUSTERED] [rowstore] INDEX index_name
ON table_name (col1, col2, ...)
WHERE [condition]
```

> **Rule 1. : You cannot create a filtered Index on a clustered Index** bcuz it make no sense if we create a clustered index, the entire table should be re-organize and ordered. and We need only subset of data

> **Rule 2. : You cannot create a filtered Index on a columnStore Index** bcuz It make no sense as we need all the columns by filtering out the rows.

We can combine the ```UNIQUE INDEX``` together with the ```FILTERED INDEX```. There is no restrictions. 

```sql
SELECT * FROM Sales.customers
WHERE country = 'USA';

CREATE NONCLUSTERED INDEX Idx_Customer_country ON Sales.customers (country)
WHERE country = 'USA';
--Index will be build that will be focused & targeted only for subset of data.
-- Duplicates are allowed

CREATE UNIQUE NONCLUSTERED INDEX Idx_Customer_id ON Sales.customers (id)
WHERE status = 'Inactive' 
--Duplicates are not allowed as It is filtered + unique
```
Open Object Explorer > database > Table > Indexes > Filtered Index which can be normal or unique both

Here, in this filter query if we use ```SELECT * FROM Sales.customer WHERE country = 'USA';``` It will work with faster speed, But ```SELECT * FROM Sales.customer WHERE country = 'Germany';``` The query will perform slow as all those rows inside this query is not part of Filtered Index. So, filtered index will be not used here.

</details>

<!----------Filtered Indexes------------------->

<details>
  <summary> <b>How to choose the Right Index ? </b> </summary>

How to use the right index? <br/>

| When to use which index |
|-------------------------|

**HEAP**
> Fast **INSERTs** for staging and temporary tables.

Heap Structure, It is a table without any index, So in this scenario we don't have to use any indexes. <br/>
In case if we want to have fast inserts/Write performance then don't take any index. Stay with default with the heap structure of table. <br/>
Usually we use it in not very important table like staging tables or temporary tables where we want to insert the data fast and then get rid of the data later on. So, there is no need to utilize any index.

**Clustered Index**
> for **Primary Keys** if not, then for date columns.

> When we say CLustered Index that means **CLustered RowStore Index**

> RowStore Clustered Index -> OLTP (Online Transactions Processing, e.g. : Banking, eCommerce Sales, Logs etc)

Clustered Index, we use Clustered Index for **Primary Keys**. <br/>
It is even a default from the database,if we create constraints like primary keys on table then SQL will create clustered Index. This is the main usage of Clustered Index.

If there is no primary key on our table, then we can pick another column where sorting the data is important like e.g. a date column, It could be good candidate for our clustered Index.


**ColumnStore Index**
> For **Analtical** Queries and to Reduce size of Large data.

> ColumnStore Index -> OLAP (Online Analytical Processing e.g. : Data WareHouse, Complex Data Analysis & Reporting, Business Intelligence etc ) 

If we have like big complex analytical queries where we are aggregating a lot of data doing data-aggregations then we have to go for Column Store Index. Because It gives us amazing performance.

As well as if we're struggling with the size of tables like we have a huge super large table then we can use the ColumnStore Index. Because It compress the data and reduce the size of whole table.

**Non-Clustered Index**
> For **non-Primary Key** columns like (Foreign Keys, Joins and Filters)

We usually use Non-Clustered Index for non-Primary Keys columns. That means except primary key columns rest al of the columns could be candidate for non-clustered index.

There are a lot of reasons why we would do that. e.g. ```Foreign Keys``` or using it on the columns that are used in order to ```JOIN``` two tables or columns that are used for the filters like ```WHERE``` clause.

So, There are many scenarios where we can use the Non-Clustered Index but Not for the Primary keys.

**Filtered Index**
> Target **Subset** of data and Reduce size of the index

We use filtered Index in order to target a subset of data. So if in our query or analysis we're only focusing on a subset of data all the time , It makes no sense to have one big index for all the data, here we can use the filter index to have focused and target subset of data.

Of course if the size of the index is a problem then we can use a filter index in order to reduce the over all size of the storage of the index.


**Unique Index**
> Enforce **Uniqueness** and Improve Query Speed

We can use the Unique Index in order to ensure Data-Integrity of our table. As well it might improve  slightly the performance of our query and that's bcuz SQL has less task to do if the index is unique. Once SQL Engine find a match, It will skip the search.

This was the quick summary and guide on when to use which index type that helps in finding the right index.

</details>

<details>
  <summary> <b>Index Management and Monitoring </b> </summary>

- We have created our indexes in our database, our query is optimize and we have now fast performance. But the job is not done yet.

- Bcuz over the time indexes get fragmented, outdated and unused. This leads to a poor performance in queries and also increase the storage cost. So, Overall performance of our database is going to drop down.

- So, Indexes are like having a car, It need maintenance. So, We need to change oil, tyre and do servicing of the car. Same thing goes for indexes. We have to maintain them. They need attention to keep everything running smoothly.

Let's see How we should manage, maintain and monitor the indexes we have created on our tables inside database of our project.

|**INDEX MANAGEMENT**          |
|------------------------------|
| 1. Monitor Index Usage       |
| 2. Monitor Missing Indexes   |
| 3. Monitor Duplicate Indexes |
| 4. Update Statistics         |
| 5. Monitor Fragmentations    |

1st and most important task is **to monitor the usages of our indexes**. <br/>
We have to ask ourself over the time : <br/>
"Are we really using the indexes that we have created?" <br/>
"Are they really helping in improving the speed or performance of our queries?" <br/>
"was It just a good idea to create indexes at start of the project and later no one is using those indexes?" <br/>

This is very crucial because if we're having unused indexes, So They are consuming unnecessary storage spaces as well as Write/Insert performance in the table is slow.

So, now task is to find out the usage of each index that we have created? <br/>
How we can do that? <br/>
So, We have created multiple indexes on the table. <br/>
Go to Object Explorer > Database > Tables > Indexes > here you will find all the indexes on the table.
Now we can see all those informations by using a special Stored Procedure provided by the SQL Server called **```sp_helpindex```**. 
- It is a system stored procedure that comes with the database.
- This stored procedure only need only one value and that is the table name.

```sql
--List all indexes on a specific table
sp_helpindex 'Sales.customers';
```
| index_name | index_description | index_keys |
|------------|-------------------|------------|
| idx_Customer_countryScore | nonclustered located on PRIMARY | country, score   |
| idx_Customer_custFirstName| nonclustered columnstore located on PRIMARY | NULL |
| idx_Customer_FirstName | nonclustered located on PRIMARY | first_name |
| idx_Customer_LastName | nonclustered located on PRIMARY | last_name |

index_name is the name of index which we provided while creating the indexes. <br/>
Primary is the name of the file group where the data is stored. As a default indexes are stored as Primary. <br/>
Index_keys is nice information to understand which keys are used or columns for the index. 

Our task is how to monitor the usage of Indexes.
In databases we have a lot of schemas and tables that has protocol the metadata of our database. And in SQL Server we have a special we have a special schema called **```Sys```** System Schema.

> **```Sys``` System Schems** : contains metadata about the database tables, views, indexes, desc, columns etc.
```sql
--Monitor Index Usage
SELECT * FROM sys.indexes;

SELECT
    tb.name AS table_name
    idx.object_id,
    idx.name AS index_name,
    idx.type_desc AS index_type,
    idx.is_primary_key,
    idx.is_unique,
    idx.is_disabled
FROM sys.indexes idx
JOIN sys.tables tb
ON idx.object_id = tb.object_id
ORDER BY tb.name, idx.name
```
We get a huge list of all the indexes that we have and a lot of information for each index. We don't have have to understand each column. We just have to focus on main important informations from this table.  

Now we don't want the object_id, instead we need full_name of the table and there are a lot of indexes in the result are irrelevant for our database. So in order to get full_name and filter the table, we have another metadata table ```sys.tables```so join it with ```sys.indexes```.

Now We have only required columns and rows. with this we have a nice list of all indexes that are created on our tables inside database of our project.

We're not done yet, our task is how to monitor the usage of Index? <br/>
In order to get the usage for each of those indexes, we have a special view called **dynamic management view** and there SQL Server provide a lot of statistics about the usage of the index.

> **Dynamic Management View (DMV)** : provides real-time insights into Database performance and system health.

```sql
SELECT * FROM sys.dm_db_index_usage_stats
```
Here we will have information about indexes how many time it updated, scans, seeks, when was last usage of index etc.

Let's integrate this view ```sys.dm_db_index_usage_stats``` with the ```sys.index``` and ```sys.tables``` in query.

```sql
SELECT
    tb.name AS table_name
    idx.object_id,
    idx.name AS index_name,
    idx.type_desc AS index_type,
    idx.is_primary_key,
    idx.is_unique,
    idx.is_disabled,
    dmv.user_seeks,
    dmv.user_scans,
    dmv.user_lookups,
    dmv.user_updates,
    dmv.last_user_seek,
    dmv.last_user_scan,
    COALESCE(dmv.last_user_seek, dmv.last_user_scan) AS last_update
FROM sys.indexes idx
JOIN sys.tables tb
ON idx.object_id = tb.object_id
LEFT JOIN sys.dm_db_index_usage_stats dmv
ON dmv.object_id = tb.object_id AND dmv.object_id = idx.object_id
ORDER BY tb.name, idx.name
```
Here, we're doing left join otherwise if we do inner join, we will only get used indexes but we need full build of all of our indexes in the database.

Why we

</details>

<!---------------Indexes------------------>

## 10.2 Partitions
<details>
  <summary> <b>Partitions </b> </summary>
</details>
<!----------------Partitions----------------->


## 10.3 Performance Tips

<details>
  <summary> <b>Performance Tips </b> </summary>
</details>
<!---------------Performance Tips------------------>







