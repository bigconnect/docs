# Sponge Manager

The Manager is the main entry point for using Sponge. It's a web application where users can manage and monitor crawling projects.

The first thing to do is login in the application with the default user **admin** and password **admin**.&#x20;

{% hint style="danger" %}
Please change your password after logging in to prevent unauthorised access to your cloud instance.
{% endhint %}

![BigConnect Sponge login screeen](<../../../.gitbook/assets/image (608).png>)

After logging in, you will see the list of projects. If this is the first time you login to Sponge, you don't have any projects.

![BigConnect Sponge list of projects](<../../../.gitbook/assets/image (531).png>)

The next thing to do is to create a new project.&#x20;

## Working with Projects

To create a new scraping project, navigate to the **Projects** menu item and choose **New Project**. A popup is displayed where you need to enter project information.

![BigConnect Sponge new project dialog](<../../../.gitbook/assets/image (567).png>)

* **Name** - the name of your Project
* **Description** - a small description of your Project
* **Committer** - choose a Committer from the list of configured commiters
* **Agent** - choose an Agent from the list of configured Agents

Once all information has been filled in, click the **Save** button. The project will be created and it will show in the Projects list.

![BigConnect Sponge list of projects](<../../../.gitbook/assets/image (454).png>)

The next step is to configure our Project. Click on the <img src="../../../.gitbook/assets/image (422).png" alt="" data-size="line"> icon to go to the Project detail page.

![BigConnect Sponge project detail page](<../../../.gitbook/assets/image (547).png>)

In here you can configure everything related to your project, you can see a preview of data being scraped and the events emitted during the crawling process.&#x20;

There are also two actions that can be performed using the two red buttons:

* **Delete Project** - deletes the project and all its data
* **Clear Data** - clears the scraped data and project state. You will use this while testing with different project settings&#x20;

### Project Settings

This is the main tab you see when editing a project. It contains general settings for your project such as the project name, it's description and which Agent to use. &#x20;

{% hint style="info" %}
You won't have to change the Agent for small-scale deployments. The LocalAgent is \
pre-configured for your cloud instance.
{% endhint %}

### Crawler&#x20;

![BigConnect Sponge crawler settings](<../../../.gitbook/assets/image (521).png>)

In this section, you can configure how the website should be crawled:

* **Num Threads** - how many threads the crawler will use for fetching pages
* **Max Depth** - how many levels deep to crawl from start URL(s).&#x20;
* **Filter Extensions** - filters scraped URLs based on a comma-separated list of extensions. Extensions are typically the last characters of a file name, after the last dot.
* **Start URLs** - URLs to start crawling from
* **Include Patterns** - Crawl only URLs that validate these [Regex patterns](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html).&#x20;
* **Exclude Patterns** - Exclude URLs from crawling that validate these [Regex patterns](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/regex/Pattern.html)
* **User Agent** - the User Agent string that will be reported by the crawler to the target website
* **Stay on Domain** - Only follow URLs for the domains included in Start URLs
* **Stay on Port** - Only follow URLs for HTTP ports included in Start URLs
* **Stay on Protocol** - Only follow URLs that have the same protocol as the ones in **StartURLs**

