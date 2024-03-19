# Managing Countries

Liferay contains a list of available countries as defined by the [ISO 3166 Standard](https://www.iso.org/iso-3166-country-codes.html), but you can use the Countries Management app to modify an existing entry or add a new entry.

NOTE

For liferay DXP 7.4 U36 - U40 (or Liferay Portal 7.4 GA36 - GA40), the Countries Management app is hidden from the UI and must be enabled with the use of feature flags. See [Enable the Countries Management App](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#enable-the-countries-management-app) below to see the steps.

### [Edit an Existing Country](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#edit-an-existing-country) <a href="#edit-an-existing-country" id="edit-an-existing-country"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Countries Management_
2. Select a country by clicking its name, or click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) on the country and click _Edit_. Use the search bar to locate a specific country quickly.
3. Make your desired changes and click _Save_. The country has now been modified.

### [Deactivate a Country](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#deactivate-a-country) <a href="#deactivate-a-country" id="deactivate-a-country"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Countries Management_.
2. Locate a country by scrolling through the list of countries. Use the search bar to locate a specific country quickly.
3. Click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) on the Country and click _Deactivate_.
4. Click _OK_ to confirm, and the country is deactivated.

### [Activate a Country](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#activate-a-country) <a href="#activate-a-country" id="activate-a-country"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Countries Management_
2. Click _Filter and Order_ next to the search bar and select _Inactive_ to see a list of deactivated countries. Use the search bar to locate a specific country quickly.
3. Click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) on the Country and click _Activate_. The country is now active.

### [Delete a Country](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#delete-a-country) <a href="#delete-a-country" id="delete-a-country"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Countries Management_.
2. Locate a country by scrolling through the list of countries. Use the search bar to locate a specific country quickly.
3. Click _Options_ (![Options icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-options.png)) on the Country and click _Delete_.
4. Click _OK_ to confirm and the country is deleted.

WARNING

Make sure you intend to delete an entry before you click _OK_. The entry cannot be recovered once deleted.

### [Add a New Country](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#add-a-new-country) <a href="#add-a-new-country" id="add-a-new-country"></a>

1. Open the Global Menu (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)). Navigate to _Control Panel_ → _Configuration_ → _Countries Management_
2.  Click _Add_ (![Add icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) next to the search bar. A new page appears for adding a country.

    ![A new page appears for adding a country.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/managing-countries/images/02.png)
3. Complete the form to add a new country. Name, Two-Letter ISO Code, Three-Letter ISO Code, and Number are required fields.
4. Click _Save_, and the new country has been added.

### [Enable the Countries Management App](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/managing-countries#enable-the-countries-management-app) <a href="#enable-the-countries-management-app" id="enable-the-countries-management-app"></a>

By default, the Countries Management App is hidden from the UI. To enable the UI,

1.  Add the following property to your [`portal-ext.properties`](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/reference/portal-properties) file:

    `feature.flag.LPS-122223=true`

    If using Docker, use the following [`environment variable`](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/installing-liferay/using-liferay-docker-images/configuring-containers#portal-properties) upon first startup:

    `--env LIFERAY_FEATURE_PERIOD_FLAG_PERIOD__UPPERCASEL__UPPERCASEP__UPPERCASES__MINUS__NUMBER1__NUMBER2__NUMBER2__NUMBER2__NUMBER2__NUMBER3_=true`
2.  Restart your server. The app now appears.

    ![See the Countries Management app in the UI.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/managing-countries/images/01.png)
