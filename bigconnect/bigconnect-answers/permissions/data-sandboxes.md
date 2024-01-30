# Data sandboxes

Data sandboxes let you give granular permissions to rows and columns for different groups of people.

Say you have an Accounts table with information about your customers. If you want to reuse one dashboard for different teams (say that the Customer Success team should see account emails, but everyone else should not), you can use a sandbox to automatically filter that dashboard for each team.

Or, if your customers want to log into your Answers themselves for self-service analytics, you can use a sandbox to make sure each customer can only view the rows that match their customer ID.

Basically, sandboxes let people use data in Answers without ever seeing any sensitive or irrelevant results that you don’t want them to see.

## **How sandboxes work**

Data sandboxes work by displaying a filtered version of a table, instead of the original table, to a specific group.

You can think of a data sandbox as a bundle of permissions that includes:

* The filtered version of a table that will replace your original table, everywhere that the original table is used in Answers.
* &#x20;The group of people who should see the filtered version of the table.

You can define up to one data sandbox for each table/group combo in your Answers. That means you can display different versions of a table for different groups, such as “Answers Accounts for Sales” to your salespeople, and “Answers Accounts for Managers” for sales managers.

## Types of data sandboxes

Data sandboxes show specific data to each person based on their user attributes. You can:

* Restrict **rows** for specific people with a basic sandbox.
* Restrict **columns** (as well as rows) for specific people with a custom sandbox (also known as an advanced sandbox).

<table><thead><tr><th width="256.3333333333333"></th><th>Basic sandbox (filter by a column in the table)</th><th>Custom sandbox (use a saved SQL question)</th></tr></thead><tbody><tr><td>Restrict rows by filtering on a single column</td><td>✅</td><td>✅</td></tr><tr><td>Restrict rows by filtering on multiple columns</td><td>❌</td><td>✅</td></tr><tr><td>Restrict columns</td><td>❌</td><td>✅</td></tr><tr><td>Edit columns</td><td>❌</td><td>✅</td></tr></tbody></table>

## Basic data sandboxes: filter by a column in the table

To **restrict rows**, use a basic sandbox. A basic sandbox displays a filtered version of a table instead of the original table to a group. The filter works by setting a column in the table to a specific user attribute value.

For example, you can create a basic sandbox to filter the Accounts table for a group so that:

* A person with the user attribute value “Basic” will see rows where `Plan = "Basic"` (rows where the Plan column matches the value “Basic”).
* A person with the user attribute value “Premium” will see the rows where `Plan = "Premium"` (rows where the Plan column matches the value “Premium”).

## Custom data sandboxes: use a saved question to create a custom view of a table

To **restrict columns** as well as rows, use a custom sandbox (also known as an advanced sandbox). A custom sandbox displays the results of a saved SQL question in place of your original table.

For example, say your original Accounts table includes the columns: ID, Email, Plan, and Created At. If you want to hide the Email column, you can create a “Sandboxed Accounts” SQL question with the columns: ID, Plan, and Created At.

A custom sandbox will display the “Sandboxed Accounts” question result instead of the original Accounts table, to a specific group, everywhere that Accounts is used in Answers.

You can also use a custom sandbox to:

* Display an edited column instead of hiding the column.
* Pass a user attribute to a SQL parameter.
* Pass a user attribute to a Markdown parameter.

## Limitations

Things that don’t play well in a sandbox.

### Groups with native query permissions (access to the SQL editor) cannot be sandboxed

People with SQL editor access to a database will always be able to query any original table from that database. This is because Answers cannot parse a SQL query — if Answers can’t find a table, then Answers can’t sandbox the table.

Native query permissions will get automatically **disabled** for any groups that you add to a data sandbox. If you need to prevent people with SQL access from querying specific tables in a database, you can edit your database users, roles, and privileges.

### SQL questions cannot be sandboxed

Since Answers can’t parse SQL queries, the results of SQL questions will always use original tables, not sandboxed tables.

Use collection permissions to prevent sandboxed groups from viewing saved SQL questions with restricted data.

### Non-SQL databases cannot be sandboxed

Data sandbox permissions are unavailable for non-SQL databases such as Apache Druid or MongoDB.

## Prerequisites for basic sandboxes

* A group of people to be added to the basic sandbox.
* User attributes for each person in the group.

