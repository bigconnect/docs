# Database services

By using _database services_, a [PE feature](https://www.orbeon.com/pricing), you can use data stored in any table of a relational database, for instance to dynamically populate a dropdown, or to pre-populate fields based on a value entered by users.

### Format of database query responses <a href="#format-of-database-query-responses" id="format-of-database-query-responses"></a>

A SQL query, when successful, returns an XML document with the following format:

* A root element which is always `<response>`.
* It contains one nested `<row>` element per row.
* Each such element in turn contains one element per selected column, derived from the column name:
  * The name is converted to lowercase.
  * Underscores (`_`) replaced by dashes (`-`).

For example, values for the `dept_no` column end up in `<dept-no>`.

See below for concrete examples.

### Populating a dropdown <a href="#populating-a-dropdown" id="populating-a-dropdown"></a>

In what follows, we'll see how you can populate a _Department_ dropdown in your form using values stored in an `departments` table of your relational database.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzNqdB3FwrZMusPE%252Fdatabase-services-dropdown.png%3Fgeneration=1528743351489883%26alt=media\&width=768\&dpr=4\&quality=100\&sign=1d5a8fa32efc9f0ecf66860a02af7936a7af29e0c547f3041d17d294baa01014)

Dropdown

#### 1. Connect with the database <a href="#id-1.-connect-with-the-database" id="id-1.-connect-with-the-database"></a>

You start, in Form Builder, by creating a new database service, clicking _Add_ in the left sidebar under _Database Services_. This opens the _Database Service Editor_.

Under _Datasource_ you type the name of a datasource you setup in your application server. This is the the JNDI name of the datasource, without the `jdbc/` part. If you type `employees`, Orbeon Forms will look for `java:comp/env/jdbc/employees`.

If you're using Tomcat, the simplest way of setting up a datasource is to edit Tomcat's `server.xml`, there add a `<Context>` for Orbeon Forms if you don't have one already, and inside it add a `<Resource>` pointing to your database. On Tomcat, you also need to put the database JDBC driver in Tomcat's `lib` directory.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzNtDt\_aoOt2cFza%252Fdatabase-services-connect-db.png%3Fgeneration=1528743349725789%26alt=media\&width=768\&dpr=4\&quality=100\&sign=ab1e6362843729035ae7e24b4f123066a9173e73ea98a1a0f41995c31ab8ea25)

Connect to the database

#### 2. Write the SQL query <a href="#id-2.-write-the-sql-query" id="id-2.-write-the-sql-query"></a>

Still in the _Database Service Editor_, you write the SQL query to run in the database. When that query runs, Orbeon Forms creates an XML document with the returned data, and you'll be referring to parts of that document when linking the database service to a specific dropdown.

The user interface in Form Builder looks like this:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-0942d6608f9ee3f2aa38801dec271f24594f0ecd%252Fdatabase-services-example.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a1f0d09dbdeeb74cc4d003045054d6ee88b638eaca4df8ea151e027d3f5163ae)

Database Service Editor

Example response:

Copy

```
<response>
    <row>
        <dept-no>d009</dept-no>
        <dept-name>Customer Service</dept-name>
    </row>
    <row>
        <dept-no>d005</dept-no>
        <dept-name>Development</dept-name>
    </row>
</response>
```

This diagram summarizes the mapping:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzNvIKleyzuJTLEg%252Fdatabase-services-run-query.png%3Fgeneration=1528743337270607%26alt=media\&width=768\&dpr=4\&quality=100\&sign=36783364dae8649a1ed20bd6e6f7b3c75c0b745498e080883bdb91a6236922bd)

Mapping of columns to XML elements

#### 3. Link the dropdown <a href="#id-3.-link-the-dropdown" id="id-3.-link-the-dropdown"></a>

To "link" the service to the dropdown, you create a new action, set it to run on _Form Load_, call the service you earlier named `list-departments`. Here is how the user interface looks like:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-c37798c604d0d52115b4692f8564e46a0fb83f0f%252Faction-itemset-example-general.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=a2ce8af8b24c0696ec410f7e27ea2efdefbe17b377e255b7d293ba837c5d98e5)

Action general settings

With the result of the service you want to set the list of possible values of an a _Department_ dropdown you have in the form. This is where you extract data from the XML document seen earlier, and you do this with 3 XPath expressions. The first points to the "rows", and will almost always be `/response/row`. Next, you need to tell Orbeon Forms where it can find, inside the `<row>`, the label (the text shown to users) and the value (what is stored in data when users make a selection).

Here is how the user interface looks like:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-26010143703fc9008759d4afeca2e39b3557871b%252Faction-itemset-example-response.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=ef1642b859d6cd65da4b27274f9e91de1142e1be384017988e20d31d4e1d0718)

Action response

