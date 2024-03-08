# Creating A/B Tests

Before creating A/B tests, remember these things:

* You can create a test for a default Experience or a personalized Experience mapped to a Segment.
* You can only create an A/B Test for one page or Experience at a time.
* You cannot edit an Experience that is part of a running A/B Test.
* You cannot create a test for an Experience that already has an active A/B Test running.
* When you delete a Content Page or Experience that is part of an A/B Test, you also delete the test.
*   If you close the A/B Test side panel without [running the test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/running-and-monitoring-ab-tests), the test is saved as a draft.

    ![When you configure a test without running it, the test is saved as a draft](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/creating-ab-tests/images/05.png)

To create the A/B test, [create the test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests#creating-the-test) first, and then create a [test variant](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests#creating-the-test-variant).

IMPORTANT

Verify that your environment [meets the requirements](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/verifying-ab-test-requirements) for A/B Testing before creating the test.

### [Creating the Test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests#creating-the-test) <a href="#creating-the-test" id="creating-the-test"></a>

1. Go to the Content Page you want to test.
2. In the Control Menu, click _A/B Testing_ (![A/B Test icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-ab-testing.png)).
3.  Click _Create Test_.

    ![Click Create Test to get started.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/creating-ab-tests/images/04.png)
4. Enter a _Test Name_ and optionally, a _Description_.
5. Select the goal to track with your test:
   * _Bounce Rate_: Percentage of those who visit, don’t exhibit any activity (like _click_ or _scroll_), and navigate away without visiting another page.
   * _Click_: Percentage of those who click on the page, per session.
6.  Click _Save_ to save the test as a _Draft_ (not yet visible to visitors).

    NOTE

    You can always edit or delete the new A/B Test by clicking the _Actions_ button next to the A/B Test name. Deleted tests are not recoverable (i.e., not sent to the Recycle Bin). These options are not available for an active running test.

### [Creating the Test Variant](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests#creating-the-test-variant) <a href="#creating-the-test-variant" id="creating-the-test-variant"></a>

A test variant is a customization of the Experience you want to optimize with A/B Testing. Each A/B Test supports one variant.

1. Open the A/B Test sidebar panel for your Content Page.
2. If you selected the _Click_ goal for the A/B Test, you must also select the element you want to test:
   1.  Under the _Active Test_ tab and _Click Goal_ section, click _Select Clickable Element_.

       * You can target collection display fragments, links, buttons, and input fields with an Element ID for the _Click_ goal.
       * You can also enter the _Element ID_ of the element you want to test.
       * To highlight the Page element selected, click the Preview (![Preview](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-preview.png)) icon.
       * While the [test is running](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/running-and-monitoring-ab-tests), the _Change Clickable Element_ button is unavailable and the _Element ID_ cannot be changed.

       ![Click the Content Page Element you want to test or enter its Element ID in the Click Goal section](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/creating-ab-tests/images/03.png)
   2. Click the element you want to test on the Content Page.
   3.  Click _Set Element as Click Target_.

       ![Click the Set Element as Click Target button to select it](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/optimizing-sites/ab-testing/creating-ab-tests/images/01.png)

       NOTE

       The click target element applies to the whole A/B Test and must be present in the variant.
3. Under the _Variants_ section, click _Create Variant_.
4. Enter a name for the Variant and click _Save_.
5. Click the _Edit_ (![Edit icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-edit.png)) button next to the variant’s name. The current Content Page opens in editing mode as the baseline for the variant.
6. Edit the variant with the changes you want to test. For example, if you want to test different call-to-action text, update the button with the new text.
7. Click _Save Variant_.

After creating your test and test variant, you can [run the test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/running-and-monitoring-ab-tests).

\
