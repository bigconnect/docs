---
description: Tutorial on how to import data in BigConnect Explorer
---

# Loading Data

There are multiple ways in which a user can bring new data into the system:

* Drag & Drop on the Graph and Map
* Import CSV and XLS files using the Wizard
* Import from a database using the Wizard
* Import from a database using Cypher
* Import from a CSV file using Cypher
* Import using the Data Collector \(for large/automated imports\)

Each type of import can be used based on the business case. If you want to load just a few objects, you can use Drag & Drop for example. If you have a CSV file you can either use the Wizard or Cypher. For more complex scenarios you can use the Data Collector.

### Using Drag & Drop

Login to BigConnect Explorer using the default username **admin** and password **admin.** If this is the first time you login, you will be taken to the default dashboard created for you.

![](../../../.gitbook/assets/image%20%2875%29.png)

Go to **Analyze** in the upper menu bar.

![](../../../.gitbook/assets/image%20%28621%29.png)

Click on the **Graph** card and an empty graph will be created for you:

![](../../../.gitbook/assets/image%20%28533%29.png)

Now, let's download all sample files from [https://github.com/bigconnect/demos/tree/master/explorer/basics](https://github.com/bigconnect/demos/tree/master/explorer/basics) and save them somewhere on your station:

* coronavirus.txt
* audio.mp3
* panga.jpg
* video.mp4

Once you finished downloading, return to BigConnect Explorer and drag & drop all downloaded files on the empty graph.

{% hint style="info" %}
You can also click on the UPLOAD card and select the files using the upload dialog
{% endhint %}

A popup will be shown to ask you how you want to load the files into the system. Just leave the defaults and click **Import**

![](../../../.gitbook/assets/image%20%28432%29.png)

The system will load and process the files. After a short while you should see four items on your graph:

![](../../../.gitbook/assets/image%20%28341%29.png)

Note that BigConnect automatically detected the mime-type of each file and assigned the appropriate icons.

Click on each one of the four items on the graph and you will see the details for the selected item in the details drawer:

![](../../../.gitbook/assets/image%20%28216%29.png)

You will see that for each type of item \(audio, video, image and text\), the drawer will look slightly different, according to the type of the selected item:

![BigConnect Explorer Item Types](../../../.gitbook/assets/explorer-items.png)

We will look more into what these mean and how to work with them later on.

### Loading CSV and XLS files

There are two ways to upload CSV and XLS files:

1. From the **Ingest** menu item
2. Drag & Drop a CSV/XLS file on the Graph or Map

We will continue with option 1. 

