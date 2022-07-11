# Searching your tables and questions

Adding filters to your questions can make searching text in your questions a breeze. You can follow along with the examples in this article using the Sample Database included with each Answers installation.

### Search within a question

We’ll start by clicking the **Browse Data** button, selecting `Sample Database`, and opening the `Products` table.

To search within questions, we use a customizable filter. We’ll click the **Filter** button and select the column we want to filter by.

Let’s select `Title`. Then use the dropdown menu that currently says `Is`. A popup with the **text filter options** will appear to offer us:

* `Is`
* `Is not`
* `Contains`
* `Does not contain`
* `Is empty`
* `Not empty`
* `Starts with`
* `Ends with`

We’ll select `Starts with` and type “rustic” in the search box on the **Filter sidebar**. As we’re typing “rustic”, a dropdown will give us a preview of our search results. We’ll click **Add Filter** to add the filter to our question.

![Fig. 1. Creating a search widget by adding a filter to a question.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/create-search-filter.gif)

With the filter applied, our question will only list product titles that start with “rustic”.

#### Update your search filter

Now that we’ve added a filter, our search filter is at the top of our table. We can click on the filter description, which says `Title starts with rustic`.

![Fig. 2. Close up picture of the simple question search filter.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/search-widget.png)

In the popup, we can use the text box to replace the current search term, for example with “Marble.” We can also change how the widget filters the results by clicking the dropdown menu that currently says `Starts with`, for example by changing our **text filter options** to `contains`. In figure 3, we update our search widget from `Title starts with rustic` to `Title contains Marble`, and make the search case sensitive.

![Fig. 3. Using the search widget to change what we're searching for in the Title column of the Products table.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/updating-search-widget.gif)

#### Advanced search with Custom Expressions

Custom expressions are a way to make advanced queries with numbers or text. Let’s say we want to filter results by products that contain either “Rustic” or “Clock” in their title. Let’s create a custom expression by clicking the **Filter** button and selecting **Custom Expression** at the bottom of the sidebar. If we start typing, we’ll see suggestions pop up that we can use in our custom expression.

The **Custom Expression sidebar** contains tools to create advanced filters or add custom columns to a question. Let’s use out the “contains” function.

![Fig. 4. The Custom Expression sidebar is open and an expression beginning with \`contains()\` is visible.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/contains-cust-exp.png)

We’ll write our base query like so: (`contains([Title] , "Rustic")`).

* `[Title]` is the column to filter
* `"Rustic"` is the term to filter for

We’ll add a space and type `OR`, then we’ll write another text search function: `OR contains([Title] , "clock")`, and click **Done**.

![Fig. 5. Creating a Custom Expression that searches through the Title column of the Products table for the words 'Rustic' or 'Clock.'](https://www.metabase.com/learn/images/searching-your-tables-and-questions/create-custom-expression.gif)

### Searching on foreign keys

We can use foreign keys to search a table based on data from a connected table. To be able to search on foreign keys with strings as well as ID numbers, admins will need to change the data model settings to match figure 7. From the main Answers navigation bar, click on the **gears icon** to open up the **Admin Panel**, click on the **Data Model tab**, and select `Sample Database`. Next, click on the `Reviews` table and click on the **gears icon** next to the `Product_ID` field.

In our example, the `Product_ID` column is set up as a foreign key, with `search box` filter settings, and a display value of `Title`. This way when we filter on the `Product_ID` we can filter by title instead of ID number, and the table will display the title of each product instead of its ID number.

![Fig. 6. In the data model for the Product\_ID field in the Reviews table, the admin has saved the Filtering on this field settings as Search box and the Display values as Title.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/data-model-info.png)

With our metadata set, we’ll return to the home page, click the **Browse Data** button, select `Sample Database`, and open the `Reviews` table. We’ll add a search widget by clicking the **Filter** button and selecting `Product ID` in the **Filter sidebar**. In our new filter widget, we’ll type “Practical”, and select one of the values in the dropdown, like “Practical Bronze Computer”.

![Fig. 7. Creating a search widget for the Reviews table using the title of the product instead of the ID.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/foreign-key-searching.gif)

### Add a search widget to your SQL question

In a normal database, we’d need to craft a new SQL query for every search term, but Answers makes searching our database easy, even when using SQL queries. All we need to do is insert a variable in the `WHERE` clause of a query.

In the upper right of the main navigation bar, we’ll click on the **pencil icon** to write SQL.

```
SELECT *
FROM REVIEWS
[[WHERE UPPER(Body) LIKE UPPER(CONCAT('%', {{search_term}}, '%'))]]
```

As soon as we start typing within the curly brackets `{{}}`, the search bar will appear above the SQL input area Answers will kick out the variables sidebar.

![Fig. 8. The SQL Editor with the example query typed into the SQL text area.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/sql-q.png)

#### SQL question breakdown

In plain English, this SQL query says, “Show me reviews where the body of the review includes the search term, whether or not the two are capitalized in the same way.”

Let’s break down the elements in line three:

* `[[]]` The brackets make the filter optional. If no search term is supplied to the variable, the query will return unfiltered rows.
* `WHERE` filters the results.
* `UPPER()` capitalizes both the text being searched and the search term, so they will match even if there is a difference in case.
* `BODY` is the column to filter.
* `LIKE` searches for a pattern.
* `CONCAT()` (short for concatenate) combines strings.
* `'%'` A percent symbol is a wild card. If positioned before the search term additional text can come before the term. Wrapping the search term with `%` searches for the term even if it’s surrounded by text.
* `{{}}` The double curly brackets wrap a variable.
* `search_term` The variable that the filter widget will supply with text to search for.

#### Variables sidebar

In figure 9 below, the variable type has selected `Text`, but there are several other options as well:

* Number
* Date
* Field filter

Filters can be both complex and insanely useful; check out filter widgets and field filters to get a sense for the many ways they can provide valuable insights.

Below `Variable type` is `Filter widget label`, which automatically uses the text we type between the curly brackets as our label.

For additional examples and advice on typing SQL queries that support a search widget, click the **Help tab** towards the top of the sidebar.

![Fig. 9. The variable type options, creating a new placeholder for the widget label, and scrolling through the 'Help' tab.](https://www.metabase.com/learn/images/searching-your-tables-and-questions/variables-sidebar.gif)