This diagram summarizes the mapping:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzNx3LSR0Xowy\_9-%252Fdatabase-services-link-to-dropdown.png%3Fgeneration=1528743350132293%26alt=media\&width=768\&dpr=4\&quality=100\&sign=95ad1d83d7906490ad813c8631de78dcc5cd3bd56c40095159f9d105b232f010)

Link to the dropdown

#### 4. Select a value <a href="#id-4.-select-a-value" id="id-4.-select-a-value"></a>

Finally, when your form runs and users make a selection in the dropdown, the _value_, here the content of the `<dept-no>` element, is used to populate the element corresponding to the field in the form data.

### Populating fields using another field value <a href="#populating-fields-using-another-field-value" id="populating-fields-using-another-field-value"></a>

Say that when users enter a value in _employee number_, you want to lookup the corresponding employee in your database and populate other fields, _First name_ and _Last name_, based on the information you find about that employee. We've already know how to establish a [connection with the database](https://doc.orbeon.com/form-builder/advanced/services-and-actions/database-services#1-connect-with-the-database), so let's start by seeing how we can use the value of a field in a SQL query.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzO08Bq2LaVhV81h%252Fdatabase-services-poulate-fields.png%3Fgeneration=1528743349698306%26alt=media\&width=768\&dpr=4\&quality=100\&sign=171bfca42655c1ba15b0ad12aade80969b19b077f88578f80092ed0bded993da)

Populate fields

#### 1. Set a service parameter <a href="#id-1.-set-a-service-parameter" id="id-1.-set-a-service-parameter"></a>

Your SQL query can contain _parameters_. Those look like: `<sql:param type="xs:string" select=""/>`. The `type` attribute corresponds to the SQL type to use (e.g. `xs:string`, `xs:decimal`, …). The `select` attribute must be left blank; it is filled-out by the Actions Editor when the service is called.

When the query runs, the value of each parameter is set to the current value of a form field, and you define the mapping between parameter in the SQL query and form field when you create an action. That mapping is done by position; e.g. in the above query, you'll want to set parameter 1 to the control containing an employee id.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzO2tbjM9xocY4s1%252Fdatabase-services-set-service-parameter.png%3Fgeneration=1528743361474226%26alt=media\&width=768\&dpr=4\&quality=100\&sign=c9cf9ebe35357009715bd43bbf2e463cd93e62895d17df7d107efdc28c9d8951)

Set service parameter

#### 2. Set control values <a href="#id-2.-set-control-values" id="id-2.-set-control-values"></a>

Finally, you use the _Set Response Control Values_ section of the _Actions Editor_ dialog to extract the information you're interested in from the XML produced based on the result from the query, and populate fields in the form.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkBzO4zmrXNw\_WeAfU%252Fdatabase-services-set-control-values.png%3Fgeneration=1528743356234389%26alt=media\&width=768\&dpr=4\&quality=100\&sign=45336ef4bb2a49af62c16fc90b981dace74e8046b577f5ae722149f7209dc4fe)

Set control values

### Example: fields pre-population <a href="#example-fields-pre-population" id="example-fields-pre-population"></a>

Say you will be passing a request parameter `employee-id=100` to your form, and want to load data from a relational database about the employee with specified id, this to pre-populate some fields, such their first name, last name, hire data, and department, as shown in the screenshot below.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F33554898276b75da962ca58c7e637581a9c2f188.png%3Fgeneration=1616107270054605%26alt=media\&width=768\&dpr=4\&quality=100\&sign=9d72109a4ca11d0b7678a685d5c9ef22b11a425edebd355dce3a1ab5c2c263ec)

Pre-populated fields

You can do this as follows, in Form Builder:

1. Create a field to hold the employee id passed through the request parameter. You might want to put this field in another section, for instance named "(Internal)", that you hide from end users by setting in its Section Settings dialog, the Visibility to No. For the field, you can use a Hidden Field, or a Calculated Value if you'd like to see its value at runtime when debugging the form.
2. In the Control Settings for that field, name it `employee-id`, and in the Formulas tab, set its Initial Value to `xxf:get-request-parameter('employee-id')`.
3. Create a database service that retrieves the information about the employee, with a statement like the one below.
4. Create an action that, on form load, runs the database service, passing the value of the `employee-id` field, and setting the value of the first name, last name, hire data, and department fields to values returned by the SQL query.

Copy

```
SELECT *
  FROM employees
 WHERE employee_id = <sql:param type="xs:string" select=""/>
```

You can get the [source of the form described above](https://gist.github.com/orbeon/1e25851b7219f765a2cde373a72e9dea), and run it for yourself on the instance of Orbeon Forms deployed on demo.orbeon.orbeon, which has the `employees` table used by the above query. After you publish the form, try opening its `/new` page, passing the request parameter `?employee-id=100`.
