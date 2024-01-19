# Configuring Site Settings

General settings range from core configuration, like your Site’s Membership Type, to finer details like Documents and Media indexing options.

### Details <a href="#details" id="details"></a>

_Details_ provides the same menu you filled out when first creating your Site. This allows an administrator to change the description and membership type of a Site.

#### MEMBERSHIP OPTIONS <a href="#membership-options" id="membership-options"></a>

The membership type can be set as open, restricted, or private based on the privacy needs of the Site. Users can join and leave an open Site at will. To join a restricted Site, users must be added by the Site administrator, but they can request membership through the Sites section of the Control Panel. A private Site works like a restricted Site but is hidden from users who aren’t members.

#### SITE HIERARCHIES <a href="#site-hierarchies" id="site-hierarchies"></a>

Sites can be organized into hierarchies. At the bottom of the Details sub-section is the Parent Site section. When you select the parent Site for the Site you’re currently on, a checkbox appears for limiting membership to members of the parent Site.

### Pages <a href="#pages" id="pages"></a>

Under Pages you can view your Site’s Public or Private Pates, if any exist. If they don’t exist, a _Site Templates_ selector appears for creating pages with a Site Template.

![Figure 1: Selecting a Site Template.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158171/selecting-site-template.png)

### Categorization <a href="#categorization" id="categorization"></a>

_Categorization_ helps administrators organize the Site and allows for users to easily find your Site and its content through search and navigation.

### Site URL <a href="#site-url" id="site-url"></a>

The _Friendly URL_ option lets you set your Site’s URL paths. Friendly URLs are used for both public and private pages. The public Site base URL is `https://localhost:8080/web`, and the private one is `https://localhost:8080/group`. Each friendly URL must be unique.

For example, setting the friendly URL of your default Site to `/lunar-resort` makes your Site’s public home page’s URL `https://localhost:8080/web/lunar-resort/home`. The private Site’s URL is thus `https://localhost:8080/group/lunar-resort/home`.

Note that if you add a friendly URL for your instance’s home page, you should update your instance’s Home URL field so that page requests to `http://localhost:8080` redirect properly:

1. Go to _Configuration_ → _Instance Settings_ → _Instance Configuration_ → _General_ in the Control Panel.
2. Under _Navigation_, in the _Home URL_ field enter your home URL (i.e. _/web/lunar-resort/home_).

Once you’ve entered this setting, page requests to `localhost:8080` redirect to the friendly URL of your portal instance’s new home page.

You can also configure Virtual Hosts, which connects a domain name to a Site, under _Site URL_. You can use this to define a domain name (i.e., `www.lunar-resort.com`) for your Site. This can be a full domain or a subdomain. You can use this to host a number of web sites as separate Sites on one portal server.

![Figure 2: When configuring virtual hosts, the public and private pages of a site can be configured to different domains.](https://liferay-support.zendesk.com/hc/article\_attachments/360030158191/settting-virtual-hosts.png)

For instance, if you set this up for the Lunar Resort’s development network, users in that Site would access `developers.lunar-resort.com`, provided that the Lunar Resort instance’s network administrators created the domain name and pointed it to the portal server.

1. With your provider, set the DNS name _developers.lunar-resort.com_ to point to your portal instance’s IP address.
2. In the Virtual Host tab for the Developers Site, set the URL to _http://developers.lunar-resort.com_

This helps users quickly access their Site without having to recall an extended URL. The _Site URL_ option is listed under the General tab.

### Documents and Media <a href="#documents-and-media" id="documents-and-media"></a>

Below that is _Documents and Media_, which lets you enable/disable Directory Indexing. If on, Site administrators can browse your Site’s Documents and Media files and folders. For example, a Site administrator of a Site called _Lunar Resort_ can browse documents at `http://localhost:8080/documents/lunar-resort` if this option is enabled.

### Site Template <a href="#site-template" id="site-template"></a>

If you created your Site using a Site Template, this section appears and displays information about the link between the Site Template and the Site. Specifically, you can see which Site Template was used and whether or not it allows modifications to the pages inherited from it by Site administrators.&#x20;

### Asset Auto Tagging <a href="#asset-auto-tagging" id="asset-auto-tagging"></a>

_Asset Auto Tagging_ lets you enable or disable the use of any Asset Auto Tagging rules on your site.&#x20;

### Custom Fields <a href="#custom-fields" id="custom-fields"></a>

_Custom Fields_ only appears if you’ve created them in Control Panel → _Configuration_ → _Custom Fields_.&#x20;
