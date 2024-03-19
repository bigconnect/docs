# Building Forms with Conditional Fields

Forms can become dynamic forms where the answer to one question may allow the respondent to skip the next three questions, or require the respondent to answer additional questions. Thus, each field can become a conditional field. Once you have decided which fields are conditional, use a Form Rule to determine what happens based on the answer to that question (for example, skips page 3 and goes to page 4 on a multi-page form).

Each Form Rule contains a Condition and an Action; see the [Form Rules Overview](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/form-rules-overview) article to learn more.

You should [create a form](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/creating-forms) that has all the fields first before adding the Form Rules. (See the [Forms Field Types Reference](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/forms-field-types-reference) for more information about each field type.) Once all the fields have been created, add the Form Rules. Here is a list of Form Rules:

* [Show-Hide Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#show-hide-rule)
* [Enable-Disable Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#enable-disable-rule)
* [Jump-to-Page Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#jump-to-page-rule)
* [Autofill Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#autofill-rule)
* [Require Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#require-rule)
* [Calculate Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#calculate-rule)

### [Show-Hide Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#show-hide-rule) <a href="#show-hide-rule" id="show-hide-rule"></a>

Use the _Show-Hide_ Rule if you want respondents to answer additional questions. For example, a camp registration form has the question whether the camp participant is over 18 years old. If the answer is _NO_, the form displays an additional field asking for the guardian’s name. Otherwise, the field asking for the guardian’s name is hidden if the participant answers _YES_.

See the [Using the Show-Hide Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-show-hide-rule) article to learn more.

### [Enable-Disable Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#enable-disable-rule) <a href="#enable-disable-rule" id="enable-disable-rule"></a>

Like the Show-Hide Rule, the _Enable/Disable_ Rule makes a field editable based on one or more conditions. This is useful for entering optional information or gathering data from only users who fall under a particular group. For example, a campsite or a doctor’s office might ask people to list medications or allergies that might affect treatment. This field is editable only if the user selects Yes.

See the [Using the Enable-Disable Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-enable-disable-rule) article to learn more.

### [Jump to Page Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#jump-to-page-rule) <a href="#jump-to-page-rule" id="jump-to-page-rule"></a>

Another option is to use the _Jump to Page_ Form Rule. As its name indicates, respondents can jump to a different page in the Form based on their answers. This is useful if some pages do not apply to all the form’s users.

See the [Using the Jump-to-Page Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-jump-to-page-rule) article to learn more.

### [Autofill Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#autofill-rule) <a href="#autofill-rule" id="autofill-rule"></a>

The _Autofill_ Rule filters a form field’s options based on pre-configured conditions. One common use is populating states and provinces, countries and regions, and then applying a conditional filter. For example, respondents select their geographic region (Europe, North America, Asia, etc.), then select a country (Germany, USA, Malaysia, etc.). You should first connect a [data provider](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/data-providers/data-providers-overview) before using the _Autofill_ Rule. See [Using the REST Data Provider to Populate Form Options](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/data-providers/using-the-rest-data-provider-to-populate-form-options) to learn more.

### [Require Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#require-rule) <a href="#require-rule" id="require-rule"></a>

You can use a _Require_ rule to make a field required based on one or more conditions. Require Rules work in association with other Rules, such as the _Show_ Rule. For example, a car rental form has a section where respondents indicate if they are applying a discount to the rental. If the answer is _YES_, the respondents must enter the promo code in order to get the discount.

See the [Using the Require Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-require-rule) article to learn more.

### [Calculate Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/building-forms-with-conditional-fields#calculate-rule) <a href="#calculate-rule" id="calculate-rule"></a>

Create _Calculate_ Form rules that populate a numeric field by calculating its value based on other fields. For example, a Calculate rule can calculate a camp or conference’s total registration costs by multiplying the number of attendees and the registration fee (17 people at $130 each).

IMPORTANT

Calculations are limited to numeric fields.

See the [Using the Calculate Rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-calculate-rule) article to learn more.

\
