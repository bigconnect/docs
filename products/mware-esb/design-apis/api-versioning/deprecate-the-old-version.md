# Deprecate the Old Version

When you publish a new version of an API, you have to maintain the old versions of the API until all the subscribers move to the new version. However, it would be best if you encourage subscribers to use the latest version. For this use case, you can use '**Deprecate old versions after publishing the API**' option when publishing the new version.

1.  Sign in to the API Publisher as a user who has the `publisher` role assigned to themselves.

    `https://<hostname>:9443/publisher`
2. Click on the API that you created in the [previous tutorial](https://apim.docs.wso2.com/en/4.2.0/design/api-versioning/create-a-new-api-version/) (e.g., `PizzaShackAPI 2.0.0`).
3. Click **Lifecycle**. The API Lifecycle page appears.
4. Check **Deprecate old versions after publishing the API** checkbox, if you want to deprecate previous versions.
5.  Click **Publish**.

    Info

    The **Publish** button is only accessible to users who have the `publisher` permission.



    <figure><img src="https://apim.docs.wso2.com/en/4.2.0/assets/img/learn/publish-version-api.png" alt=""><figcaption></figcaption></figure>
