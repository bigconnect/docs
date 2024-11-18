# Risk Management

Our risk management template provides a ready-to-use solution for recording, assessing, and working with data around risks, incidents, and improvement initiatives.

It’s fully customizable, offering the perfect basis for creating your own custom solution utilizing your own data, logic, and processes.

In this guide, we cover everything you need to know about working with our template, including what it does, how it’s built, and the key ways that you can modify it to suit your needs.

### What does this template do?

At its core, our application is based around submission and approval workflows for risks, incidents, and improvements.

This includes:

* Enabling **App Users** to submit new risks, incidents, or improvements. They also can view and cancel their own previous submissions.
* Routing submissions to **Power Users** for review and approval.
* Providing records of all ongoing and previous submissions.
* Notifying stakeholders of changes to the status of submissions or when there are new submissions that need their attention.

Our template utilizes simple approval flows for each type of submission, with two distinct user roles in addition to our **App Admin** role.

It’s built on top of five tables within Mware Application Development Framework’s database. We’ll examine what each of these is and what it stores a little later.

Let’s check out the core functionality first.

#### CRUD operations

Our application enables different kinds of users to perform specific CRUD actions across the various underlying tables, with user permissions varying according to their roles.

**App Users** can create new records on our Improvements, Risks, or Incidents tables using dedicated data submission forms, populating only a subset of each table’s columns.

<figure><img src="https://files.readme.io/6806d4815318b3fa6729e66de8de6c76dbda0fe78bd578cdc37c20173171a95e-Risk_Management_Template_1.gif" alt=""><figcaption></figcaption></figure>

However, they have limited permissions to **Read** or **Update** entries across the tables. Specifically, they’re only able to access their own previous submissions. Within these, they can update the **Status** attribute to **Canceled** via a **Row Action**.

<figure><img src="https://files.readme.io/f96209a0a016a4b4ba76c839ba65d0686cfdb13441a910c1f22fcc41edf0c857-Risk_Management_Template_2_Previous_Submissions.png" alt=""><figcaption></figcaption></figure>

This works the same way for **Improvements, Risks**, and **Incidents**.

#### Approval flows

**Power Users** inherit all permissions from our **App Users**. On top of this, they can view all incoming submissions across our three tables.

They can also carry out **Update** actions relating to certain fields.

Specifically, they can use **Row Actions** to either **Approve** or **Reject** the row in question, updating its status attribute. At the same time, they can use a form UI to populate the **Feedback** column**.**

If a submission is **Approved**, it can also be assigned to a specific **Improvement Project** for implementation by populating a relationship column.

<figure><img src="https://files.readme.io/84e3c0c5f6cf6c7968eaef00ff73d48f729218d8044e4ec1bdf991ff6d8a2676-Risk_Management_Template_3_APproval_Flow.png" alt=""><figcaption></figcaption></figure>

#### Tracking submission statuses

Both classes of users are able to track the status of the submissions that are relevant to them.

Again, this matches the level of data exposure that’s provided for each Role, as we laid out earlier.

So, **App Users** can access dedicated screens where they can check the status of any of their own previous submissions.

Meanwhile, **Power Users** are able to edit submissions, regardless of the user who created them, including viewing or updating their statuses.

#### Automated emails

Lastly, our application utilizes Mware Application Development Framework’s built-in email automation actions to send notifications to stakeholders based on defined events in our approval flows.

Specifically, the original creator of a submission across any of our three tables is notified when it has a response.

In the case of a **Rejection** or **Approval**, the user is notified of this outcome. When they **Cancel** their own submissions, they’ll also receive email confirmation of this.

**Power Users** receive a summary of the submissions that require their attention at fixed intervals via a separate automation.

Take a look at our documentation on working with email automations.

### How does it work?

Now that we know what our template does, we can drill deeper into how it is built across the different sections of the Mware Application Development Framework builder.

#### User roles

