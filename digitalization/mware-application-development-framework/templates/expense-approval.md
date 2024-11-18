# Expense Approval

Our expense approval template offers a ready-to-use solution for enabling employees to submit claims for review and authorization. However, it’s also fully customizable, providing a basis for creating your own bespoke solution in Mware Application Development Framework.

In this guide, we’re outlining how our template works, how it’s built, and a few examples of changes you could make to reflect your own internal expense processes.

Let’s jump in.

### What does this template do?

Handling expenses is one of the most common administrative tasks across businesses in all industries. At the most basic level, this involves:

1. Enabling employees to submit structured expense data.
2. Routing submitted expenses to the appropriate authority for review.
3. Applying defined business logic to approve or reject submissions.
4. Recording outcomes and communicating them to stakeholders.

Our template is built around a simple approval workflow with two types of users.

**Employees** can submit itemized claims relating to specific company **events**.

Their **Managers** can then view, review, and approve these claims.

This is built on top of our internal database.

Let’s break down the core functionality.

#### CRUD Operations

The core of our app is allowing different types of users to carry out defined CRUD operations on our underlying data. This relies on providing users with differing permissions to access data or carry out actions subject to their roles.

Firstly, **Employees** can view expenseable events and create new claims using a dedicated data collection form.

<figure><img src="https://files.readme.io/0068a35a9b25a19e1c1c00ad3ecd5b1627ef03354f56219901fecca0f2e4293d-Expense_Management_Template_1_Gif.gif" alt=""><figcaption></figcaption></figure>

They can also view their own previous claims, including their current statuses, but not the claims of their colleagues.

Within existing claims, they can update the **Status** attribute to **Cancelled**.

Users with the higher **Manager** role inherit all permissions from **Employees**. On top of this, they can view all claims, as well as carrying out update actions, enabling them to approve or reject submissions.

<figure><img src="https://files.readme.io/ed31ff426be74816f288de99392a4598a4db9e99d4b6fd18128b382b8f097a99-Expense_Management_Template_2_Approve_Screen.png" alt=""><figcaption></figcaption></figure>

#### Receipt uploads

To help validate claims and maintain effective records, users can submit receipts with their expense submissions.

Our submission form features a **File Upload** component, which users can use to provide evidence supporting their claims.

This is stored in an **Attachment** column alongside each individual expense item.

#### Approval flows

Our template is based around a simple, single-step approval workflow, with one class of users responsible for authorizing all expenses. This works by updating the **Status** attribute of the original claim to either **Approved** or **Rejected**.

These are performed using **Row Actions**, enabling approvers to respond to expense claims at the press of a button, rather than manually updating the **Status** attribute.

#### Tracking change statuses

Within any approval workflow, tracking the status of submissions as they progress is critical.

**Employees** can view their previous expense claims to check if the status has updated when a reviewer responds. As we’ll see in the next section, they’ll also be notified when the **status** of their claim is updated.

<figure><img src="https://files.readme.io/9b05b3aef3e58b6d33d329084b20813f6626772e60bc3e34ad655300ba88725f-Expense_Management_Template_3_My_Claims_Screen.png" alt=""><figcaption></figcaption></figure>

#### Automated emails

A key time sink within approval processes is handling communications. Specifically, notifying users that something needs their attention. To solve this problem, our template utilizes email automations triggered by specific user actions.

In our workflow, this occurs at the point at which an expense claim receives a response.

When this happens, the user who initially submitted the request is emailed, informing them of the outcome.

This utilizes **Email** actions within the respective **Reject** and **Approve** Row Actions. We’ll examine how **Row Actions** work a little later.

Take a look at our documentation on working with email automations.

### How does it work?

Now that we have an understanding of how our template functions from the perspective of end-users, we can move on to thinking about how it’s actually built.

Within Mware Application Development Framework, app-building is divided into separate concerns. Let’s check out how our template works across each section of the builder.

#### User roles

The custom RBAC tools allow us to configure the exact data, screens, and actions that different users are able to access.

We can add or edit roles using our visual RBAC editor and grant them access to specific tables or Views within Mware Application Development Framework Data section.

<figure><img src="https://files.readme.io/f2f650c96200e47e334d6204a3aff100e861a39b814dd799aa2330509f62ce2d-Expense_Management_Template_4_RBAC_Final_Version.png" alt=""><figcaption></figcaption></figure>

Our template utilizes three distinct roles to control how user cohorts interact with data across our apps.

