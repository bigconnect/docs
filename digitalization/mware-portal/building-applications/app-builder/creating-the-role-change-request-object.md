# Creating the Role Change Request Object

Consider a Human Resources application that collects employee requests to change Roles within the software engineering department: the Role Change Request App. This app starts with the employee submitting a request form. Processing then proceeds to the Current Manager, then to the Receiving Manager, and finally to Human Resources. At each step a different form is presented, and the object accumulates data. By the end of the process the whole data object is complete.

![The Role Change Request object flows through several steps in this Workflow Powered App.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/reference/app-builder/creating-the-role-change-request-object/images/01.png)

Here you’ll create the backing object, form views, and table views. See [Creating the Role Change Request App](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-app) to create the Workflow Powered App itself.

For detailed instructions on creating App Builder objects, form views, and table views, see [Creating a Standard Application](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-a-standard-application). The same object can back a Standard App or a Workflow Powered App. The difference is in how you create the app from the elements of the App Builder framework. You can create the object, form views, and table views from the Objects entry in Applications Menu (![Applications Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → Applications → App Builder.

### [**Object:** Create the object named _Role Change Request_.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#object-create-the-object-named-role-change-request) <a href="#object-create-the-object-named-role-change-request" id="object-create-the-object-named-role-change-request"></a>

### [**Form:** Create the _Employee Request_ form.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#form-create-the-employee-request-form) <a href="#form-create-the-employee-request-form" id="form-create-the-employee-request-form"></a>

| Field Type       | Field Label            | Options                                                                                               | Required | Other Properties |
| ---------------- | ---------------------- | ----------------------------------------------------------------------------------------------------- | -------- | ---------------- |
| Text             | Name                   | Not applicable (NA)                                                                                   | ✔        | NA               |
| Text             | Current Manager’s Name | NA                                                                                                    | ✔        | NA               |
| Text             | Current Role           | NA                                                                                                    | ✔        | NA               |
| Select from List | Requested Role         | <p>Support Engineer<br>Backend Developer<br>Frontend Developer<br>Technical Writer<br>IT Engineer</p> | ✔        | NA               |

### [**Form:** Create the _Current Manager’s Assessment_ form.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#form-create-the-current-managers-assessment-form) <a href="#form-create-the-current-managers-assessment-form" id="form-create-the-current-managers-assessment-form"></a>

| Field Type       | Field Label                                                                               | Options          | Required | Other Properties |
| ---------------- | ----------------------------------------------------------------------------------------- | ---------------- | -------- | ---------------- |
| Single Selection | Current Manager: Is the Transfer Approved?                                                | <p>Yes<br>No</p> | ✔        | NA               |
| Single Selection | Does the employee exhibit skills and experiences requisite with the requested role?       | <p>Yes<br>No</p> | ✔        | NA               |
| Text             | Detail the employee’s skills and characteristics that will make the transfer successful   | NA               | ✘        | Multiple Lines   |
| Text             | Detail the employee’s skills and characteristics that might inhibit a successful transfer | NA               | ✘        | Multiple Lines   |
| Date             | When can the transfer occur?                                                              | NA               | ✘        | NA               |

### [**Form:** Create the _Potential Manager’s Assessment_ form.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#form-create-the-potential-managers-assessment-form) <a href="#form-create-the-potential-managers-assessment-form" id="form-create-the-potential-managers-assessment-form"></a>

| Field Type       | Field Label                                                 | Options                                                                              | Required | Other Properties |    |
| ---------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------ | -------- | ---------------- | -- |
| Single Selection | Receiving Manager: Is the Transfer Approved?                | <p>Yes<br>No</p>                                                                     | ✔        | NA               |    |
| Single Selection | Is there an available role for the employee?                | <p>Yes<br>No</p>                                                                     | ✔        | NA               |    |
| Single Selection | Do you have concerns about the suitability of the employee? | <p>Yes<br>No</p>                                                                     | ✘        | NA               |    |
| Single Selection | Are training resources available?                           | <p>Yes<br>Unnecessary: The employee is already proficient in the subject matter.</p> | ✘        | NA               |    |
| Text             | Comments                                                    | NA                                                                                   | ✘        | Multiple Lines   | NA |

### [**Form:** Create the _HR Assessment_ form.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#form-create-the-hr-assessment-form) <a href="#form-create-the-hr-assessment-form" id="form-create-the-hr-assessment-form"></a>

| Field Type       | Field Label                                                          | Options          | Required | Other Properties |
| ---------------- | -------------------------------------------------------------------- | ---------------- | -------- | ---------------- |
| Single Selection | HR Representative: Is the Transfer Approved?                         | <p>Yes<br>No</p> | ✔        | NA               |
| Single Selection | Does the transfer require a reassessment of the employee’s benefits? | <p>Yes<br>No</p> | ✔        | NA               |

### [**Table:** Create the _Role Change Request Master List_ table.](https://learn.liferay.com/web/guest/w/dxp/building-applications/reference/app-builder/creating-the-role-change-request-object#table-create-the-role-change-request-master-list-table) <a href="#table-create-the-role-change-request-master-list-table" id="table-create-the-role-change-request-master-list-table"></a>

| Column 1 | Column 2       | Column 3                                   | Column 4                                     | Column 5                                     | Column 6                     |
| -------- | -------------- | ------------------------------------------ | -------------------------------------------- | -------------------------------------------- | ---------------------------- |
| Name     | Requested Role | Current Manager: Is the Transfer Approved? | Receiving Manager: Is the Transfer Approved? | HR Representative: Is the Transfer Approved? | When can the transfer occur? |
