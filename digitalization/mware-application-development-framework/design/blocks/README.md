# Blocks

Blocks in Mware Application Development Framework are pre-built components comprised of multiple Components themselves. They perform a common functionality and take away the complexity of having to repeat yourself.

At the time of writing, there are seven types of blocks available.

\


### Adding a Block

To add a block to your application, head over to the _Design_ section, then press `Add component`. The blocks can be found under the _Blocks_ category.

<figure><img src="https://files.readme.io/ea39a70b86b611cc999282263b786e5179d1d9c188108b493db35c31f60910ea-Screenshot_2024-08-28_at_09.01.37.png" alt="List of available blocks"><figcaption><p>List of available blocks</p></figcaption></figure>

Once you choose one of the blocks you want to have, it will appear in the design window.

\


### Configuring Blocks

Configuration of blocks is done by clicking the block you just added to open the Settings Panel.

<figure><img src="https://files.readme.io/dbaa97b37f30367f3c51744cd1e837210a82be382f129c9e00ae96e144f100a2-Screenshot_2024-08-28_at_09.04.33.png" alt="Table block"><figcaption><p>Form block</p></figcaption></figure>

The most important selection you will have to make first is picking which data table you want to use. Click the _Data_ dropdown to see the list of available Datasources.

Other settings are composed of the different [Components](https://docs.budibase.com/docs/components) that were encapsulated within the block. To learn about which components are used for each block, look at the respective section below.

#### Empty text

Most blocks support a custom message when no data is found. This is most likely to occur when [filtering](https://docs.budibase.com/docs/searchfilter-data) returns no results. For example:

<figure><img src="https://files.readme.io/3ff071c3136092b2012b452101c77972aee5658f61d2f10cff7c3ad4c35d7692-Screenshot_2024-08-28_at_09.06.56.png" alt="Custom no rows found text"><figcaption><p>Empty text setting</p></figcaption></figure>

<figure><img src="https://files.readme.io/7e5caecf417c638b63b3a63257ab0bd4f56ec4129dbefb39f7df5a6649ffb241-Screenshot_2024-08-28_at_09.08.20.png" alt=""><figcaption></figcaption></figure>

You can also use Bindings in your message.

### Types of blocks

| Name                  | Description                                                                                                                                                                                                                                                                                                                              |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cards block           | <p>The <em>Cards Block</em> merges three different components into one. In order, it uses Data Provider, Repeater and Card components.<br><br>All the configuration options relevant to all three components are available in the sidebar. Additionally, you can easily add search fields to allow users to filter your card blocks.</p> |
| Repeater block        | The _Repeater Block_ is a quick way to have both the Data Provider and the Repeater component into one.                                                                                                                                                                                                                                  |
| Form block            | The Form Block is a comprehensive component that makes building different types of forms much more convenient.                                                                                                                                                                                                                           |
| Multi-Step Form Block | The Multi-Step Form Block allows large forms to be broken up into pages.                                                                                                                                                                                                                                                                 |
| Chart block           | The _Chart Block_ combines a Data Provider with a nested Chart that matches the type chosen from the _Chart Type_ dropdown menu.                                                                                                                                                                                                         |
| Row explorer block    | Allows you to view data in an alternative way. It combines a searchable and scrollable list of cards on the left hand side, whilst displaying a detail form on the right for the selected card.                                                                                                                                          |

\


### Ejecting blocks

As mentioned, blocks combine various components for convenience. Whilst there are many settings, sometimes blocks wont provide the full flexibility you are after. In this case, you can _eject_ the block.

An ejected block will split a single block component into its constituent components. To eject a block, you can press the `Eject block` button in the settings panel, or select the option from the component tree.

<figure><img src="https://files.readme.io/8b91651-eject_block.png" alt="Eject block button in the settings panel"><figcaption><p>Eject block button in the settings panel</p></figcaption></figure>

\


<figure><img src="https://files.readme.io/1cf729c06b32fb2deab5b36d8c4a31f536ba542678c3cac7d2d9cd0ec26726b0-Screenshot_2024-08-28_at_09.38.39.png" alt="Eject block from component tree"><figcaption><p>Eject block from component tree</p></figcaption></figure>

\


<figure><img src="https://files.readme.io/3cffadb5d3ad077e3f28c4b6b79bc0fe03d4f8a7d34c162c304e09c817b220ae-Screenshot_2024-08-28_at_09.39.37.png" alt="Ejected content"><figcaption><p>Ejected content</p></figcaption></figure>
