# Using the Script Engine

The portal provides a robust script engine for executing [Groovy](http://groovy-lang.org/) scripts to maintain your portal instance. You can execute scripts to perform maintenance tasks involving data cleanup, user maintenance operations, bulk API invocations, or even system level operations.

![The Script Console executes Groovy scripts and provides context variables, such as the current actionRequest. The Script Console was designed for invoking the portal Services.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-script-engine/images/01.png)

### Navigating to the Script Engine <a href="#navigating-to-the-script-engine" id="navigating-to-the-script-engine"></a>

The Script Console is available in the Control Panel. To use the Script Engine, follow these steps:

1. Log in as an Admin user
2.  Open the Product Menu and navigate to the Control Panel and select _Configuration_ → _Server Administration_ → _Script_.

    ![The Script Console is a tab within the System Administration menu.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-script-engine/images/02.png)



## Running Scripts From the Script Console <a href="#running-scripts-from-the-script-console" id="running-scripts-from-the-script-console"></a>

The Script Console provides a single view for executing Groovy scripts and printing their output. It has predefined variables that facilitate working with widgets and users. The following topics get you started with the Script Console:

* Running a Sample Script in the Script Console
* Predefined variables available in the Script Console
* Tips for running scripts in the Script Console

IMPORTANT

The Script Console is for system operations and maintenance—it’s not for end users. Limit Script Console access to portal administrators.

Start with running the Script Console’s sample script.

### Running a Sample Script in the Script Console <a href="#running-a-sample-script-in-the-script-console" id="running-a-sample-script-in-the-script-console"></a>

Here’s how to run the sample script in the Script Console:

1. Sign in as an administrator.
2. In the Product Menu, navigate to _Control Panel_ and select _Configuration_ → _Server Administration_.
3.  Click on _Script_. This is the Script Console. The default sample script prints the User count to the console output.

    ```groovy
    // ### Groovy Sample ###

    number = com.liferay.portal.kernel.service.UserLocalServiceUtil.getUsersCount();

    out.println(number);
    ```
4.  Click _Execute_ and check the Script Console _Output_ for the User count.

    ![The Script Console's sample Groovy script prints the User count to Script Console output.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-script-engine/running-scripts-from-the-script-console/images/01.png)

The Groovy sample invokes the portal service utility `UserLocalServiceUtil` to get the user count. Then it uses `out` (a built-in `PrintWriter`) to write the count to the Script Console.

NOTE

If you use _System.out.println_ instead of _out.println_, your output is printed to the portal’s log file rather than to the Script Console.

### Predefined Script Console Variables <a href="#predefined-script-console-variables" id="predefined-script-console-variables"></a>

Here are the predefined variables available to scripts in the Script Console:

| Variable         | Class                              |
| ---------------- | ---------------------------------- |
| `out`            | `java.io.PrintWriter`              |
| `actionRequest`  | `javax.portlet.ActionRequest`      |
| `actionResponse` | `javax.portlet.ActionReponse`      |
| `portletConfig`  | `javax.portlet.PortletConfig`      |
| `portletContext` | `javax.portlet.PortletContext`     |
| `preferences`    | `javax.portlet.PortletPreferences` |
| `userInfo`       | `java.util.Map<String, String>`    |

#### Variable Usage <a href="#variable-usage" id="variable-usage"></a>

This script demonstrates using the `actionRequest` variable to get the portal instance’s `Company`:

```groovy
import com.liferay.portal.kernel.util.*

company = PortalUtil.getCompany(actionRequest)
out.println("Current Company:${company.getName()}\n")

out.println("User Info:")
userInfo.each {
        k,v -> out.println("${k}:${v}")
}
```

![Here's an example of invoking a Groovy script that uses the predefined out, actionRequest, and userInfo variables to print information about the Company and User.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/using-the-script-engine/running-scripts-from-the-script-console/images/02.png)

### Tips <a href="#tips" id="tips"></a>

Keep these things in mind when using the Script Console:

* There is no undo.
* There is no preview.
* Permissions checking is not enforced for local services.
* Scripts are executed synchronously. Avoid executing scripts that might take a long time.

Use the Script Console cautiously and test your scripts on non-production systems before running them on production.

The script engine can be used outside of the Script Console, such as in a Kaleo Workflow. Using the script engine in workflows is next.

\
