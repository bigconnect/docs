# Stat Card

The `Stat Card` component is a really simple UI element to quickly display a number from a database, calculated or hardcoded in a card format, allowing you do display some quick data.

<figure><img src="https://files.readme.io/89b2dc7-stat-cards.png" alt=""><figcaption></figcaption></figure>

All three label properties (Title, Value, Label) support Bindings through Handlebars or JavaScript functions. In the example above you can see this demonstrated in a mixture of ways, there's a calculation happening on an average latitude, there's a number of rows displayed from the database, and there's the current user's `firstname` displayed as the title.

### Adding a Stat Card

The Stat Card is located under `Card > Stat Card` in the design section. A wrapping Data Provider is not required as the Stat Card can also display information about the user, or run JavaScript functions. However, if you want to display data from Data Sources you'll need to add the Stat Card as a child of a Data Provider that has the Data Source connected.

![](https://files.readme.io/e3d98be-Screenshot\_2023-07-04\_at\_11.43.54.png)

### Displaying Data

If you want to display data in a Stat Card, you can use Bindings in the Settings Panel. There are three values you can set using Bindings.

| Setting | Description                                                          |
| ------- | -------------------------------------------------------------------- |
| Title   | The top-most text on the card                                        |
| Value   | The middle text in blue, which is also the biggest text on the card. |
| Label   | The descriptor on the bottom of the card, which is the smallest      |
