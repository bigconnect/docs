# Message Board Categories

Message Board categories organize threads by topic. This makes it easier to find the right topic for discussion, and can also help discussions stay on topic. For example, a tropical fishkeeping message board may have separate categories for freshwater and saltwater topics.

This article shows you how to create and manage message board categories.

### Adding Categories <a href="#adding-categories" id="adding-categories"></a>

Follow these steps to create a message board category:

1. Open the Message Boards app in your scope’s Site Administration menu, as described in Creating Message Boards.
2.  Click the _Add_ icon (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029925892/icon-add.png)) and select _Category_. This opens the Add Category form.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029925952/message-boards-add-category.png" alt="Figure 1: You have several options to create a message board category for your needs." data-size="original">

    Figure 1: You have several options to create a message board category for your needs.
3. Enter a name and description for the category.
4.  Select the category’s _Display Style_. This controls how threads in the category appear. By default, you can choose these display styles:

    **Default:** Classic display style for general purpose discussions.

    **Question:** Threads appear in a question and answer style.

    You can create custom display styles and make them available for selection in this form. You must set the available display styles via the \[portal property] `message.boards.category.display.styles`. Similarly, you can set the default display style in `message.boards.category.display.styles.default`.
5. Open the _Mailing List_ section of the form and set the mailing list options you want. To enable a mailing list for the category, set the _Active_ toggle to _YES_. To enable anonymous emails in the list, set the _Allow Anonymous Emails_ toggle to _YES_. The default for both toggles is _NO_.&#x20;
6.  Open the _Permissions_ section and set the category’s permissions. The _Viewable by_ selector lets you pick who can view the category:

    * Anyone (Guest Role)
    * Site Members
    * Owner

    To show more permissions options, click _More Options_. A table appears with the rest of the category’s permissions, which you can assign to the Guest and Site Member roles:

    **Delete:** Remove the category.

    **Permissions:** View and modify permissions.

    **Add File:** Attach a file to any of your messages.

    **Reply to Message:** Respond to existing messages.

    **Lock Thread:** Stop any further additions or modifications to a thread’s messages.

    **Update:** Edit the category.

    **Subscribe:** Receive notifications on new and modified posts.

    **View:** View the category.

    **Add Message:** Post a new thread.

    **Move Thread:** Move a thread to a different category or subcategory.

    **Add Subcategory:** Add a new category within this category.

    **Update Thread Priority:** Modify a thread’s priority.

    Note that after creating a category, you can revisit its permission options by clicking the category’s _Actions_ icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910251/icon-actions.png)) and selecting _Permissions_.
7. Click _Save_ when you’re finished. Your category now appears in the table.

As you add categories to a message board, they appear on the message board’s home screen. The list displays the category names and the numbers of subcategories, threads, and posts in each one.

![Figure 2: Categories help you organize threads so users can find topical threads that interest them.](https://liferay-support.zendesk.com/hc/article\_attachments/360029925912/message-boards-home.png)

### Adding Subcategories <a href="#adding-subcategories" id="adding-subcategories"></a>

Categories can contain as many subcategories as you like. If, however, you nest categories too deep, users can have trouble finding them.

Follow these steps to add a subcategory to a category:

1. Click the category’s name in the list, then click the _Add_ icon (![Add](https://liferay-support.zendesk.com/hc/article\_attachments/360029925892/icon-add.png)) and select _Category_. The Add Category form appears.
2. Fill out the Add Category form with the values and settings you want to use for the subcategory. This form is populated with the parent category’s properties by default.
3. Click _Save_ when you’re finished. Your subcategory now appears in the table.

### Moving and Merging Categories <a href="#moving-and-merging-categories" id="moving-and-merging-categories"></a>

Each category can have any number of threads, and you can add as many categories and subcategories as you wish. You can also move and merge categories.

Follow these steps to move a category or merge it with another:

1. Click the category’s _Actions_ icon (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029910251/icon-actions.png)) and select _Move_. This brings up the Move Category form.
2. Select a new parent category via the _Select_ button under the _Parent Category_ field. Note that this field is empty for top-level categories.
3. If you want to merge the category with the selected parent category, select _Merge with Parent Category_.
4. Click _Move_.

![Figure 3: The Move Category form lets you move and merge categories.](https://liferay-support.zendesk.com/hc/article\_attachments/360029925932/mb-move-merge.png)
