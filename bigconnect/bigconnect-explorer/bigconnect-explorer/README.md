---
description: Tutorials on working with BigConnect Explorer
---

# Quick Start

This tutorial will teach you the basics of data loading and visualisation with BigConnect Explorer. The main points addressed in the following sections are:

* Intro: schemas and workspaces
* Importing data
* Enriching data
* Searching data
* Using the Graph
* Using the Map
* Using the Dashboard
* Using Charts
* Using Cypher
* Collaboration
* Administration

## Intro: Schemas and Workspaces

### Schemas

BigConnect Explorer is an analysis tool aimed at working with unstructured data. It relies on a massively distributed graph storage system where all data is indexed and stored as vertices, edges and properties.

The system uses schemas to know how to store and query data. A schema is basically a data model comprised of Concepts, Relations and Properties:

* _Concepts_ can be anything like Person, Company, Car, IP Address, Bank Account, Transaction, Event etc. They have a hierarchical structure and a root concept named **Thing** is provided.&#x20;
* _Relations_ represent how Concepts are connected, like (Person)-\[**hasFriend**]->(Person), (Person)-\[**ownsCar**]->(Car), Company-\[**hasAddress**]->(Address) etc.
* _Properties_ can be defined for both Concepts and Relations and can be things like Age, Gender, Account Number, Plate Number, Name etc. _**Concepts inherit their properties from their parent concept.**_

You can think of Concepts and Relations like database tables and their properties as table columns.

{% hint style="info" %}
The system provides a default setup of Concepts, Relations and Properties. Users can alter the default setup by defining their own Concepts, Relations and Properties depending of their business case.&#x20;
{% endhint %}

![Default schema for Concepts](<../../../.gitbook/assets/image (527).png>)

{% hint style="warning" %}
There are some default Concepts, Relations and Properties that are system-defined and cannot be changed. They are need for BigConnect Explorer to work properly and you cannot change or delete them.&#x20;
{% endhint %}

### Workspaces

Everything that happens in BigConnect Explorer is inside a workspace. When a user first logins, a default workspace will be created for him and he can create additional workspaces as needed.&#x20;

{% hint style="warning" %}
Workspaces are isolated. **What happens in a workspace with data and visualizations stays in that workspace**. If a user decides to make the data available to other users, he needs to either publish the changes or share a workspace with other users.&#x20;

When a users publishes the changes in a workspace, the data in that workspace becomes public.
{% endhint %}

This is a first element of security. Workspaces and their data are not visible among them unless the owner of the workspace shares or publishes the data in that workspace.

All visualisations are specific to a workspace. This means that the dashboard, graphs, maps, and charts are visible only in that workspace. A new workspace will always start with a default (customizable) layout.

{% hint style="warning" %}
**The Schema can be specific to each workspace**. A new workspace will use the PUBLIC schema, but a user might want to create additional objects inside the schema that are specific to its workspace and business case. \
\
When he chooses to publish his data to make it available for everybody, he will also publish the changes made to the schema and the changes made to the schema will be merged in the PUBLIC schema.
{% endhint %}
