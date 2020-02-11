# Sponge Manager

The Manager is the main entry point for using Sponge. It's a web application where users can manage and monitor crawling projects.

![](../../.gitbook/assets/image%20%2838%29.png)

## Working with Projects

To create a new scraping project, navigate to the **Projects** menu item and choose **New Project**. A popup is displayed where you need to enter project information.

![](../../.gitbook/assets/image%20%28244%29.png)

* **Name** - the name of your Project
* **Description** - a small description of your Project
* **Committer Config** - choose a Committer from the list of configured commiters
* **Agent** - choose an Agent from the list of configured Agents
* **Schema** - choose a Schema from the list of configured Schemas

Once all information has been filled in, click the **Save** button. The project will be created and it will show in the Projects list.

![](../../.gitbook/assets/image%20%28189%29.png)

The next step is to configure our Project. Click on the  ![](../../.gitbook/assets/image%20%28400%29.png) icon to go to the Project detail page.

![](../../.gitbook/assets/image%20%28440%29.png)

There are two actions and several actions that can be performed from the Project detail page:

* **Delete Project** - deletes the project and all its data
* **Clear Data** - clears the scraped data and project state

{% hint style="info" %}
The **Preview Data** section will display scraped records as they come in and is only available for the SQLCommitter.
{% endhint %}

### Crawl Settings

In this section, various settings for the web scraped can be configured:

* **Num Threads** - how many thread the scraper will use for fetching pages
* **Max Depth** - how many level deep to crawl from start URL\(s\). 
* **Filter Extensions** - filters scraped URLs based on a comma-separated list of extensions. Extensions are typically the last characters of a file name, after the last dot.
* **Start URLs** - URLs to start crawling from
* **Include Patterns** - Crawl only URLs that validate these Regex patterns
* **Exclude Patterns** - Exclude URLs from crawling that validate these Regex patterns
* **Stay on Domain** - Only follow URLs for the domains included in Start URLs
* **Stay on Port** - Only follow URLs for HTTP ports included in Start URLs
* **Stay on Protocol** - Only follow URLs that have the same protocol as the ones in StartURLs

### Constant Fields

![](../../.gitbook/assets/image%20%28246%29.png)

These fields will have constant values for all scraped records. They are useful to mention the name of the scraped source, it's url, category, type etc.

![](../../.gitbook/assets/image%20%2837%29.png)

General configuration settings for the project.

### Committer Mapping

Here you can configure how source fields map to destination fields. 

![](../../.gitbook/assets/image%20%28416%29.png)

### Starting and Monitoring

Projects can be started from the project list page, by clicking the Play button. When you try to start it, a popup will ask the user when the project should be started:

![](../../.gitbook/assets/image%20%28373%29.png)

You can choose to start it right away using **Run now** or scheduled using **Run scheduled**.

When you choose the **Run Scheduled** option, the project will be run every N units of time \(Hours, Days or Minutes\):

![](../../.gitbook/assets/image%20%28267%29.png)

Running a scheduled project can be cancelled from the project details page using the **Remove Schedule** button.

{% hint style="info" %}
Once started, the project will report a progress bar that shows **Links crawled vs Links processed**. It does not show the actual progress in terms of percentage because that would be impossible. This is why the status bar keeps updating every few seconds.
{% endhint %}

## Administration

{% hint style="info" %}
The administration screen is only accessible to users with Admnistrator privileges.
{% endhint %}

### Committer Configuration

Committers are destinations where Sponge stores scraped data. Out-of-the-box Sponge supports the following destinations:

* XML Files \(default\)
* JSON Files
* SQL Databases
* BigConnect

Adding or changing committers can be done from the **Administration -&gt; Committer Configuration** page.

![](../../.gitbook/assets/image%20%28108%29.png)

Click the **New Committer** button to add a new committer. A popup will be displayed with the following information:

