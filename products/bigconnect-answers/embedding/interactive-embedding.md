# Interactive embedding

**Interactive embedding** is what you want if you want to offer [multi-tenant, self-service analytics](https://www.metabase.com/learn/customer-facing-analytics/multi-tenant-self-service-analytics).

Interactive embedding is the only type of embedding that integrates with your [permissions](https://www.metabase.com/docs/latest/permissions/introduction) and [SSO](https://www.metabase.com/docs/latest/people-and-groups/start#authentication) to give people the right level of access to [query](https://www.metabase.com/glossary/query\_builder) and [drill-down](https://www.metabase.com/learn/questions/drill-through) into your data.

[Interactive embedding demo](https://www.metabase.com/docs/latest/embedding/interactive-embedding#interactive-embedding-demo)

To get a feel for what you can do with interactive embedding, check out our [interactive embedding demo](https://www.metabase.com/embedding-demo).

To see the query builder in action, click on **Reports** > **+ New** > **Question**.

[Quick start](https://www.metabase.com/docs/latest/embedding/interactive-embedding#quick-start)

Check out the [Interactive embedding quick start](https://www.metabase.com/learn/customer-facing-analytics/interactive-embedding-quick-start).

[Prerequisites for interactive embedding](https://www.metabase.com/docs/latest/embedding/interactive-embedding#prerequisites-for-interactive-embedding)

1. Make sure you have a [license token](https://www.metabase.com/docs/latest/paid-features/activating-the-enterprise-edition) for a [paid plan](https://store.metabase.com/checkout/login-details).
2. Organize people into Metabase [groups](https://www.metabase.com/docs/latest/people-and-groups/start).
3. Set up [permissions](https://www.metabase.com/docs/latest/permissions/introduction) for each group.
4. Set up [SSO](https://www.metabase.com/docs/latest/people-and-groups/start#authentication) to automatically apply permissions and show people the right data upon sign-in. In general, **we recommend using** [**SSO with JWT**](https://www.metabase.com/docs/latest/people-and-groups/authenticating-with-jwt).

If you’re dealing with a [multi-tenant](https://www.metabase.com/learn/customer-facing-analytics/multi-tenant-self-service-analytics) situation, check out our recommendations for [Configuring permissions for different customer schemas](https://www.metabase.com/learn/permissions/multi-tenant-permissions).

If you have your app running locally, and you’re using the Pro Cloud version, or hosting Metabase and your app in different domains, you’ll need to set your Metabase environment’s session cookie samesite option to “none”.

[Enabling interactive embedding in Metabase](https://www.metabase.com/docs/latest/embedding/interactive-embedding#enabling-interactive-embedding-in-metabase)

1. Go to **Settings** > **Admin settings** > **Embedding**.
2. Click **Enable**.
3. Click **Interactive embedding**.
4. Under **Authorized origins**, add the URL of the website or web app where you want to embed Metabase (such as `https://*.example.com`).

[Setting up embedding on your website](https://www.metabase.com/docs/latest/embedding/interactive-embedding#setting-up-embedding-on-your-website)

1. Create an iframe with a `src` attribute set to:
   * the [URL](https://www.metabase.com/docs/latest/embedding/interactive-embedding#pointing-an-iframe-to-a-metabase-url) of the Metabase page you want to embed, or
   * an [authentication endpoint](https://www.metabase.com/docs/latest/embedding/interactive-embedding#pointing-an-iframe-to-an-authentication-endpoint) that redirects to your Metabase URL.
2. Optional: Depending on the way your web app is set up, set [environment variables](https://www.metabase.com/docs/latest/configuring-metabase/environment-variables) to:
   * [Add your license token](https://www.metabase.com/docs/latest/configuring-metabase/environment-variables#mb\_premium\_embedding\_token).
   * [Embed Metabase in a different domain](https://www.metabase.com/docs/latest/embedding/interactive-embedding#embedding-metabase-in-a-different-domain).
   * [Secure your interactive embed](https://www.metabase.com/docs/latest/embedding/interactive-embedding#securing-interactive-embeds).
3. Optional: Enable communication to and from the embedded Metabase using supported [`postMessage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) messages:
   * [From Metabase](https://www.metabase.com/docs/latest/embedding/interactive-embedding#supported-postmessage-messages-from-embedded-metabase)
   * [To Metabase](https://www.metabase.com/docs/latest/embedding/interactive-embedding#supported-postmessage-messages-to-embedded-metabase)
4. Optional: Set parameters to [show or hide Metabase UI components](https://www.metabase.com/docs/latest/embedding/interactive-embedding#showing-or-hiding-metabase-ui-components).

Once you’re ready to roll out your interactive embed, make sure that people **allow** browser cookies from Metabase, otherwise they won’t be able to log in.

[Pointing an iframe to a Metabase URL](https://www.metabase.com/docs/latest/embedding/interactive-embedding#pointing-an-iframe-to-a-metabase-url)

Go to your Metabase instance and find the page that you want to embed.

For example, to embed your Metabase home page, set the `src` attribute to your [site URL](https://www.metabase.com/docs/latest/configuring-metabase/settings#site-url), such as:

```
http://metabase.yourcompany.com/
            
            
```

```
            
```

```

                Copy        
```

To embed a specific Metabase dashboard, use the dashboard’s URL, such as:

```
http://metabase.yourcompany.com/dashboard/1
            
            
```

```
            
```

```

                Copy        
```

[Pointing an iframe to an authentication endpoint](https://www.metabase.com/docs/latest/embedding/interactive-embedding#pointing-an-iframe-to-an-authentication-endpoint)

Use this option if you want to send people directly to your SSO login screen (i.e., skip over the Metabase login screen with an SSO button), and redirect to Metabase automatically upon authentication.

You’ll need to set the `src` attribute to your auth endpoint, with a `return_to` parameter pointing to the encoded Metabase URL. For example, to send people to your SSO login page and automatically redirect them to `http://metabase.yourcompany.com/dashboard/1`:

```
https://metabase.example.com/auth/sso?return_to=http%3A%2F%2Fmetabase.yourcompany.com%2Fdashboard%2F1
            
            
```

```
            
```

```

                Copy        
```

If you’re using [JWT](https://www.metabase.com/docs/latest/people-and-groups/authenticating-with-jwt), you can use the relative path for the redirect (i.e., your Metabase URL without the [site URL](https://www.metabase.com/docs/latest/configuring-metabase/settings#site-url)). For example, to send people to a Metabase page at `/dashboard/1`:

```
https://metabase.example.com/auth/sso?jwt=<token>&return_to=%2Fdashboard%2F1
            
            
```

```
            
```

```

                Copy        
```

You must URL encode (or double encode, depending on your web setup) all of the parameters in your redirect link, including parameters for filters (e.g., `filter=value`) and [UI settings](https://www.metabase.com/docs/latest/embedding/interactive-embedding#showing-or-hiding-metabase-ui-components) (e.g., `top_nav=true`). For example, if you added two filter parameters to the JWT example shown above, your `src` link would become:

```
https://metabase.example.com/auth/sso?jwt=<token>&redirect=%2Fdashboard%2F1%3Ffilter1%3Dvalue%26filter2%3Dvalue
            
            
```

```
            
```

```

                Copy        
```

[Cross-browser compatibility](https://www.metabase.com/docs/latest/embedding/interactive-embedding#cross-browser-compatibility)

To make sure that your embedded Metabase works in all browsers, put Metabase and the embedding app in the same top-level domain (TLD). The TLD is indicated by the last part of a web address, like `.com` or `.org`.

Note that your interactive embed must be compatible with Safari to run on _any_ browser in iOS (such as Chrome on iOS).

[Embedding Metabase in a different domain](https://www.metabase.com/docs/latest/embedding/interactive-embedding#embedding-metabase-in-a-different-domain)

> Skip this section if your Metabase and embedding app are already in the same top-level domain (TLD).

If you want to embed Metabase in another domain (say, if Metabase is hosted at `metabase.yourcompany.com`, but you want to embed Metabase at `yourcompany.github.io`), you can tell Metabase to set the session cookie’s SameSite value to “none”.

You can set session cookie’s SameSite value in **Admin settings** > **Embedding** > **Interactive embedding** > **SameSite cookie setting**.

SameSite values include:

* **Lax** (default): Allows cookies to be sent when someone navigates to the origin site from an external site (like when following a link).
* **None**: Allows all cross-site requests. Incompatible with most Safari and iOS browsers, such as Chrome on iOS. If you set this environment variable to “None”, you must use HTTPS in Metabase to prevent browsers from rejecting the request.
* **Strict** (not recommended): Never allows cookies to be sent on a cross-site request. Warning: this will prevent users from following external links to Metabase.

You can also set the the [`MB_SESSION_COOKIE_SAMESITE` environment variable](https://www.metabase.com/docs/latest/configuring-metabase/environment-variables#mb\_session\_cookie\_samesite).

If you’re using Safari, you’ll need to [allow cross-site tracking](https://support.apple.com/en-tj/guide/safari/sfri40732/mac). Depending on the browser, you may also run into issues when viewing emdedded items in private/incognito tabs.

Learn more about [SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite).

[Securing interactive embeds](https://www.metabase.com/docs/latest/embedding/interactive-embedding#securing-interactive-embeds)

Metabase uses HTTP cookies to authenticate people and keep them signed into your embedded Metabase, even when someone closes their browser session. If you enjoy diagrammed auth flows, check out [Interactive embedding with SSO](https://www.metabase.com/learn/customer-facing-analytics/securing-embeds).

To limit the amount of time that a person stays logged in, set [`MAX_SESSION_AGE`](https://www.metabase.com/docs/latest/configuring-metabase/environment-variables#max\_session\_age) to a number in minutes. The default value is 20,160 (two weeks).

For example, to keep people signed in for 24 hours at most:

```
MAX_SESSION_AGE=1440
            
            
```

```
            
```

```

                Copy        
```

To automatically clear a person’s login cookies when they end a browser session:

```
MB_SESSION_COOKIES=true
            
            
```

```
            
```

```

                Copy        
```

To manually log someone out of Metabase, load the following URL (for example, in a hidden iframe on the logout page of your application):

```
https://metabase.yourcompany.com/auth/logout
            
            
```

```
            
```

```

                Copy        
```

If you’re using [JWT](https://www.metabase.com/docs/latest/people-and-groups/authenticating-with-jwt) for SSO, we recommend setting the `exp` (expiration time) property to a short duration (e.g., 1 minute).

[Supported postMessage messages _from_ embedded Metabase](https://www.metabase.com/docs/latest/embedding/interactive-embedding#supported-postmessage-messages-from-embedded-metabase)

To keep up with changes to an embedded Metabase URL (for example, when a filter is applied), set up your app to listen for “location” messages from the embedded Metabase. If you want to use this message for deep-linking, note that “location” mirrors “window.location”.

```
{
  "metabase": {
    "type": "location",
    "location": LOCATION_OBJECT_OR_URL
  }
}
            
            
```

```
            
```

```

                Copy        
```

To make an embedded Metabase page (like a question) fill up the entire iframe in your app, set up your app to listen for a “frame” message with “normal” mode from Metabase:

```
{
  "metabase": {
    "type": "frame",
    "frame": {
      "mode": "normal"
    }
  }
}
            
            
```

```
            
```

```

                Copy        
```

To specify the size of an iframe in your app so that it matches an embedded Metabase page (such as a dashboard), set up your app to listen for a “frame” message with “fit” mode from Metabase:

```
{
  "metabase": {
    "type": "frame",
    "frame": {
      "mode": "fit",
      "height": HEIGHT_IN_PIXELS
    }
  }
}
            
            
```

```
            
```

```

                Copy        
```

[Supported postMessage messages _to_ embedded Metabase](https://www.metabase.com/docs/latest/embedding/interactive-embedding#supported-postmessage-messages-to-embedded-metabase)

To change an embedding URL, send a “location” message from your app to Metabase:

```
{
  "metabase": {
    "type": "location",
    "location": LOCATION_OBJECT_OR_URL
  }
}
            
            
```

```
            
```

```

                Copy        
```

[Group strategies with sandboxing](https://www.metabase.com/docs/latest/embedding/interactive-embedding#group-strategies-with-sandboxing)

If you want multiple people from a single customer account to collaborate on questions and dashboards, you’ll need to set up one [group](https://www.metabase.com/docs/latest/people-and-groups/managing#groups) per customer account.

You can handle [data sandboxing](https://www.metabase.com/docs/latest/permissions/data-sandboxes) with a single, separate group that just sandboxes your data. For example, each person could be part of a customer group that sets up data permissions with sandboxing via a certain attribute that applies to everyone across all your customer accounts.

Additionally, each person within a single customer account could also be a member of a group specific to that customer account. That way they can collaborate on collections with other people in their organization, without seeing stuff created by people from other customers’ accounts.

[Showing or hiding Metabase UI components](https://www.metabase.com/docs/latest/embedding/interactive-embedding#showing-or-hiding-metabase-ui-components)

To change the interface of your interactive embed, you can add parameters to the end of your embedding URL. If you want to change the colors or fonts in your embed, see [Customizing appearance](https://www.metabase.com/docs/latest/configuring-metabase/appearance).

For example, you can disable Metabase’s [top nav bar](https://www.metabase.com/docs/latest/embedding/interactive-embedding#top\_nav) and [side nav menu](https://www.metabase.com/docs/latest/embedding/interactive-embedding#side\_nav) like this:

```
your_embedding_url?top_nav=false&side_nav=false
            
            
```

```
            
```

```

                Copy        
```

![Top nav and side nav disabled](https://www.metabase.com/docs/latest/embedding/images/no-top-no-side.png)

Options include:

* [Action buttons](https://www.metabase.com/docs/latest/embedding/interactive-embedding#action\_buttons)
* [Additional info](https://www.metabase.com/docs/latest/embedding/interactive-embedding#additional\_info)
* [Breadcrumbs](https://www.metabase.com/docs/latest/embedding/interactive-embedding#breadcrumbs)
* [Header](https://www.metabase.com/docs/latest/embedding/interactive-embedding#header)
* [Locale](https://www.metabase.com/docs/latest/embedding/interactive-embedding#locale)
* [Logo](https://www.metabase.com/docs/latest/embedding/interactive-embedding#logo)
* [New button](https://www.metabase.com/docs/latest/embedding/interactive-embedding#new\_button)
* [Search](https://www.metabase.com/docs/latest/embedding/interactive-embedding#search)
* [Side nav](https://www.metabase.com/docs/latest/embedding/interactive-embedding#side\_nav)
* [Top nav](https://www.metabase.com/docs/latest/embedding/interactive-embedding#top\_nav)

[`action_buttons`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#action\_buttons)

Visible by default on question pages when the [header](https://www.metabase.com/docs/latest/embedding/interactive-embedding#header) is enabled.

To hide the action buttons such as **Filter**, **Summarize**, the query builder button, and so on:

```
header=false&action_buttons=false
            
            
```

```
            
```

```

                Copy        
```

![Action buttons](https://www.metabase.com/docs/latest/embedding/images/action-buttons.png)[`additional_info`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#additional\_info)

Visible by default on question and dashboard pages, when the [header](https://www.metabase.com/docs/latest/embedding/interactive-embedding#header) is enabled.

To hide the gray text “Edited X days ago by FirstName LastName”, as well as the breadcrumbs with collection, database, and table names:

`header=false&additional_info=false`

![Additional info](https://www.metabase.com/docs/latest/embedding/images/additional-info.png)[`breadcrumbs`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#breadcrumbs)

Shown by default in the top nav bar. Collection breadcrumbs show the path to the item (i.e., the collection(s) the item is in). To hide the breadcrumbs:

```
breadcrumbs=false
            
            
```

```
            
```

```

                Copy        
```

[`header`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#header)

Visible by default on question and dashboard pages.

To hide a question or dashboard’s title, [additional info](https://www.metabase.com/docs/latest/embedding/interactive-embedding#additional\_info), and [action buttons](https://www.metabase.com/docs/latest/embedding/interactive-embedding#action\_buttons):

`header=false`

[`locale`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#locale)

You can localize the user interface via a parameter. For example, to set the locale to Spanish:

```
locale=es-ES
            
            
```

```
            
```

```

                Copy        
```

Check out the [locales Metabase supports](https://github.com/metabase/metabase/tree/master/locales). And read more about [localization](https://www.metabase.com/docs/latest/configuring-metabase/localization).

[`logo`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#logo)

Whether to show the logo that opens and closes the sidebar nav. Default is true. How Metabase displays the logo depends on the `side_nav` setting. Here’s a rough breakdown of how these two parameters interact:

If `logo=true` and:

* `side_nav=true`: Looks like regular Metabase (with whatever logo you have set).
* `side_nav=false`: There is no sidebar, so nothing happens when you hover over the logo.

If `logo=false` and:

* `side_nav=true`: Metabase shows the generic sidebar icon, with a gray color in normal state, and a brand color on hover.
* `side_nav=false`: There is no side nav nor logo, so the breadcrumbs move all the way to the left of the screen.

[`new_button`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#new\_button)

Hidden by default. To show the **+ New** button used to create queries or dashboards:

```
top_nav=true&new_button=true
            
            
```

```
            
```

```

                Copy        
```

[`search`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#search)

Hidden by default. To show the search box in the top nav:

```
top_nav=true&search=true
            
            
```

```
            
```

```

                Copy        
```

[`side_nav`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#side\_nav)

The navigation sidebar is shown on `/collection` and home page routes, and hidden everywhere else by default.

To allow people to minimize the sidebar:

```
top_nav=true&side_nav=true
            
            
```

```
            
```

```

                Copy        
```

![Side nav](https://www.metabase.com/docs/latest/embedding/images/side-nav.png)[`top_nav`](https://www.metabase.com/docs/latest/embedding/interactive-embedding#top\_nav)

Shown by default. To hide the top navigation bar:

```
top_nav=false
            
            
```

```
            
```

```

                Copy        
```

![Top nav bar](https://www.metabase.com/docs/latest/embedding/images/top-nav.png)

`search`, `new_button`, and `breadcrumbs` all depend on `top_nav` being set to `true`. If these three children (`search`, `new_button`, and `breadcrumbs`) are all false, Metabase will hide the top nav bar.