First, please download the **people.csv** sample file from the following location: [https://raw.githubusercontent.com/bigconnect/demos/master/explorer/aml/people.csv](https://raw.githubusercontent.com/bigconnect/demos/master/explorer/aml/people.csv)

It's a sample file that contains 2000 fictional persons. 

Choose **Ingest** from the top menu bar:

![](../../../.gitbook/assets/image%20%28597%29.png)

Click on the **Upload your files** link under the **Files** card. The following screen will be displayed:

![](../../../.gitbook/assets/image%20%285%29.png)

Drag and drop the **addresses.csv** file on the gray container or click on the gray container and choose the **addresses.csv** file. The file will be displayed in the gray container:

![](../../../.gitbook/assets/image%20%28272%29.png)

Click **Import** to continue and preview the file.

![](../../../.gitbook/assets/image%20%2889%29.png)

Now we need to tell BigConnect Explorer how to map the fields from the CSV file to our schema. This process is called mapping**.** Click on the green **Map Structured Data** button.

![](../../../.gitbook/assets/image%20%28398%29.png)

Select the first row of the table that contains the header fields \(id, first\_name, last\_name etc.\):

![](../../../.gitbook/assets/image%20%28380%29.png)

The system extracted the field names from the CSV header. Now we will go through each column and tell the system how to import it:

1. Click on the ID column.

![](../../../.gitbook/assets/image%20%2845%29.png)

2. The system asks us how to map this column. We want to map it to a new entity that has the concept **Person**. Click on the **New Concept...**  field to browse existing concepts defined in BigConnect and choose the **Person** concept:

![](../../../.gitbook/assets/image%20%28521%29.png)

3. Now the system will ask on which property of the Person concept we want to map this column on:

![](../../../.gitbook/assets/image%20%28529%29.png)

4. Since we don't have an existing property to map on, we need to create one. Just type the **uniqueid** in the **Choose Property...** field and click on the **Create "uniqueid"** dark row.

![](../../../.gitbook/assets/image%20%28479%29.png)

5. A small section will be displayed asking to specify the **Data Format** for our new field: 

![](../../../.gitbook/assets/image%20%28472%29.png)

6. Choose **String** and click on the green **Create "uniqueid"** button:

![](../../../.gitbook/assets/image%20%28516%29.png)

7. Check the **Use this column as unique identifier for this entity** checkbox and then click **Save**.

![](../../../.gitbook/assets/image%20%28339%29.png)

8. This is the "hard" way of doing the mapping where we have control of exactly how each field is mapped. The "easy" way is to let the system auto-map all fields. We will do that for the rest of the fields.

Click on the **Automap** link:

![](../../../.gitbook/assets/image%20%28276%29.png)

9. Select our previously created entity Person\#1 from the **Auto-map to** dropdown and click **Save**:

![](../../../.gitbook/assets/image%20%28540%29.png)

10. Click on **Preview.** The system will parse all rows from the file and validate that they can be imported:

![](../../../.gitbook/assets/image%20%28436%29.png)

11. Finally click on **Import** button to launch the import process in the background. You can monitor the progress of all background tasks by clicking on the **Activity** icon in the top menu bar:

![](../../../.gitbook/assets/image%20%28481%29.png)

### 

### Loading from Databases

Loading data from a database requires the appropriate JDBC driver JAR file to be copied into the **/opt/bdl/lib/explorer/lib/** folder. **Please note that a restart of BigConnect Explorer  is required.**

{% hint style="info" %}
BigConnect Explorer ships with a MySQL driver that works with MySQL and MariaDB databases.
{% endhint %}

The first step is to setup a local MySQL demo database and import a table with data in the database. Access your BDL machine and login with the **bdl** user, then issue the following commands:

```text
sudo mysql

DROP DATABASE IF EXISTS bdldemo;
CREATE DATABASE bdldemo DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON bdldemo.* TO 'bdldemo'@'%' IDENTIFIED BY 'bdldemo';
GRANT ALL ON bdldemo.* TO 'bdldemo'@'localhost' IDENTIFIED BY 'bdldemo';
FLUSH PRIVILEGES;

exit
```



Download the **bdldemo\_addresses.sql** script from: [https://raw.githubusercontent.com/bigconnect/demos/master/explorer/basics/bdldemo\_addresses.sql](https://raw.githubusercontent.com/bigconnect/demos/master/explorer/basics/bdldemo_addresses.sql)

Connect to the mysql database with the **bdldemo** user and password and import the **addresses** table:

```text
mysql -u bdldemo -D bdldemo --password=bdldemo < [full_path_to_bdldemo_addresses_sql_script]
```

where _**\[full\_path\_to\_bdldemo\_addresses\_sql\_script\]**_ is the absolute path to the bdldemo\_addresses.sql script \(eg: /home/demo/Downloads/bdldemo\_addresses.sql\)

Choose **Ingest** from the top menu bar:

![](../../../.gitbook/assets/image%20%28597%29.png)

Click on the **MySQL** link under the **SQL** card. The following screen will be displayed:

![](../../../.gitbook/assets/image%20%28457%29.png)

Click on **Add** to create a new Connection:

![](../../../.gitbook/assets/image%20%2814%29.png)

Fill in the following details:

* Name: **MySQL**
* Description: **MySQL Connection**
* Driver Class: **com.mysql.jdbc.Driver**
* JDBC URL: **jdbc:mysql://localhost:3306/bdldemo**
* Username: **bdldemo**
* Password: **bdldemo**

![](../../../.gitbook/assets/image%20%28477%29.png)

Click on the **Save** button to create the connection. At this point, the connection details provided will be validated against the database and if everything is ok, the connection is created.

![](../../../.gitbook/assets/image%20%28511%29.png)

Click on the highlighted arrow to expand the MySQL connection and click on the plus circle to create a new Data Source:

![](../../../.gitbook/assets/image%20%28138%29.png)

A multi-step wizard will be displayed, asking for the name of the Data Source and an SQL query:

![](../../../.gitbook/assets/image%20%2871%29.png)

Enter the following details and click **Next**:

* Name: **addresses**
* Select Statement: **SELECT \* FROM addresses**

![](../../../.gitbook/assets/image%20%28415%29.png)

A preview of the data in the addresses table is be displayed. Click **Next** to continue to the Data Mapping part:

![](../../../.gitbook/assets/image%20%28223%29.png)

Now we need to tell BigConnect Explorer how to map the fields from the SQL query  to our schema. This process is called data mapping**.** We need to take the columns one by one and tell the system how they should be mapped:

1 Click on the **id** column in the right table.

![](../../../.gitbook/assets/image%20%28249%29.png)

2. Type **address** in the Concept field and click on the dark **Create "address"** button.

![](../../../.gitbook/assets/image%20%28528%29.png)

3. Click on the green **Create "address"** button to create the address Concept.

![](../../../.gitbook/assets/image%20%28307%29.png)

4. Type **uniqueid** in the Property field and click on the dark **Create "uniqueid"** button. Choose **String** as the **Data Format** for the new field and click on **Create "uniqueid"**. 

![](../../../.gitbook/assets/image%20%28303%29.png)

5. Check the **Identifier** checkbox to tell BigConnect that this field uniquely identifies our entity and click **Save**.

6. Click on the **city** column in the right table and choose **address\#1** in the **Map to** dropdown. Type **city** in the **Property** field and create a new field with **String** Data Format following the same steps from above. **Save** the mapping and move to the next column.

{% hint style="info" %}
Don't check the **Identifier** checkbox for any other field, because we only want the uniqueid field to be our identifier. 
{% endhint %}

7. Repeat the step 6 for the **country** and **street** columns. In the end you should have the following mapping:

![](../../../.gitbook/assets/image%20%28624%29.png)

Click **Next** and check the **Run on finish** checkbox:

![](../../../.gitbook/assets/image%20%28245%29.png)

Click **Next** to continue and then click **Save** on the Review Final Configuration step to start the import in the background.

You can monitor the progress of all background tasks by clicking on the Activity icon in the top menu bar:

![](../../../.gitbook/assets/image%20%28301%29.png)



### Loading with Cypher

BigConnect Explorer supports loading data from databases and CSV files using the Cypher query language.

You can use Cypher queries to load files from an URL using the Cypher Lab:

```sql
WITH 'file:////tmp/csv_file.csv' AS url
LOAD CSV WITH HEADERS FROM url AS row
WITH row
CREATE (a:entity) 
SET a.field1 = row.csvfield1, a.field2 = row.csvfield2
.....
```

You can also load data from HTTP URLs. Just put your desired URL in the WITH clause.

To load data from database tables using Cypher, you first need to register the JDBC driver using a Cypher procedure:

```sql
CALL jdbc.driver('com.mysql.jdbc.Driver')
```

Then you can load the data using  a table or an SQL query:

```sql
CALL jdbc.load(
  'jdbc:mysql://localhost:3306/bdldemo?user=bdldemo&password=bdldemo',
  'select * from addresses'
) YIELD row
CREATE (a:entity) 
SET a.field1 = row.sqlfield1, a.field2 = row.sqlfield2
....
```







