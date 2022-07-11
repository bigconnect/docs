# Custom templates

A custom template can be used to help streamline the deployment of a container or stack.

## Viewing the list of custom templates

To view a list of custom templates, from the menu select **App Templates** then select **Custom Templates**.

## Creating a new custom template

### Entering the basic information

Click **Add Custom Template** then complete the details:

![](<../../../.gitbook/assets/image (766).png>)

### Selecting the build method

Next, choose the build method that suits your needs. You can use the web editor to manually enter your docker-compose file, upload a docker-compose.yml file from your local computer, or pull the compose file from a Git repository.

#### Web editor

Paste the contents of your docker-compose file into the box provided. Once all the details have been completed, click **Create custom template**.

![](<../../../.gitbook/assets/image (722).png>)

#### Upload

Click **Select file** to browse for a docker-compose file to upload. Once all the details have been completed, click **Create custom template**.

![](<../../../.gitbook/assets/image (729).png>)

#### Git repository

Fill in the details for your Git repository.

![](<../../../.gitbook/assets/image (753).png>)

If your repository requires access authentication, toggle **Authentication** on then enter the username and password. When all the details have been entered, click **Create custom template**.

## Variables in templates

Custom templates support the use of variables to provide further customization of the deployed stack. A stack can define a variable that can then be adjusted by the user at deployment.

Variables are identified in stacks with `{{ }}`. For example, the following stack provides a `MYSQL_PASSWORD` variable:

![](<../../../.gitbook/assets/image (734).png>)

When a variable is defined, options appear to customize how the variable appears when deploying the stack. You can set the **label**, **description** and **default value**.

When a template is deployed, any variables that have been configured are editable:

![](<../../../.gitbook/assets/image (754).png>)
