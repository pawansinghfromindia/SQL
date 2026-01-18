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

<img width="200" height="500" alt="image" src="https://github.com/user-attachments/assets/e1b25386-f049-4583-8225-12d3a2bb7dcd" />

On the other hand, think about the Non-Clustered Index as the Index page that we can find at the end of the book.
The index of the book is a very detailed list of topic, terms & keywords where it points exactly to the loaction where we can find it in the book. And the content/topic of the book is not sorted like the index of book.
This is exactly what the Non-Clustered Index does. It is co-existing with the data, It is an extra list where it can point exactly where to find data inside our table. 


</details>

<!---------------Indexes------------------>
****


## 10.2 Partitions
<details>
  <summary> <b>Partitions </b> </summary>
</details>
<!----------------Partitions----------------->


# 10.3 Performance Tips

<details>
  <summary> <b>Performance Tips </b> </summary>
</details>
<!---------------Performance Tips------------------>







