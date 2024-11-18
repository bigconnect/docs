# Displaying text

Presenting text content in a structured and readable format is crucial for user experience. Mware Application Development Framework has several UI components for text, such as Paragraph, Headline, and a Markdown Viewer.

### Paragraph

The Paragraph component displays text inside your application. It's the `<p>` equivalent in `html`. But of course it supports Bindings so you can render any text you like from any of the available Datasource or JavaScript functions.

<figure><img src="https://files.readme.io/8a900bb-Screenshot_2023-01-25_at_14.15.20.png" alt="Adding a Paragraph"><figcaption><p>Adding a Paragraph</p></figcaption></figure>

Then, you can use Bindings within the _Text_ setting in the Settings Panel, or simply enter static text by just typing in the box, or some combination of both.

For example, you may want to display the total number of rows coming back from a Data provider.

<figure><img src="https://files.readme.io/013eecc605542f1445ce30aef8e54df565ecadcde9280fc17ea45f4603b3dbbd-Screenshot_2024-09-26_at_14.01.30.png" alt=""><figcaption></figcaption></figure>

Paragraphs also support newlines:

| Text binding                                                                  | Design view                                                                   |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| ![](https://files.readme.io/b96f842-Screenshot\_2023-01-25\_at\_14.37.47.png) | ![](https://files.readme.io/9b193d2-Screenshot\_2023-01-25\_at\_14.38.12.png) |

#### Customizing text styling

The Paragraph component has several ways of styling the text using the Settings Panel.

| Setting               | Description                                                                            |
| --------------------- | -------------------------------------------------------------------------------------- |
| Size                  | The size of the text, choose from several presets ranging from `extra small` to `3XL`. |
| Color                 | Change the color of the text. Press the `X` on the bottom to reset to default.         |
| Bold/Italic/Underline | Adds the respective bold, italic and underline styling. Can be combined.               |
| Alignment             | The alignment of the text. Choose between left, right, center and justify.             |

#### Headline

The Headline component is identical to the Paragraph component, with the simple caveat that the sizes are three ahead.

For example a _Medium_ Headline is the equivalent of a _2XL_ Paragraph.

<figure><img src="https://files.readme.io/8a48ae2-Screenshot_2023-01-25_at_14.31.53.png" alt="Adding a Headline"><figcaption><p>Adding a Headline</p></figcaption></figure>

\


### Markdown viewer

The Markdown viewer component is similar to the paragraph element. It can display text in your application. But the difference is, that it can render markdown. It is really useful to display text coming from any markdown source, including the Long Form Field element.

<figure><img src="https://files.readme.io/19a29ca6a6aa455681abb473ee8db8ec778f7a7efcb99bb6f7b177cfb1d7f487-Screenshot_2024-09-26_at_14.17.30.png" alt=""><figcaption></figcaption></figure>

The _Markdown Viewer_ component has full support of all markdown elements as displayed above, including headers, images and tables.

<figure><img src="https://files.readme.io/4d78dac-Screenshot_2023-01-25_at_14.11.10.png" alt="Adding the Markdown Viewer component"><figcaption><p>Adding the Markdown Viewer component</p></figcaption></figure>

From the Settings Panel you can configure the value of the markdown viewer element, where you can also use Bindings.\


### Related components

There are a number of other components that can display text, on top of the additional functionality they provide.

These include:

* Button
* Card
* Embed
* Link
* Stat Card
* Tag
* Text inputs
