# Creating Message Boards Categories

Message Boards Categories organize threads by topic. Only authenticated users with the requisite permissions (at minimum, _Add Category_, _Add Subcategory_) have the ability to create categories. To learn more about Roles and Permissions in general, see Roles and Permissions.

### Adding Categories <a href="#adding-categories" id="adding-categories"></a>

The most common way to add a new category is by directly using the _Message Boards_ widget.

1.  On the Message Boards widget, click the _Add Category_ button.

    ![Add a Message Board category.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/01.png)
2. Enter a name for the category (for example, **Staff Introductions**).
3. Enter a description.
4.  Select the category’s _Display Style_. This controls how threads in the category appear. By default, you can choose these display styles:

    * **Default:** Classic display style for general purpose discussions.
    * **Question:** Threads appear in a question and answer style.

    ![Select a display style.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/04.png)
5. In the _Mailing List_ section, leave the _Active_ toggle to _NO_ to disable a mailing list for the category. If switched to _YES_, fill in the mail server settings. For more information, see the [Setting up Mail](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/setting-up-liferay/configuring-mail) article.
6.  Leave the _Allow Anonymous Emails_ toggle to _NO_ to prevent anonymous users from posting to the category. Otherwise, if you want to allow anonymous users to send emails and post to the category, toggle the switch to _YES_.

    ![Configure the message board category.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/02.png)
7. Leave the _Viewable by_ to **Anyone (Guest Role)** for now. For more information about the different permissions available, see the Message Boards Permissions Reference
8. Click _Save_.

The new category now appears in the table.

New categories appear on the message board’s home screen. The list displays the category names and the numbers of subcategories, threads, and posts in each one.

### Modifying Permissions <a href="#modifying-permissions" id="modifying-permissions"></a>

Follow these steps to configure permissions for a Message Board category:

1. Click _Actions_ icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) next to the _Category_.
2. Click _Permissions_.
3.  Check the boxes to grant permissions to the desired roles.

    For more information about each permission, see Message Boards Permissions Reference.

    ![Modify permissions for a Message Board category.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/06.png)
4. Click _Save_ when finished.

### Adding Subcategories <a href="#adding-subcategories" id="adding-subcategories"></a>

Categories can contain as many subcategories as you like.

Follow these steps to add a subcategory to a category:

1. Click the category’s name in the list (continuing the example above: **Staff Introductions**).
2. Click the _Add_ icon (![Add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)) and select _Category_.
3. Enter a name for the subcategory.
4. Enter a description for the subcategory.
5. Although the subcategory inherits the parent category’s settings, administrators and content creators can change the values for the subcategory’s _Display Style_ and _Mailing List_ options.
6. Click _Save_.

The subcategory now appears in the table.

![Adding a subcategory](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/07.png)

### Moving and Merging Categories <a href="#moving-and-merging-categories" id="moving-and-merging-categories"></a>

Administrators can also move and merge categories.

Follow these steps to move a category or merge it with another:

1. Click the category’s _Actions_ icon (![Actions](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png)) and select _Move_. This brings up the Move Category form.
2. Select a new parent category via the _Select_ button under the _Parent Category_ field. Note that this field is empty for top-level categories.
3. If you want to merge the category with the selected parent category, select _Merge with Parent Category_.
4.  Click _Move_.

    ![Move and merge categories.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/message-boards/user-guide/creating-message-boards-categories/images/03.png)

Regardless of how many categories (and subcategories) there are, a category is just a container to organize a message board’s threads. To start creating threads, see the Creating Threads article.

\