**Employees** are able to view expensible events, submit itemized expenses, and view the current state of their own previous submissions. They can also update or cancel claims that they have created but not submitted.

**Managers** can view, review, and respond to all **Employees** requests.

**App Admins** have full CRUD access across all data tables.

We’ll see the database Views we’ve created for each in the next section.

#### Data structure

As we said earlier, our application is built on top of Mware Application Development Framework's database.&#x20;

Here’s how our data model works.

**Tables**

Our app utilizes three internal tables:

1. **Events** - storing information relating to company events which expenses can be claimed against.
2. **Expense Claim** - storing information about employees’ expense submissions.
3. **Expense Item** - storing information about individual items within expense claims.

Additionally, our template’s data model uses the **Single User** data type to establish relationships between our data and it’s internal **Users** table, enabling us to assign rows to specific end users.

The Events table stores columns with the following names and data types:

* **Title** - Text,
* **Description** - Long-Form Text,
* **Location** - Text,
* **Start Date** - Date,
* **End Date** - Date,
* **Claim Deadline** - Date,
* **Max Claim** - Number,
* **Claims** - Relationship,
* **Image** - Single Attachment.

The Expense Claim table stores:

* **Summary** - Formula,
* **Submitter** - Single User,
* **Reviewer** - Single User,
* **Status** - Single Select,
* **Date** - Date,
* **Expense Items** - Relationship,
* **Event** - Relationship,
* **Total** - Number,
* **Submitter Notes** - Long-Form Text,
* **Reviewer Notes** - Long-Form Text.

Lastly, our Expense Item table stores the following attributes:

* **Description** - Text,
* **Price** - Number,
* **Category** - Single-Select,
* **Claim** - Relationship,
* **Receipt** - Single Attachment.

We’ll check out some examples of how you might want to modify these later.

**Relationships**

Our three tables are linked using Relationship data type.

This allows us to define how individual entries across each of our tables relate to each other. We can then utilize attributes from directly related tables across **View Joins** within Data section or throughout our Designs and Automations.

Our app relies on two Relationships:

* **A Many-To-One Relationship** from the **Expense Item** table to the **Expense Claim** table.
* **A Many-To-One Relationship** from the **Expense Claim** table to the **Events** table.

<figure><img src="https://files.readme.io/9064c029ce1d91f753439cf91bf6536afc4fa309f816f37dc4ffaf19b027bd17-Expense_Approval_Template_Relationship_Configuration.png" alt=""><figcaption></figcaption></figure>

As we said a second ago, we also use **Single User** attributes to link specific pieces of data to users within it’s internal **Users** table.

There are two instances of this in our app, both within the **Expense Claim** table:

* **Submitter** - An attribute that links each entry to the **User** that initially created the request. The value of this is set automatically, using the _default to current user_ option.
* **Reviewer** - An attribute that assigns records the **Reviewer** of the expense request.

**Views**

A **View** is a subset of our data that’s used to define which rows, columns, actions, or aggregated data specific users can access across one or more tables.

In Mware Application Development Framework, these can be used to centrally configure all aspects of how our end-users interact with connected data within the Data section. Once we’ve created a view, we can use it to autogenerate role-specific screens and automations in just a few clicks.

Our expense approval app utilizes several views. Here’s a summary of the views we’ve created across our three tables.

On top of the **Events** table, we’ve created one view called **Employee Events**. This provides a **Read-Only** version for the **Events** table for users with the **Employee** role, omitting the **Expense Claims** column.

<figure><img src="https://files.readme.io/08a00a2f0f32455b5416180d1222cc2e8b95c5401174e4ecade65d6d04ddb9fd-Expense_Management_Template_5_Employee_Columns.png" alt=""><figcaption></figcaption></figure>

Within the **Expense Claims** table, we’ve created three views, called **Claims to Review**, **Submitted Claims**, and **Unsubmitted Claims**.

The **Claims to Review** View is reserved for the **Manager** role for both **Read** and **Write** actions. It also uses a filtering expression to only include the rows where the **Status** attribute is equal to **Submitted**. All columns are writable except **Submitter Notes**, which is read-only.

<figure><img src="https://files.readme.io/c50338d4b8caebf3e379732b0241e19fd9f2028809516fe0ecb9c0aeea0e361a-Expense_Management_Template_6_Filter.png" alt=""><figcaption></figcaption></figure>

