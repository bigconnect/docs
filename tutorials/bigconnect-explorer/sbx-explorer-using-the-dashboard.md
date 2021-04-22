# Using the Dashboard

Each workspace has its own unique Dashboard where users can place widgets to get a quick overview of the most interesting things. It is also the first thing a users will see when he will login to BigConnect Explorer:

![Example Social Media Dashboard](../../.gitbook/assets/image%20%28602%29.png)



When a new workspace is created, a Dashboard will be created as well with some default widgets. 

![](../../.gitbook/assets/image%20%28267%29.png)

To edit the dashboard layout and place or rearrange widgets, click on the ![](../../.gitbook/assets/image%20%28467%29.png) button. The Dashboard will enter in Edit mode where you can add, remove, reposition and resize widgets.

![](../../.gitbook/assets/image%20%28507%29.png)

The following widgets are available:

* **Behaviour**: runs the selected behavior and shows the results
* **Chart**: displays the selected chart
* **Entity Counts**: shows an overview of all entities available in the system
* **Notifications**: shows available notifications
* **Relationship Counts:** shows an overview of all relationships available in the system
* **Rich Text**: shows an overview of all entities available in the system
* **Saved Search**: shows the results of a saved search and allows to create aggregations
* **Search Results**: shows the results of a saved search in tabular form
* **Welcome**: shows the welcome text

We will look into the most important ones: **Saved Search**, **Chart** and **Rich Text.**

### Saved Search widget

This is the most powerful widget that can do a lot of things:

* display the actual results of a saved search
* create an aggregation on one of the fields available in search results
* change the display type for an aggregation
* sort results

The first thing you need to do to use this widget is to have a saved search. To achieve this, head to the **Discover** page, enter some search terms and once you are happy, give it a name and save it.

For this example, we will import some Facebook Posts. Login to BigConnect Explorer and open the Cypher Lab from the top menu bar. Enter the following details to login to Cypher Lab:

* **Connection URL:** bolt://localhost:10242
* **Username**: admin
* **Password**: admin

Once you are connected, copy and paste the following query in the Lab to load the sample FB posts:

```sql
WITH 'https://raw.githubusercontent.com/bigconnect/demos/master/explorer/basics/fb_posts.csv' AS url
LOAD CSV WITH HEADERS FROM url AS row
CREATE(p:socialPost) 
SET p.text = row.text, p.socialLikes = row.reactions,
p.socialComments = row.comments, p.postid = row.post_id,
p.sourceDate = datetime.fromepochmillis(date.parse(row.date, 'ms', 'yyyy-MM-dd HH:mm:ss'))
```

Now let's load the comments associated with these posts. Copy and paste the following query in the Lab:

```sql
WITH 'https://raw.githubusercontent.com/bigconnect/demos/master/explorer/basics/fb_comments.csv' AS url
LOAD CSV WITH HEADERS FROM url AS row
MATCH(p:socialPost { postid: row.post_id })
CREATE(p)-[:hasSocialComment]->(c:socialComment)
SET c.text = row.text, c.socialLikes = row.reactions, 
c.author = row.source,
c.sourceDate = datetime.fromepochmillis(date.parse(row.date, 'ms', 'yyyy-MM-dd HH:mm:ss'))
```

Now go back to BigConnect Explorer and re-login. Select to the Discover menu item, type **\*** in the search field and filter by the **Social Comment** entity. You will see only the FB posts:

![](../../.gitbook/assets/image%20%28252%29.png)

Save the search and give it the name: **FB Posts**

![](../../.gitbook/assets/image%20%28144%29.png)

Now go to the **Dashboard** and add a **Saved Search** widget. Click the "**Configure Saved Search…**" link and choose **FB Posts** from the Saved Search drop-down. You will ses the results of our previously saved search:

![](../../.gitbook/assets/image%20%28522%29.png)

Now for the fun part. Click on the configuration wheel on the Saved Search widget and click the **Add** link next to Aggregation:

![](../../.gitbook/assets/image%20%28405%29%20%281%29.png)

Choose the **Text** property from the **Property to Count** dropdown:

![](../../.gitbook/assets/image%20%28364%29.png)

The widget will now show an aggregation of the most frequent used terms in the Text field. Change the visualization to **Tag Cloud** to see things in a word cloud:

![](../../.gitbook/assets/image%20%28562%29.png)

{% hint style="info" %}
You can increase the size of the aggregation by clicking on on the property name \(eg. Text in our case\) and change the value under the property name \(eg. from 10 to 50\). 
{% endhint %}

You can perform Count, Histogram and Geo-Coordinate cluster aggregations, depending on the type of property you want to aggregate. You can also sort the results or exclude certain terms from a Count aggregation.

For example, change the property name to **Source Date**,  choose **Histogram** as the type of Aggregation and set the interval to **1 days**:

![](../../.gitbook/assets/image%20%2862%29.png)

You will now see the distribution of post dates within the 1 day boundary.