A basic sandbox displays a filtered table in place of an original table, to a specific group. The filter on the table will change for each person in the group, depending on the value of a person’s user attribute.

For example, you can set up a basic sandbox so that:

* Someone with the user attribute value “Basic” will see a version of the Accounts table with a filter for `Plan = "Basic"` (rows where the Plan column matches the value “Basic”).
* Someone with a “Premium” attribute will see a version of the Accounts table with the filter `Plan = "Premium"`.

## Choosing user attributes for data sandboxes

**User attributes are required for basic sandboxes** and optional for custom sandboxes. When adding a new user attribute, you’ll set up a key-value pair for each person.

The user attribute key is used to look up the user attribute value for a specific person. User attribute keys can be mapped to parameters in Answers.

The **user attribute value** must be an exact, case-sensitive match for the filter value of a sandboxed table. For example, if you’re creating a basic sandbox on the Accounts table with the filter `Plan = "Basic"`, make sure that you enter “Basic” as the user attribute value. If you set the user attribute value to lowercase “basic” (a value which doesn’t exist in the Plan column of the Accounts table), the sandboxed person will get an empty result instead of the sandboxed table.

Examples of user attributes in play:

* Restricting rows in basic sandboxes
* Restricting rows in custom sandboxes
* Displaying custom text in Markdown dashboard cards

## Creating a basic sandbox

1. Make sure to do the prerequisites for basic sandboxes first.
2. Go to **Admin settings** > **Permissions**.
3. Select the database and table that you want to sandbox.
4. Find the group that you want to put in the sandbox.
5. Click on the dropdown under **Data access** for that group.
6. Select “Sandboxed”.
7. Click the dropdown under **Column** and enter the column to filter the table on, such as “Plan”.
8. Click the dropdown under **User attribute** and enter the user attribute **key**, such as “User’s Plan”.

> If you have saved SQL questions that use sandboxed data, make sure to move all of those questions to admin-only collections. For more info, see Permissions conflicts: saved SQL questions.

## Prerequisites for custom sandboxes

* A group of people to be added to the advanced data sandbox.
* An admin-only collection, with collection permissions set to **No access** for all groups except Administrators.
* A saved SQL question with the rows and columns to be displayed to the people in the custom sandbox, stored in the admin-only collection.
* Optional: if you want to restrict **rows** in a custom sandbox, set up user attributes for each of the people in the group.

### Creating a SQL question for Answers to display in an custom sandbox

In an advanced data sandbox, Answers will display a saved question in place of an original table to a particular group.

**Use a SQL question** to define the exact rows and columns to be included in the sandbox. If you use a query builder (GUI) question, you might accidentally expose extra data, since GUI questions can include data from other saved questions or models.

> Make sure to save the SQL question in an admin-only collection (collection permissions set to **No access** for all groups except Administrators). For more info, see Permissions conflicts: saved SQL questions.

### Displaying edited columns in an custom sandbox

Aside from excluding rows and columns from an custom sandbox, you can also **display edited columns** (without changing the columns in your database).

For example, you can create a “Sandboxed Accounts” SQL question that truncates the Email column to display usernames instead of complete email addresses.

If you edit a column, the schema of the saved SQL question (the question you want to display in the sandbox) must match the schema of the original table. That means the “Sandboxed Accounts” SQL question must return the same number of columns and corresponding data types as the original Accounts table.

You cannot add a column to a custom sandbox.

## Creating a custom sandbox

1. Make sure to do the prerequisites for custom sandboxes first.
2. Go to **Admin settings** > **Permissions**.
3. Select the database and table that you want to sandbox.
4. Find the group that you want to put in the sandbox.
5. Click on the dropdown under **Data access** for that group.
6. Select “Sandboxed”.
7. Select “Use a saved question to create a custom view for this table”.
8. Select your saved question. The question should be written in SQL. If the question contains parameters, those parameters must be required (they cannot be optional).
9. Optional: restrict rows based on people’s user attributes.

> If you have saved SQL questions that use sandboxed data, make sure to move all of those questions to admin-only collections. For more info, see Permissions conflicts: saved SQL questions.

## Restricting rows in an custom sandbox with user attributes

You can set up an custom sandbox to restrict different rows for each person depending on their user attributes. For example, you can display the “Sandboxed Accounts” question with the filter `Plan = "Basic"` for one group, and the filter `Plan = "Premium"` for another group.