The **Submitted Claims** View provides **Read-Only** access to **Employees**, using a filtering expression to only include rows where the **Status** is **Submitted** and the **Submitter** is equal to the **Current User**.

The **Unsubmitted Claims** View provides the same access for **Employees**, except the **Status** filter is set to **Not Submitted**.

Our **Expense Items** table has a single View, giving **Employees** access only to the rows where the **Submitter** attribute in the related **Expense Claims** table is equal to the **Current User**.

#### User interfaces

Our app features several distinct screens for each role. Check out our docs page on working with screens within Mware Application Development Framework.

We’ve created a home page as an initial screen for all **Employee** users. This allows them to navigate to expensible events. From here, they can navigate to a form where they can start logging itemized expenses.

When they click this, a new **Expense** row is created for the **Employee** to start populating with related **Items**.

<figure><img src="https://files.readme.io/3769f9782b2fda4c75606adba9d828ec3be58337b91c7060105655e80f7e4d0b-Expense_Management_Template_7_Button_Actions.png" alt=""><figcaption></figcaption></figure>

**Employees** can also access a a screen for viewing their previous submissions. This features two tables - one displaying all of their submitted claims and the other for claims that are currently in review.

Take a look at our documentation on [working with data in design](https://docs.budibase.com/docs/working-with-data) to learn more about how this works.

<figure><img src="https://files.readme.io/9e3b30d74661cc1c85d2b45837d017eb46fab7f26a589cddddf33e7616a02cb9-Expense_Management_Template_8_Tables.png" alt=""><figcaption></figcaption></figure>

**App Admins** can access a dedicated CRUD screen for creating, viewing, and editing **Events**.

<figure><img src="https://files.readme.io/728e36bd87a23a585f0534807868ae7504bbc8a0d649a5f321e4984517cdad13-Expense_Management_Template_9_Admin_Events.png" alt=""><figcaption></figcaption></figure>

Managers are able to view the details of submitted claims and either approve or reject them using **Row Actions**, as we’ll see in the following section.

#### Automations

Our app relies on automations for carrying out user actions in a controlled manner, including to create, approve, cancel, and reject claims. Take a look at our automation docs to learn more about automating workflows in Mware Application Development Framework.

The majority of these are created using **Row Actions**. These are user-triggerable automations that relate to a specific row in one of our tables or Views.

Our **Employees** can trigger two automations. The first of these, **Submit**, triggers when a user submits their claim, setting the **Date** to the current timestamp and the **Status** to **Submitted**.

<figure><img src="https://files.readme.io/03245e6f2a20217cdaa4e1f78ad4ed97b0c8a11772ccea3efd27802af8a7a619-Expense_Management_Template_10_Submit_Automation.png" alt=""><figcaption></figcaption></figure>

The other attributes, including the linked **Expense Items,** are then populated with a **Save Row** button action on the form UI itself.

**Employees** can also trigger a **Row Action** called **Cancel**. With a button push, they can update the **Status** of their claim to **Cancelled**, removing it from the review process without deleting the original record.

<figure><img src="https://files.readme.io/576f14982f9d479bd6b8160320d75ef99625d26d362dfd1236e1b69e601ee9f1-Expense_Management_Template_11_Cancel.png" alt=""><figcaption></figcaption></figure>

**Managers** also use **Row Actions** to respond to claims. Depending on whether they hit **Approve** or **Reject**, the **Status** is updated to **Approved** or **Rejected**. Then, the original requestor is emailed, notifying them.

<figure><img src="https://files.readme.io/247cd5bfbd559f28bc90651a53f048eea1cb8f6ca2d093692c72293c095a1295-Expense_Management_Template_12_Approve.png" alt=""><figcaption></figcaption></figure>

### Customizing our change request template

Mware Application Development Framework templates offer fully functional solutions for key use cases. However, they’re also intended as a basis for further customization, enabling you to quickly create apps with your own data and business logic.

Let’s check out some of the key modifications you might want to make to our expense approval template.

#### Updating the data model

The Data section is the core of the app-building experience in Mware Application Development Framework. As we said earlier, our application is built on top of three tables. We can modify this by either swapping in an external database or making changes to our existing one.

To connect to an external database, we’d need to recreate our internal DB’s views and access rules before swapping the existing UIs and automations for your new data.

Alternatively, we can alter the schema of our existing tables using the Data section’s spreadsheet-like interface.

Say we wanted to add a new **Text** column to our **Expense Items** table, to store the **Vendor**, for more detailed records. We could do so using the **Plus** icon.

<figure><img src="https://files.readme.io/4adc6d57800e15bb17eccffc06768208e99ceceef46136612f0af64ea5fdd2ee-Expense_Management_Template_13_Add_Column.png" alt=""><figcaption></figcaption></figure>

#### Adding and altering user roles

User permissions for both accessing data and carrying out actions on it are also configured within the Data section. Another key way we can modify our template is by altering this, including adding or removing roles, or updating the permissions granted to existing ones.

We can add entirely new roles under **Manage Roles**. Mware Application Development Framework offers branching inheritance for custom roles within a visual editor. So, we can create hierarchical roles, as well as ones with entirely separate permissions.

For instance, we might want to create an **Auditor** role that has **Read-Only** access across all data, but no **Write** permissions for any of our tables.

To achieve this, we’d create a parallel branch of permissions.

<figure><img src="https://files.readme.io/a832856d88108ea0c63b9f3f392ecff97052a51e5b44564eb3950792b2dc4ccd-Expense_Management_Template_14_Auditor_Role.png" alt=""><figcaption></figcaption></figure>

We’d then need to create **Auditor** Views of our data tables and assign read-only access to all attributes under the **Columns** tab.

<figure><img src="https://files.readme.io/87e3699b6f647a683b13cfaca61b689996ecc8b4f14a079432d1bc775aca1390-Expense_Management_Template_15_Auditor_View.png" alt=""><figcaption></figcaption></figure>

#### Adding extra screens

We can also modify our template by altering its UIs. We can add or remove screens or make adjustments to the components on existing ones.

Let’s carry on with our example of providing **Read-Only** access to our **Expense Claims** to our new **Auditor** role.

From our **Auditor View**, we’d hit Screens to generate a table UI with side panels.

<figure><img src="https://files.readme.io/2f00f172f4af5081bef9eef1f47694f925c139b4f2c5c392f41f72d5b0a96357-Expense_Management_Template_16_Auditor_Screen_Generation.png" alt=""><figcaption></figcaption></figure>

Then, all we need to do is make a few tweaks to the screen that’s outputted, including removing the **Create** form and setting the **Update** form to **View**.

<figure><img src="https://files.readme.io/12adc35b4be9623aa4ab705b429b3c31cf1849a4ac6533a7174102151e48c7fe-Expense_Management_Template_17_Auditor_Screen.png" alt=""><figcaption></figcaption></figure>

#### Adding your own business rules

Mware Application Development Framework templates are an ideal starting point for creating custom applications to enforce your own business rules while retaining our pre-built interfaces.

For example, we could provide an action to allow our auditor to flag certain submissions as suspicious.

To do this, we’d start by adding a **Row Action** to our new **Auditor View**. We’ll call this **Flag Expense**.

<figure><img src="https://files.readme.io/abc5a5a3bc98ce1d29509bafdfbf1e7979e085d89c1be31ab35ca6bac1dd9f31-Expense_Management_Template_20_Flag_Expense.png" alt=""><figcaption></figcaption></figure>

We’re then brought to the automation editor.

<figure><img src="https://files.readme.io/d4c4086094ad7b2c62c503ed84e7180201ac3928ab8a8ff62c558e8dca7dceba-Expense_Management_Template_21_Flag_automation.png" alt=""><figcaption></figcaption></figure>

Back in the Data section, we’ll update the **Status** column on our **Expense Claims** table, adding an option called **Flagged**.

<figure><img src="https://files.readme.io/af0a2982e6073aaf55c06707efbb9d127d1a89ede87119c0b8ab765bdc292a9e-Expense_Management_Automation_21_Flagged_Status.png" alt=""><figcaption></figcaption></figure>

Within our **Row Action**, we’ll then use the same flow as our existing automation rules, only this time we’ll update the relevant row’s **Status** to **Flagged**.

<figure><img src="https://files.readme.io/a7bda6964ad97beea32650985f1353a3ee87d8bbdae4ee63a9e7ac105b352708-Expense_Management_Template_22_Flag_Flow.png" alt=""><figcaption></figcaption></figure>

Lastly, on our new **Auditor** screen, we’ll add a **Button** to the existing **Table** component, allowing them to trigger our **Row Action**.

<figure><img src="https://files.readme.io/b5ef956298a78e5e9a6d8bdf38260da7833211623988a93f2b275bcfd984985f-Expense_Management_Template_23_Flag_Expense_Button.png" alt=""><figcaption></figcaption></figure>
