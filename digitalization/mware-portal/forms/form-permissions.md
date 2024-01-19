# Form Permissions

To access a form’s permissions, first navigate to the Forms app in _Site Administration_ (your site’s menu) → _Content & Data_ → _Forms_. Then click the form’s _Actions_ button (![Actions](https://liferay-support.zendesk.com/hc/article\_attachments/360030163491/icon-actions.png)), and select _Permissions_.

By default, you can grant these permissions for a form:

**Delete:** Delete the form.

**Permissions:** Access and configure the form’s permissions.

**Update:** Update form entries.

**Add Form Instance Record:** Submit form entries.

**View:** View the form.

![Figure 1: You can configure a forms permissions.](https://liferay-support.zendesk.com/hc/article\_attachments/360030194992/forms-form-permissions.png)

Figure 1: You can configure a form's permissions.

Note that guest users can view and fill out forms by default. The _Guest_ Role has _View_ and _Add Form Instance Record_ permissions.

**Note:** By default, all users inherit the Guest Role’s permissions. The Guest Role represents unauthenticated visitors of your Site. If you want to let Guest users submit forms (the default setting), it makes sense that authenticated users can also. To disable automatic inheritance of the Guest Role’s permissions, set this property in your `portal-ext.properties` file:

```
`permissions.check.guest.enabled=false`
```
