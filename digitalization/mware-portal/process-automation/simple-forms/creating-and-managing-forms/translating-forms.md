# Translating Forms

Forms can be be translated and presented in any language that is currently enabled on the current site.

To add a translation:

1. Begin by viewing an existing form (e.g., _Guest Feedback Survey_) to be translated.
2. Click the (+) sign next to the language dropdown menu.
3.  Select a language to translate the form to.

    ![Select the desired language.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/translating-forms/images/01.png)
4.  A new language tab is added to the form. Translation text entered does not override the default language.

    ![View the additional form in the next tab.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/translating-forms/images/03.png)
5. Enter a new title.
6.  To modify a field’s label, click on the field. This opens the field configuration menu.

    ![Translate the field's labels.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/translating-forms/images/02.png)
7. Enter the translation.
8. Continue until all the desired fields have been translated.
9. Click _Save Form_ when finished.

NOTE

Each form field’s messages are editable in the translation, using the field settings sidebar. Other changes made to the field’s settings apply to all translations (e.g., marking the field as required) no matter which translation they are made for. The Accept if Input [validation setting](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/validation-and-masking) is a little different: it’s only editable in the default language, although the corresponding Value and Error Message fields are translatable.

### [Viewing Translated Forms](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/translating-forms#viewing-translated-forms) <a href="#viewing-translated-forms" id="viewing-translated-forms"></a>

Authenticated users that have configured a matching default language for the site now see the translated form by default.

![Authenticated users whose language settings are localized will see the form in their language.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/translating-forms/images/04.png)

If accessed in the [Form widget on a Liferay DXP page](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/sharing-forms-and-managing-submissions/sharing-forms), the Form is displayed in the User’s language automatically. If there’s no translation available in a user’s selected language, the default language of the Form is displayed.

### [Viewing Translated Form Records](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/translating-forms#viewing-translated-form-records) <a href="#viewing-translated-form-records" id="viewing-translated-form-records"></a>

Starting in Liferay 7.4, The language of each form record is recorded in two locations for administrators: [exported form data](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/exporting-and-importing-forms) and from within the View Entries screen for a form.

To view the entries and their languages in the Liferay Forms application,

1. Open the Site Administration menu and go to Content & Data → Forms.
2.  Open the Actions (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) menu for a form and click _View Entries_.

    ![Administrators can easily see the form record language.](https://resources.learn.liferay.com/images/dxp/latest/en/process-automation/forms/creating-and-managing-forms/translating-forms/images/05.png)

\
