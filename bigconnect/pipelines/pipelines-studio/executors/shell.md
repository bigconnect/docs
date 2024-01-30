# Shell

The Shell executor executes a shell script every time it receives an event. Use the Shell executor as part of an event stream.

When you configure the executor, you define the shell script that you want to execute and environment variables to propagate configuration for the script. You also specify the maximum amount of time for the shell script to run. After the specified time elapses, the executor stops the script.

Note: When using Shell executors in pipelines, you should configure Data Pipelines to use Shell executor impersonation mode.

For more information about dataflow triggers and the event framework, see Dataflow Triggers Overview.

### Data Pipelines Shell Impersonation Mode <a href="#ariaid-title2" id="ariaid-title2"></a>

Enable the Data Pipelines shell impersonation mode to enable the secure use of shell scripts. You enable the impersonation mode by configuring the shell impersonation mode property in the Data Pipelines configuration file. Enabling the impersonation mode is not required, but strongly recommended. You can also configure related shell and sudo properties as needed.

The Shell executor runs a user-defined shell script each time the stage receives an event. By default, Data Pipelines executes the script as the operating system user who starts Data Pipelines. Thus, using the default configuration means that the shell script can stop Data Pipelines as well as any other tasks the user has the rights to perform.

When you enable shell impersonation mode, the scripts are executed by the user who starts the pipeline. To use this option, the Data Pipelines user who starts the pipeline must have a corresponding operating system user account, and sudo must be configured to allow passwordless use. For greater security, you can also limit the permissions for the operating system user account to restrict its access.

To configure Data Pipelines to use the shell impersonation mode, perform the following steps:

1.  For each user who starts Shell executor pipelines, create a matching user account in the operating system and configure permissions as needed.

    For example, if Data Pipelines users Ops1 and Ops2 start all pipelines, create Ops1 and Ops2 user accounts in the operating system and grant them limited permissions.
2. Ensure that the each of the operating system users has passwordless sudo for Data Pipelines.
3.  Uncomment the following property in the Data Pipelines configuration file:

    ```
    stage.conf_com.streamsets.pipeline.stage.executor.shell.impersonation_mode=CURRENT_USER
    ```
4. Restart Data Pipelines.

### Script Configuration <a href="#ariaid-title4" id="ariaid-title4"></a>

When configuring a shell script for the Shell executor, note the following details:

1.  When you configure the shell script, ensure that the script returns zero (0) to indicate successful execution. For example, in a bash script, you can use "exit 0" to return the required zero value.

    A script that does not return zero might run successfully when tested on the command line, but will generate errors when used by the Shell executor in a pipeline.
2.  You cannot use expressions directly in the shell script. To use Data Pipelines expressions in a script:

    * Use the Environment Variables property in the stage to declare environment variables for the script. Create an environment variable for each expression that you want to use.
    * Use the environment variables as needed in the script.

    For example, say you want to perform an action on a file that was closed by a Hadoop FS destination - one that you cannot perform with the HDFS File Metadata executor. And you want to use the filepath field in the event record to specify the absolute path to the closed file.

    So you can define a filepath environment variable using the following expression: `${record:value('/filepath')}` as shown below, then use the filepath environment variable in the script:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Shell-EnvVariables.png)
