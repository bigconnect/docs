# Forms Configuration Reference

Here’s an overview of all the Forms configurations so you can quickly find a configuration you need.

### Forms Options <a href="#forms-options" id="forms-options"></a>

You can export and import forms between Sites. To access the _Export / Import_ menu, navigate to _Site Administration_ → _Content and Data_ → _Forms_. Click the ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png) icon in the upper right then _Export / Import_.

![You can export or import forms.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/01.png)

See Exporting and Importing Forms and Exporting/Importing Site Pages and Content to learn more.

### Form Settings <a href="#form-settings" id="form-settings"></a>

Each form has its own Settings menu.

To access the Settings menu, choose the desired form (for example, _Guest Survey Feedback_). Click the ![Settings](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-settings.png) icon in the upper right.

#### Form Options <a href="#form-options" id="form-options"></a>

![The Settings menu contains these options.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/02.png)

| Section                | Field                                     | Description                                                                                                                                                                                                                   |
| ---------------------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| General                | Require User Authentication               | Require respondents to sign in before submitting the form.                                                                                                                                                                    |
| General                | CAPTCHA                                   | Require answering CAPTCHA questions when submitting the form.                                                                                                                                                                 |
| General                | Save Answers Automatically                | Autosave the form periodically as the respondent fills it out.                                                                                                                                                                |
| General                | Select a Storage Type                     | Set the storage adapter for managing submitted records; not editable after the form is saved.                                                                                                                                 |
| General                | Select a Workflow                         | Select a workflow definition to review the form submission; workflow is deactivated by default.                                                                                                                               |
| Personalization        | Redirect URL on Success                   | Specify a redirect URL once the form is submitted successfully.                                                                                                                                                               |
| Personalization        | Submit Button Label                       | Change the label of the Submit button to something more appropriate for your specific use case (e.g, _Vote_ or _Register_).                                                                                                   |
| Notifications          | Send an Email Notification for Each Entry | Specify whether an email notification sent to the form creator each time a form record is submitted.                                                                                                                          |
| Submissions (7.4 Only) | Limit to One Submission per User          | Allow only authenticated users to fill out the form, and prevent multiple form submissions. Users visiting the form after submitting see a warning message. You can customize this message.                                   |
| Submissions (7.4 Only) | Show Partial Results to Respondents       | Allow respondents to view the current Forms Report data for the form. Enabling this warns the user against submitting sensitive information.                                                                                  |
| Submissions (7.4 Only) | Expiration Date/Never Expire              | Set the date when the form expires. After this date, no submissions are allowed. Form users see an expiration notification when accessing the form. You must disable the _Never Expire_ setting to choose an expiration date. |

#### Email Notifications <a href="#email-notifications" id="email-notifications"></a>

Here you can configure the Forms app to send a notification email each time a form entry is submitted. You should set up a mail server first; see Configuring Mail to learn more.

![You can add notifications to a form.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/03.png)

| Field        | Description                                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------------------------- |
| From Name    | The sender’s name; this could be the Site name, the form name, or anything else informative to the recipient. |
| From Address | The sender’s email address; You can use `noreply@example.com` to prevent recipients from replying.            |
| To Address   | The recipient’s email address (e.g., `test@example.com`)                                                      |
| Subject      | An informative subject line tells the recipient what happened.                                                |

To learn more, see Configuring Form Notifications.

### Form Widget Configuration <a href="#form-widget-configuration" id="form-widget-configuration"></a>

You can configure the _Form_ widget deployed to a Site Page. To access the _Configuration_ menu, click the ![Options](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-app-options.png) icon next to the widget title → _Configuration_.

#### Setup <a href="#setup" id="setup"></a>

Here you can choose the desired Form to be used in this widget.

![Select the desired form to be used in this widget.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/04.png)

#### Sharing <a href="#sharing" id="sharing"></a>

Here you can embed this application on other platforms besides the portal.

![Select the desired platform where the Forms app can be embedded in.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/05.png)

#### Scope <a href="#scope" id="scope"></a>

Here you can change the widget’s scope from Global, Site, or Page.

![Select the desired scope for the Form Widget.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/06.png)

### Instance Settings <a href="#instance-settings" id="instance-settings"></a>

The Forms configuration entry in Instance Settings is identical to the Forms entry in System Settings (below); the instance settings override the system settings, which will set the default values across the system. See Understanding Configuration Scope for more information.

For a description of each configuration property, see System Settings.

### System Settings <a href="#system-settings" id="system-settings"></a>

There is currently only one system level Forms configuration entry. To configure the properties in the portal UI,

1. Go to the _Global Menu_ (![global icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_.
2. Click _System Settings_.
3. Click _Forms_ under the _Content and Data_ section.

![Configure Forms in the System Settings menu.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/forms-configuration-reference/images/07.png)

Alternatively, configure the backing `DDMFormWebConfiguration` service with a `.config` file named

```log
com.liferay.dynamic.data.mapping.form.web.internal.configuration.DDMFormWebConfiguration.config
```

Place the file in `Home/osgi/configs`. See Using Configuration Files for more information.

| Field                                                                                  | Description                                                                                                                                                                                                                                                                                                                                             |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Autosave Interval (`autosaveInterval`)                                                 | Sets the value in minutes to auto-save a form; setting 0 disables auto-save.                                                                                                                                                                                                                                                                            |
| CSV Export (`csvExport`)                                                               | Determines whether administrators can download Form Entries as CSVs.                                                                                                                                                                                                                                                                                    |
| Default Display View (`defaultDisplayView`)                                            | Sets how the Forms are displayed in the search container.                                                                                                                                                                                                                                                                                               |
| Guest Upload File Extensions (`guestUploadFileExtensions`)                             | Enter the comma-separated list of accepted file extensions. By default the following list is supported: `doc`, `docx`, `jpeg`, `jpg`, `pdf`, `png`, `ppt`, `pptx`, `tiff`, `txt`, `xls`, and `xlsx`.                                                                                                                                                    |
| Guest Upload Maximum file Size (`guestUploadMaximumFileSize`)                          | Set the maximum file size Guest User can upload via the Upload field. This is configured for authenticated Users in the Documents and Media application’s configuration, and system-wide limits are defined in the Upload Servlet Request System Settings entry, in the field named Overall Maximum Upload Request Size. By default the limit is 25 MB. |
| Maximum Repetitions for Upload Fields (`maximumRepetitionsForUploadFields`)            | If the field is configured as repeatable, set maximum number of times the field can be added to the form by the User (applies equally to Guest Users and logged in Users). By default the limit is 5.                                                                                                                                                   |
| Maximum Submissions for Guest Upload Fields (`maximumSubmissionsForGuestUploadFields`) | Set the maximum number of times a Guest User can submit a form that includes a Guest-enabled Upload field. The Guest User’s IP address is used to keep track of submissions. By default the limit is 5.                                                                                                                                                 |
