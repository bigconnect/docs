# Running and Monitoring A/B Tests

You can configure these parameters when you run the A/B Test:

**Traffic Split:** The percentage of visitors randomly split between the original and the variant when visiting the page. Visitors are randomly split between the original and the variant and always see the same page until the A/B Test finishes.

**Confidence Level Required:** Represents the accuracy of your test. The higher the required confidence level, the longer it takes to declare the winner.

The _Estimated Time to Declare Winner_ provides an estimation of the test duration. This estimation is based on the Traffic Split and Confidence Level Required configurations, as well as on the estimated page traffic (based on the traffic history provided by Analytics Cloud).

![A/B Test Run Configurations are tunable to your requirements.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/running-and-monitoring-ab-tests/images/01.png)

After you create the test, review the test status by clicking _A/B Testing_ (![A/B Test icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-ab-testing.png)).

Liferay DXP only shows your test’s status and the winner when the test finishes. You can manage the other aspects of your A/B Test in Analytics Cloud. For more information, see [A/B Testing](https://learn.liferay.com/analytics-cloud/latest/en/optimization/a-b-testing.html) in the Analytics Cloud documentation.

When the A/B Test finishes, you can review the test results and publish the preferred variant. For more information, read [Reviewing A/B Test Results and Publishing Test Variants](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/reviewing-ab-test-results-and-publishing-test-variants).

### [Running the A/B Test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/running-and-monitoring-ab-tests#running-the-ab-test) <a href="#running-the-ab-test" id="running-the-ab-test"></a>

NOTE

Before running a test, you must create an A/B Test. For more information, read [Creating A/B Tests](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests).

1. Go to the Content Page where you want to run the A/B Test
2. In the Control Menu, click the _A/B Testing_ button (![A/B Test icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-ab-testing.png)).
3. If you have other Experiences for the Content Page, select the Experience.
4. Under the _Active Test_ section, click _Review and Run Test_.
5. Optionally, configure the _Traffic Split_ and _Confidence Level Required_ settings for your test.
6. Click _Run_ and then click _OK_.

You can cancel a running test at any time by clicking _Terminate Test_. To delete an A/B Test, you must terminate the test first.

\
