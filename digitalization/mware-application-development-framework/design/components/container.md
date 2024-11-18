# Container

The container now has two different layouts. You can toggle between these two modes by switching between the icons of the layout setting. By default, the grid-based layout is selected. If you wish to use the old layout, you must click the first displayed icon.

![Grid layout settings](https://files.readme.io/5ba8f036594f72b3efc52e3927395031f7080b238d1ad4f7ce71d022b6cffe54-Screenshot\_2024-09-10\_at\_12.20.36.png)

### Grid-based

This layout allows users to drag, resize, and place their components anywhere on the screen. It also allows you to resize components based on the view mode you have currently selected in the builder. This allows you to build responsive screens easily for mobile and tablet devices.

To drag, hover over the component and click on its label. This will allow you to drag the component freely within the builder's design area. If you don't click on the label, you will just be able to resize the component.

### Flexbox

A Container is a `div` with pre-defined styles. They are the building blocks of your application and critical for responsive design. Containers are used to structure your application and layout other components/elements in a structured way. You can use flexbox to layout the items within your container.

<figure><img src="https://files.readme.io/fe98df8-CleanShot_2022-03-23_at_13.31.27.png" alt=""><figcaption></figcaption></figure>

> ### 📘Learn Flexbox
>
> Budibase containers use flexbox to layout components. A great resource for learning flexbox is [flexbox froggy.](https://flexboxfroggy.com/)

### Container settings

#### Flexbox

There are several general settings for the container component, most of which are shared with other components as well.

![](https://files.readme.io/1588d53762edc3de0efa54001d8f6223c51d20760bee64def91dc810a438729f-Screenshot\_2024-09-10\_at\_12.21.10.png)

| Setting          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Layout           | Switch between the flexbox and grid-base layouts.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Name             | Change the name of your component, and how it reads within the component tree.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Direction        | Change the direction of the components nested within your container. You can layout your components in a row (→) or a column (↓).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Horizontal align | <p>The horizontal. align setting is used to align the items on the main axis, the direction in which flex-direction has set the flow. The initial value is <strong>left</strong> which will line the items up at the start edge of the container, but you could also set the value to <strong>right</strong> to line them up at the end, or <strong>center</strong> to line them up in the center.<br><br>You can also use the value <strong>stretch</strong> to take all the spare space between the items, and share it out evenly between the items so there will be an equal amount of space between each item.</p> |
| Vertical align   | <p>The vertical align property will align the items on the cross axis. The initial value is <strong>top</strong> which aligns the items along the top of the container (like they're handing from a roof), but you could also set the value to the <strong>bottom</strong> to align the items along the bottom (like they're standing on the floor), or <strong>middle</strong> to line them up in the center.<br><br>You can also use the value to <strong>stretch</strong>, to stretch the items to fill the height of the container.</p>                                                                             |
| Size             | Where the **grow** option deals with adding space in the main axis, the **shrink** property controls how it is taken away.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Gap              | The **gap** property sets the gaps (gutters) between items stored within a container. You can select the different sizes of gaps you would like between your items.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Wrap             | <p>Select the <strong>wrap</strong> to wrap your components onto multiple lines. This is critical when designing for mobile resolutions.<br><br>With <strong>nowrap</strong>, the items are laid out in a single line which may cause the flex container to overflow.</p>                                                                                                                                                                                                                                                                                                                                               |
| On click         | By defining [actions](https://docs.budibase.com/docs/actions) within the `On Click` setting, the container will be treated like a button including the cursor change. Any click events of buttons or other containers within a container with _On Click_ actions set will be ignored.                                                                                                                                                                                                                                                                                                                                   |

#### Container background colour

You can also splash some color into your apps with containers!

![](https://files.readme.io/5190f3e-Screenshot\_2022-04-28\_at\_09.38.24.png)

To clear the selected color, click on the `X` in the bottom right of the panel.

For additional flare, there are a range of gradients available.

| Title            | Gradient                                                                      |
| ---------------- | ----------------------------------------------------------------------------- |
| Warm Flame       | ![](https://files.readme.io/1a42339-Screenshot\_2022-04-28\_at\_09.58.00.png) |
| Night Fade       | ![](https://files.readme.io/fc0fe3c-Screenshot\_2022-04-28\_at\_10.04.07.png) |
| Spring Warmth    | ![](https://files.readme.io/b17c898-Screenshot\_2022-04-28\_at\_10.00.24.png) |
| Sunny Morning    | ![](https://files.readme.io/6d1c984-Screenshot\_2022-04-28\_at\_10.00.59.png) |
| Winter Neva      | ![](https://files.readme.io/be3cf7c-Screenshot\_2022-04-28\_at\_10.01.13.png) |
| Tempting Azure   | ![](https://files.readme.io/b91a8a2-Screenshot\_2022-04-28\_at\_10.01.36.png) |
| Heavy Rain       | ![](https://files.readme.io/8d459f9-Screenshot\_2022-04-28\_at\_10.02.02.png) |
| Deep Blue        | ![](https://files.readme.io/de2f85d-Screenshot\_2022-04-28\_at\_10.02.20.png) |
| Near Moon        | ![](https://files.readme.io/dfe425f-Screenshot\_2022-04-28\_at\_10.04.59.png) |
| Wild Apple       | ![](https://files.readme.io/35ae5bb-Screenshot\_2022-04-28\_at\_10.05.20.png) |
| Plum Plate       | ![](https://files.readme.io/59146cd-Screenshot\_2022-04-28\_at\_10.05.44.png) |
| Peach Kiss       | ![](https://files.readme.io/2d10e99-Screenshot\_2022-04-28\_at\_10.06.03.png) |
| Flamingo Sunrise | ![](https://files.readme.io/cd3f175-Screenshot\_2022-04-28\_at\_10.06.31.png) |
| Bud Mist         | ![](https://files.readme.io/6b32daa-Screenshot\_2022-04-28\_at\_10.06.56.png) |
| Ballet Slipper   | ![](https://files.readme.io/2d88d97-Screenshot\_2022-04-28\_at\_10.07.20.png) |
| Black Noir       | ![](https://files.readme.io/6fccf79-Screenshot\_2022-04-28\_at\_10.07.41.png) |

You can also add background colors to your [Screens](https://docs.budibase.com/docs/screens)

\


### Component layout

When using containers to layout your components, the most important setting is _Direction_ as this will determine the flow of components.

<figure><img src="https://files.readme.io/6a3ed4ea25275ae9c048553ce2b154f6adbf1bf467d933f9c00acd8195aafa83-Screenshot_2024-09-12_at_12.35.34.png" alt="Screen column layout"><figcaption><p>Container column layout</p></figcaption></figure>

<figure><img src="https://files.readme.io/32ffd44b23c9d4964031606dfd4818f3871922b3a9a77b45f54672fc9e759336-Screenshot_2024-09-12_at_12.35.50.png" alt="Screen row layout"><figcaption><p>Container row layout</p></figcaption></figure>

If you need additional layout you can add additional containers. For example what if we wanted to group 'THREE' and 'FOUR' in column direction, but have an overall row layout for the screen. In this case we need to add a container and nest P3 and P4 like so:

<figure><img src="https://files.readme.io/a1c8a0e071fab3bf20021674399edfaae2a66b11f27c2201783ac11f2508ac7d-Screenshot_2024-09-12_at_12.38.14.png" alt="Combining row and column directions"><figcaption><p>Multi-container row column directions</p></figcaption></figure>

Containers are a really flexible way to arrange your components in the **Design** section. You can nest any number of containers with different styles and layout options.

You may, however be interested in Using sections or Field groups to get out-of-the-box layouts.
