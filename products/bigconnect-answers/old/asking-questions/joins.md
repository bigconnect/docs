# Joins

Joins are a fact of life when it comes to analyzing data, and with Answers we try to make getting the answers to your questions as easy possible—even if that requires some more complex tactics. In this article, we’ll cover how to join tables in Answers using simple and custom questions in the Notebook Editor, and give some context on relational databases, tables, and keys, to give you a better understanding of how joins work.

### Automatic joins

Joins in SQL allow you to combine data from multiple tables. Where possible, Answers makes those connections for you, making it easy for people to work with data across multiple tables. You have to have foreign key relationships defined in your data model to allow Answers to detect those relationships and connect those tables in the data model. If those foreign keys are not configured, administrators can always use the Data Model section to specify those relationships.

We’ll cover keys in depth below, but for now, let’s see an implicit join in action. We’ll use the Sample Database included with Answers as our data so you can try it out for yourself.

* From the top navigation bar, select **+ New**.
* Choose **Question**.
* Select **Raw data** > **Sample Database** as your data source.
* Then choose the `Orders` table.
* Select **Visualize**, and Answers will display a list of orders from the `Orders` table.
* Click on the **Settings button** at the bottom left of the screen. As shown in figure 1, Answers will slide out a sidebar of **Table options** and present you with a list of columns to choose from.

