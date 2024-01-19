# Action: Show and Hide

With a show and hide rule, you use one or more conditions to determine whether to show or hide a field if the condition evaluates to _true_.

To set this example up, add these fields to a form:

* _I am 18 Years Old or Older_, a required single selection field with two options: _Yes_ and _No_.
* _Legal Guardian Email Address_, a text field that accepts valid email addresses (use text field validation to dictate input type).

**Example:** If you’re under 18 years old, you need the approval of a legal guardian to drive a sled in a sled dog race (even if you’re racing chickens, not dogs). The form for registering your chicken team asks you the age of the driver. If you enter a number less than 18, the Legal Guardian Email Address field appears.

To configure a Show/Hide rule,

1. Open the Rules tab of the Edit Form page and click the Add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193732/icon-add.png)) button.
2.  Define the rule:

    * If the _I am 18 years old or older_ field is equal to the Value _No_, show the _Legal Guardian Email Address_ field.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162431/forms-rule-development.png" alt="Figure 1: Build form rules quickly by defining your conditions and actions." data-size="original">

    Figure 1: Build form rules quickly by defining your conditions and actions.

    * Save the rule.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162451/forms-rule-list.png" alt="Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does." data-size="original">

    Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does.

Now the _Legal Guardian Email Address_ field is only displayed in the form if the user selects _No_ in the _I am 18 years old or older_ field.

Show rules let you keep a field hidden until some condition is met.
