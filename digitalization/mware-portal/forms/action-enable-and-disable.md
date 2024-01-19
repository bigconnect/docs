# Action: Enable and Disable

Use an enable/disable rule to make a field editable based on one or more conditions.

**Example:** Part of the race registration fee pays for dog food. You don’t have to feed your chicken team with the provided dog food though. There’s a single selection field that asks _Would you like to use the provided dog food?_. If you select _Yes_, you can select how much food, in US pounds, you’ll need for your team throughout the race. Since you’re racing chickens, you’ll select _No_, and the _Amount (US lb.)_ field is disabled.

To follow the example, add a single selection field _Would you like to use the provided dog food?_ with two options: _Yes_ and _No_.

Add a numeric field called _Amount (US lb.)_ and make it an Integer. Use field validation to make sure it’s not greater than _100_.

To set up the enable/disable rule,

1. Open the Rules tab of the Edit Form page and click the Add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030162511/icon-add.png)) button.
2.  Define the rule:

    * If the _Would you like to use the provided dog food?_ field is equal to _Yes_, enable the _Amount (US lb.)_ field.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162491/forms-enable-rule.png" alt="Figure 1: Build form rules quickly by defining your conditions and actions." data-size="original">

    Figure 1: Build form rules quickly by defining your conditions and actions.

    * Save the rule.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030162531/forms-enable-rule2.png" alt="Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does." data-size="original">

    Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does.

Now users can fill out the amount of dog food they’ll need only if they specify that they do indeed want to use the provided food.