![Fig. 1. Orders table with the Settings sidebar open to the Table options section. You can add, remove, and edit columns, including More columns from tables that Answers can automatically join to the Orders table.](https://www.metabase.com/learn/images/joins-in-metabase/add-column-join-table.png)

The **Visible columns** are the columns currently in the query of the question, which in this case includes all the columns from the `Orders` table. You can add and remove columns, and change column settings (which differ depending on the column’s field types).

In addition to the **Visible columns** you’ll see a section called **More columns**. This section will include columns from two other tables: `Products` and `People`. (We’ll get to why the `Reviews` table is conspicuously absent later in the article).

Answers has already “joined” the `Orders` table to the `Products` and `People` table, which allows you to add columns from these tables.

For example, from this **Settings sidebar**, scroll down to find the `Products` table, and click on the `+` next to the `CATEGORY` column to add it to **Visible columns**. You’ll see a new column, `Products → Category`, in the visible columns, with the `PRODUCT →` prefix indicating that the column is not native to the `Orders` table.

By clicking on a value in an ID column and selecting **View details**, you can check to see which tables are connected to that data. For example, from the `Orders` table, you can view the details on an entry in the `Product ID` column. In the case in figure 2, Answers tells us that the product, “Lightweight Wool Computer” (ID: 146), is connected to the `Orders` table and the `Reviews` table.

![Fig. 2. Clicking on a value in an ID column and selecting View details will show you, among other data, which tables the record is connected to.](https://www.metabase.com/learn/images/joins-in-metabase/lightweight-wool-computer.png)

Note that the actual product ID value is 146, not “Lightweight Wool Computer.” Answers allows administrators to change how column values are displayed.

![Fig. 3. The field settings for the PRODUCT\_ID field in the Orders table. Admins can use the Display values setting to display a human-readable title instead of an ID.](https://www.metabase.com/learn/images/joins-in-metabase/display-values.png)

As shown in figure 3, when viewing the `Orders` table, the foreign key, `PRODUCT_ID`, displays the value from the `TITLE` column of the `Products` table.

We’ll dig into keys below, but first let’s take a look at an explicit join.

### Example explicit join

Let’s try a simple join. We’ll create a new question by clicking on **+ New** > **Question** > **Raw Data** > **Sample Database**. We’ll select the `Orders` table. In the **Data section**, click on the **join icon** to add another table.

![Fig. 4. To add tables to your question, click on the join icon (the Venn Diagram).](https://www.metabase.com/learn/images/joins-in-metabase/join-icon.png)

If we add the `Products` table, Answers will automatically fill in the foreign key relationship, since it knows that the field, `PRODUCT_ID`, contains foreign keys that reference values in the `ID` column (the entity key) of the `Products` table.

![Fig. 5. Joining the Orders table to the Products table by linking the foreign key (PRODUCT\_ID) in the Orders table to the entity key (ID) in the Products table.](https://www.metabase.com/learn/images/joins-in-metabase/orders-and-products.png)

Here’s what the query in figure 5 is saying: for each row in the `Orders` table, use the value in the `PRODUCT_ID` column to find the corresponding row(s) with the same value in the `ID` column of the `Products` table, and return a new row with the values from the columns in both the `Orders` and `Products` tables.

If you’re interested, here’s the basic SQL at work under the hood:

```
SELECT
  *
FROM
  Orders
  LEFT JOIN Products ON Orders.PRODUCT_ID = Products.ID
```

(Note that if you convert the question to SQL, you’ll get a much more involved SQL query, but both queries return the same data.)

You may have noticed in the simple question mode above that you couldn’t add columns from one of the tables in the Sample Database, the `Reviews` table. To understand why, we’ll need some background on relational databases.

### Relational databases

Relational databases (like PostgreSQL and MySQL) store data in tables (relations) that generally represent an entity of some kind, like orders or products. These tables comprise columns (attributes of the entity) and rows (sometimes called records). If you’ve ever worked with spreadsheet software before, a table is akin to a sheet with columns and rows.

| Term   | Description                | Examples                           |
| ------ | -------------------------- | ---------------------------------- |
| Table  | Entity                     | Person, Order, Product             |
| Column | Attribute of that entity   | Address, Description, ID           |
| Row    | Instance of that attribute | CA, 7, “Lightweight Wool Computer” |

Some of these columns contain special attributes, called keys.

### The keys to joins

Each table has a special column that contains distinct keys, known as entity keys or primary keys, that uniquely identifies each row in the table. These columns typically contain ID numbers, and they can be automatically generated by the database, or by an application (e.g., an employee ID).

A second type of key, known as a foreign key, stores a reference to the entity key of a row in another table. To join tables, you will use foreign and entity keys to tell the database how to combine data from multiple tables.

Let’s use the **Data Browser** to look at some keys with an example table in the Sample Database. From the top navigation bar in Answers, go to **Browse Data**, and click on the Sample Database. As shown in figure 6, you’ll see cards for each table. Hover over a card to see additional options, and click on the **book icon** that appears next to **Learn about this table**.

![Fig. 6. Go to Browse Data from the top navigation bar, select Sample Database, and click on the book icon to learn about this table.](https://www.metabase.com/learn/images/joins-in-metabase/learn-about-this-table.png)

This table reference page contains several tabs:

* Details
* Fields in this table
* Questions about this table
* X-ray this table

Select the **Fields in this table tab** to view the field name, field type, and data type for each field in the table, as shown in figure 7.

![Fig. 7. Select Fields in this table tab to view the field name, field type, and data type. The Orders table contains an entity key (ID) and two foreign keys, USER\_ID and PRODUCT\_ID.](https://www.metabase.com/learn/images/joins-in-metabase/fields-in-this-table.png)

The `Orders` table contains one entity key (a.k.a. primary key), `ID`, and two foreign keys: `USER_ID` and `PRODUCT_ID`:

* The foreign key `USER_ID` is associated with the entity key `ID` of the `People` table.
* The foreign key `PRODUCT_ID` is associated with the entity key `ID` of the `Products` table.

We can combine data from `Orders`, `People`, `Products` by joining on these keys. Joining simply directs Answers to line up the records from one table, and use the foreign key values in each row to combine data from the other table by finding the corresponding row (or rows) with the matching entity key value.

{% hint style="info" %}
Answers defaults to left joins.
{% endhint %}

### Multiple joins

With that context on tables, keys, and joins, let’s see if we can link the `Orders` table to the `Reviews` table. Stated generally: if our starting table (the left table) doesn’t contain a foreign key that references the table we want to join to, how would we go about joining them?

If we try to join the `Orders` table to the `Reviews` tables, Answers won’t know what to do (figure 8).

![Fig. 8. If there is no foreign key relationship, Answers won't know what to do, and it will prompt you to select a foreign key relationship. Which, in the case of the Orders and Reviews tables, does not exist.](https://www.metabase.com/learn/images/joins-in-metabase/orders-and-reviews.png)

The `Orders` table lacks a foreign key for the `Reviews` table, which is why Answers didn’t automatically connect the two tables.

Let’s turn to the data browser to find out which foreign keys the `Reviews` table includes (figure 9).

![Fig. 9. The only foreign key in the Reviews table is PRODUCT\_ID.](https://www.metabase.com/learn/images/joins-in-metabase/fields-in-reviews.png)

So, here’s our situation:

* The `Orders` table has foreign keys to the `Products` and `People` tables.
* The `Reviews` table has a foreign key to the `Products` table.

To connect the `Orders` table to the `Reviews` table, we’ll need to join them via the `Products` table. We’ll use a custom question to specify the joins. Here’s our notebook:

![Fig. 10. Joining the Orders table to the Reviews table via the Products table.](https://www.metabase.com/learn/images/joins-in-metabase/orders-products-reviews.png)

Note that Answers will show “Previous results” as the left table to join until you specify the keys. Once you tell Answers to join the previous results on `Products.ID = Reviews.Product_ID`, Answers will show the Products table left-joined to the reviews table in the Join data section (like in figure 11).

If you visualize the results, you’ll see a table of all of the columns from all three tables: `Orders`, `Products`, and `Reviews`. Note that because a single product can have multiple reviews, we’ll see multiple rows for the same product and order, one for each review.

#### Joins with multiple conditions

Now let’s try adding multiple conditions to a join. This could be useful if you want to cut down on duplicate rows or express things about your data that a single condition join can’t. Let’s say we wanted to see all the orders for products that were placed in the same month we added that product to our inventory.

Using the same example as above as a starting point, click the blue **+** button next to your first condition to add another in the same join. You’ll want to use an inner join here, as that will only return values that match in both tables.

As you can see below, we’ve joined the `Orders` and `Products` tables on two columns, Product ID and creation date, so now we can see all of the orders placed in the same month that the product ordered was added to our inventory.

Note that while the `CREATED_AT` fields include full date and time information, these results indicate where the _months_ match, rather than the exact timestamp.

![Fig. 11. Joining the Orders table to the Products table by linking the foreign key (PRODUCT\_ID) in the Orders table to the entity key (ID) in the Products table and by creation date.](https://www.metabase.com/learn/images/joins-in-metabase/multiple-join-conditions.png)

### Column selection

In addition to joining our tables, we can be selective about which columns are visible in our question’s results. In the **Notebook Editor**, you can select the columns Answers displays (figure 12).

![Fig. 12. Selecting columns in the Notebook Editor.](https://www.metabase.com/learn/images/joins-in-metabase/select-columns-notebook-editor.png)

![Fig. 13. Click the Settings button to add or remove columns from your question.](https://www.metabase.com/learn/images/joins-in-metabase/column-options.png)

You’ll notice that Answers helpfully makes columns from the `People` table available to add as well, so you now have the full Sample Database at your disposal.

Have fun joining tables in your own datasets, and remember: if you get stuck, be sure to consult the data browser to learn which tables have the foreign keys you need to join them.
