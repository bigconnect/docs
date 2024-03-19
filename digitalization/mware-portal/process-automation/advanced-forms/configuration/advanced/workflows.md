# Workflows

### About workflows <a href="#about-workflows" id="about-workflows"></a>

Forms often participate in _workflows_, allowing collaboration between different users often having different tasks to perform depending on their role. Workflows are implemented using a combination of Orbeon Forms' capabilities:

* You can require authentication for certain pages. This way, should users try to access those pages without being logged in, they will be redirected to a login page. You can also require users not only to be logged in but to have a specific role.
* In Form Builder, using the Permissions dialog, you can define what actions (such as filling out a new form, reading or updating data), can perform be performed by which users.
* You can enable or disable some of the buttons shown at the bottom of the form, say depending on the role of the user viewing the form.
* In Form Builder, with formulas, you can decide whether certain fields should be hidden or made read-only, for instance based on who the user is. You can do the same for entire sections of the form.

As an example, in the following section we’ll explore how to use some of those features to implement a specific simple workflow.

### Form filled by the public, reviewed by staff <a href="#form-filled-by-the-public-reviewed-by-staff" id="form-filled-by-the-public-reviewed-by-staff"></a>

Consider the case of a city government who wants to put on their website a form allowing residents to report potholes. In that case, you would have 2 classes of users:

* Residents: they can fill and submit the form, most likely without having to be logged into the system.
* City staff: they will be logged into the system, can see all the submissions done by residents, and take further actions based on the reports. We’ll assume you have an authentication system in place for city employees, and that the subset of those employees who should be granted access to the reports submitted by citizen have the role `public-works-staff`.

In Form Builder, in the left sidebar, switch to the Advanced tab, click on Permissions, and fill out the dialog as follows:

![Permissions for pothole submission workflow](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-96fdc7f79d68034b8c768ab8c729a17e80d1057a%252Fworkflows-pothole.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=6c2a4910eb2ef3080e36bff6b2094e4320760e271e02dc0a0004c024c9c83fa7)

This will ensure that only users with the role `public-works-staff` can see any of the submitted data. Next, you’ll want to require users accessing submissions to be authenticated. Say the form app name is `public-works` and form name `report-pothole`. Then:

* The page to fill out a new instance of the form, often called the _new page_, is at `/fr/public-works/report-pothole/new`.
* The page to access all the submissions, often call the _summary page_, is at `/fr/public-works/report-pothole/summary`.

So you’ll want to require any user accessing the summary page to have the `public-works-staff` role. This can be done by editing the Orbeon Forms `web.xml`, adding the following, inside the root element `<web-app>`:

```
<security-constraint>
    <web-resource-collection>
        <web-resource-name>Public Works</web-resource-name>
        <url-pattern>/fr/public-works/report-pothole/summary</url-pattern>
    </web-resource-collection>
    <auth-constraint>
        <role-name>public-works-staff</role-name>
    </auth-constraint>
</security-constraint>
<login-config>
    <auth-method>FORM</auth-method>
    <form-login-config>
        <form-login-page>/fr/login</form-login-page>
        <form-error-page>/fr/login-error</form-error-page>
    </form-login-config>
</login-config>
<security-role>
    <role-name>forms-user</role-name>
</security-role>
```

Alternatively, if requests to your application server or servlet container (e.g. Tomcat) first go through a frontend web server, like Apache HTTP or IIS, you can also set this up at that level, instead of relying on the above configuration in the `web.xml`.

That’s it! With this, anyone will be able to access your form new page to submit new potholes, while only authorized city staff will be able to view the submissions that have been made through the form.
