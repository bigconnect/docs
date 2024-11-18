# Formula

Formula columns allow you to generate values from an expression.\
For example, you may wish to concatenate first and last names.

Just like with the other data types, start by creating a new column. Select Formula and you will be presented with a textbox. Use Handlebars to construct the expression you are interested in and hit the Save Column button.

If you want some assistance when writing your expression you can click the lightning bolt icon to the right of the formula field. This will open a modal where you can select the fields and handlebars expressions you want directly.

![](https://files.readme.io/b39a364-formula.png)

### Static formulas

The default type of formula is **Dynamic**, which means the formula will be calculated every time data is _read_.&#x20;

Static formulas on the other hand are calculated and every time a row is _created_ or _updated_. Additionally, the static formulas values are themselves saved into the DB which allows them to be used for filtering.

External databases only support dynamic formulas.

<figure><img src="https://files.readme.io/46205c6-Screenshot_2023-09-05_at_08.50.44.png" alt="Saving a &#x27;Static&#x27; formula"><figcaption><p>Saving a 'Static' formula</p></figcaption></figure>

### Fields from relationships

A common use case for formulas is to access fields from a linked table.\
For example, you may have one manufacturer for many products, and in your products table you wish to display the manufacturers industry.

To do this, create a formula column in the products table and then grab the Industry field from the first linked manufacturer:

<figure><img src="https://files.readme.io/e56ee86-Screenshot_2022-04-14_at_22.22.32.png" alt="" width="375"><figcaption></figcaption></figure>

\


<figure><img src="https://files.readme.io/7b00adeb552b84a28993ff8eb891b02d34730cd11bd52cc4b2b6fa341dd1c6be-Screenshot_2024-09-19_at_13.33.19.png" alt=""><figcaption></figcaption></figure>

### Tutorial: Test scores using sum

Show the total score of a test by summing each of the question scores.

**Steps**

1. Create a 'Questions' table and a 'Tests' table.
2. Establish a One Test -> Many Questions relationship.
3. Add a _Number_ type column named 'Points' to the 'Questions' table.

<figure><img src="https://files.readme.io/63d7ba7f008a81535c1161dee8a6883343686fff35728789dad0f0c9d9974f0b-Screenshot_2024-09-19_at_15.33.39.png" alt=""><figcaption></figcaption></figure>

4. In the 'Tests' table, create a new _Formula_ column named 'Total'. Enter the binding:

Text

```
{{ sum (pluck Questions 'Points') }}
```

The first handle bar expression to get evaluated is the **pluck** function. This takes the array of related _Questions_ and maps it to an array of their 'Points' values.\
Then the sum function simply adds up each value from that array of points.

The equivalent in JavaScript is:

JavaScript

```javascript
return $("Questions").map(q => q.Points).reduce((a, b) => a + b)
```

<figure><img src="https://files.readme.io/98bdc22a3d3699b93427785437458d0a9d14bf74091a513a9839c6c5a83093ad-Screenshot_2024-09-19_at_15.33.50.png" alt=""><figcaption></figcaption></figure>

> ### 📘Number typing
>
> If you want to numerically sort on a formula field or use numeric operators in a filter for a static formula field, then you must use JavaScript bindings.\
> Handlebars always return a string type.
