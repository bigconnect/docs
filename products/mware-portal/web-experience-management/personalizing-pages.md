# Personalizing Pages

Administrators can designate pages or sections of Widget Pages as customizable. When a user visits such a page, a notification appears stating that the user can customize the page. Users can make customizations only in the sections of pages designated by administrators. Customizations are based on the rows and columns of a page layout. Page customizations are only visible to the user who made the customizations. By default, Site members can make page customizations but non-Site members and guests can’t.

### Enabling Page Customizations <a href="#enabling-page-customizations" id="enabling-page-customizations"></a>

To enable page customizations as an administrator,

1. Click _Configure Page_ from the _Options_ button next to the Page you want to let Site members modify.
2. Select the _Advanced_ tab at the top of the page and expand the _Customization Settings_ area.
3.  Click the _Customizable_ selector button to activate customizations.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360030158771/page-customizations.png" alt="Figure 1: To enable page customizations, click on the Configure Page button next to the page, expand the Customization Settings area, and click on the Customizable button." data-size="original">

    Figure 1: To enable page customizations, click on the \*Configure Page\* button next to the page, expand the \*Customization Settings\* area, and click on the \*Customizable\* button.
4. Select the sections of the page that should be customizable.
5. Enable one or more of the _Customizable_ sections so Site members can customize sections of the page. Regions that you’ve designated as customizable are colored blue.

When Site members visit your customizable page, they see an extended Control Menu with a notification saying _You can customize this page_. Site members can toggle whether to view or hide the customizable regions. If you toggle the selector to view customizable regions, the regions on the page are color-coded to help distinguish customizable vs. non-customizable sections of the page.

![Figure 2: Customizable regions are colored green and non-customizable regions are colored red.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158751/color-coded-customizable-regions.png)

### Customization Permissions <a href="#customization-permissions" id="customization-permissions"></a>

Administrators must grant users permission to customize pages under the Site section. This can be achieved by assigning permission to a Role, then assigning this Role to the appropriate users. For example, if you want users to be able to customize your customizable pages, assign the _Customize_ permission to the Role _User_. If you want Site members to be able to customize their Sites’ customizable pages, accept the default setting. By default, the _Customize_ permission is assigned to the Role _Site Member_.

The _Customize_ permission also lets users customize the look and feel of apps and import or export app settings.

### Customizing Pages <a href="#customizing-pages" id="customizing-pages"></a>

With customization active, Site members can access the Add menu from the top right side of the screen when viewing their customizable page, which lets them add apps to the customizable sections of the page. If they click _View Page without my customizations_, the Add menu disappears.

Users can make two kinds of customizations to customizable regions:

1. They can configure applications within the customizable regions.
2. They can add apps to or remove apps from the customizable regions.

_Reset My Customizations_ from the _Options_ button restores a user’s customized page to match the default page, discarding their customizations so they can start anew.

![Figure 3: Customizable areas are highlighted green when organizing apps on the page.](https://liferay-support.zendesk.com/hc/article\_attachments/360030189852/customizable-regions.png)

Users can’t change a non-instanceable app’s configuration inside a customizable region since those apps are tied to the Site where they’ve been added.

### Viewing Customized Pages <a href="#viewing-customized-pages" id="viewing-customized-pages"></a>

Site members can also choose between viewing their customized page and viewing the default page by selecting the _Options_ button (![Options](https://liferay-support.zendesk.com/hc/article\_attachments/360030189812/icon-options.png)) from the Control Menu and clicking the _View Page without my customizations_ or _View My Customized Page_.

Administrators of customizable pages have the same two views as Site members: the _default page_ view and the _customized page_ view. Changes made to the default page affect all users, whereas changes made to the customized page affect only the administrator who made the changes. Changes made by administrators to non-customizable sections in the default view are immediately applied for all users. Changes made by administrators to customizable sections, however, do _not_ overwrite users’ customizations.

### Customization Example <a href="#customization-example" id="customization-example"></a>

As an administrator,

1. Go to _Configure_ for the _Welcome_ page.
2. Go to the _Advanced_ tab and activate Customizations.
3. Set the main column of the Welcome page of the Lunar Resort Site to be customizable.

As a regular user,

1. Navigate to the _Welcome_ page.
2. Click _Add_ → _Widgets_.
3. Locate the _Language Selector_ widget and add it to the page.

The Language Selector application lets users select their language to view a translation of your Site into their native language.&#x20;
