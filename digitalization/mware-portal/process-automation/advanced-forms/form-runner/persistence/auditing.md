# Auditing

Auditing (also known as the _auditing trail_) guarantees that all the operations on the database:

* Are performed in a non-destructive manner.
  * This means that it is possible for a DBA to revert changes that have been made.
* Every change has indication of when that change was made and who did that change (if the user is known by the system).
  * This means that you can see who did what and when.

This feature is only available when using a [relational persistence layer](https://doc.orbeon.com/form-runner/persistence/relational-db).

Note that Orbeon Forms as of Orbeon Forms 2018.2 doesn't provide a user interface for this feature.

### Usage <a href="#usage" id="usage"></a>

For auditing to work meaningfully, Form Runner needs to know who is presently using the application.

For details, see [Setup users for access control](https://doc.orbeon.com/form-runner/access-control/users).

### Implementation for relational databases <a href="#implementation-for-relational-databases" id="implementation-for-relational-databases"></a>

When you use a relational persistence layer:

* Every table has the following 4 columns:
  * `created` tells you when given the data (e.g. attachment to a form for the `orbeon_form_data_attach`).
  * `last_modified` tells you when the data was last changed.
  * `username` tells you who did that change.
  * `deleted` tells you if the data is marked as deleted, and hence invisible to users.
* Once a row is added to a column, it is never updated or deleted. Only new rows are added.
* When data is first added to a table, `created` and `last_modified` have the same value. Then, when this data is modified, another row is added: `created` is copied over and `last_modified` is set to the current time stamp.
* When data is deleted by users, a new row is added. This row is a copy of the latest row for the data that is being deleted, except for `last_modified` which is set to the current time stamp and `deleted` which is set to `Y`.

### Disabling auditing <a href="#disabling-auditing" id="disabling-auditing"></a>

As of Orbeon Forms 2018.2, it is not possible to disable this feature. However, you can [purge old data using SQL](https://doc.orbeon.com/form-runner/persistence/purging-old-data) as needed.

[\[SINCE Orbeon Forms 2023.1\]](https://doc.orbeon.com/release-notes/orbeon-forms-2023.1)

You can also use the [Purge function](https://doc.orbeon.com/form-runner/features/purging-historical-data) from the Forms Admin page.
