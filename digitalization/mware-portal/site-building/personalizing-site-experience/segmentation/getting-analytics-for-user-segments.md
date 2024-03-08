# Getting Analytics for User Segments

To use Analytics Cloud with User Segments, you must first connect your DXP data source to Analytics Cloud and enable synchronization of users and analytics. For more information about Analytics Cloud, including instructions for connecting it with DXP, see the official [Analytics Cloud Documentation](https://learn.liferay.com/analytics-cloud/latest/en/index.html).

IMPORTANT

Synchronization with Analytics Cloud is not instant, so once you have connected Analytics Cloud and Liferay DXP, you must first wait for the users and data to synchronize. After that completes, you can create Segments in Analytics Cloud to capture data in DXP.

Also note that segments created in Analytics Cloud sync with Liferay DXP. However, segments created in Liferay DXP do not sync with Analytics Cloud.

Follow these steps to get Segment analytics:

1.  [Create a Segment in Analytics Cloud](https://learn.liferay.com/analytics-cloud/latest/en/people/segments/creating-segments.html) if you haven’t already.

    NOTE

    Only Segments that contain at least one member are synchronized with Liferay DXP. This means that empty Segments created with Analytics Cloud are unavailable to use on Liferay DXP.
2. Once the Segment is synced, go to the _Segments_ page.
3. Click on the new Segment to view and customize it.

> Product image may differ slightly depending on your Liferay DXP version.

![When you see Analytics Cloud Segments in the list of Segments, they are marked with the Analytics Cloud icon.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/segmentation/getting-analytics-for-user-segments/images/02.png)

When you click on the Analytics Cloud Segment, you are directed to Analytics Cloud to edit the Segment and change your criteria. You cannot edit Analytics Cloud Segments in Liferay DXP.

\
