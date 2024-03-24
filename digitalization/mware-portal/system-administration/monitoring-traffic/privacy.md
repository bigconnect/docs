# Privacy

This guide explains the privacy implications of tracking your visitors’ web analytics data, and how Matomo can easily be configured to ensure that your users’ privacy is respected.

Matomo ensures the privacy of your users and analytics data. When using Matomo, YOU keep control of your data, nobody else does. Your data is stored in your own MySQL database, and logs or report data will never be sent to other servers by Matomo.

To ensure further security, after you have installed Matomo, we recommend that you:

1. make your Matomo server more secure by undertaking a few extra security checks
2. follow the guide below to enable important Privacy features

By design, Matomo ensures that your analytics data is only accessible to the Matomo administrator, meaning it is completely secure. This guide will explain how to easily make your favourite web analytics tool “privacy compliant”.

The majority of privacy settings within Matomo can be found by clicking the _**cog icon**_ ![Settings Cog Icon](https://m-img.org/spai/w\_50+q\_lossless+ret\_img+to\_webp/matomo.org/wp-content/uploads/2013/09/settings-menu-item-e1659083453168.png) within the top menu while logged in as Super User, which takes you to the **Settings** page. On this page, there is a **Privacy** menu available in the main navigation along the left-hand side of the page.

<figure><img src="../../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### Step 1) Automatically Anonymise Visitor IPs and replace User IDs <a href="#step-1-automatically-anonymise-visitor-ips-and-replace-user-ids" id="step-1-automatically-anonymise-visitor-ips-and-replace-user-ids"></a>

By default, IP Anonymisation is enabled in Matomo. This means Matomo stores in the database each new visitor IP address (ipv4 or ipv6 format) with the last components removed to protect user’s privacy. IP anonymisation is a good way to protect users with static IP address as otherwise their browsing history would be easily tracked across several days and even across websites tracked within the same Matomo server. IP addresses also identify the specific network a user comes from which can also reveal geographic data, often down to the level of a single individual.

To ensure that you do not store the visitor IP, which is _Personally Identifiable Information_ (PII), please go to Administration > Privacy > Anonimyze data, to enable IP anonymization, and check you have **2 bytes or 3 bytes masked** from the IP address. You can do this through the following steps:

1. Click **Anonymize data** within the Privacy menu on the Matomo **Settings** page.
2. Click **Anonymize Visitors’ IP addresses** so a green tick is shown.
3.  Select how many bytes of the visitor’ IPs should be masked by clicking the option you want so that a green bullet is shown. Some countries may also regulate the number of bytes you need to remove before an IP is considered anonymised. If you are interested in knowing where your users are, review the geolocation considerations below, otherwise, you should typically select the default **2 byte(s)**.\


    <figure><img src="https://m-img.org/spai/w_848+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2013/11/privacy.png" alt=""><figcaption></figcaption></figure>
4. The next step is to select whether the anonymised IP address or the full IP address is used for Geolocation. If you select **No** for this setting, make sure you let users know this within your privacy policy, and provide a link to the privacy policy of any third parties that you use to process data.
5. Click the big green **Save** button to update your settings.

The steps above will anonymise all IP addresses from the moment you enable those settings. If you have already been using Matomo for any length of time, there is a chance you have existing PII such as visitors IP addresses or User IDs stored. In this case, you should also anonymise your existing data.&#x20;

**Geolocation Considerations when IP masking**

IP addresses are used by Matomo for identifying where users come from. If you are not interested in finding out where your users are visiting from, you can safely anonymise 3 byte(s) in the process above which provides the most privacy. However, if you are interested in a visitor’s location you have two options:

*   **Privacy Prioritised Geolocation:** Set the number of bytes to **1 bytes(s)** (more accurate geolocation) or **2 byte(s)** (less accurate geolocation) and then select **Yes (Recommended for privacy)** to use the anonymised IP address for enriching visits. Your settings should look like this:\


    <figure><img src="https://m-img.org/spai/w_848+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2022/03/ip-masking-location-matched.png" alt=""><figcaption></figcaption></figure>
*   **Accuracy Prioritised Geolocation:** Select any number of byte masking options to anonymise the IP address within your database, but select **No** on the option to use anonymised IP addresses when enriching visits. Your settings should look something like this:\


    <figure><img src="https://m-img.org/spai/w_633+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2022/03/ip-masking-3-bytes.png" alt=""><figcaption></figcaption></figure>

**Does IP Anonymisation work with IPv6?**

Yes! While the examples above are shown in IPv4 format, it is worth noting that IP anonymisation works with newer IPv6 addresses too.

**Replace User ID with a pseudonym**\
While you are masking IP addresses, there is another setting within this section that allows you to “Replace User ID with a pseudonym” if you are using Matomo’s User ID feature. The User ID feature can be a very powerful tool if users log in to your website. It can track their activity across devices and visits by tracking a unique user ID while visitors are logged in.

When you enable the option to to “Replace User ID with a pseudonym”, the User ID will be replaced by a pseudonym to avoid directly storing and displaying personally identifiable information such as an email address. In technical terms: given your User ID, Matomo will process the User ID pseudonym using a salted hash function. Replacing a User ID with a pseudonym is not the same as anonymisation. In GDPR terms: the User ID pseudonym still counts as personal data. The original User ID could still be identified if certain additional information is available (which only Matomo and your data processor has access to).

User IDs are inherently PII, and therefore you will likely require consent. This is true even if you replace them with a pseudonym. Therefore, the biggest benefit to pseudonymisation of User IDs is that it provides a level of privacy to website users when analytics data is reviewed. Below is a screenshot of the same visitor in the visit log before and after pseudonymisation.

**Visit Log Before Pseudonymisation**\


<figure><img src="https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2022/03/user-id-pii.png" alt=""><figcaption></figcaption></figure>

**Visit Log After Pseudonymisation**\


<figure><img src="https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2022/03/user-id-pseudonym.png" alt=""><figcaption></figcaption></figure>

If you would like to respect your user’s privacy while reviewing your website analytics, then there are three simple steps to enable this feature:

1. Click **Anonymize data** within the Privacy menu on the Matomo **Settings** page.
2. Click **Replace User ID with a pseudonym** so a green tick is shown.
3. Click the big green **Save** button.

### Step 2) Regularly delete old raw visitor data <a href="#step-2-regularly-delete-old-raw-visitor-data" id="step-2-regularly-delete-old-raw-visitor-data"></a>

In Administration > Privacy > Anonymize data, under the section “Regularly delete old raw data” you can configure Matomo to automatically delete your old raw data (old visitor logs) from the database. For privacy reasons, we highly recommend that you **keep the detailed Matomo logs for only 3 to 6 months** and delete older log data.

Deleting old raw data also has one other important advantage: it will free database space which will, in turn, slightly increase performance.

<figure><img src="https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2012/01/Delete-old-visitor-logs.png" alt=""><figcaption></figcaption></figure>

### Step 3) Include a Web Analytics Opt-Out Feature on Your Site <a href="#step-3-include-a-web-analytics-opt-out-feature-on-your-site" id="step-3-include-a-web-analytics-opt-out-feature-on-your-site"></a>

On your website, **in your existing privacy policy page** or in the ‘Legal’ page, you can actually add a way for your visitors to “opt-out” of being tracked by your Matomo server. By default, all of your website visitors are tracked, but if they opt-out by clicking on the opt-out checkbox, a cookie ‘piwik\_ignore’ will be set. All visitors with a piwik\_ignore cookie will not be tracked.

In Administration > Privacy > Users opt-out, you will be able to copy and paste the opt-out form code:

<figure><img src="https://m-img.org/spai/w_890+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2012/01/opt-out-analytics-piwik1.png" alt=""><figcaption></figcaption></figure>

The opt-out form code solution is simple to use and recommended for almost every use. You can make the opt-out look great on your website by customising the Opt-out iframe colors and fonts (text color, background color, font size, and font family).

Here below is the example opt-out form for this website. You can opt out from being tracked on demo.matomo.cloud (You will need to disable any ad-blockers in order to see the example):

In cases where you need to customise more aspects of the opt-out form (for example change the opt-out text itself), you can implement a custom visitor opt-out form using JavaScript.

### Step 4) Respect DoNotTrack preference <a href="#step-4-respect-donottrack-preference" id="step-4-respect-donottrack-preference"></a>

Do Not Track is a technology and policy proposal that enables users to opt-out of tracking by websites they do not visit, including analytics services, advertising networks, and social platforms. For more information about DoNotTrack, check out [donottrack.us](http://donottrack.us/).

In Administration > Privacy > Users opt-out, under “Support Do Not Track preference” you can enable/disable Do Not Track support:

<figure><img src="https://m-img.org/spai/w_408+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2012/01/1.8-Do-not-track-privacy-400x100.png" alt=""><figcaption></figcaption></figure>

Note: tracking requests and cookies will still be created even for people who don’t want to be tracked. To prevent any requests and cookies when people don’t want to be tracked, add the following function in the JavaScript Tracking code: `_paq.push(["setDoNotTrack", true]);`

Finally, note that if your audience is widely using IE10, IE11, or Maxthon, then you will not track some of your audience because these older browsers have the DoNotTrack feature enabled by default for all users.

### Step 5) Create a Privacy Policy page on your website(s) <a href="#step-5-create-a-privacy-policy-page-on-your-websites" id="step-5-create-a-privacy-policy-page-on-your-websites"></a>

A privacy policy is a statement or a legal document that discloses some or all of the ways a party gathers, uses, discloses, and manages a customer or client’s data. It fulfills a legal requirement to protect a customer or client’s privacy.

When you use Matomo to track your visitors, we recommend updating your Privacy Policy to explain how Matomo is used and what data it gathers. We provide a Privacy Policy template here for Matomo users that you can copy on your site.

### Step 6) Learn about personal data regulations such as GDPR <a href="#step-6-learn-about-personal-data-regulations-such-as-gdpr" id="step-6-learn-about-personal-data-regulations-such-as-gdpr"></a>

In Matomo go to Administration > Privacy > GDPR Overview to find details about awareness & documentation, individuals’ rights, security procedures, and data retention.

### Step 7) Anonymising data that has already been tracked <a href="#step-7-anonymising-data-that-has-already-been-tracked" id="step-7-anonymising-data-that-has-already-been-tracked"></a>

When you have tracked data in the past and realised later you tracked maybe too much data or some personal data, you can easily anonymize the affected data using our tool to anonymize previously tracked raw data.

### Step 8) How to track people in Matomo without asking them for consent or a cookie banner? <a href="#step-8-how-to-track-people-in-matomo-without-asking-them-for-consent-or-a-cookie-banner" id="step-8-how-to-track-people-in-matomo-without-asking-them-for-consent-or-a-cookie-banner"></a>

As part of privacy legislation worldwide including GDPR but also ePrivacy, it is often required to display a cookie banner informing users about cookies, or consent must be obtained before tracking visitors’ personal data.

### Step 9) Anonymize Ecommerce Order ID <a href="#step-9-anonymize-ecommerce-order-id" id="step-9-anonymize-ecommerce-order-id"></a>

By default Matomo stores your Ecommerce Order IDs to allow for closer analysis and to help prevent tracking duplication. However, because an Order ID can be cross-referenced with another system, typically an eCommerce shop, the Order ID may count as personal information under the GDPR. If you prefer not to track personal data in Matomo, you may enable “Anonymize Order ID” to avoid storing the Ecommerce Order ID number. The steps to do this are:

1. Click **Anonymize data** within the **Privacy** menu on the Matomo **Settings** page.
2. Click **Anonymize Order ID** so a green tick is shown.
3. Click the big green **Save** button.

When you enable this option, an Order ID will be automatically anonymised so no personal information will be tracked. It is important to note that if you enable this feature, in some cases duplicate orders may be counted if people refresh the order confirmation page as there is no way to distinguish unique orders.

### Step 10) Optional Privacy Preferences <a href="#step-10-optional-privacy-preferences" id="step-10-optional-privacy-preferences"></a>

There is more you can consider when it comes to privacy:

* As the Matomo administrator, you may decide that giving access to real-time & visits log features are not necessary for your team members who use Matomo. In this case, you can disable the visits log, real-time and visitor profile.
* If you track the number of websites with the same Matomo server, all your websites’ code will contain the Matomo server URL in the Javascript code. To prevent other users from finding out all your websites, you can Hide the Matomo Server URL in your JavaScript using this technique (FAQ)
* Some countries legislation requires websites to control which cookies they set based on user preferences. You can easily disable all Matomo Cookies for particular visitors or for all visitors by calling a Javascript function in the Matomo code.
* You can also add Privacy Policy and Terms & Conditions links in Matomo for enhanced trust, transparency, and legal compliance across your platform and embedded widgets.
* In Matomo version 4 and above, you can anonymise referrer details to avoid accidentally collecting private data.
* In Matomo version 5 and above, URLs with the file:// protocol will not be tracked by default to avoid accidentally collecting private data. If needed this can be enabled within the JavaScript tracker.

\
Personal data analytics collected and why
-----------------------------------------

The collection of personal data is not inherently bad, and it is often desirable as part of the analytics process. Matomo’s privacy-focused design ensures you only collect personal information when needed, and that you always have the ability to anonymise any personal data.

Matomo collects and processes the following personal data by default:

* **IP Address** – This is used to identify the location of a user. There are multiple levels of anonymisation possible, and each can be checked against a database on your server for varying levels of accuracy. By default, Matomo automatically anonymises the IP address by obscuring the last part.
* **URLs and Page Titles** – These help you understand how people use your site but can contain personal data. An example is if you provide custom profile pages and URLs for registered users.
* **Referrer URLs** – These can contain personal data such as Facebook/Google referral IDs and even third-party profile URLs.
* **Tracking Cookie IDs** – These are unique identifiers which can help determine whether a visit is from a new or returning user.
* **Geolocation Data** – This is useful for understanding the geographic trends of your website’s visitors. However, it can also be used to identify where a specific user is, if linked to a User ID or if you only have a few users in remote locations.
* **Site Searches** – As you have no control over what users input to your website’s search bar, it is possible that a user may enter personal data when seeking information.

Other personal data may be collected based on your specific settings and plugins. Some examples are:

* **Custom Events** – These help you understand how people use your site but can contain personal data. An example is if you provide custom profile pages and URLs for registered users.
* **Heatmap and Session Recordings** – Used for optimising the design and flow of a site. Visit data may make it obvious who is using the site in a recording. It is possible to mask content areas where you expect personal data to appear.
* **User ID** – This might be a username or email address collected when a user logs in. It is an optional feature that can be used to aid with counting unique visits and to track user-level interactions across devices and time.
* **Ecommerce Order IDs** – These are used to avoid duplication of tracking; however, they can be traced back to the specific order which almost certainly contains personal data such as name/address.
* **Custom Dimensions, Events and Variables** – These are all customisable tracking mechanisms which could contain personal data if specifically configured to do so. For example, if a health-focused site decided to track known diagnoses alongside a user’s page views.
