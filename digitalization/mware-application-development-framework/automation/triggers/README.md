# Triggers

Each automation must have a single trigger step, this is essentially a watcher - waiting for a particular condition to be met at which point the process will kick-off.

<figure><img src="https://files.readme.io/80daee5903974c40382047861918daf640e0ba1f83a5a429a3ec1f9127f705d8-Screenshot_2024-10-24_at_09.02.33.png" alt=""><figcaption></figcaption></figure>

Currently, there are six ways to trigger an automation:

1. When a new record/row is created in a table
2. When a new record/row is deleted in a table
3. When a new record/row is updated in a table
4. An external trigger in the form of a JSON Webhook
5. When a certain action happens in an app
6. CRON trigger (trigger this automation at a specific time or day)

> ### 📘Row creation/update/deletion triggers
>
> When utilizing row based triggers, it should be noted this only works for rows created through Mware Application Development Framework. If you are using an external datasource, like say Postgres or MySQL, then rows created externally within the database will not trigger automations.

Triggers are internal to the system and therefore cannot easily be altered

It is also possible to create custom triggers using the external Webhook - create an external application that will trigger on your required conditions, and target your Webhook!
