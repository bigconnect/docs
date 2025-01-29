# Event Tracking

There are two main ways to set up event tracking within Matomo. The first is with the Tag Manager plugin built into Matomo. The second is by adding snippets of JavaScript code to your website itself. You can find more details on how to set up both methods below.

### How To Set Up Event Tracking With Matomo Tag Manager (Recommended) <a href="#how-to-set-up-event-tracking-with-matomo-tag-manager-recommended" id="how-to-set-up-event-tracking-with-matomo-tag-manager-recommended"></a>

If you have [Matomo Tag Manager](https://matomo.org/docs/tag-manager/) installed, it is the easiest way to implement event tracking on your site. You can simply create a Tag for each type of event you want to track. Below you will find an example step-by-step guide for creating an Event that tracks email link clicks on your site.

1. [Set up and configure Matomo Tag Manager](https://matomo.org/docs/tag-manager/#how-can-i-set-up-matomo-tag-manager-on-my-website) (create a container and install the container code)
2. Click on **Tag Manager** in Matomo and visit the **Tags** page.
3. Click the big green button to **Create New Tag**.
4. Select the **Matomo Analytics** tag type.
5. Give your [_**Tag**_](https://matomo.org/docs/tag-manager/#tags) a descriptive name. For example; **Matomo Event – Email Link Click**
6. Confirm your Matomo configuration. If you are not sure, use the default configuration.
7. Select **Event** as the **Tracking Type**.
8. You are now ready to add your event structure in the next few fields. Following our example, this would be:
   1. **Event Category:** Contact
   2. **Event Action:** Email Link Click
   3. **Event Name:** \{{ClickDestinationUrl\}}
   4.  **Event Value:**\
       &#xNAN;_**Note:**_ In the example above the Event Name is dynamically populated from a tag manager [_**Variable**_](https://matomo.org/docs/tag-manager/#variables) which pulls the data from the website itself. You can browse these from the square code icon next to the field.\


       <figure><img src="https://m-img.org/spai/w_320+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2014/04/event-name-variable.png" alt=""><figcaption></figcaption></figure>
9. The next step is to select your [_**Trigger**_](https://matomo.org/docs/tag-manager/#triggers). Click the link to **Create a new trigger now** which will pop up several options. For this example, you would select **All Links Click** in the **Click** section which will bring up options to customise which links to track as events.
10. For this example you can use _**All Email Links**_ as the trigger **Name**.
11. To limit the trigger so that it specifically targets email links, you’ll need to add a few details to the **Only trigger when (optional)** section. These steps will make sure only email links are counted:
    1. Select **Click Destination URL** in the first field
    2. Select **starts with** for the second field.
    3. Type _**mailto:**_ in the last field which is the prefix used to create email links.
    4. Finalise the trigger by clicking the big green button to **Create New Trigger**.
12. Now that the Trigger is attached to the Tag settings you can click to **Create New Tag**.
13. Finally you will need to publish your container. You can do this from the success message that appears when you create a new tag or by selecting [**Publish**](https://matomo.org/docs/tag-manager/#publish) in the menu.\
    ![Matomo Tag Created Successfully](https://m-img.org/spai/w_848+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2014/04/Matomo-tag-created.png)
14. Once your new event tracking tag is live, you should test that it is working. To do this, visit your site and click on an Email link. Then log in to Matomo, go to the **Visitors** section and click to view the **Visits Log**. This allows you to review visits in real-time and you should be able to see your visit with the event shown on the right-hand side.\


    <figure><img src="https://m-img.org/spai/w_848+q_lossless+ret_img+to_webp/matomo.org/wp-content/uploads/2014/04/event-tracking-visit-log.png" alt=""><figcaption></figcaption></figure>

### How To Set Up Matomo Event Tracking With JavaScript <a href="#how-to-set-up-matomo-event-tracking-with-javascript" id="how-to-set-up-matomo-event-tracking-with-javascript"></a>

The second method of setting up event tracking requires a little more technical confidence. You will need to integrate a JavaScript snippet directly into your website’s code. Links are one of the most common elements for integrating with event tracking.

JavaScript offers a function called **onClick** that you can add to your links. A standard HTML link looks a little something like this:

```
<a href="mailto:name@example.com" title="Email Us">Email Us</a>
```

Links in HTML contain the base tag **a** and several attributes including the link set under **href**. You can extend HTML links with the JavaScript **onClick** function so they look a little something like this:

```
<a href="mailto:name@example.com" title="Email Us" onclick="_paq.push(['trackEvent', 'Contact', 'Email Link Click', 'name@example.com']);">Email Us</a>
```

As you can see above, the code largely follows the same format as the first link above, but with an additional **onclick** attribute. The actual JavaScript function that triggers the event is contained within the quotation marks. From the example above that would be:

```
_paq.push(['trackEvent', 'Contact', 'Email Link Click', 'name@example.com']);
```

The above code example tracks the following event data:

* **Event Category:** Contact
* **Event Action:** Email Link Click
* **Event Name:** name@example.com

It works by telling Matomo that you want to push data into the platform `_paq.push([ ]);` and `trackEvent` defines the type of data being sent. Matomo then recognises the next two to four attributes as the Event’s; Category, Action, Name, Value.

You might also have noticed that the above code only shows three values after `trackEvent`. This is because the Event Value attribute is optional. If a Name or Value is not required, you can simply leave out the final parameters, rather than adding blank ones. For an example of an event where you may want to track a value, please see the sample below:

```
_paq.push(['trackEvent', 'eCommerce', 'Add to Wishlist', 'Smartphone', 1000]);
```

The above code example tracks when an item is added to a wishlist, with the value set to the item’s cost:

* **Event Category:** Ecommerce
* **Event Action:** Add to Wishlist
* **Event Name:** Smartphone
* **Event Value:** 1000

You may notice that as the value is a number, it does not require single quotation marks as the previous text strings do.
