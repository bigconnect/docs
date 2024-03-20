# Segments Admin Settings

You can configure Segments behavior at the system or instance level. With these settings, you can enable/disable segmentation, set the interval for syncing Analytics Cloud segments with the portal, create session-based segment properties, and more.

To access segment settings,

1.  Open the _Global Menu_ ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png), click the _Control Panel_ tab, and go to _System Settings_ or _Instance Settings_.

    NOTE

    Beginning with version 7.4, you can configure segment settings at the instance level.
2. Click _Segments_ under Content and Data.

See Understanding Configuration Scope to learn how system and instance scopes work.

### Analytics Cloud Segments <a href="#analytics-cloud-segments" id="analytics-cloud-segments"></a>

![View and configure Analytics Cloud Segments settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/segments-admin-settings/images/01.png)

Analytics Cloud segments aggregate individuals based on common attributes and behavior. You can integrate this data with the portal’s segments to deliver content of interest to both known and anonymous users.

You can access these settings under _Analytics Cloud Segments_:

**Anonymous User Segments Cache Expiration Time**: Set a time limit (in seconds) for how long the anonymous segment cache is stored. When the time expires, the cache is cleared.

**Interest Terms Cache Expiration Time**: Set a time limit (in seconds) for how long the interest terms cache is stored. When the time expires, the cache is cleared. See Understanding Interests for more information.

**Update Interval**: Set the interval (in minutes) for receiving updates from Analytics Cloud segments. This setting appears only under system settings.

### Session Property Vocabularies <a href="#session-property-vocabularies" id="session-property-vocabularies"></a>

With _Session Property Vocabularies_, you can create session-based Segment properties using predefined values. This helps when you want users to select properties from a list of predefined values, because it defines segments while eliminating manual entry errors. See Session Property Vocabularies for more information.

### Segments Service <a href="#segments-service" id="segments-service"></a>

![View and configure Segments Service settings.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/segments-admin-settings/images/02.png)

You can access these settings under _Segments Service_:

**Enable Segmentation**: Enable or disable segmentation. While disabled, the portal does not display personalized variations of collections and experiences to end users.

**Enable Assign Roles by Segment**: Enable or disable assigning roles to users dynamically using global segments. While enabled, you can assign roles automatically to groups of changing individuals. See Assigning Roles to User Segments for more information.

**Compound Segments Interval**: Set the interval (in minutes) for checking and refreshing compound segments based on their rules. This setting appears only under system settings.

NOTE

Please be aware of Configuration Scope. Settings disabled at the system level cannot be enabled at the instance level, but settings enabled at the system level can be disabled at the instance level.

\
