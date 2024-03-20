# Using Multiple Display Page Templates to Create Multi-Step Applications

Display page templates display content at a dedicated URL. To use this URL and view the content, you have to first define a template to display it.

Generally, a default display page template is set for a specific content type. You can, however, associate multiple display page templates with the same content type and map any of those options to links. This means users can create applications (e.g., forms mapped from custom objects) with multiple steps. By using multiple display templates, you can create forms with multiple steps, because each step is linked to a different display page template.

Using display page templates, objects, and Form Container fragments, you can create a page that allows users to add an object entry and edit it in a different page. To do that, it is necessary to create a custom object, a display page template to edit the object entry, and a content page where the user can enter a new object entry and view a list of existing object entries.

### Tasks Custom Object <a href="#tasks-custom-object" id="tasks-custom-object"></a>

1. Create a custom object and label it _Task_.
2. Click on the _Task_ object.
3. Under the Fields tab, add a text-type Field labeled _Description_.
4. Under the Actions tab, add an _Action_ and label it _Edit_.
5.  Under Action Builder, select _Standalone_ as the Trigger.

    This option allows the action to be mapped to a button.
6.  Select _Update an Object Entry_ as the Action.

    This option updates the object entry using a selected value whenever the trigger is activated.
7.  Click _Add_ in Values and choose the _Description_ field. Under New Value, enter _description_ (the name of our field). This replaces the object entry’s Description field value with the new value.

    ![Create the Edit action to update the object description value](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/01.png)
8. Add an _Error Message_ and click _Save_.
9. Under the Details tab, select a _Scope_ for the object and click _Publish_ at the top right corner.

Your object is now published, and it can be used to generate a form using a Form Container fragment in the content page or the display page template.

![Creating the Task object](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/02.gif)

### Content Page to Display the Application <a href="#content-page-to-display-the-application" id="content-page-to-display-the-application"></a>

1. Create a content page. Use the Blank template and name the page _List of Tasks_.
2. Add a _Grid_ fragment to the page. Click on it and set the _Number of Modules_ to _two_ on the right panel.
3.  In the Fragments and Widgets sidebar, look for the Form Container fragment. Drag and drop it on the left grid module, click _None_, and map it to the Task object.

    On the left grid module, users can add a new entry to the object.
4.  Click on the _Form Container_ fragment and set the Success Interaction to _Stay in Page_.

    Once users add an entry to the object, the page is updated but nothing else happens.
5. Add a _Heading_ fragment to the right grid module. To edit it, double click on it. Enter _List of Tasks_ as the heading.
6.  Add a _Collection Display_ fragment to the right grid module under the Heading. Click on the fragment. Under the General tab, click on _Collection_.

    On the right grid module, users can see a list of Tasks.
7.  Under the Collection Providers tab, select _Tasks_.

    The Collection Provider displays the entries in Tasks in any way you design it.
8.  Add a _Grid_ to the Collection Provider and set the _Number of Modules_ to _two_ on the right panel.

    ![The grids are marked in red.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/03.png)
9.  Add a _Heading_ fragment to the left grid module in your collection display and click on it twice. Under the Mapping tab, choose Description as the field.

    This action maps the text added to the Description field as the Heading content.
10. Add a _Button_ fragment to the right grid module in your collection display and click on the text twice to edit it. Change the text to _Edit_.
11. Publish the page

Although you can view the page, add an object entry, and see the list of tasks updated, it is still incomplete. The Edit display page template needs to be created and you need to map the Edit button to the page in order to direct the user to the Edit display page template.

![Creating a content page to display the application](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/04.gif)

### Edit Display Page Template <a href="#edit-display-page-template" id="edit-display-page-template"></a>

1.  Create a display page template, name it _Edit_, and select _Task_ as the content type.

    The display page template is now associated with the Task object.
2.  In the Fragments and Widgets sidebar, look for the Form Container fragment. Drag and drop it into the page, click _None_, and map it to the Task object.

    A form is generated automatically using the fields from the selected object definition.

    WARNING

    Form fields from master page templates are not populated on display pages. If you add a form container linked to an object in the display page’s master page, it is not possible to visualize the form fields related to the object.
3.  Select the Form Container. Set its Success Interaction to _Go to Page_, and select the List of Tasks page.

    Once the form is submitted, the List of Tasks page appears.
4.  Click the _Submit_ button twice. Under Field, select the _Edit_ action.

    When users click the submit button, the List of Tasks page appears, and they also activate the Edit action that replaces the previous Description value with the newly added one.

TIP

Form Container fragments enabled in display page templates. You can also edit the Form Container fragment and use only a subset of input fragments. Then, when the object entry is updated, only the available fields are updated.

Edit the fragments as you see fit and save the Form Container fragment as a composition to be used in both content pages and display pages.

![Creating the Edit display page template](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/05.gif)

### SEO Considerations About the Display Page Templates <a href="#seo-considerations-about-the-display-page-templates" id="seo-considerations-about-the-display-page-templates"></a>

If the display page is marked as default,

* The `Sitemap.xml` configuration is enabled, and customers can customize it according to their requirements (there’s no breaking change).
* When the default display page is mapped as a specific display page, a `<link>`element is added with the attribute `rel="canonical"` to the `<head>` section, pointing to the URL of the asset as the canonical page.

These changes are made automatically if the display page is not marked as default:

* The display page is not indexed in the `Sitemap.xml`.
* Robots configuration is disabled.
* The value `"noindex, nofollow"` is set in the Robots configuration.
* When another page has a link to the display page template, add the attribute `rel="nofollow"` to the `<a>` tag on the template.
* If a display page marked as default exists for the same object, a `<link>` element is added with the attribute `rel="canonical"` to the `<head>` section, pointing to the default display page template as the canonical page.

![SEO configuration page for the display page template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/06.png)

### Resulting Application <a href="#resulting-application" id="resulting-application"></a>

Before using the resulting application,

1. In your List of Tasks content page, click on the _Edit_ button twice.
2.  Under the Link tab, choose _Mapped URL_ under Link and choose the _Edit_ display page template.

    When users click the Edit button, they are directed to the Edit display page template, where they can edit the entry and resubmit the form, returning to the List of Tasks content page.
3. Publish the List of Tasks page.

Now you can access the List of Tasks page and test the application. Add a task description on the left side of the page. The page updates, and you can see the new task description on the right in your collection display.

If you click on the Edit button, you are directed to the Edit display page where you can edit the entry. When you click submit, you are directed back to the List of Tasks page.

TIP

You can create a display page template as an initial step and configure the Form Container’s Success Interaction to Go to Entry Display Page, pointing to the next display page template. This creates a chained multi-step form.

![Testing your resulting application.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/displaying-content/using-display-page-templates/using-multiple-display-page-templates-to-create-multi-step-applications/images/07.gif)
