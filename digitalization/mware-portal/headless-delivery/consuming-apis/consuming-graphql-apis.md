# Consuming GraphQL APIs

Liferay DXP contains [GraphQL](https://graphql.org/) APIs for most of its applications. Here, you’ll learn how to consume them. This takes only three steps:

1. Identify the API you wish to consume.
2. Identify the site containing the data you need.
3. Make the API call using credentials that have access to the data.

You need a running Liferay DXP to call its GraphQL APIs.

Start a new Liferay instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.112-ga112
```

Sign in to Liferay at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

### [Identify the Service to Consume](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#identify-the-service-to-consume) <a href="#identify-the-service-to-consume" id="identify-the-service-to-consume"></a>

Follow these steps:

1. Go to `http://localhost:8080/o/api`.
2. At the top-left of the screen that appears, pick _headless-delivery_ from the selector. This is the category that contains the Blog posting API.
3. Click the _Show GraphQL_ button at the top-right of the screen to open Liferay’s [GraphiQL](https://github.com/graphql/graphiql) browser.
4. Click the _Docs_ link below the button you just clicked. Now you can browse the API.
5. GraphQL separates read and write operations by calling the first a _query_ and the second a _mutation_. Since the first thing you want to do is post a blog entry, click _mutation_.
6.  A list appears of the entire API use the search at the top or scroll down and find the call to `createSiteBlogPosting`:

    ```graphql
    createSiteBlogPosting(
      blogPosting: InputBlogPosting
      siteKey: String!
      ): BlogPosting
    ```

NOTE

You can also discover your local installation’s APIs by requesting the schema directly:

`curl 'http://localhost:8080/o/graphql' -H "Content-Type: application/json" --data '{"query":"query{ __schema{ queryType{ name fields{ name args{ name } description } } } }","variables":{}}'`

This URL does not require authentication, but it’s quite cumbersome to manage the returned schema. For this reason, it’s better to use the included GraphQL client.

![The included GraphQL client has a schema documentation browser.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/consuming-apis/consuming-graphql-apis/images/01.png)

Since the API requires you to know the Site containing the Blog where the entry should be posted, you must find the Site ID first.

### [Identify the Site Containing the Data](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#identify-the-site-containing-the-data) <a href="#identify-the-site-containing-the-data" id="identify-the-site-containing-the-data"></a>

Now you must find the Site ID:

1. Go to `http://localhost:8080`.
2. Open the _Global Menu_ ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png), click the _Control Panel_ tab, and go to _Sites_ → Sites.
3. Click the _Actions_ button ![Actions Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-actions.png) next to the Liferay Site and choose _Go to Site Settings_.
4. Go to _Site Configuration_.

The Site ID appears at the top of the Details section. It’s an Integer, like `20122`.

### [Make the Service Call using Credentials with Access to the Data](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#make-the-service-call-using-credentials-with-access-to-the-data) <a href="#make-the-service-call-using-credentials-with-access-to-the-data" id="make-the-service-call-using-credentials-with-access-to-the-data"></a>

Now you have everything you need to make the call. All web services must be accessed using credentials that have access to the data you’re requesting. The included GraphQL client authenticates using your browser. If your plan is to write a standalone client, you should authorize users via [OAuth2](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2).

During development, it’s much easier to use Basic Auth, which passes credential data in the URL. Since this is insecure, _never use this method for production._

#### [Calling a GraphQL API Using Basic Auth (During Development Only)](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#calling-a-graphql-api-using-basic-auth-during-development-only) <a href="#calling-a-graphql-api-using-basic-auth-during-development-only" id="calling-a-graphql-api-using-basic-auth-during-development-only"></a>

To call a service using Basic Auth, provide the credentials in the URL:

```bash
curl --request POST --url http://localhost:8080/o/graphql \ -u test@liferay.com:learn  --header 'content-type: application/json' --data '{"query":"query {blogPostings(filter: \"\", page: 1, pageSize: 10, search: \"\", siteKey: \"20122\", sort: \"\"){ page  items{ id articleBody headline  creator{ name }}}}"}'
```

#### [Calling a Service Using OAuth2](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#calling-a-service-using-oauth2) <a href="#calling-a-service-using-oauth2" id="calling-a-service-using-oauth2"></a>

NOTE

The use of GraphQL with OAuth2 is supported in Liferay DXP 7.4 U77+/Liferay Portal 7.4 GA77+

For production, create an [OAuth2 application](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/creating-oauth2-applications) and use the OAuth2 process to get an authorization token. Once you have the token, provide it in the HTTP header:

```bash
curl --request POST --url http://localhost:8080/o/graphql -H "Authorization: Bearer d5571ff781dc555415c478872f0755c773fa159" --header 'content-type: application/json' --data '{"query":"query {blogPostings(filter: \"\", page: 1, pageSize: 10, search: \"\", siteKey: \"20122\", sort: \"\"){ page  items{ id articleBody headline  creator{ name }}}}"}'
```

### [Getting and Posting Data](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#getting-and-posting-data) <a href="#getting-and-posting-data" id="getting-and-posting-data"></a>

In the top left window of the GraphQL client, place this code, which retrieves all blog entries:

```graphql
query {blogPostings(filter:"",page:1,pageSize:10,search:"",siteKey:"20122",sort:"")
	{page
     items {
        id
  		articleBody
  		headline
  		creator
  			{name
            }
  }
  }
}
```

Click the play button to run it, and you’ll see there aren’t any blog entries:

```json
{"data":{"blogPostings":{"page":1,"items":[]}}}
```

Now you’ll post a blog entry.

#### [Posting a Blog Entry](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#posting-a-blog-entry) <a href="#posting-a-blog-entry" id="posting-a-blog-entry"></a>

The GraphQL schema revealed the call that must be made to post a blog entry.

1.  Construct a JSON document containing the entry you wish to publish:

    ```json
    {
      "blog": {
          "articleBody": "This Blog entry was created by calling the GraphQL service!",
          "headline": "GraphQL Blog Entry"
      }
    }
    ```
2.  Construct the GraphQL query based on the schema documentation:

    ```log
    mutation CreateBlog($blog: InputBlogPosting){
      createSiteBlogPosting(blogPosting: $blog, siteKey: "20122" ) {
        headline
        articleBody
        id
        friendlyUrlPath
      }

     }
    ```
3. Make the request by visiting `http://localhost:8080/o/api` again. Click the _Show GraphQL_ button.
4. Paste your JSON document into the Query Variables box at the lower left.
5. Paste your mutation in the unlabeled box at the top right.
6. Run your query by clicking the play button at the top.

The blog entry you added now appears in the GraphQL client’s right pane:

![The GraphQL client makes it easy to call GraphQL services directly from your browser.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/consuming-apis/consuming-graphql-apis/images/02.png)

Liferay DXP returns a JSON representation of your blog entry that contains the fields you requested in the mutation:

```json
{
  "data": {
    "createSiteBlogPosting": {
      "headline": "GraphQL Blog Entry",
      "articleBody": "This Blog entry was created by calling the GraphQL service!",
      "id": 35541,
      "friendlyUrlPath": "graphql-blog-entry"
    }
  }
}
```

NOTE

You can make these requests with any web client, such as cURL:

`curl --request POST --url http://localhost:8080/o/graphql -u test@liferay.com:learn --header 'content-type: application/json' --data '{"query":"mutation CreateBlog($blog: InputBlogPosting){ createSiteBlogPosting(blogPosting: $blog, siteKey: \"20122\" ) { headline articleBody id friendlyUrlPath } } ","variables":{"blog":{"articleBody":"This Blog entry was created by using cURL to call the GraphQL service!","headline":"cURL GraphQL Blog Entry"}},"operationName":"CreateBlog"}'`

#### [Getting All Blog Entries](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#getting-all-blog-entries) <a href="#getting-all-blog-entries" id="getting-all-blog-entries"></a>

Now you can repeat the first query you did:

```graphql
query {blogPostings(filter:"",page:1,pageSize:10,search:"",siteKey:"20122",sort:"")
	{page
     items {
        id
  		articleBody
  		headline
  		creator
  			{name
            }
  }
  }
}
```

Liferay DXP returns JSON containing the blog entry you posted:

```json
{
  "data": {
    "blogPostings": {
      "page": 1,
      "items": [
        {
          "id": 35541,
          "articleBody": "This Blog entry was created by calling the GraphQL service!",
          "headline": "GraphQL Blog Entry",
          "creator": {
            "name": "Test Test"
          }
        }
      ]
    }
  }
}
```

#### [Getting a Single Blog Entry](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#getting-a-single-blog-entry) <a href="#getting-a-single-blog-entry" id="getting-a-single-blog-entry"></a>

The API call from the GraphQL schema for getting a single Blog entry has only one parameter:

```log
blogPosting(
   blogPostingId: Long
): BlogPosting
```

Since the query above revealed your Blog post’s ID, you can retrieve just the post you want:

```graphql
query {blogPosting(blogPostingId: 35541)
  { id
    headline
    articleBody}
}
```

Paste this into the top left window of the client and click the _Play_ button. It returns the same blog entry:

```json
{
  "data": {
    "blogPosting": {
      "id": 35541,
      "headline": "GraphQL Blog Entry",
      "articleBody": "This Blog entry was created by calling the GraphQL service!"
    }
  }
}
```

#### [Deleting a Blog Entry](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-graphql-apis#deleting-a-blog-entry) <a href="#deleting-a-blog-entry" id="deleting-a-blog-entry"></a>

Deleting a blog entry, like creating one, is a mutation. Its call is almost the same as getting a single blog entry:

```log
deleteBlogPosting(
  blogPostingId: Long
): Boolean
```

Using the client, you can make the call like this:

```graphql
mutation {
  deleteBlogPosting(blogPostingId: 35541)
}
```

This call returns a Boolean in a JSON document denoting success or failure:

```json
{
  "data": {
    "deleteBlogPosting": true
  }
}
```

Congratulations! You’ve now learned how to call Liferay DXP’s GraphQL services. Remember that the examples above use Basic Auth: for production, use OAuth2 to call services in a secure way.
