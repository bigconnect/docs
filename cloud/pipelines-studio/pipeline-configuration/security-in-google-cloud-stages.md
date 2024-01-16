# Security in Google Cloud Stages

You must configure the following Google Cloud stage types to pass credentials to Google Cloud:

* Google BigQuery stages
* Google Cloud Storage stages
* Google Pub/Sub stages

You can provide credentials using one the following options:

* Google Cloud default credentials
* Credentials in a file
* Credentials in a stage property

### &#x20;Default Credentials <a href="#ariaid-title2" id="ariaid-title2"></a>

You can configure a Google Cloud stage to use Google Cloud default credentials. When using Google Cloud default credentials, the pipeline checks for the credentials file defined in the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

If the environment variable doesn't exist and BigConnect Pipelines is running on a virtual machine (VM) in Google Cloud Platform (GCP), the pipeline uses the built-in service account associated with the virtual machine instance.

For more information about the default credentials, see [Finding credentials automatically](https://developers.google.com/identity/protocols/application-default-credentials) in the Google Cloud documentation.

Complete the following steps to define the credentials file in the environment variable:

1.  Use the Google Cloud Platform Console or the `gcloud` command-line tool to create a Google service account and have your application use it for API access.For example, to use the command line tool, run the following commands:

    ```
    gcloud iam service-accounts create my-account
    gcloud iam service-accounts keys create key.json --iam-account=my-account@my-project.iam.gserviceaccount.com
    ```
2.  Store the generated credentials file in a local directory external to the BigConnect Pipelines installation directory.For example, if you installed BigConnect Pipelines in the following directory:

    ```
    /opt/sdc/
    ```

    you might store the credentials file at:

    ```
    /opt/sdc-credentials
    ```
3.  Add the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to the appropriate file and point it to the credentials file.

    [Modify environment variables](https://docs.streamsets.com/portal/datacollector/3.22.x/help/datacollector/UserGuide/Configuration/DCEnvironmentConfig.html#concept\_zhl\_rb3\_qcb) using the method required by your installation type.

    Set the environment variable as follows:

    ```
    export GOOGLE_APPLICATION_CREDENTIALS="/var/lib/sdc-resources/keyfile.json"
    ```
4. Restart BigConnect Pipelines to enable the changes.
5. On the Credentials tab for the stage, for the Credential Provider property, select Default Credentials Provider.

### Credentials in a File <a href="#ariaid-title3" id="ariaid-title3"></a>

You can configure a Google Cloud stage to use credentials in a Google Cloud service account credentials JSON file.

Complete the following steps to use credentials in a file:

1.  Generate a service account credentials file in JSON format.

    Use the Google Cloud Platform Console or the `gcloud` command-line tool to generate and download the credentials file. For more information, see [Generating a service account credential](https://cloud.google.com/storage/docs/authentication#generating-a-private-key) in the Google Cloud Platform documentation.
2.  Store the generated credentials file on the BigConnect Pipelines machine.

    As a best practice, store the file in the BigConnect Pipelines resources directory, `$SDC_RESOURCES`.
3. On the Credentials tab for the stage, for the Credential Provider property, select Service Account Credentials File. Then, enter the path to the credentials file.

### Credentials in a Property <a href="#ariaid-title4" id="ariaid-title4"></a>

You can configure a Google Cloud stage to use credentials specified in a stage property. When using credentials in stage properties, you provide JSON-formatted credentials from a Google Cloud service account credential file.

You can enter credential details in plain text, but best practice is to secure the credential details using runtime resources or a credential store.

Complete the following steps to use credentials specified in stage properties:

1.  Generate a service account credentials file in JSON format.

    Use the Google Cloud Platform Console or the `gcloud` command-line tool to generate and download the credentials file. For more information, see [Generating a service account credential](https://cloud.google.com/storage/docs/authentication#generating-a-private-key) in the Google Cloud Platform documentation.
2. As a best practice, secure the credentials using runtime resources or a credential store.
3. On the Credentials tab for the stage, for the Credential Provider property, select Service Account Credentials. Then, enter the JSON-formatted credential details or an expression that calls the credentials from a credential store.
