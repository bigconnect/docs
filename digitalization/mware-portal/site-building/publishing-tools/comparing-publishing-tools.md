# Comparing Publishing Tools

Liferay provides two publishing tools for content creation and site building: Publications and Staging. Publications is recommended over Staging in nearly all use cases because it provides a superior collaboration experience and greater integration with other Liferay features. The one exception is if you need to host your working and production environments on separate servers. For this you must use Remote Live staging.

### [Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/comparing-publishing-tools#publications) <a href="#publications" id="publications"></a>

Publications is a change-tracking tool for editing sites and content. With it, contributors can create working environments called _publications_ where they make edits without affecting the production environment’s published site. Each publication is company-scoped and can group changes across multiple sites and asset libraries. You can create as many publications as needed for your projects and teams.

By default, access to a publication is limited to its creator. However, creators can invite other members and assign them roles specific to each publication. Since publications are independent, contributors can make and publish their changes without affecting one another. Contributors to a publication can work on their own timelines and publish their changes when ready.

![Group changes into publications and publish them when ready.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/comparing-publishing-tools/images/01.png)

During the publishing process, Liferay checks for conflicts with production and prompts the user to review and resolve them. Some conflicts can be resolved automatically, while others require manual resolution. Once published, the changes are propagated to other publications if there are no conflicts. Publications maintains a detailed history of published changes so you can quickly review and revert changes if needed.

Together, these features provide a superior editing and collaboration experience. See [Enabling Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/enabling-publications) to set up Publications for your instance.

IMPORTANT

Beginning with Liferay 7.4 U44/GA44, Publications is fully integrated with [Workflow](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow), so you can enable workflows for entities tracked by Publications.

### [Staging](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/comparing-publishing-tools#staging) <a href="#staging" id="staging"></a>

With Staging, contributors work in a shared _staging_ environment before publishing their changes to a _live_ environment. Users enable staging for sites and asset libraries individually. When enabled, Liferay creates a separate copy of the original site or library and hosts them on either the same server ([Local Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-local-live-staging)) or on separate servers ([Remote Live](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/configuring-remote-live-staging)).

![Liferay creates a separate copy of the original site or library.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/comparing-publishing-tools/images/02.png)

All changes are site- or asset library-scoped. Within the site or asset library, you can determine the exact pages and application data you want to stage. Only changes to explicitly staged data are tracked. Publish your changes when ready, either individually or all together, while the live environment handles incoming user traffic.

While Publications is recommended over Staging, Remote Live staging is necessary if you use a multi-tier architecture for your deployment model or have different security requirements for your working and production environments. This is because Publications does not currently offer a remote option. In all other use cases, use Publications.

IMPORTANT

If you must use Staging, you should enable it at the beginning of development. When enabled, Staging immediately begins copying data between your environments, which can cause errors if a lot of data already exists. Staging works best by publishing data frequently in small blocks. Publishing changes incrementally helps avoid publishing errors.

### [Feature Comparison](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/comparing-publishing-tools#feature-comparison) <a href="#feature-comparison" id="feature-comparison"></a>

| Feature                                                                                | Publications | Staging |
| -------------------------------------------------------------------------------------- | ------------ | ------- |
| Host your working and production environments on the same server                       | ✔            | ✔       |
| Host your working and production environments on separate servers                      | ✘            | ✔       |
| Supports Asset Libraries                                                               | ✔            | ✔       |
| Create and manage separate working environments for different projects and teams       | ✔            | ✘       |
| Comment on blocks of changes                                                           | ✔            | ✘       |
| Use custom workflows                                                                   | ✔            | ✔       |
| Preview changes before publishing                                                      | ✔            | ✔       |
| Review differences between working and production environments                         | ✔            | ✘       |
| Include changes from multiple sites or asset libraries in the same working environment | ✔            | ✘       |
| Schedule when to publish changes                                                       | ✔            | ✔       |
| Automatically send notifications/alerts for publishing events                          | ✔            | ✔       |
| Maintain a detailed history of all published changes                                   | ✔            | ✘       |
| Revert published changes to pages                                                      | ✔            | ✔       |
| Revert published changes to content                                                    | ✔            | ✘       |

#### [Supported Pages and Content Types](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/comparing-publishing-tools#supported-pages-and-content-types) <a href="#supported-pages-and-content-types" id="supported-pages-and-content-types"></a>

Liferay 7.4 U86+/GA86+

Liferay’s publishing tools support the following types of pages and content:

| Feature                      | Publications | Staging |
| ---------------------------- | ------------ | ------- |
| Accounts (Contact Info Only) | ✔            | ✘       |
| Blogs                        | ✔            | ✔       |
| Commerce Entities            | ✘            | ✘       |
| Content Pages                | ✔            | ✘       |
| Custom Objects               | ✘            | ✘       |
| Documents and Media          | ✔            | ✔       |
| Forms                        | ✔            | ✔       |
| Knowledge Base               | ✔            | ✔       |
| Message Boards               | ✔            | ✔       |
| Organizations                | ✔            | ✘       |
| Page Templates               | ✔            | ✔       |
| Site Templates               | ✔            | ✘       |
| Users (Contact Info Only)    | ✔            | ✘       |
| Web Content                  | ✔            | ✔       |
| Widget Pages                 | ✔            | ✔       |
| Wiki                         | ✔            | ✔       |

IMPORTANT

Content pages do not support Staging’s [page variations](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/staging/page-versioning) feature.

\
