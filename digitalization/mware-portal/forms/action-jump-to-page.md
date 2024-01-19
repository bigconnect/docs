# Action: Jump to Page

Use a Jump to Page rule to navigate automatically to a specific page in the form based on one or more conditions. This is useful if some pages don’t apply to all the form’s users. Even fields marked as required on the skipped pages can be successfully skipped using this rule.

This action doesn’t appear in the rule builder unless the form has multiple pages.

To follow the example here,

1. Create a second form page called _Team Information_.
2. On the _first_ form page, create a single selection field labeled _Are you a returning racer, with the same team?_ with two options: _Yes_ and _No_.
3. Create a text field on the _second_ form page called _Animal Name_.
4. Create a third form page called _Final Confirmation_.

**Example:** There’s a question on the _Team Information_ page of the dog sled race registration form that asks _Are you a returning racer with the same team?_ If you select _Yes_, when you click the form’s _Next_ button, you skip to the final page of the form, since there’s no need to fill out your animal’s name again. Their monogrammed T-shirts will be ready at the start of the race.

To configure the Jump to Page rule,

1. Open the Rules tab of the Edit Form page and click the Add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193932/icon-add.png)) button.
2.  Define the rule:

    * If field _Are you a returning racer, with the same team?_ is equal to the Value _Yes_, Jump to Page _Final Confirmation_.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193872/forms-jump-to-page.png" alt="Figure 1: Build form rules quickly by defining your conditions and actions." data-size="original">

    Figure 1: Build form rules quickly by defining your conditions and actions.

    * Save the rule.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030193912/forms-jump-to-page2.png" alt="Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does." data-size="original">

    Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does.

Once the form User fills out the first page and clicks _Next_, the rule condition will evaluate hte answer to the field and either proceed to the next page or take the action of skipping to the page inidicatedd in the rule.

If you use an _is not equal to_ condition for form fields on two different pages, the condition is checked after leaving the page of the first form field, and evaluates to _true_ since there’s a value in the first field and no value in the second field. It’s best to use this condition with fields existing on the same page.
