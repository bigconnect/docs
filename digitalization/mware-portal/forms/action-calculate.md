# Action: Calculate

Calculate rules let you automatically populate a numeric field by calculating its value based on other fields. Calculations are limited to numeric fields.

To follow the example below:

1. Create 5 Numeric fields called _Animal Weight 1 (US lb.)_, _Animal Weight 2 (US lb.)_, etc.
2. Create a Numeric field called _Total Food Required (US lb.)_.

**Example:** A 16-dog sled team can consume 2,000 US lb. of food during the Iditarod. This equates to about 0.25 lb. of food per lb. of animal, if the race lasts ten days. We’ll use five numeric fields for animal weight instead of sixteen here, because it’s tedious to create sixteen fields, even with the field duplication function. When the form user enters the weight of each animal the Total Food Required field should be calculated based on this simple formula:

```
Animal Weight 1, Animal Weight 2, ... = AW1, AW2, ...
Total Food Required = TFW

(AW1 + AW2 + AW3 + AW4 + AW5) * 0.25 = TFW
```

To configure a calculate rule:

1. Open the Rules tab of the Edit Form page and click the Add (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360030162711/icon-add.png)) button.
2.  Define the rule:

    * If field _Animal Weight 1_ is greater than 0, Calculate the sum of the Animal Weight fields, multiplied by 0.25.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030194072/forms-calculate-rule.png" alt="Figure 1: Build calculate actions with a handy calculator." data-size="original">

    Figure 1: Build calculate actions with a handy calculator.

    * Save the rule.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030194052/forms-calculate-rule2.png" alt="Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does." data-size="original">

    Figure 2: Once a rule is saved, it is displayed so that you can easily understand what it does.

The calculation is defined using the embedded calculator. Use a mix of numeric field values, mathematical operators, and constants to define calculation rules.
