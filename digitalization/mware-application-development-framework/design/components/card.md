# Card

Use the Card component to display lists of data to your users. Cards can contain content, images, and actions, and are a great alternative to the table component; especially, if you want to present an image for each card. You can switch between vertical and horizontal variants of the card using the settings panel.

Cards are highly flexible and have a lot of configuration options. We'll dive into those below

<figure><img src="https://files.readme.io/0b85713-design-like-a-pro.gif" alt=""><figcaption></figcaption></figure>

> ### 📘Cards Block
>
> Want to quickly convert your data to a list of cards? Use the Cards Block instead

### Layout

The card component has two different layout modes. Vertical and Horizontal. The vertical mode has every component aligned vertically, whereas the horizontal mode has the image left of the text elements.

For example purposes, I've loaded a list of countries into the database, with flags as images connected to it. In vertical mode, this looks like the following screenshot

#### Vertical Mode

<figure><img src="https://files.readme.io/acd2e6c-cards-vertical.png" alt="2126"><figcaption><p>Vertical Cards Mode</p></figcaption></figure>

As you can see, there's 3 items per row. However, this is purely dependent on the screen size. When you change the width of your browser window this can easily change to 1 (narrow or mobile), or 4 (or more) when you make your window wider.

The vertical mode is enabled by default, but in case horizontal mode is enabled, all you have to do is deselect "Horizontal" from the Settings Panel.

The order of elements visible in a card is from top to bottom, Image, Title, Subtitle and Description.

#### Horizontal Mode

To enable horizontal mode, you need to check `Horizontal` in the Settings Panel.

![](https://files.readme.io/8158dd0-horizontal-mode.png)

Immediately, the design will change into horizontal mode, which looks like the following screenshot

<figure><img src="https://files.readme.io/69d8f87-cards-horizontal.png" alt="2116"><figcaption><p>Horizontal Cards Mode</p></figcaption></figure>

As you can see, the image has shifted to the left of the text elements, where the text elements themselves have not changed.

### Card Elements

The card consists of several default elements, which are bindable through Handlebars or JavaScript functions. Read more about binding in our [Bindings](https://docs.budibase.com/docs/bindings) documentation. All elements can be configured in the settings panel.

| Element     | Description                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------- |
| Title       | The top-most text element inside a card, printed as the largest font and bold                  |
| Subtitle    | Directly below the title                                                                       |
| Description | Below the subtitle, in the smallest font.                                                      |
| Image       | The image which can be displayed on top, or on the left of the card (depending on layout mode) |

### Button

Next to the default elements, there's an optional button element. You can enable the button by checking the `Button` in the settings panel

<figure><img src="https://files.readme.io/e2230a5-button-settings.png" alt="" width="375"><figcaption></figcaption></figure>

The button text can be configured through [Bindings](https://docs.budibase.com/docs/bindings), or simply text as demonstrated above. The button will then be added to the card as displayed below

<figure><img src="https://files.readme.io/beda3aa-cards-with-button.png" alt=""><figcaption></figcaption></figure>

Right now the button doesn't do anything yet, when you click it, but that can be configured through the `Define Actions` button in the settings panel.

> ### 👍Define Actions
>
> To learn more about defining actions, read the Actions documentation

### Tutorial

This tutorial is primarily going to focus on the card component itself as the `Card Block` variant has already been discussed above. This tutorial will be broken down into steps, going from a blank screen to multiple cards and related details shown. Lastly, this tutorial will be using the inbuilt DB as well as the sample data which it provides.

1. Add a data provider component (In this case I'm going to use the Employee's table).
2.  Add a repeater component nested inside the data provider.\


    <figure><img src="https://files.readme.io/f62036f-small-Screenshot_2023-05-17_at_12.49.19.png" alt="" width="375"><figcaption></figcaption></figure>
3.  Add the card component nested inside the new repeater which you created.\


    <figure><img src="https://files.readme.io/ed833f2-small-Screenshot_2023-05-17_at_12.53.13.png" alt="" width="375"><figcaption></figcaption></figure>
4.  Bind up all relevant information you wish to display within the card. From the title to the link, or even add a button with relevant actions you want to perform when clicked. For this example I was just show some basic information contained within the employee's table.\


    <figure><img src="https://files.readme.io/b9b02bae2b5ac7ca69ba0489c37be9e1e6ce78df03b8c70085cd3e8ea27e6669-Screenshot_2024-09-26_at_13.13.03.png" alt=""><figcaption></figcaption></figure>

At the end you should have a screen similar to the image below.

<figure><img src="https://files.readme.io/a483275c2dddf66e0222178854b495d524557ed8b3dc915c0bb4056d2a38ae28-Screenshot_2024-09-26_at_13.13.39.png" alt=""><figcaption></figcaption></figure>

Updated about 2 months ago
