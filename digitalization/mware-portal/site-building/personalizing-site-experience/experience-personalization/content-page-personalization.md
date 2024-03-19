# Content Page Personalization

You can tailor your Content Page to different groups of users, providing each group a personalized experience, rather than a generic one. You can also leverage Content Page personalization to conduct A/B Testing (for more information, see [Content Page Experiences and A/B Testing](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/content-page-personalization#content-page-experiences-and-ab-testing)).

Personalizing your Content Page for different users is a two-step process:

1. [Define a Segment](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/segmentation/creating-and-managing-user-segments) for the users you want to provide a personalized experience.
2. [Create an Experience](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/creating-and-managing-experiences) with customized content for your Segment.

### [Understanding How Experiences Work](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/content-page-personalization#understanding-how-experiences-work) <a href="#understanding-how-experiences-work" id="understanding-how-experiences-work"></a>

You can create multiple experiences for the same Content Page, each Experience covering a particular need. For example, you can create different experiences for those who log in and for those who browse anonymously. Each Experience contains the content and layout that is most appropriate for each Segment.

Consider the following information when you create multiple experiences for the same Content Page:

* Every Content Page includes a _Default_ experience. When you create a new Experience, _Default_ is used as a template.
* You can modify the Default experience, but existing experiences based on Default don’t change.
* You can duplicate any experience except Default.
* You can reorder Experiences in the selection dialog using the Up (![Up](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-up.png)) and Down (![Down](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-angle-down.png)) controls.
*   When you have multiple experiences in the same Content Page, their order determines their preference:

    * Experiences at the top of the list take precedence over the ones below.
    * When a user meets the criteria for more than one Experience, the Experience listed first is the active one.
    * An Experience is inactive when placed below an experience targeted to the _Anyone_ Segment.
    * New Experiences are listed below the _Default_ experience and inactive by default.

    TIP

    When multiple Experiences target the same Segment, Liferay DXP shows the active one next to the Experience’s name.

To better understand how the Experiences’ preference works, consider this example of a Content Page with four custom Experiences:

* _Anonymous User_ and _Anonymous User Promotions_, which apply to the non-authenticated user Segment.
* _Authenticated User_: Applies to the _Authenticated User_ Segment.
*   _No Customization_: Applies to the _Anyone_ Segment.

    ![The order of the experiences in the user interface determines their preference.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/personalizing-site-experience/experience-personalization/content-page-personalization/images/06.png)

In this example:

* A non-authenticated user sees the Experience _Anonymous User_ (the one with the _Active_ label). This user doesn’t see the _Anonymous User Promotions_ Experience because it appears below _Anonymous User_.
* Authenticated users don’t see the _Authenticated user_ Experience, because the _No customization_ Experience applies to the _Anyone_ Segment and it’s listed first.
* Anyone not in the _Anonymous User_ or _Authenticated User_ Segments sees the _No customization_ Experience because it targets _Anyone_.

### [Content Page Experiences and A/B Testing](https://learn.liferay.com/web/guest/w/dxp/site-building/personalizing-site-experience/experience-personalization/content-page-personalization#content-page-experiences-and-ab-testing) <a href="#content-page-experiences-and-ab-testing" id="content-page-experiences-and-ab-testing"></a>

When you [create an A/B Test](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/creating-ab-tests) in Liferay DXP, you choose an Experience for the test. This Experience can be the Default one or any other you have created.

To avoid changes in the Experience that can interfere with the A/B Test results, you cannot edit an Experience that is part of a running A/B Test. In this situation, the Experience shows a locker icon (![locker](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-lock.png)) next to its name.

For more information about A/B Testing in Liferay DXP and Liferay Analytics, see [A/B Testing](https://learn.liferay.com/web/guest/w/dxp/site-building/optimizing-sites/ab-testing/ab-testing).

\
