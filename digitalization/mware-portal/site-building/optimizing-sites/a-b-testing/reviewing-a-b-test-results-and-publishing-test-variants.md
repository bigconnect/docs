# Reviewing A/B Test Results and Publishing Test Variants

When the A/B Test finishes, you can find the test results in Analytics Cloud and Liferay DXP. Analytics Cloud declares a winner when the _Confidence Level Required_ is achieved during the duration of the test. To understand how to configure the Confidence Level Required for your test, see [Creating A/B Tests](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests).

TIP

You receive a [notification in Liferay](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/notifications-and-requests/user-guide/managing-notifications-and-requests) when the A/B Test finishes.

To review the A/B Test results and publish your variant,

1. Go to the Content Page where the A/B Test is running.
2. In the Control Menu, click the _A/B Testing_ flask icon (![A/B Test icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-ab-testing.png)).
3. If you have other Experiences for the Content Page, select the _Experience_.
4.  Review the test results under the _Active Test_ section:

    * _Winner Declared_: The confidence level requirement was met.
    * _No Winner_: The confidence level requirement was not met for the duration of the test.

    ![Review A/B Test Results from the A/B Test panel](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/reviewing-ab-test-results-and-publishing-test-variants/images/01.png)
5.  From the Variants section, you can

    * _Publish_ the variant, which is highlighted with the check mark.
    *   _Discard Test_, to ignore the A/B Test recommendations and keep the current Content Page.

        ![You can publish the winning Variant or discard the A/B Test results.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/reviewing-ab-test-results-and-publishing-test-variants/images/02.png)

    NOTE

    When you publish the A/B Test variant, the variant becomes active for all users visiting the Content Page.

You can click the _View Data in Analytics Cloud_ button from the A/B Tests panel to go to the Analytics Cloud dashboard and view other test statistics. For more information, see [A/B Testing Analytics](https://learn.liferay.com/analytics-cloud/latest/en/optimization/a-b-testing.html).

\
