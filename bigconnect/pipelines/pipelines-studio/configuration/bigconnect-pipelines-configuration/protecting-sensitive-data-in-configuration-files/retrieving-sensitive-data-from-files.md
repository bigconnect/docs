# Retrieving Sensitive Data from Files

Use the `file` function in configuration files to retrieve sensitive data from a local file.

You can store a single piece of information in a file. When BigConnect Pipelines starts, it retrieves the sensitive data from the referenced files.

1.  Create a text file for each configuration value that you want to safeguard. Include only one configuration value in each file.\
    \
    Ensure that the file does not include extra characters, such as a newline character, after the sensitive data. For example, you might run the following command to ensure that the file does not include a newline character:

    ```
    echo -n '<password>' > password-file.txt
    ```
2.  Save the file in a local directory that BigConnect Pipelines can access.

    For a Cloudera Manager installation of BigConnect Pipelines, be sure to save the file outside of the BigConnect Pipelines configuration directory, `$SDC_CONF`, because the configuration directory changes when BigConnect Pipelines restarts. For all other BigConnect Pipelines installations, you can save the file in the configuration directory and then simply enter the file name when you use the `file` function.
3.  In the configuration file, set the relevant value to the `file` function and the appropriate file path and name.\
    \
    For a Cloudera Manager installation of BigConnect Pipelines, enter an absolute path to the file as follows:

    ```
    ${file("/tmp/mydata/password-file.txt")}
    ```

    For all other BigConnect Pipelines installations, you can enter an absolute path to the file or you can simply enter the file name if you stored the file in the BigConnect Pipelines configuration directory, `$SDC_CONF`. For example:

    ```
    ${file("password-file.txt")}
    ```
