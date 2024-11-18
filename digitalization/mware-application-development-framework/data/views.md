# Views

Learn more about views within Mware Application Development Framework

Views are virtual tables that present data from a table with a preset filter or grouping.

When you create a View within Mware Application Development Framework, it presents you with all the data from its linked table. This provides you with a canvas to manipulate your data without changing the data within the original table. This is crucial, as it protects your original data. It also allows you to create:

1. Filters
2. Sort
3. Hide columns

When designing your UI, you can use a View with auto-generated screens, as well as for custom UI development. Say you have a table of Sales employees, and their total sales, you can create a view which gives you the total sales of all Sales employees. You can then reference the data from this View in the _Design_ section of Mware Application Development Framework.

### How to create a new view

To create a new view, click the `Create view` button, and give it a name.

### Filters

One of the core features of views is the ability to filter rows according to particular criteria. When a Row is filtered, it is not deleted—it's just hidden from the particular view you're using and remains within the table (views are virtual tables!).

Try adding filters when:

* You need to focus on rows that meet certain conditions (e.g. Classes between May and June, and taught by Albert Einstein).
* You need to display certain information, to certain people (e.g Each salesperson can only view their own commission).

#### How to create a new filter

To create a new filter:

1. Click the `Filter` button
2. Click `Add Filter`
3. Select the Column you would like to filter
4. Select a filter function
5. Type your query
6. Click `Save`

You can also save a filtered table as a new view:

<figure><img src="https://files.readme.io/787ea63058e416325529f8bd200f09ac9e6b4b4ef9632de18e7a4ac16f99f790-Screenshot_2024-09-20_at_14.56.27.png" alt=""><figcaption></figcaption></figure>