Our risk management template relies on three user roles within Mware Application Development Framework’s RBAC system. RBAC enables us to assign a role to individual users in order to grant them access to appropriate data and actions.

We’ll see how permissions are actually granted in the next section.

Users with the **App User** role can primarily create submissions across our three tables. They can also view key details of their own previous submissions, including their status, or cancel the submission.

**Power Users** can view all submissions, as well as triggering approval or rejection automations and assigning them to projects. They also inherit all permissions from **App Users**.

Roles are configured using Mware Application Development Framework’s visual RBAC editor. We can use branch UIs to define how roles relate to one another, including permission inheritance.

![RBAC](https://files.readme.io/589a3b1728c81e8149fcf339b17ceb551280d7ce4f7f5d7a2b69eb030194d471-Risk\_Management\_Template\_4\_RBAC.png)

We can use this to define which actions and data each role is permitted to access from within the data section, as we’ll see in the following section.

Users with the **App Admin** role have full CRUD permissions across all tables.

#### Data structure

Our template utilizes Mware Application Development Framework’s internal database, enabling us to define and manage schemas using our Data section’s visual editor.

Our application relies on several interrelated internal tables, as well as role-specific **Views** based on these.

Let’s break it down.

**Tables**

As we said earlier, our template utilizes five internal tables. These are:

* **Glossary** - Storing definitions of our different kinds of submissions. This is provided as a reference for **App Admins**.
* **Risks** - For storing submissions relating to potential risks.
* **Incidents** - Where incidents are recorded and tracked.
* **Improvements** - Allowing customers to suggest new improvement initiatives.
* **Improvement Projects** - Which accepted Risks, Incidents, or Improvements can be related to.

Our app also relies on it’s internal **Users** table to enable individual rows across these to be assigned to specific users, as we’ll see in the next section.

Our glossary simply stores definitions for each of our other tables.

Each of our other tables stores data that’s required to record and process submissions. Here’s a summary of the attributes within each.

**Improvement Projects:**

* **title** - Text,
* **id** - Number,
* **created\_by** - Single User,
* **last\_updated\_by** - Single User,
* **description** - Long Forn Text,
* **created\_at** - Date,
* **status** - Single Select,
* **start\_date** - Date,
* **end\_date** - Date,
* **project\_manager** - Single User,
* **improvement** - Relationship,
* **risks** - Relationship,
* **incidents** - Relationship,
* **updated\_at** - Date.

**Improvements:**

* **title** - Text,
* **id** - Number,
* **start\_date** - Date,
* **end\_date** - Date,
* **created\_at** - Date,
* **updated\_at** - Date,
* **description** - Long Form Text,
* **created\_by** - Single User,
* **last\_updated\_by** - Single User,
* **Improvement Project** - Relationship,
* **status** - Single Select.
* **feedback -** Long Form Text.

**Risks:**

* **title** - Text,
* **id** - Number,
* **created\_at** - Date,
* **updated\_at** - Date,
* **created\_by** - Single User,
* **last\_updated\_by** - Single User,
* **description** - Long Forn Text,
* **improvement\_project** - Relationship,
* **status** - Single Select,
* **feedback** - Long Form Text.

**Incidents:**

* **title** - Text,
* **id** - Number,
* **reported\_by** - Single User,
* **employee\_involved** - Single User,
* **date\_of\_incident** - Date,
* **date\_created** - Date,
* **notes** - Long Form Text,
* **status** - Single Select,
* **Improvement Project** - Relationship,
* **feedback** - Long Form Text.

**Relationships**

Our app utilizes two distinct types of relationships.

Relationships between tables are created using a special **Relationship** column. This allows us to configure links between rows across tables, either within our Data section or in end-user applications.

We’ve used three instances of this to create **Many-To-One** relationships from our **Improvement Projects** table to the **Improvements**, **Risks**, and **Incidents** tables.

<figure><img src="https://files.readme.io/ddd7cac92e1f2fb624554c0bb9fe9a190925853e36368146bdd266f95f9aac3d-Risk_Management_Template_5_Relationship.png" alt=""><figcaption></figcaption></figure>

Rows in each of our tables can also be linked to records in it’s internal **Users** table. This allows us to record which user created or updated certain submissions, as well as assigning rows to particular owners.

In Budbase, we have two dedicated columns for linking tables to **User** records - the **Single User** or **Multi-User** columns.

Our app utilizes Single User columns across each of our tables to represent various relationships with users.

<figure><img src="https://files.readme.io/a83343fccca7b29cb054707ca3d7f373dd5a75db81e418ab5a39994aafe66198-Risk_Management_Template_6_Single_User.png" alt=""><figcaption></figcaption></figure>

**Views**

**Views** are subsets of data across one or more tables, with specific rules around accessing and interacting with data.

In Mware Application Development Framework, these are used to centrally configure what each user role can do with our stored data, including granting read or write permissions at the row or column level. We can then use these to autogenerate CRUD UIs and automation rules.

Our underlying tables have their **Access** settings to only be readable or writable by **App Admins**.

<figure><img src="https://files.readme.io/4440dc24826254b47ba77c2905d2ff0a1f8f357cc3097078638aaad04a577a14-Risk_Management_Template_7_Access.png" alt=""><figcaption></figcaption></figure>

Each of our tables has distinct Views for each of our two core user roles.

For **App Users**, the views for our **Risks, Incidents**, and **Improvement** tables are broadly similar, enabling them to create records, read their own submissions, or set the status attribute to cancel via a **Row Action**.

The specific attributes that users can read or write are then defined under the **Columns** tab, so **App Users** are able to access the columns that are required to initially create their submission, check its status, or perform a cancellation.

<figure><img src="https://files.readme.io/85d6e2269619e80f992cacff2ec1822ecfa66cf7ff1f7b1c00c78248b37be0c9-Risk_Management_Template_8_Columns.png" alt=""><figcaption></figcaption></figure>

Each of these tables has two additional **Views** for **Power Users**. The first provides full **Read** and **Write** access across the tables, along with two **Row Actions** to either **Approve** or **Reject** submissions.

This is attached to the respective **Review** screens for each of our submissions tables.

The other provides **Read-Only** access for each of the submission tables. This populates the **Projects** screen.

We can also use **Views** to perform key calculations on our database tables. Our template features views for each of our three submissions tables to provide counts of submissions for individual users based on their statuses.

For instance, we have a **View** called **My Improvements Count**. Using the **Configure Calculations** tab, we’ve set this to return the **Count** of rows, **Grouped By** the **status** attribute.

This will return each unique status, along with the number of relevant entries in our table.

<figure><img src="https://files.readme.io/4baa0d4546379966139bfd4f60e098bd00a11dd48089e3e6e3a0039b8c4eb150-Risk_Management_Template_Configure_Calculations.png" alt=""><figcaption></figcaption></figure>

However, we don’t want to include any submissions that have either been canceled or rejected. We also only want to include the rows that are related to the current user in the front end.

So, we’ve applied three filtering rules. The first includes rows that have been submitted by the current user. The other two exclude rows where the **status** is **Rejected** or **Canceled**.

<figure><img src="https://files.readme.io/bad0a5ab39cba567fd070cf756e74b7d96f3c16dbb9a96301490e368d4e1cb2f-Risk_Management_Template_Calculation_Filters.png" alt=""><figcaption></figcaption></figure>

These are then displayed on the home screen for App Users, as we’ll see in a moment.

#### User interfaces

Our application uses a combination of fully custom screens and UIs built around Mware Application Development Framework’s autogenerated layouts.

Check out our docs page on working with screens within Mware Application Development Framework.

The screens available for each user cohort across our three types of submissions follow a broadly similar pattern.

**App Users** can access three kinds of screens. Firstly, there are three data collection forms, where they can create records on our three submission tables. These rely on Mware Application Development Framework’s autogenerated **Form** layout, with minor changes to their design.

<figure><img src="https://files.readme.io/79a838eeb6dfe1efd76e2588a22bd1bc74d2fd4e90ea3310506ffcc87507293b-Risk_Management_Template_9_Form_Block.png" alt=""><figcaption></figcaption></figure>

We’ve provided fully custom screens for **App Users** to view, track, or cancel their own submissions. These are made up of a variety of nested components wrapped in **Data Providers** and **Repeaters** to populate display elements with values from the relevant Views.

Take a look at our documentation on working with data in design to learn more about how this works.

<figure><img src="https://files.readme.io/99194350dbfb5159e908609716f5848328831830e1849e48ee9cdeb2937de193-Risk_Management_Template_10_My_View_Screen.png" alt=""><figcaption></figcaption></figure>

We’ve also created a dedicated home screen for **App Users**. This provides links to navigate to the screen for each type of submission. Its design relies on stylized **Container** and **Paragraph** components.

The **Container** relating to each type of submission displays the total number of entries that the user has created, along with how the active submissions break down by status.

First, we have a **Paragraph** component that displays the total number of each type of submission by the current user, by calculating the length of the relevant **View**. So, for improvements, we use `{{ My Improvements Data Provider.Rows Length }} Submitted`

The My Improvements Data Provider has a filtering expression to exclude entries where the **status** is set to **canceled**.

<figure><img src="https://files.readme.io/d83258cc1235619afa57c55999c98ebe5ce6600ea36a64d639e084f9685e7140-Risk_Management_Template_Home_Screen_Count_of_all_Submissions.png" alt=""><figcaption></figcaption></figure>

Beneath this, we have a second **Paragraph** component which is wrapped in a **Data Provider** and **Repeater**.

These components will connect to a data source and display each of the elements nested within them for each individual row. In our case, we’re displaying the **status** and **count** from our **View Calculation**, using:

```
{{ Improvements Counts Repeater.My Improvements Counts.Count id }} {{ Improvements Counts Repeater.My Improvements Counts.status }}
```

<figure><img src="https://files.readme.io/7fb8e31bc4f513fa66cbb883d366a05545c1d8c50c44ffa999bc421f3eef3f95-Risk_Management_Template_View_Calculation_UI.png" alt=""><figcaption></figcaption></figure>

**App Users** can also use buttons to navigate to either the submission form or table UI for each respective type of submission.

Our **Power User** screens for reviewing submissions follow the same pattern as the **App User** versions we saw earlier, with the additional ability to **Approve** or **Reject** records.

They can add additional details on a modal form that’s displayed when they hit reject.

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXdHh4hq18oScgXUVFzA3Fb5wCZjU9pCfrhU8BOHhw9AUe4APQRG2p0724vqtXPDbF5dhWN0i-57dEowcbb2bxS1Bn9CDlSVVKJ43fV0GKcUCzaIR183z3zg9tcKTUCqLF58O59tMYCtTkKkvdgVxN48FIoT?key=SihkkX1BAxABYFc82mbJ3A" alt=""><figcaption></figcaption></figure>

**Power Users** can also access a similar screen for viewing or editing **Improvement Project** records, including reading all of the details of associated submissions, via the **View** we discussed earlier.

#### Automations

In Mware Application Development Framework, the Automation section is used to create workflow logic within our applications, including enabling users to take specific actions. Take a look at our automation docs to learn more.

Our template makes extensive use of **Row Actions**. These are user-triggerable automation flows that run on a specific row within a database table. These can be created from the Data section, under the **Row Actions** tab within a database table or view.

The **App User** view for each of our submission tables features a **Row Action** called **Cancel**.

Once triggered, this performs two actions. First, it updates the **Status** attribute of the trigger row to **Cancelled**. Then, it sends a template email to the user, confirming that this has been successful.

<figure><img src="https://files.readme.io/92e37c5f9ab7b972d71e136ce9137b8acd1516bc769ca4313fc316566f063aac-Risk_Management_Template_13_Cancel_Automation.png" alt=""><figcaption></figcaption></figure>

**Power Users** have two additional **Row Actions** for each submission table, called **Approve** and **Reject**. These follow the same structure as our **Cancel** action, changing the **Status** to **Approved** and **Rejected**, respectively.

They then send an email template to the original submitter, notifying them of the outcome.

<figure><img src="https://files.readme.io/1bd9564b9da9ee408e381d1cc0e6f2b2d00eb638d65dba94669aebe320ce22f5-Risk_Management_Template_14_Accept_Automation.png" alt=""><figcaption></figcaption></figure>

Lastly, we’ve built an Automation called **Outstanding Attributes**, using a **Chron Trigger**. This enables us to set rules which are executed at defined intervals. Ours runs every Monday and Thursday morning at 09:30.

We’ve set our Cron Trigger manually, but we also have the option of using AI to generate an expression.

<figure><img src="https://files.readme.io/800382d96bb2e70e56b300cbbe19dfd9b057b22b5a4fb38df443d9923e7e61cc-Risk_Management_Template_Outstanding_Automation.png" alt=""><figcaption></figcaption></figure>

This uses three **Query Rows** steps to return all of the rows from our **Risks**, **Incidents**, and **Improvements** tables with **Status** attributes set to **Pending**.

We then use a **Condition** block, with some custom JavaScript, to count the total number of rows across all three tables that are currently **Pending**. If this is **not 0**, we continue.

Lastly, we use an **Email** action to notify relevant users that submissions need their attention. By default, this is set to notify the current user, but this can be overridden to target a specific email address or a comma-separated set of emails.

Alternatively, we could configure this to notify all users with a particular role, such as **Power User** or **App Admin**.

By default, this Automation is set to **Paused**.

<figure><img src="https://files.readme.io/1a37f54ab1288f7e7c9b46fce6148d3f47480052fdf722a036048b85b3b1ea18-Risk_Management_Templaet_Outstanding_Automation_2.png" alt=""><figcaption></figcaption></figure>

### Customizing our risk management template

Mware Application Development Framework templates are created as ready-to-use solutions, but they also provide a basis for you to build your custom workflow apps.

Let’s think about some of the key ways that you can modify our change management template within Mware Application Development Framework.

#### Updating the data model

The Data section is the core of every Mware Application Development Framework app. Our template utilizes Mware Application Development Framework’s internal database. So, we have two broad options for customizing our data model. We could swap this for an entirely different database, or we could alter the schema of our existing one.

If we wanted to connect to an external SQL database, for example, we could recreate the various Views that we have in place for our existing database before connecting this to our existing UIs and automations.

We can also easily modify the schema of our existing database, including adding, removing, or editing existing tables and columns, using the spreadsheet-like interface in Mware Application Development Framework’s Data section.

For example, say we wanted to add a new Long Form Text column to our **Risks** table, called inspector\_notes.

We could do this by hitting the **+** icon at the top of our table.

<figure><img src="https://files.readme.io/378b0edfbcb4da0816c0dfea3d9456e5d0d69e24a32b30b10e9ecb6c2f1b712c-Risk_Management_Template_15_Inspector_Notes.png" alt=""><figcaption></figcaption></figure>

At the same time, we might update our existing **status** attribute by adding an option called **Inspected**.

<figure><img src="https://files.readme.io/45f4c2550ebc002186b44615696fde813d65dc97fd028da3fe09bc3ef59d2f70-Risk_Management_Template_16.5_Inspected_Status_Option.png" alt=""><figcaption></figcaption></figure>

#### Adding and altering user roles

Another key area where you can modify our template is its user roles. Mware Application Development Framework offers a fully visual interface for creating and configuring user roles, including with branching inheritance.

We’re going to create a new role called **Inspector** this will inherit permissions from the existing **App User** role, but it will be placed in its own branch, separate from the **Power User** role.

This means that **Inspectors** will be able to create any submissions that **App Users** can, as well as whatever additional permissions we grant them. However, this will be unrelated to the existing **Power User** role’s permissions.

<figure><img src="https://files.readme.io/e702861d6223edd2093d90efcd98f6dfddc5e8b692660d60959fe101fcdef0c6-Risk_Management_Template_16_Inspector_ROle.png" alt=""><figcaption></figcaption></figure>

To go along with this, we’ll create a new **View** and set its **Access** to the **Inspector** role. We’ll then make all columns except for **Inspector Notes** read-only.

<figure><img src="https://files.readme.io/f6ccf942d0ba2f87347182686e20f5e856690370fcf81f113d0f36afbe4eda98-Risk_Management_Template_17_Inspector_Notes.png" alt=""><figcaption></figcaption></figure>

We’ll also set a filter to only include entries where the **status** is set to **Pending**.

<figure><img src="https://files.readme.io/7ff81ef735ee52482e5a7f5e508531c278d981e187b0c950cca1ff29a16d26d8-Risk_Management_Template_18_Filter.png" alt=""><figcaption></figcaption></figure>

Now, **Inspectors** have permission to view all pending **Risks** entries, and add their notes.

#### Adding extra screens

Next, we need to provide an interface for our inspection workflow.

This will be relatively simple, enabling **Inspectors** to read our entries and populate the **Inspector Notes** field.

From our new view, we’ll start by autogenerating a **Table** screen with the **Side-Panel Form** option.

<figure><img src="https://files.readme.io/4e43832efa6ef431e8633e5da03ba0b715fde816bcad36b4dedbe63d1f19d7f1-Risk_Management_Template_19_Generate_Screens.png" alt=""><figcaption></figcaption></figure>

Then, within the Design section, we’ll make a couple of UX tweaks. Specifically, we want to remove the **Create Form**. On our remaining **Edit** form, we’ll remove the **Delete** button and set all fields except for **Inspector Notes** to **Disabled**.

<figure><img src="https://files.readme.io/18a7551e6903d4eb45d0a5bf27f0c5f7d78b0a6748422bbcdf7be519601dc3c2-Risk_Management_Template_20_Form.png" alt=""><figcaption></figcaption></figure>

#### Adding your own business rules

Lastly, we can use the Automation section to add our own application logic. To carry on our example we want to add an automation flow so that when the **Inspection Notes** field is populated, the **status** is updated to **Inspected**.

We’ll then email the original submitted, notifying them of this update.

We’ll start by creating a **Row Action** from our new database View. We’ll call this **Inspect**.

<figure><img src="https://files.readme.io/1a624d8f11345bd5d93f709ed6d9a69e06122cbe93ce9f87339cd8bbe2fd87f2-Risk_Management_Template_21_Inspect_Action.png" alt=""><figcaption></figcaption></figure>

We’ll then mimic the structure of our existing **Row Actions**, first adding an **Update Row** step, followed by an **Email** step.

To update our row, we’ll set the **ID** to our trigger ID and the **status** to **Inspected**. We’ll populate the **Send To, Subject**, and **Body** fields of our **Email** action too.

<figure><img src="https://files.readme.io/85983e0989b3f488977fa4014b86abb3a514ebeed9e7351c3a71f76f3aad4192-Risk_Management_Template_22_Inspect_Action_Flow.png" alt=""><figcaption></figcaption></figure>

Next, we need to provide a way to trigger this automation from our app’s UI. Head back to the Design section, and we’ll add a **Button Action** to the form on our **Inspection** screen.

Here, we’ll add a **Row Action** step. Our **Form Block** has an attribute called **RowID**, which we’ll copy into the corresponding field on our button action.

<figure><img src="https://files.readme.io/d3a87a64eb9383c51f92925de12a94ee8202c28ad18ea21fb23d4e497f9ffa26-Risk_Management_Template_23_Button_Action.png" alt=""><figcaption></figcaption></figure>
