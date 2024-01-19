# Retrieving Sensitive Data Using Scripts

Use the `exec` function in configuration files to call a script or executable that retrieves sensitive data from an external location.

You must save the script on the local machine where BigConnect Pipelines runs. When BigConnect Pipelines starts, it runs the script to retrieve the sensitive data.

1. Develop a script or executable to retrieve each configuration value that you want to safeguard.\
   \
   Ensure that the script or executable does not include extra characters, such as a newline character, after the sensitive data.
2. Save the script or executable on the local machine where BigConnect Pipelines runs.
3.  In the configuration file, set the relevant value to the `exec` function and use the script or executable file name for the argument. Use the required syntax as follows:

    ```
    ${exec("<script name>")}
    ```

    If you save the script in the BigConnect Pipelines configuration directory, `$SDC_CONF`, enter just the script name for the argument, for example:

    ```
    ${exec("email_pwd.sh")}
    ```

    If you save the script outside of the BigConnect Pipelines configuration directory, enter an absolute path for the script name, for example:

    ```
    ${exec("/tmp/email_pwd.sh")}
    ```

    \
    \
    **Important**: Enter only the script or executable file name as the function argument. You cannot include parameters for the script within the argument. For example, `${exec("email_pwd.sh -name my_user")}` is not a valid argument. If the script or executable requires parameters, design a wrapper script to call the original script with the corresponding parameters and then call the wrapper script from the `exec` function.
