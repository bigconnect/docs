# MySQL / MariaDB

Using a MySQL Database Server as a datasource for Mware Application Development Framework&#x20;

> ### 👍SQL Databases
>
> Mware Application Development Framework is designed to integrate with any core SQL database.
>
> Read more about SQL Datasources.



***

## Tutorial: Building a HR portal

In this tutorial we will look at building a Mware Application Development Framework app from scratch using a MySQL employees sample database.

The full dataset and installation instructions can be found on [Github: datacharmer/test\_db](https://github.com/datacharmer/test\_db).\


### Adding the datasource

When you have a MySQL or MariaDB Server instance running, with the sample database installed, we can begin pulling that data into Mware Application Development Framework.

First create an app - no sample data needed.

<figure><img src="https://files.readme.io/68e790c-Screenshot_2023-01-12_at_11.24.38.png" alt=""><figcaption></figcaption></figure>

Once the new app has been created, click `Add source` and select `MySQL`, even if you are using MariaDB, or any other storage engine.

<figure><img src="https://files.readme.io/d3074a0-Screenshot_2023-01-12_at_14.36.26.png" alt=""><figcaption></figcaption></figure>

Click `Save and fetch tables` to finish.

\
SSL

You have the option to provide an SSL which prompts you with two additional fields `key` and `value`. The ssl `key` in the connection options takes a string. There are a lot of different options that can be used here, the 3 main ones are listed below.

* `ca:` The certificate(s) to trust instead of the ones Node.js is configured to trust. This refers to the value of the certificate(s) and not a filename of the certificate(s).
* `cert:` The client certificate to use in the SSL handshake.
* `ciphers:` The ciphers to use to use in the SSL handshake instead of the default ones for Node.js.

Additional information on other keys or more information on the 3 listed above can be found [here](https://github.com/mysqljs/mysql#ssl-options).

The `value` is the actual content of the SSL certificate files, and the paths provided in the MySQL configuration are pointers to these files. `/path/to/private-key.pem`

### Define existing relationships

Now that we have pulled in our tables, we need to inform Mware Application Development Framework how the tables relate to each other.

* One Employee -> Many Titles (Historical)
* One Employee -> Many Salaries (Historical)
* Many Departments -> Many Employees / Managers

To define these relationships, click on the MySQL datasource in the datasource list and scroll down to and click `Define relationship`.

<figure><img src="https://files.readme.io/cb4d4db-Screenshot_2023-01-12_at_14.47.32.png" alt=""><figcaption></figcaption></figure>

For the one employee to many titles and salaries, define a relationship like so:

<figure><img src="https://files.readme.io/c2898d9-Screenshot_2023-01-13_at_11.01.12.png" alt="One employee -> Many titles (matching on emp_no)"><figcaption><p>One employee -> Many titles (matching on emp_no)</p></figcaption></figure>

For the relationship between **departments** and **employees**, there are joining tables, e.g. **dept\_emp**. Usually we would setup a [many-to-many relationship](https://docs.budibase.com/docs/sql-datasource#many-to-many) using the joining table as the _Through_ table. Unfortunately in this case, the joining tables do not meet the requirements outlined by Mware Application Development Framework :

<figure><img src="https://files.readme.io/e7fbe2d-Screenshot_2023-01-24_at_14.32.32.png" alt="dept_emp is not a suitable Budibase joining table"><figcaption><p>dept_emp is not a suitable joining table</p></figcaption></figure>

<figure><img src="https://files.readme.io/25b5618-Screenshot_2023-01-24_at_14.33.20.png" alt="from_date and to_date are invalid" width="360"><figcaption><p>from_date and to_date are invalid</p></figcaption></figure>

Luckily, this is an easy fix. Within MySQL Workbench, or through an alter SQL statement, make the **from\_date** and **to\_date** fields _nullable_ for both joining tables.

SQL

```sql
ALTER TABLE `employees`.`dept_emp` 
CHANGE COLUMN `from_date` `from_date` DATE NULL ,
CHANGE COLUMN `to_date` `to_date` DATE NULL ;
```

<figure><img src="https://files.readme.io/3a97ed0-Screenshot_2023-01-24_at_14.34.37.png" alt="dept_emp is now a suitable joining table" width="360"><figcaption><p>dept_emp is now a suitable joining table</p></figcaption></figure>

Adding the Many -> Many relationships for **dept\_emp** should now work.

> ### 📘
>
> Make sure to re-fetch the tables to pull down the new schema information!

<figure><img src="https://files.readme.io/dc6c57f-Screenshot_2023-01-24_at_14.36.34.png" alt="Many Departments -> Many Employees"><figcaption><p>Many Departments -> Many Employees</p></figcaption></figure>

An additional Many -> Many relationship cannot be added for two tables that already have a Many -> Many relationship established, but for this tutorial we do not need the **dept\_manager** table.

Because the Many -> Many relationship between **departments** and **employees** will pull down a huge amount of information, we will also create a Custom SQL query called _Get Departments Only_ to simply get the departments without any employee data when needed.

<figure><img src="https://files.readme.io/ef5e5a4-Screenshot_2023-01-24_at_15.19.04.png" alt="Simple select statement with no relationship data"><figcaption><p>Simple select statement with no relationship data</p></figcaption></figure>

In the end your relationships table should look something like so:

<figure><img src="https://files.readme.io/a1cedaf-Screenshot_2023-01-24_at_14.37.53.png" alt=""><figcaption></figcaption></figure>

By default the display name for the relationships will be the ID, but this isn't very readable:

<figure><img src="https://files.readme.io/1dc770b-Screenshot_2023-01-16_at_11.14.36.png" alt=""><figcaption></figcaption></figure>

To improve upon this, we can select a display column for each table. This will be the column that appears in the relationship pills. As an example, for the **employees** table, edit the **first\_name** column and toggle `Use as table display column` on.

<figure><img src="https://files.readme.io/c961295-Screenshot_2023-01-16_at_09.06.08.png" alt="Setting the first_name to be the display column"><figcaption><p>Setting the first_name to be the display column</p></figcaption></figure>

The employee relationship in the related tables, such as **titles**, is now more human readable. You can also use Formula columns to combine the first and last names of the employee into a single display column.

Create a new formula column in the employees table called 'full\_name' with the following binding expression: `{{ first_name }} {{ last_name }}`. Set this column as the table display column.

![](https://files.readme.io/9e0a2e4-Screenshot\_2023-01-16\_at\_11.15.34.png)

\


### Org chart screen

Now that we have the data pulled into Mware Application Development Framework, let's create a screen that allows HR to see the various departments and the org structure for each.

#### Step 1 - Adding the row explorer

On the home screen, click `Add component` and select the _Row Explorer Block_. For the table select **departments**, and remove the height - we will display all nine departments at once without the need for scroll.

Next in the _Cards_ section, set the _Title_ to match the **dept\_name**, and remove the _Subtitle_ and _Description_.

<figure><img src="https://files.readme.io/0a23cb5-Screenshot_2023-01-18_at_09.46.13.png" alt="Selecting the department name as the card title"><figcaption><p>Selecting the department name as the card title</p></figcaption></figure>

<figure><img src="https://files.readme.io/0d521d6-Screenshot_2023-01-18_at_09.54.47.png" alt="List of clickable department names"><figcaption><p>List of clickable department names</p></figcaption></figure>

**(Optional) Adding icons for each department**

To add a little bit of flavour to the row explorer, we can provide an _Image URL_ for each department. One way to do this would be to host the images in a public repository and name the files to match the department names. Another approach is to create a column in your table to store a URL icon path, which is what we'll do in this example.

1. Within Mware Application Development Framework , click on the **departments** table, and click `Create column`.
2. Add a [Text](https://docs.budibase.com/docs/text) column called **icon\_url**.
3. For each department row, add a URL path to an appropriate icon. I have found [svgrepo.com](https://www.svgrepo.com/) to be a good resource for free SVG icons. For your convenience, I have provided SQL update statements:

MySQL

```sql
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/428772/marketing-market-social.svg' WHERE dept_no = 'd001';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/381027/finance-bank-piggy-business-money.svg' WHERE dept_no = 'd002';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/218257/human-resources-search.svg' WHERE dept_no = 'd003';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/430190/production-industrial-automation.svg' WHERE dept_no = 'd004';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/428736/development-cognitive-progress.svg' WHERE dept_no = 'd005';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/422994/quality-premium-certificate.svg' WHERE dept_no = 'd006';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/430192/sales-shop-analytics.svg' WHERE dept_no = 'd007';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/119008/research.svg' WHERE dept_no = 'd008';
UPDATE departments SET icon_url = 'https://www.svgrepo.com/show/429955/customer-service-hotel.svg' WHERE dept_no = 'd009';
```

4. In the Design section, click on the Row Explorer block and set the Image URL to the **icon\_url** field.

<figure><img src="https://files.readme.io/8704746-Screenshot_2023-01-18_at_13.41.43.png" alt="Selecting an image url"><figcaption><p>Selecting an image url</p></figcaption></figure>

5. The image icons are not in the correct aspect ratio or a consistent size, but we can use the following [Global styling](https://docs.budibase.com/docs/custom-css#global-styling) to format the background image as needed:

HTML

```html
<style>
.spectrum-Card-coverPhoto {
  background-size: 64px 64px;
  background-repeat: no-repeat;
}
</style>
```

6. We also do not need the search, so we should Eject the block and delete the form. Your builder should appear like so:

<figure><img src="https://files.readme.io/df7f486-Screenshot_2023-01-18_at_14.00.54.png" alt=""><figcaption></figcaption></figure>

#### Step 2 - Adding the manager and employee cards

After ejecting the row explorer block, we need to change the Data provider source to the _Get Departments Only_ as we do not want the relational employee data in this simple list of departments.

In addition, the auto-generated click action of the `Spectrumcard` will need to be updated. Click `Define actions` and change the state value to the _dept\_no_ from the query:

<figure><img src="https://files.readme.io/cb0670d-Screenshot_2023-01-24_at_15.23.23.png" alt=""><figcaption></figcaption></figure>

At the moment upon previewing the app, a basic form is shown on click of each department row. Instead we want to display a card for the department managerial team and their employees.

First delete the Form block and button components from the container. Next we want to make sure the clicked department name is available in the explorer panel. To do this, we will push the clicked row department name into App state.

Click on the `Spectrumcard` component and within the Settings Panel click `Define actions`. Then click `Add Action` -> `Update State` and set a variable called _DepartmentName_ as follows:

<figure><img src="https://files.readme.io/f44299b-Screenshot_2023-01-18_at_14.12.22.png" alt=""><figcaption></figcaption></figure>

Be sure to click the `Save` button!

Whilst we could use multiple nested repeaters to pull in all of the department detail we want to display, it is much more efficient to use a Custom SQL query. Use the following SQL:

SQL

```sql
SELECT * FROM departments d
JOIN dept_manager dm on d.dept_no = dm.dept_no
JOIN employees e on dm.emp_no = e.emp_no
JOIN titles t on t.emp_no = e.emp_no
WHERE d.dept_name = {{ dept_name }} AND (e.emp_no, t.from_date) in (select t.emp_no, max(t.from_date) from titles t group by emp_no)
```

Also add a binding for the **dept\_name**, and give the query the name _Get Managers by Department Name_. Make sure to save!

<figure><img src="https://files.readme.io/25ac3f3-Screenshot_2023-01-24_at_14.23.08.png" alt=""><figcaption></figcaption></figure>

Back to the Design section, add a Cards Block for the managers using the SQL query datasource just added, passing in the binding: `{{ State.DepartmentName }}`. This will replace the form block we removed.

<figure><img src="https://files.readme.io/6bb10dd-Screenshot_2023-01-24_at_14.25.46.png" alt=""><figcaption></figcaption></figure>

Within the _Cards_ section set the _Title_ to `{{ Managers.Get Managers by Department Name.first_name }} {{ Managers.Get Managers by Department Name.last_name }}`.

For the _Description_, we want to display the managers current (most recent) job title: `{{ Managers.Get Managers by Department Name.title }}`

With the card block setup, it is also worth adding a headline component to indicate which department has been selected. Above the cards block, add the headline component with the title `{{ State.DepartmentName }}`. Add some styling such as margin bottom and _Extra Large_ size.

A preview of the app should show the following so far:

<figure><img src="https://files.readme.io/e3155f6-tabs_bug.gif" alt=""><figcaption></figcaption></figure>

\


### Employees screen

As this data set has thousands of employees per department, cards are not an ideal way to display the data. Instead we can display this data in a paginated Table block filtered on an encoded department ID passed via URL variables.

To get started, click `Add screen` and select `Autogenerated screens`. Tick the **employees** table and confirm.

Next we want to be able to pass through the **emp\_no** and **dept\_name** from the home screen. To support this, configure the screen route as follows:

<figure><img src="https://files.readme.io/a31b79b-Screenshot_2023-01-18_at_17.52.18.png" alt="Employees screen with two variables"><figcaption><p>Employees screen with two variables</p></figcaption></figure>

Use the name URL variable in the _Title_ of the table block.

<figure><img src="https://files.readme.io/7ce80ef-Screenshot_2023-01-18_at_17.55.05.png" alt="Access the department name from the URL"><figcaption><p>Access the department name from the URL</p></figcaption></figure>

With so many records, search will be very important. Under _Search Fields_ select some searchable fields.

<figure><img src="https://files.readme.io/230109c-Screenshot_2023-01-19_at_10.07.57.png" alt=""><figcaption></figcaption></figure>

Now let's make use of the `{{ URL.id }}` binding to filter our employees by department. `Define filters` of the table block. The **dept\_no** will be available via the **dept\_emp** relationship, and we can match that against the ID in the URL as follows:

<figure><img src="https://files.readme.io/c494ff9-Screenshot_2023-01-24_at_15.16.04.png" alt=""><figcaption></figcaption></figure>

Also set the _Scroll Limit_ of the table block to 50.

We will also want to `Configure columns`. Click on `Add all columns` and then press the `x` beside the **departments** column. We don't want to display the relationship because it will be the same for all rows.

#### Linking from the home screen

The employee screen is ready to go, but we still need to link it to the home screen.

Add a [Link](https://docs.budibase.com/docs/link) component underneath the _Managers_ card block. Add some margin top for some space. Give the URL the following binding: `/employees/{{ State.cAF3fs8fNJ }}/{{ State.DepartmentName }}`. The ID state will be different in your case.

<figure><img src="https://files.readme.io/6e346f7-linking.gif" alt=""><figcaption></figcaption></figure>

\


### &#x20;Salary history

The **salaries** table holds a historic record of each employees salary at a particular time. This time series can be displayed in a chart.

Eject the Table block in the _employees_ screen.

Within the newly exposed _Details side panel_, select the _Details form block_ and deselect **departments** and **salaries** from the _Fields_ dropdown. We already know the department from the table heading, and we will be displaying the salaries in a chart instead of the [Multi-select picker](https://docs.budibase.com/docs/multi-select-picker).

<figure><img src="https://files.readme.io/63f7e05-Screenshot_2023-02-07_at_14.07.32.png" alt=""><figcaption></figcaption></figure>

We also want to make sure that we pass the correct ID through to state when clicking on a row. Click on the Table underneath the Data provider, and click `Define actions` under the _On Row Click_ section of the Settings Panel.

By default you will see the value binding of `{{ Clicked row._id }}` for the **Update State** action. Replace this with `{{ Clicked row.emp_no }}` as we want to use the actual primary key and not the URL-safe encoded link.

<figure><img src="https://files.readme.io/a71a0f8-Screenshot_2023-03-14_at_08.34.49.png" alt=""><figcaption></figcaption></figure>

#### Adding the chart

Under the _Details form block_ add a Chart block. Choose a _Chart Type_ of 'Line' and the **salaries** table as the datasource. Set the title to 'Salary History'.

<figure><img src="https://files.readme.io/6cf80fd-Screenshot_2023-02-07_at_14.29.21.png" alt="Salaries Line Chart block"><figcaption><p>Salaries Line Chart block</p></figcaption></figure>

Next `Define filters` to add a filter on the employee number as follows:

![](https://files.readme.io/cd47442-Screenshot\_2023-03-14\_at\_08.36.10.png)

> ### 📘
>
> The State ID will only be set when a row is clicked, thus the chart display in the builder may display differently than the previewed/published app.

Finally select the **from\_date** column as the _Label Col._ and the **salary** column as the _Data Col._. A _Stepline_ curve is the ideal way to display this data.

| Config                                                                        | Result                                                                        |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| ![](https://files.readme.io/a40684d-Screenshot\_2023-02-07\_at\_14.42.07.png) | ![](https://files.readme.io/1c35f3b-Screenshot\_2023-02-07\_at\_14.46.03.png) |
