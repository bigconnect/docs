# Designing for mobile

### Key responsive settings and defaults

#### Breakpoints

Breakpoints are the points in screen size or orientation where an app is triggered to adjust its layout. Breakpoints are how we define our app when it's being used on mobile, tablet, or desktop. Mware Application Development Frameworkcomes with default breakpoints. Users cannot edit these breakpoints. You can visualize the breakpoints (and your design) within the design section by selecting the device toggle (see GIF below).

<figure><img src="https://files.readme.io/cd665a8d93f1cf6491b5eacebd36806041da7132c047e1a6e5248d81612f9a32-Screen_Recording_2024-09-03_at_11.11.18.gif" alt=""><figcaption></figcaption></figure>

#### Flexbox

Flexbox is a one-dimensional layout method for arranging items in rows or columns. Items flex (expand) to fill additional space or shrink to fit into smaller spaces. Mware Application Development Framework uses flexbox to help users structure their apps. You can access the flexbox properties within the [Container](https://docs.budibase.com/docs/container) component - your components must be wrapped in a container for Flexbox to arrange your components.

#### Flex wrap

The flex-wrap property sets whether flex items are forced onto one line or can wrap onto multiple lines. If wrapping is allowed, it sets the direction in that lines are stacked. So, within Mware Application Development Framework, if you have a row of cards and you select flex-wrap, the cards will 'stack'. In the GIF below, selecting the flex-wrap icon tells the browser to 'wrap' the elements within that container and allow them to stack - therefore free the initial component to take up the remaining space in the row.

<figure><img src="https://files.readme.io/e54de2a-CleanShot_2022-04-20_at_11.02.47.gif" alt="1200"><figcaption><p>flex wrap</p></figcaption></figure>

#### Grid Layout

Using Grid layout, it is possible to drag-and-drop components into place, independently of other elements on the screen.

![](https://files.readme.io/816ec2c969bd7702607d46b79e7f77457269675ac5f893016080531438c702fe-Screen\_Recording\_2024-09-03\_at\_11.35.02.gif)

#### Update a component's setting based on the device

You can conditionally tell Mware Application Development Framework to update a component's setting if the user is on a particular device. For example, you may want to position your form labels to the left when the user is on desktop and above when on mobile. Here's a quick demonstration:

#### Hamburger menu

A hamburger menu is a menu button that enables a drop-down function for navbar links that helps save space in restricted designs. If you use either the side navigation or top navigation layout, Mware Application Development Framework will add a hamburger menu to your app. This is only visible when your users are on a mobile device.

<figure><img src="https://files.readme.io/67d111e61be2bc70480e5154c8a15a56801c45043673ce7f70d7fad04aa0e927-Screen_Recording_2024-09-03_at_11.24.50.gif" alt=""><figcaption></figcaption></figure>