* **Name** - the name of your committer that will show up throughout the system
* **Description** - a small description of what you committer does
* **Committer** - choose from a list of provided committers
* _**Properties**_ - Each committer has its own configuration properties that describe how to connect to the target system. The properties for each type of committer are described below.

#### SQLCommitter

Used for storing scraping data in a database. This commiter will automatically create a database table for each project, using the project's name as a guideline.

![](../../.gitbook/assets/image%20%28295%29.png)

* **DB Driver Class** - fully-qualified class name of the JDBC driver to use \(eg. com.mysql.jdbc.Driver\)
* **DB Driver Path** - the full path to the JDBC **.jar** file of the driver
* **DB Connection URL** - JDBC connection url \(eg. jdbc:mysql://localhost:3306/sponge\)
* **DB Username** - database user
* **DB Password** - database password
* **DB Commit Batch Size** - how many scraped records to cache before committing to the database.

{% hint style="warning" %}
The configured database user needs to have the following database permissions:

* create, delete, update tables
* insert, update, delete table rows
{% endhint %}

{% hint style="success" %}
Sponge includes the MySQL JDBC driver by default.
{% endhint %}

#### File Committers

Used for storing scraped data in raw, JSON or XML files in a configured folder.

![](../../.gitbook/assets/image%20%28300%29.png)

* **Directory** - full path to a filesystem folder where to store scraped data
* **Committer** - one of _FileSystemCommitter, JSONFileCommitter, XMLFileCommitter_

{% hint style="warning" %}
The OS user that owns the Sponge Manager or Sponge Agent processes needs to have write permissions to write to the configured folder.
{% endhint %}

#### BigConnect Committer

Used for storing scraped data as Documents in BigConnect \(ie. entities with Concept Type = Document\)

![](../../.gitbook/assets/image%20%28179%29.png)

* **Bc URL** - The HTTP\(S\)  URL to BigConnect Explorer
* **Bc Username** - BigConnect Username
* **Bc Password** - BigConnect User password

### Agent Configuration

Agents run scraping jobs on local or remote machines. Sponge Manager includes a local agent that can start scraping jobs as subprocesses.

Adding or changing agents can be done from the **Administration -&gt; Agent Configuration** page.

![](../../.gitbook/assets/image%20%2820%29.png)

To register a new Agent, click the **Create new Agent** button and fill the following information:

* **Name** - A descriptive name of the Agent
* **Host** - The host on which the Agent is deployed and running
* **Port** - The port used by the agent to listed for incoming connections

{% hint style="warning" %}
Please make sure that no firewall blocks communication between Sponge Manager and the agent host and port.
{% endhint %}

### Schema Configuration

Schemas are collections of Destination Fields used by a projects. Schemas are used mostly for data consistency, enforcing one or more projects to respect a specific destination system layout. 

For example, consider you would like to scrape multiple news websites and store all data in database tables with the same columns: `title, text, date, author` and the `text` and `title` fields should always have non-null values. This is where schemas come in handy. You define it once and then reference it in one or more projects. It's a way to enforce that scraped data is consistent with your objectives.

Adding or changing schemas can be done from the **Administration -&gt; Schema Configuration** page.

![](../../.gitbook/assets/image%20%28183%29.png)

To register a new Schema, click the **Create new Schema** button and fill the following information:

* **Name** - a descriptive name for your schema \(eg. News Article\)
* **Fields**
  * **Name** - name of the Destination Field \(_**please follow database naming conventions**_\)
  * **Type** - field data type: String, Number, Boolean, Date
  * **Required** - whether the field is required to have values or not. 

{% hint style="danger" %}
If a scraped record has required fields, the committer will check required fields to have non-null values. If **all required fields have non-null values**, the record is stored in the destination system, otherwise the entire scraped record is ignored.
{% endhint %}

### System Configuration

You can configure various aspects from the System Configuration page like

* Base filesystem folder for storing scraped data
* HTTP proxy
* User Agent for scraping 

Changing system configuration properties can be done from the **Administration -&gt; System Configuration** page.

