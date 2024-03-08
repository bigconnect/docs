# Using Publication Templates

To simplify creating publications with shared details and contributors, use publication templates. You can create a list of users to invite and assign roles and use tokens to auto-populate names and descriptions.

To create a publication template,

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ), go to the _Applications_ tab, and click _Publications_.
2.  Click _Actions_ ( ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) ) in the Application Bar and select _Templates_.

    ![Open the Publications application, click the Actions button in the Application Bar, and select Templates.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/01.png)
3. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ).
4.  Enter a name and description for the template.

    ![Enter a name and description for the template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/02.png)
5.  (Optional) Set this template as the default for new publications.

    **Default Template**: Manually created publications use this template by default, but you can select an alternative template during publication creation. When [Sandbox Only mode](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/enabling-publications#how-to-enable-publications) is enabled, automatically created publications use the template if a Default Sandbox Template isn’t set.

    **Default Sandbox Template**: Automatically created publications use this template when [Sandbox Only mode](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/enabling-publications#how-to-enable-publications) is enabled. You can still create publications manually using an alternative template.

    NOTE

    You can only have one default template and one default sandbox template at a time.
6.  Enter a name and description to use for publications created with the template.

    You can use string template tokens to generate values dynamically:

    * `${RANDOM_HASH}`
    * `${TODAY_DATE}`
    * `${CURRENT_USERNAME}`

    ![Enter a name and description to use for publications created with the template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/03.png)
7.  Click _Invite Users_ to specify the default users and role assignments for publications created with the template.

    ![Click Invite Users to specify the default users and role assignments for publications created with the template.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/04.png)

    To add contributors, search for users and select a role. See [Collaborating on Publications](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/collaborating-on-publications) for more information.

    ![Search for users and select a role.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/05.png)

    NOTE

    If you invite users without permission to access the Publications application, Liferay automatically assigns them the [Publications User](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/publications-permissions#publications-user-role) role.
8. Click _Save_.

The new template appears in the Publication Templates page.

![Templates appear in the Publication Templates page](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/06.png)

Users can select the template when [creating a publication](https://learn.liferay.com/web/guest/w/dxp/site-building/publishing-tools/publications/creating-and-managing-publications#creating-a-publication). Selecting the template automatically populates the name and description fields and invites the default contributors.

![Users can select the template when creating a publication.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/publishing-tools/publications/using-publication-templates/images/07.png)

\
