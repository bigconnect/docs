# Appearance

Appearance settings give you the option to white label Metabase to match your company’s branding.

If you’re looking for date, time, number, or currency formatting, see [Formatting defaults](https://www.metabase.com/docs/latest/data-modeling/formatting).

[Changing Metabase’s appearance](https://www.metabase.com/docs/latest/configuring-metabase/appearance#changing-metabases-appearance)

Click on the gear icon at the bottom of the navigation sidebar and select **Admin settings** > **Settings** > **Appearance**. Here’s what you can do:

[Application name](https://www.metabase.com/docs/latest/configuring-metabase/appearance#application-name)

You can change every place in the app that says “Metabase” to something like “Acme Analytics,” or whatever you want to call your Metabase app.

[Font](https://www.metabase.com/docs/latest/configuring-metabase/appearance#font)

This is the primary font used in charts and throughout the Metabase application (your “instance font”). See [Fonts](https://www.metabase.com/docs/latest/configuring-metabase/fonts).

[Color palette](https://www.metabase.com/docs/latest/configuring-metabase/appearance#color-palette)

You can customize colors in both the application UI and in the Metabase charts.

[User interface colors](https://www.metabase.com/docs/latest/configuring-metabase/appearance#user-interface-colors)![User interface colors](https://www.metabase.com/docs/latest/configuring-metabase/images/user-interface-colors.png)

You can customize the colors that Metabase uses throughout the app:

* **First color:** The main color used throughout the app for buttons, links, and the default chart color.
* **Second color:** The color of aggregations and breakouts in the graphical query builder.
* **Third color:** Color of filters in the query builder, buttons and links in filter widgets.

[Chart colors](https://www.metabase.com/docs/latest/configuring-metabase/appearance#chart-colors)![Chart colors](https://www.metabase.com/docs/latest/configuring-metabase/images/chart-colors.png)

You can choose up to 24 hex values. If you choose fewer than 24 colors, Metabase will auto-generate colors to fill in the rest of the values.

Custom colors are unavailable for:

* [Number charts](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#numbers)
* [Trend charts](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#trends)
* [Funnel charts](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#funnel-charts)
* Conditional formatting ([tables](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#tables) and [pivot tables](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#pivot-tables))
* [Maps](https://www.metabase.com/docs/latest/questions/sharing/visualizing-results#maps)

[Logo](https://www.metabase.com/docs/latest/configuring-metabase/appearance#logo)

You can replace Metabase’s familiar, tasteful, inspired-yet-not-threateningly-avant-garde dotted M logo with your very own logo. For things to work best, the logo you upload should be an SVG file that looks good when it’s around 60px tall. (In other words, ask the nearest designer for help.)

[Favicon](https://www.metabase.com/docs/latest/configuring-metabase/appearance#favicon)

The URL or image that you want to use as the favicon. Note that if you use a relative path, that path isn’t relative to the Metabase JAR, but to the webserver. So unless you’re using a reverse-proxy, the path will be relative to the frontend resources available to the JAR.

[Landing page](https://www.metabase.com/docs/latest/configuring-metabase/appearance#landing-page)

The landing page is what people will see whenever they login. You can set the URL to a collection, question, dashboard or whatever, just make sure that everyone has access to that URL.

[Loading message](https://www.metabase.com/docs/latest/configuring-metabase/appearance#loading-message)

This message is the text Metabase presents when it’s loading a query. Options include:

* “Doing science…” (the default)
* “Running query…”
* “Loading results…”

[Metabot](https://www.metabase.com/docs/latest/configuring-metabase/appearance#metabot)![Metabot toggle](https://www.metabase.com/docs/latest/configuring-metabase/images/metabot.png)

You can decide whether to display our little friend on the home page.

[Lighthouse illustration](https://www.metabase.com/docs/latest/configuring-metabase/appearance#lighthouse-illustration)

Show the Metabase lighthouse image on the home and login pages.
