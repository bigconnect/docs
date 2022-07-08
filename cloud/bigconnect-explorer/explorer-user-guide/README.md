# User Guide

BigConnect Explorer provides an extensible platform to understand and work with any amounts of data, from any source and in any format. It’s an information-agnostic system where all data that flows into the system is transformed, mapped, enriched and then stored in a logical way using a semantic model of concepts, attributes and relationships. It provides an extensible, unified visual interface with tools for data discovery and analysis, collaboration, alerting and information management.

## Data Model

The core foundation of BigConnect Explorer is the dynamic, semantic data model. It represents the way you store, correlate and query all information and it’s a conceptual data model that expresses information in a factual way: objects, relations and attributes.&#x20;

Typically, the data model is used to provide meaning to the information stored in BigConnect, either at ingestion time or later on, during the life-cycle of the data. It can be defined in the beginning or any time later on, to adapt to any data structure, type or meaning. Any information stored in the system can be mapped to an object (concept), can have some relations and some attributes.&#x20;

A schema is basically a data model comprised of Concepts, Relations and Properties:

Concepts can be anything you can think of: persons, vehicles, bank transactions, phone calls, equipment, companies, locations, events, network packets, log files etc. They are linked to each other using meaningful relations like “works at”, “lives in”, “has friend”, “is brother of”, “sent from IP”, “source file” etc.

Concepts and relations can also have attributes. For a company these can be the company name, formation date, address etc. For a person these can be its first name, last name, birth date, phone number, email address etc. For a “works at” relation we can have the “start date” and “end date” attributes to denote when the person started to work at a company. For a “sent from IP” relation we can have the “timestamp” and the “user” etc.

You can think of Concepts and Relations like database tables and their properties as table columns.

## General Layout

BigConnect Explorer has a default layout comprised of the **Main Menu**, **Work Area** and a **Footer Widget**.

![BigConnect Explorer General Layout](<../../../.gitbook/assets/image (156).png>)

&#x20;The Main Menu is organized into three main sections:

* **Left navigation links**: when clicked, the corresponding section will be displayed in the Work Area.
* **Quick search**…: allows quick searching through the system. Found items can be dragged and dropped to various analysis tools present in the **Analyze** section (eg: Graph, Map)
* **Icons for various tasks:**
  * &#x20;**Spaces** <img src="../../../.gitbook/assets/image (113).png" alt="" data-size="original"> - manage the spaces you are part of
  * &#x20;**Activities** <img src="../../../.gitbook/assets/image (282).png" alt="" data-size="original"> - shows running & completed jobs
  * &#x20;**Alerts**  <img src="../../../.gitbook/assets/image (321).png" alt="" data-size="original"> - shows the number of unassigned alerts
  * &#x20;**Administration** <img src="../../../.gitbook/assets/image (382).png" alt="" data-size="original"> - administration menu
  * &#x20;**Active user** <img src="../../../.gitbook/assets/image (57).png" alt="" data-size="original"> - logged-in username with **Preferences** and **Logout** sub-menu items

## Spaces

Everything that happens in BigConnect Explorer, happens inside a workspace. When a user first logins, a default workspace will be created for him and he can create additional spaces as needed.

Spaces are isolated. What happens in a workspace with data and visualizations stays in that workspace. If a user decides to make the data available to other users, he needs to either publish the changes or share a workspace with other users.

This is a first element of security. Spaces and their data are not visible among them unless the owner of the workspace shares or publishes the data in that workspace.

All data visualizations are specific to a workspace. This means that the dashboard, graphs, maps, and charts are visible only in that workspace. A new workspace will always start with a default (customizable) Dashboard layout.

Spaces can be managed by clicking the Spaces icon in the Main Menu: <img src="../../../.gitbook/assets/image (113).png" alt="" data-size="original">. A popup menu shows the list of spaces you created or are part of. You can also add a new space by filling in the name of the space and clicking the **Add** button. The **Shared with me** section shows the spaces you were invited in and are part of. Clicking the settings icon will show the the access rights you are granted in the respective space.

![List of spaces in BigConnect Explorer ](<../../../.gitbook/assets/image (155).png>)

The spaces you created are shown in the **Choose active Space** section. To edit the name of a space or to invite other users to your space, click the space settings icon: <img src="../../../.gitbook/assets/image (530).png" alt="" data-size="original">&#x20;

![](<../../../.gitbook/assets/image (185).png>)

To invite other users to your space type in the first letters of the username and press the **Arrow Down** key. A list of users that match the search criteria is displayed. Click on the use you want to invite and set the proper access rights:

* **View**: read-only access to the space
* **Comment**: read-only access to the space and the ability to add comments to items
* **Edit**: read-write access to the space

## Footer Widget

The footer widget shows information about the active space you are in (current space).&#x20;

![BigConnect Explorer Footer Widget](<../../../.gitbook/assets/image (107).png>)

It shows the name of the space (`space1`) , the number of unpublished changes and a link to open the Timeline view for the current space.

To see unpublished changes, click the orange number badge. A popup will be displayed showing each unpublished change. Here, you can publish or undo changes one by one or all.

![](<../../../.gitbook/assets/image (86).png>)

## Timeline

The timeline shows the distribution of date properties of all items available in the current space.&#x20;

![](<../../../.gitbook/assets/image (101).png>)

{% hint style="info" %}
Items in a space are items placed on the **Graph** or **Map** tools.
{% endhint %}

You can choose which properties are displayed in the Timeline by clicking the cog icon in the top-right part of the timeline.

The timeline can be zoomed in/out using the mouse wheel. Select an interval on the timeline by left-clicking the mouse and dragging the desired interval. All objects who's chosen date properties fall in the selected interval will be selected.

![](<../../../.gitbook/assets/image (537).png>)

## Adding items

New items can be added into BigConnect in two ways:

* By clicking the "`Add items....`" menu item
* Dragging them from your computer onto the Graph or Map tools
* Using tool-specific methods (eg. righ-click on the Graph canvas -> New Entity)

When you want to add one or more items, a new dialog will popup:

![BigConnect Explorer Add item popup](<../../../.gitbook/assets/image (422).png>)

Here you can specify the following things:

* An attached file
* `Concept` type of the new item. Select an existing one or type to create a new one.
* an optional `Title`&#x20;
* an optional `Justification`
* the role needed to access the new item (also called `Visibility`)

{% hint style="info" %}
You can upload multiple files at once. A new item will be created for each uploaded file.
{% endhint %}