1. Make sure you’ve done all the prerequisites for custom sandboxes.
2. Go to the saved SQL question that will be displayed to the people in the custom sandbox.
3. Add a parameterized `WHERE` clause to your SQL query, such as `WHERE plan = {{ plan_variable }}` .
4. Save the SQL question.
5. Go to **Admin settings** > **Permissions**.
6. Find the group and table for your custom sandbox.
7. Open the dropdown under **Data access**.
8. Click **Edit sandboxed access**.
9. Scroll down and set **Parameter or variable** to the name of the parameter in your saved SQL question (such as “Plan Variable”).
10. Set the **User attribute** to a user attribute key (such as the key “User’s Plan”, _not_ the value “Basic”).
11. Click **Save**.

### How row restriction works in an custom sandbox

How user attributes, SQL parameters, and custom sandboxes work together to display different rows to different people.

A standard `WHERE` clause filters a table by setting a column to a fixed value:

```sql
WHERE column_name = column_value
```

In step 2 of the row restriction setup above, you’ll add a SQL variable so that the `WHERE` clause will accept a dynamic value. The SQL variable type must be text, number, or date:

```sql
WHERE plan = {{ plan_variable }}    
```

In steps 9-10 of the row restriction setup above, you’re telling Answers to map the SQL variable `plan_variable` to a **user attribute key** (such as “User’s Plan”). Answers will use the key to look up the specific **user attribute value** (such as “Basic”) associated with a person’s Answers account. When that person logs into Answers and uses the sandboxed table, they’ll see the query result that is filtered on:

```
WHERE plan = "Basic"
```

Note that the parameters must be required for SQL questions used to create a custom sandbox. E.g., you cannot use an optional parameter; the following won’t work:

```sql
[[WHERE plan = {{ plan_variable }}]]  
```

## Preventing data sandbox permissions conflicts

Some Answers permissions can conflict with data sandboxes to give more permissive or more restrictive data access than you intended.

Say you have an custom sandbox that hides the Email column from the Accounts table (for a particular group).

The Email column may get exposed to a sandboxed person if:

* The sandboxed person belongs to multiple data sandboxes.
* A non-sandboxed person shares the Email column from:
  * A saved SQL question
  * A public link
  * An alert, or dashboard subscription

### Multiple data sandbox permissions

Multiple data sandboxes on the same table can create a permissions conflict. You can add a person to a maximum of one data sandbox per table (via the person’s group).

For example, if you have:

* One sandbox for the group “Basic Accounts” that filters the Accounts table on `Plan = "Basic"`.
* Another sandbox for the group “Converted Accounts” that filters the Accounts table on `Trial Converted = true`.

If you put Vincent Accountman in both groups, he’ll be in conflicting sandboxes for the Accounts table, and get an error message whenever he tries to use Accounts in Answers.

To resolve data sandbox permissions conflicts:

* Remove the person from all but one of the groups.
* Remove all but one of the data sandboxes for that table (change the table’s data access to **No self-service**).

### Saved SQL questions

Data sandbox permissions don’t apply to the results of SQL questions. That is, saved SQL questions will always display results from the original table rather than the sandboxed table.

Say that you have an custom sandbox which hides the Email column from the Accounts table. If a non-sandboxed person creates a SQL question that includes the Email column, **anyone with collection permissions to view that SQL question** will be able to:

* See the Email column in the SQL question results.
* Use the SQL question to start a new question that includes the Email column.

To prevent the Email column from being exposed via a SQL question:

* Put any SQL questions that include the Email column in a separate collection.
* Set the collection permissions to **No access** for sandboxed groups that should not see the Email column.

Collection permissions must be used to prevent sandboxed groups from viewing saved SQL questions that reference sandboxed tables. That’s why, when you create an custom sandbox, you have to put the saved SQL question (the one you want to display in the sandbox) in an admin-only collection.

### Public sharing

Data sandbox permissions don’t apply to public questions or public dashboards. If a non-sandboxed person creates a public link using an original table, the original table will be displayed to anyone who has the public link URL.

To prevent this from happening, you’ll have to disable public sharing for your Answers instance.

Answers can only create a data sandbox using the group membership or user attributes of people who are logged in. Since public links don’t require logins, Answers won’t have enough info to create the sandbox.
