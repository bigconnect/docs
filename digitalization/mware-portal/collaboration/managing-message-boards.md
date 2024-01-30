# Managing Message Boards

Message boards can become unwieldy if left unmanaged. The Message Boards in Site Administration facilitates day-to-day thread administration. You may wish to assign this function to a Role that you give to one or more users. This frees you to concentrate on other areas of your Site. For example, you can create a Role called _Message Board Administrator_ scoped to the portal (globally), an Organization, or a Site. Members of a global Role can administer Message Boards throughout the portal. Members of an Organization or Site-scoped Role can only administer Message Boards in that Organization or Site, respectively.

Follow these steps to create a global Role:

1. In the Control Panel, select _Users_ → _Roles_.
2. Select or create the Role.
3.  Select the Role’s _Define Permissions_ tab and navigate to _Site Administration_ → _Content & Data_ → _Message Boards_. A screen appears that lets you configure Message Board permissions.

    <img src="https://liferay-support.zendesk.com/hc/article_attachments/360029926032/message-boards-role-permissions.png" alt="Figure 1: Define the permissions you want to use for the message boards administrators." data-size="original">

    Figure 1: Define the permissions you want to use for the message boards administrators.
4. Select the permissions you want message board administrators to have, then click _Save_.
5. Add users to this Role.

### Locking Threads <a href="#locking-threads" id="locking-threads"></a>

You may encounter threads that you think should be preserved, but stopped. You can halt activity on a thread by selecting _Lock_ from the thread’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)).

### Moving Threads <a href="#moving-threads" id="moving-threads"></a>

If someone posts a thread to the wrong category, you can move it to the proper one. Follow these steps:

1. Select _Move_ from the thread’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)). This opens the _Move Thread_ form.
2. Click the _Select_ button and select the new category.
3. If you want to add a post explaining the move, select _Add explanation post_.
4. Click _Move_ to move the thread.

### Deleting Threads <a href="#deleting-threads" id="deleting-threads"></a>

Sometimes users begin discussing topics that are inappropriate or that reveal confidential information. In this case, administrators can delete the thread from the message boards. To do so, select _Move to Recycle Bin_ from the thread’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)).

### Banning Users <a href="#banning-users" id="banning-users"></a>

Unfortunately, message board users can be abusive. In this case, you can ban the user from the message boards. While viewing any of the user’s posts in any thread, select the post’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)) and select _Ban this User_.

To reinstate a banned user, you must use the Message Boards app in Site Administration. Navigate to this app and select the _Banned Users_ tab. Select the user’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)) and select _Unban this User_.

### Splitting Threads <a href="#splitting-threads" id="splitting-threads"></a>

Sometimes a thread goes on for a while and the discussion completely changes into something else. In this case, you can split the thread where the discussion diverged and create a whole new thread for the new topic. To split a thread at a certain post, administrators can select that post’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)) and select _Split Thread_. This brings up a form that lets you add an explanation post to the split thread. Click _OK_ to split the thread.

### Editing Posts <a href="#editing-posts" id="editing-posts"></a>

Administrative users can edit anyone’s posts, not just their own. Sometimes users post links to copyrighted material or unsuitable pictures. By editing these posts, you can redact information that shouldn’t be posted, or remove content not conforming to your terms of use. You can also update the thread’s priority or mark a reply as an answer to a thread’s question.

To edit a post, select its _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)) and select _Edit_.

### Post Permissions <a href="#post-permissions" id="post-permissions"></a>

Permissions can be set not only on threads, but also on individual posts. You can choose to limit a particular conversation or post to only a select group of users. To do this, select the post’s _Actions_ menu (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360029926052/icon-actions.png)) and select _Permissions_. You can then choose which Roles have the following permissions:

* Delete
* Permissions
* Subscribe
* Update
* View

Use this, for example, to let some privileged users post on a certain thread, while others are only allowed to view it. Other combinations of these permissions are also possible.
