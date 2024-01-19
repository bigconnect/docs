# Workflow and Forms

The workflow engine is for sending a submitted asset through a workflow process before it’s published. Most assets are configured to use workflow at the instance or Site level.

![Figure 1: Workflow is enabled in the Control Panel or in Site Administration for most portal assets.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194432/forms-workflow-configuration.png)

Forms are different, so they don’t appear in the above image. There are so many use cases for forms, and there could be so many per site, that a site- or instance-scoped workflow configuration won’t serve your needs well. Instead, configure workflow for _each form_ separately.

### Enabling Workflow in a Form <a href="#enabling-workflow-in-a-form" id="enabling-workflow-in-a-form"></a>

To enable workflow in a form,

1. Open the form’s editor by opening the Menu, selecting your Site, navigating to _Content \&Data_ → _Forms_, and clicking on the form you want.
2. Click the Options button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030194452/icon-options.png)) and choose _Settings_.
3.  The Settings window has a _Select a Workflow_ drop-down. Find the workflow you want, select it, and then click _Done_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162991/form-settings.png" alt="Figure 2: Enable workflow for each form in its Settings window." data-size="original">

    Figure 2: Enable workflow for each form in its Settings window.

### Testing the Workflow <a href="#testing-the-workflow" id="testing-the-workflow"></a>

Test the workflow process:

1. Add the form to a page.
2. Click _Submit for Publication_ to submit the form entry.

Next go find the form entry in the Forms application:

1. Go back to the Forms application in the Menu in your Site’s _Content & Data_ section.
2.  Click the Form’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030194412/icon-actions.png)) and select _View Entries_.

    The entry is currently marked _Pending_.

Now approve the form record:

1. Navigate to _My Workflow Tasks_ in the User Personal Menu.
2. Click the _Assigned to My Roles_ tab.
3. Click on the form entry.
4. Click the Actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030194412/icon-actions.png)) and choose _Assign to Me_.
5. Click _Done_.
6. Click the Actions button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030194412/icon-actions.png)) again, then click _Approve_.
7. Click _Done_ again.
8. Navigate back to the View Entries screen for the form, and now the entry is marked as _Approved_.

![Figure 3: Each entrys status is visible in the Forms applications Form Entries screen.](https://liferay-support.zendesk.com/hc/article\_attachments/360030163011/forms-view-entries-status.png)