{% hint style="warning" %}
Most REJECTED\_\* crawling events are caused by incorrect **Include Patterns** and **Start URLs.**\
You can test your Regex patterns using this [tool](https://www.freeformatter.com/java-regex-tester.html).
{% endhint %}

### Behavior

This is the most advanced part. Here you will configure how the crawler will handle extracted fields.

Before we get into details, you need to understand how the Sponge behaves when crawling a site:

1. &#x20;The first thing is to download HTML/XML documents according to the specified Crawler rules mentioned in the previous section such as **Start URLs, Include Patterns, Max Depth, Filter Extensions** etc, along with their associated response headers. The source of the HTML/XML document and its response header values are stored as fields for the respective document.
2. Next, Sponge will parse the content of the HTML/XML document to extract _\<meta>_ tags and store them also as fields for the document.
3. Next, Sponge will execute all **Behavior** items: **Taggers**, **Filters** and **Splitters** to compute additional  fields for the document, convert field values, exclude documents based on rules etc.
4. Finally, Sponge will send the document and its fields to the **Committer** to store it somewhere.

A [**Tagger**](sponge-taggers.md) allows you to add to or modify existing document fields. A [**Filter**](sponge-filters.md) allows you to filter out certain incoming documents based on field values. A [**Splitter**](sponge-splitters.md) splits a document into multiple ones (e.g. comments in a news article, products on a eCommerce website).

![BigConnect Sponge Behaviour settings](<../../../.gitbook/assets/image (519).png>)

The reference list of available Taggers, Filters and Splitters can be found here:

* [Taggers](sponge-taggers.md)
* [Filters](sponge-filters.md)
* [Splitters](sponge-splitters.md)

A very useful tool while working with **Behaviors** is the **Field Preview** tab. After a first run of your project, you can see here all available fields for crawled documents, including the ones resulted from any Tagger, Filter or Splitter you applied.

![](<../../../.gitbook/assets/image (667).png>)



### Committer

Here you can choose the Committer for your project. Some Committers support field mappings, while others don't. Please refer to the [Committer Configuration](./#committer-configuration) section for more information on how to configure committers.

### Starting and Stopping

#### Starting a Project

Projects can be started from the project list page, by clicking the Play button. When you try to start it, a popup will ask the user when the project should be started:

![](<../../../.gitbook/assets/image (871).png>)

You can choose to start it right away using **Run now** or scheduled using **Run scheduled**.

When you choose the **Run Scheduled** option, the project will be run every N units of time (Hours, Days or Minutes):

![](<../../../.gitbook/assets/image (861).png>)

Running a scheduled project can be cancelled from the project details page using the **Remove Schedule** button.

{% hint style="info" %}
Once started, the project will report a progress bar that shows **Links crawled vs Links processed**. It does not show the actual progress in terms of percentage because that would be impossible. This is why the status bar keeps updating every few seconds.
{% endhint %}

#### Stopping a Project

When a project is in the RUNNING state, you can stop it from the project list page, by clicking on the stop sign next to the project.

### Monitoring

You can monitor project crawling events in the project detail page, by going to the **Event Log** tab.

![](<../../../.gitbook/assets/image (446).png>)

Here you can see the latest events being fired by the crawler, along with details about the URL being processed and its depth.

You can also monitor all documents and fields that pass through the system in the **Field Preview** tab:

![](<../../../.gitbook/assets/image (518).png>)

## Administration

{% hint style="info" %}
The administration screen is only accessible to users with Admnistrator privileges.
{% endhint %}

### Committer Configuration

Committers are destinations where Sponge stores scraped data. Out-of-the-box Sponge supports the following destinations:

* XML Files (default) stored on the local disk or [BigConnect Object Store](../../oss/)
* JSON Files stored on the local disk or [BigConnect Object Store](../../oss/)
* SQL Databases
* [Zion](broken-reference)

Adding or changing committers can be done from the **Administration -> Committers** page.

![](<../../../.gitbook/assets/image (541).png>)

Click the **New Committer** button to add a new committer. A popup will be displayed with the following information:

* **Name** - the name of your committer that will show up throughout the system
* **Description** - a small description of what you committer does
* **Committer** - choose from a list of provided committers
* _**Properties**_ - Each committer has its own configuration properties that describe how to connect to the target system. The properties for each type of committer are described below.

#### SQLCommitter

Used for storing scraping data in a database. This commiter will automatically create a database table for each project, using the project's name as a guideline.

![](<../../../.gitbook/assets/image (479).png>)

* **DB Driver Class** - fully-qualified class name of the JDBC driver to use (eg. com.mysql.jdbc.Driver)
* **DB Driver Path** - the full path to the JDBC **.jar** file of the driver
* **DB Connection URL** - JDBC connection url (eg. jdbc:mysql://localhost:3306/sponge)
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

#### JSON/XML File Committers

Used for storing scraped data in JSON or XML files in a configured local folder.

![](<../../../.gitbook/assets/image (543).png>)

* **Directory** - full path to a filesystem folder where to store scraped data
* **Committer** - one of _JSONFileCommitter, XMLFileCommitter_

{% hint style="warning" %}
The OS user that owns the Sponge Manager or Sponge Agent processes needs to have write permissions to write to the configured folder.
{% endhint %}

#### S3 JSON/XML File Committers

Used for storing scraped data in JSON/XML files in a [BigConnect Object Store](../../oss/) instance.

![](<../../../.gitbook/assets/image (417).png>)

* **Endpoint**: URL of a [BigConnect Object Store](../../oss/) instance \
  (e.g. https://storage-3122463932808256.cloud.bigconnect.io)
* **Access Key**: Tenant ID
* **Secret Key**: secret key for the Object Store
* **Bucket**: name of a bucket (a new one will be created if it doesn't exist)

#### Zion Committer

Used for storing scraped data as Documents in [Zion](broken-reference) (ie. entities with Concept Type = Document)

![](<../../../.gitbook/assets/image (613).png>)

* **Bolt URL** - The URL to a Zion Bolt server\
  (e.g. bolt://core-170393591705357.cloud.bigconnect.io:10242)
* **Username** - Zion user
* **Password** - Zion password

{% hint style="info" %}
When choosing this connector in the Project details page, you will be prompted to perform concept and field mapping.

The Zion instance needs to be available and accessible by Sponge.
{% endhint %}

### Agent Configuration

Agents run scraping jobs on local or remote machines. Sponge Manager includes a local agent that can start scraping jobs as sub-processes.

{% hint style="info" %}
You don't need to alter this configuration for small-scale deployments.&#x20;
{% endhint %}

Adding or changing agents can be done from the **Administration -> Agent Configuration** page.

![](<../../../.gitbook/assets/image (847).png>)

To register a new Agent, click the **Create new Agent** button and fill the following information:

* **Name** - A descriptive name of the Agent
* **Host** - The host on which the Agent is deployed and running
* **Port** - The port used by the agent to listed for incoming connections

{% hint style="warning" %}
Please make sure that no firewall blocks communication between Sponge Manager and the agent host and port.
{% endhint %}

### System Configuration

You can configure various aspects from the System Configuration page like

* Base filesystem folder for storing scraped data
* HTTP proxy
* User Agent for scraping&#x20;

Changing system configuration properties can be done from the **Administration -> System Configuration** page.

### User Management

Users can be managed from the **Administration -> Users** page.&#x20;

![](<../../../.gitbook/assets/image (628).png>)

You can activate/deactivate a user by clicking on the Activated/Deactivated button next to the user in the list.

![](<../../../.gitbook/assets/image (537).png>)

A user can be a regular or an administrator. The administrator user has the ROLE\_ADMIN profile and has access to the Administration section.
