# Permissions introduction

There are always going to be sensitive bits of information in your data, and thankfully Answers provides a rich set of tools to ensure that people on your team only see the data they’re supposed to.

If instead you’re wondering about what data Answers the company can see, check out our page on data privacy and security.

Key points regarding permissions

* Permissions are granted to groups, not people.
* People can be in more than one group.
* If a person is in multiple groups, they will have the _most permissive_ access granted to them across all of their groups. For example, if a person is in three groups, and any one of those groups has access to a database, then that person will have access to that database.

What you can set permissions on

* Data permissions
* Collection permissions
* Application permissions
* SQL snippet folder permissions

Data permissions

* Databases connected to Answers
* Tables and schemas in those databases
* Rows and columns, a.k.a. data sandboxing

Collection permissions

Collection permissions dictate which groups can view/edit items in collections, including:

* Questions
* Dashboards
* Models
* Events
* Timelines

Application permissions

Application permissions dictate access to Answers application-level features, including:

* **Settings**: The Settings tab in the Admin panel.
* **Monitoring access**: The Tools, Audit, and Troubleshooting tabs in the Admin panel.
* **Subscriptions and Alerts**. Which groups can create/edit dashboard subscriptions and alerts.

SQL snippet folder permissions

For plans that include SQL Snippet Folders, you can also set permissions on those folders.

## Changing permissions

Whenever you change permissions for a group, make sure you:

* Save your changes.
* Click yes to confirm your choices.
