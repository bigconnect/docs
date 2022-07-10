# Form Rules

Chickens don’t follow rules well, but dogs do. If you’re skeptical, try teaching your chicken to sit on command or herd sheep. Better yet, get a team of chickens to pull a sled in the [Iditarod](http://iditarod.com/). The Forms application is much more like the dog than the useful (southwestern omelet anyone?) but untrainable chicken, and it’s only getting more trainable as time passes.

Form rules are a good example of the trainable nature of the Forms application. With form rules, you can train your form fields to behave as you wish. There are several things you can make them do:

**Show/hide**

Based on a predefined condition, set the visibility of a form field.

**Enable/disable**

Use a predefined condition to enable or disable a field.

**Require**

Use a predefined condition to make a field required.

**Jump to Page**

Based on user input, skip over some form pages directly to a relevant page. _This rule doesn’t appear in the rule builder until a second page is added to the form_.

**Autofill with Data Provider**

Use a data provider to populate fields when a condition is met in another field.

**Calculate**

Populate a field with a calculated value using data entered in other fields.

Form rules are for changing fields and form elements by acting on conditions.

_If \[condition] do \[action]._

Once you know how to create forms, add and configure fields, and then publish forms, come back here and learn about form rules.

### The Anatomy of a Form Rule <a href="#the-anatomy-of-a-form-rule" id="the-anatomy-of-a-form-rule"></a>

Each rule consists of one or more conditions and actions.

_Conditions_ determine whether any actions are executed.

_Actions_ determine what happens if the condition is met.

Rules are stored in the database in JSON format by default.

### Creating Form Rules: Rule Builder <a href="#creating-form-rules-rule-builder" id="creating-form-rules-rule-builder"></a>

Once you create a form and lay out its fields, you’re ready to set up rules in your form:

1. Save the form.
2. Open the Rule Builder by clicking the _Rules_ tab at the top of the _Edit Form_ screen.
3. In the rule builder view, you can now begin developing your form rule. Click the _Add_ button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193692/icon-add.png)) to get started.

![Figure 1: The Rule Builder gives you a handy interface for creating dynamic form rules.](https://liferay-support.zendesk.com/hc/article\_attachments/360030193672/forms-rule-builder.png)

Before looking at each type of rule condition and action you can use to develop rules, consider the _OR_ selector box at the right side of the _Condition_ (it’s grayed out and unusable at first). You can choose _OR_ or _AND_ here, depending on what relationship the conditions should have with the action.

ORThe action is triggered if _any_ of the conditions you specify evaluates to _true_ANDThe action is triggered only if _all_ the conditions you specify evaluate to _true_

This box becomes usable once you click the Add button (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030193692/icon-add.png)) to add an extra condition.

### Conditions <a href="#conditions" id="conditions"></a>

Conditions are the gatekeepers of form rules. If the condition’s _if statement_ evaluates to _true_, the action is triggered. If it evaluates to _false_, no action happens.

A condition checks whether one field’s value

* _Is equal to_ a specific value or another field’s value.
* _Is not equal to_ a specific value or another field’s value.
* _Contains_ a specific value or another field’s value.
* _Does not contain_ a specific value or another field’s value.
* _Is empty_. This assumes you want to do something if a field _is_ empty.
* _Is not empty_. This assumes you want to do something as long as a field is _not_ empty.

One exception to this is the User condition, which is the last option in the Condition dropdown menu.

The User condition doesn’t act on a field at all. It checks whether a User belongs to a certain role. For example, if the condition

If `User` _belongs to_ `Administrator`

evaluates to _true_, an action is triggered.

A condition is the gateway into a form rule, but actions define what actually happens when the condition evaluates to _true_. The remaining articles discuss the available actions and demonstrate their use.
