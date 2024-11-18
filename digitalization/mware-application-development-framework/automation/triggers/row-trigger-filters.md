# Row trigger filters

\
Row Triggers with Filters
-------------------------

### Overview

Row triggers allow you to automate actions based on changes in your database. With the addition of filters, you can now create more specific triggers that only fire when certain conditions are met.

### Types of Row Triggers

1. **Row Created**: Triggers when a new row is added to the database.
2. **Row Updated**: Triggers when an existing row is modified.

### Adding Filters to Triggers

Both "Row Created" and "Row Updated" triggers now support filters. Filters allow you to specify conditions that must be met for the trigger to fire.

#### Example filter

<figure><img src="https://files.readme.io/5d62030df4ee9a6fa32df5b9417c3eb3f500a7dfcd7a780b7ddcb55388646aa0-Screenshot_2024-11-06_at_13.38.15.png" alt=""><figcaption></figcaption></figure>

This filter would cause the trigger to fire only when the Approved column of the affected row equals "true".

### Special Behavior for Row Updated Trigger

The Row Updated trigger has a special behavior when filters are applied:

By default, the trigger will only fire when a row transitions from not meeting the filter criteria to meeting the filter criteria.

#### Example

Consider a filter: `Status = Approved`

* The trigger will fire when a row's status changes from "Requested" to "Approved".
* The trigger will not fire if a row's status is already "Approved" and some other field (like "Due Date") changes.

This behavior ensures that the trigger only fires when the specific condition you're interested in is met, rather than on any update to a row that already meets the condition.

### Row Created Trigger Behavior

For the Row Created trigger, the filter is applied to the newly created row. If the new row meets the filter criteria, the trigger will fire.
