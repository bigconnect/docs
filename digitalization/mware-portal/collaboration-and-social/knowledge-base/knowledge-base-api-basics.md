# Knowledge Base API Basics

You can [Create Knowledge Base Articles](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/creating-knowledge-base-articles) and [Manage the Knowledge Base](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/managing-the-knowledge-base) with Liferay’s Knowledge Base app, but you can also use Liferay’s REST APIs. Call these services to create and manage content for your Knowledge Base.

### [Adding a Knowledge Base Article](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#adding-a-knowledge-base-article) <a href="#adding-a-knowledge-base-article" id="adding-a-knowledge-base-article"></a>

Start a new Liferay DXP instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/dxp:2024.q1.1
```

Sign in to Liferay at [http://localhost:8080](http://localhost:8080/) using the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then follow these steps:

1.  Download and unzip [Knowledge Base API Basics](https://resources.learn.liferay.com/dxp/latest/en/collaboration-and-social/knowledge-base/developer-guide/liferay-t3x7.zip).

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/collaboration-and-social/knowledge-base/developer-guide/liferay-t3x7.zip -O
    ```

    ```bash
    unzip liferay-t3x7.zip
    ```
2. When signed in, [retrieve the site’s ID](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-rest-services#identify-the-site-containing-the-data). You’ll use this ID in several service calls.
3.  Use the cURL script to add a new Knowledge Base article to your site. On the command line, navigate to the `curl` folder. Execute the `KnowledgeBaseArticle_POST_ToSite.sh` script with your site ID as a parameter. For example,

    ```bash
    ./KnowledgeBaseArticle_POST_ToSite.sh 1234
    ```

    The JSON response shows a new Knowledge Base article has been added:

    ```json
    {
      "articleBody" : "Foo",
      "creator" : {
        "additionalName" : "",
        "contentType" : "UserAccount",
        "familyName" : "Test",
        "givenName" : "Test",
        "id" : 20125,
        "name" : "Test Test"
      },
      "customFields" : [ ],
      "dateCreated" : "2022-07-28T21:25:57Z",
      "dateModified" : "2022-07-28T21:25:57Z",
      "description" : "",
      "encodingFormat" : "text/html",
      "externalReferenceCode" : "0bace9ad-39ea-79b5-902e-c873806b8bd7",
      "friendlyUrlPath" : "able",
      "id" : 42447,
      "keywords" : [ ],
      "numberOfAttachments" : 0,
      "numberOfKnowledgeBaseArticles" : 0,
      "parentKnowledgeBaseArticleId" : 0,
      "relatedContents" : [ ],
      "siteId" : 20121,
      "subscribed" : false,
      "taxonomyCategoryBriefs" : [ ],
      "title" : "Able"
    }
    ```
4.  Click the _Menu_ icon (![Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)) and navigate to _Content and Data_ → _Knowledge Base_. See that a new Knowledge Base article has been added.

    ![See that a new Knowledge Base article has been added.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics/images/01.png)
5.  The REST service can also be called using the Java client. Navigate out of the `curl` folder and into the `java` folder. Compile the source files:

    ```bash
    javac -classpath .:* *.java
    ```
6.  Run the `KnowledgeBaseArticle_POST_ToSite.java` class. Replace the `siteId` system property value with your site’s ID.

    ```bash
    java -classpath .:* -DsiteId=1234 KnowledgeBaseArticle_POST_ToSite
    ```

    The Java class created a new Knowledge Base article.

### [Examine the cURL Command](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#examine-the-curl-command) <a href="#examine-the-curl-command" id="examine-the-curl-command"></a>

The `KnowledgeBaseArticle_POST_ToSite.sh` script calls the REST service with a cURL command.

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/knowledge-base-articles" \
	--data-raw '
		{
			"articleBody": "Foo",
			"title": "Able"
		}' \
	--header "Content-Type: application/json" \
	--request "POST" \
	--user "test@liferay.com:learn"
```

Here are the command’s arguments:

| Arguments                                                                             | Description                                         |
| ------------------------------------------------------------------------------------- | --------------------------------------------------- |
| `-H "Content-Type: application/json"`                                                 | Indicates that the request body format is JSON.     |
| `-X POST`                                                                             | The HTTP method to invoke at the specified endpoint |
| `"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/knowledge-base-articles"` | The REST service endpoint                           |
| `-d "{\"articleBody\": \"Foo\", \"title\": \"Able\"}"`                                | The data you are requesting to post                 |
| `-u "test@liferay.com:learn"`                                                         | Basic authentication credentials                    |

NOTE

Basic authentication is used for demonstration purposes. For production, you should authorize users via [OAuth2](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2). See [Using OAuth2 to Authorize Users](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-oauth2/using-oauth2-to-authorize-users) for a sample React application that uses OAuth2.

The other cURL commands use similar JSON arguments.

### [Examine the Java Class](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#examine-the-java-class) <a href="#examine-the-java-class" id="examine-the-java-class"></a>

The `KnowledgeBaseArticle_POST_ToSite.java` class adds a Knowledge Base article by calling the knowledge-base-related service.

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	KnowledgeBaseArticle knowledgeBaseArticle =
		knowledgeBaseArticleResource.postSiteKnowledgeBaseArticle(
			Long.valueOf(System.getProperty("siteId")),
			new KnowledgeBaseArticle() {
				{
					articleBody = "Foo";
					title = "Charlie";
				}
			});

	System.out.println(knowledgeBaseArticle);
}
```

This class invokes the REST service using only three lines of code:

| Line (abbreviated)                                                                                            | Description                                                                                               |
| ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `KnowledgeBaseArticleResource.Builder builder = ...`                                                          | Gets a `Builder` for generating an `KnowledgeBaseArticleResource` service instance.                       |
| `KnowledgeBaseArticleGroupResource knowledgeBaseArticleGroupResource = builder.authentication(...).build();`  | Specifies basic authentication and generates a `KnowledgeBaseArticleResource` service instance.           |
| `KnowledgeBaseArticle knowledgeBaseArticle = knowledgeBaseArticleResource.postSiteKnowledgeBaseArticle(...);` | Calls the `knowledgeBaseArticleResource.postSiteKnowledgeBaseArticle` method and passes the data to post. |

Note that the project includes the `com.liferay.headless.delivery.client.jar` file as a dependency. You can find client JAR dependency information for all REST applications in the API explorer in your installation at `/o/api`.

NOTE

The `main` method’s comment demonstrates running the class.

The other example Java classes are similar to this one, but call different `KnowledgeBaseArticle` methods.

IMPORTANT

See [KnowledgeBaseArticleResource](https://github.com/liferay/liferay-portal/blob/7.4.3.112-ga112/modules/apps/headless/headless-delivery/headless-delivery-client/src/main/java/com/liferay/headless/delivery/client/resource/v1\_0/KnowledgeBaseArticleResource.java) for service details.

Below are examples of calling other `KnowledgeBaseArticle` REST services using cURL and Java.

### [Get Knowledge Base Articles from Site](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#get-knowledge-base-articles-from-site) <a href="#get-knowledge-base-articles-from-site" id="get-knowledge-base-articles-from-site"></a>

You can retrieve Knowledge Base articles from any site by executing the following cURL or Java command. Replace `1234` with your site’s ID.

#### [KnowledgeBaseArticles\_GET\_FromSite.sh](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticles-get-fromsitesh) <a href="#knowledgebasearticles-get-fromsitesh" id="knowledgebasearticles-get-fromsitesh"></a>

Command:

```bash
./KnowledgeBaseArticles_GET_FromSite.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/knowledge-base-articles" \
	--user "test@liferay.com:learn"
```

#### [KnowledgeBaseArticles\_GET\_FromSite.java](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticles-get-fromsitejava) <a href="#knowledgebasearticles-get-fromsitejava" id="knowledgebasearticles-get-fromsitejava"></a>

Command:

```bash
java -classpath .:* -DsiteId=1234 KnowledgeBaseArticles_GET_FromSite
```

Code:

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	Page<KnowledgeBaseArticle> page =
		knowledgeBaseArticleResource.getSiteKnowledgeBaseArticlesPage(
			Long.valueOf(System.getProperty("siteId")), null, null, null,
			null, Pagination.of(1, 2), null);

	System.out.println(page);
}
```

The Instance’s `KnowledgeBaseArticles` objects appear in JSON.

### [Get a Knowledge Base Article](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#get-a-knowledge-base-article) <a href="#get-a-knowledge-base-article" id="get-a-knowledge-base-article"></a>

Get a specific Knowledge Base article by its ID with the following cURL or Java command.

TIP

Use `KnowledgeBaseArticles_GET_FromSite.[java|sh]` to get instance `Knowledge Base Article` IDs.

#### [KnowledgeBaseArticle\_GET\_ById.sh](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-get-byidsh) <a href="#knowledgebasearticle-get-byidsh" id="knowledgebasearticle-get-byidsh"></a>

Command:

```bash
./KnowledgeBaseArticle_GET_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/knowledge-base-articles/${1}" \
	--user "test@liferay.com:learn"
```

#### [KnowledgeBaseArticle\_GET\_ById.java](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-get-byidjava) <a href="#knowledgebasearticle-get-byidjava" id="knowledgebasearticle-get-byidjava"></a>

Command:

```bash
java -classpath .:* -DknowledgeBaseArticleId=1234 KnowledgeBaseArticle_GET_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	System.out.println(
		knowledgeBaseArticleResource.getKnowledgeBaseArticle(
			Long.valueOf(System.getProperty("knowledgeBaseArticleId"))));
}
```

The `KnowledgeBaseArticle` fields appear in JSON.

### [Patch a Knowledge Base Article](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#patch-a-knowledge-base-article) <a href="#patch-a-knowledge-base-article" id="patch-a-knowledge-base-article"></a>

Do a partial edit of an existing Knowledge Base article with the following cURL and Java commands. Replace `1234` with your Knowledge Base article’s ID. Specify the field you want to modify and its new value.

#### [KnowledgeBaseArticle\_PATCH\_ById.sh](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-patch-byidsh) <a href="#knowledgebasearticle-patch-byidsh" id="knowledgebasearticle-patch-byidsh"></a>

Command:

```bash
./KnowledgeBaseArticle_PATCH_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/knowledge-base-articles/${1}" \
	--data-raw '
		{
			"articleBody": "Bar"
		}' \
	--header "Content-Type: application/json" \
	--request "PATCH" \
	--user "test@liferay.com:learn"
```

#### [KnowledgeBaseArticle\_PATCH\_ById.java](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-patch-byidjava) <a href="#knowledgebasearticle-patch-byidjava" id="knowledgebasearticle-patch-byidjava"></a>

Command:

```bash
java -classpath .:* -DknowledgeBaseArticleId=1234 KnowledgeBaseArticle_PATCH_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	KnowledgeBaseArticle knowledgeBaseArticle =
		knowledgeBaseArticleResource.patchKnowledgeBaseArticle(
			Long.valueOf(System.getProperty("knowledgeBaseArticleId")),
			new KnowledgeBaseArticle() {
				{
					articleBody = "Bar";
				}
			});

	System.out.println(knowledgeBaseArticle);
}
```

### [Put a Knowledge Base Article](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#put-a-knowledge-base-article) <a href="#put-a-knowledge-base-article" id="put-a-knowledge-base-article"></a>

Completely overwrite an existing Knowledge Base article with the following cURL and Java commands. Replace `1234` with your Knowledge Base article’s ID.

#### [KnowledgeBaseArticle\_PUT\_ById.sh](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-put-byidsh) <a href="#knowledgebasearticle-put-byidsh" id="knowledgebasearticle-put-byidsh"></a>

Command:

```bash
./KnowledgeBaseArticle_PUT_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/knowledge-base-articles/${1}" \
	--data-raw '
		{
			"articleBody": "Goo",
			"title": "Baker"
		}' \
	--header "Content-Type: application/json" \
	--request "PUT" \
	--user "test@liferay.com:learn"
```

#### [KnowledgeBaseArticle\_PUT\_ById.java](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-put-byidjava) <a href="#knowledgebasearticle-put-byidjava" id="knowledgebasearticle-put-byidjava"></a>

Command:

```bash
java -classpath .:* -DknowledgeBaseArticleId=1234 KnowledgeBaseArticle_PUT_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	KnowledgeBaseArticle knowledgeBaseArticle =
		knowledgeBaseArticleResource.putKnowledgeBaseArticle(
			Long.valueOf(System.getProperty("knowledgeBaseArticleId")),
			new KnowledgeBaseArticle() {
				{
					articleBody = "Goo";
					title = "Dog";
				}
			});

	System.out.println(knowledgeBaseArticle);
}
```

### [Delete a Knowledge Base Article](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#delete-a-knowledge-base-article) <a href="#delete-a-knowledge-base-article" id="delete-a-knowledge-base-article"></a>

Delete an existing Knowledge Base article with the following cURL and Java commands. Replace `1234` with your Knowledge Base article’s ID.

#### [KnowledgeBaseArticle\_DELETE\_ById.sh](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-delete-byidsh) <a href="#knowledgebasearticle-delete-byidsh" id="knowledgebasearticle-delete-byidsh"></a>

Command:

```bash
./KnowledgeBaseArticle_DELETE_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/knowledge-base-articles/${1}" \
	--request "DELETE" \
	--user "test@liferay.com:learn"
```

#### [KnowledgeBaseArticle\_DELETE\_ById.java](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledgebasearticle-delete-byidjava) <a href="#knowledgebasearticle-delete-byidjava" id="knowledgebasearticle-delete-byidjava"></a>

Command

```bash
java -classpath .:* -DknowledgeBaseArticleId=1234 KnowledgeBaseArticle_DELETE_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	KnowledgeBaseArticleResource.Builder builder =
		KnowledgeBaseArticleResource.builder();

	KnowledgeBaseArticleResource knowledgeBaseArticleResource =
		builder.authentication(
			"test@liferay.com", "learn"
		).build();

	knowledgeBaseArticleResource.deleteKnowledgeBaseArticle(
		Long.valueOf(System.getProperty("knowledgeBaseArticleId")));
}
```

### [Knowledge Base Folder services](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/knowledge-base/developer-guide/knowledge-base-api-basics#knowledge-base-folder-services) <a href="#knowledge-base-folder-services" id="knowledge-base-folder-services"></a>

The cURL commands and Java classes for Knowledge Base folders works in the same way as Knowledge Base articles.

| Files                                           | Description                                       |
| ----------------------------------------------- | ------------------------------------------------- |
| `KnowledgeBaseFolder_DELETE_ById.[java\\|sh]`   | Deletes a Knowledge Base folder by ID.            |
| `KnowledgeBaseFolder_GET_ById.[java\\|sh]`      | Get a specific Knowledge Base folder by ID.       |
| `KnowledgeBaseFolder_PATCH_ById.[java\\|sh]`    | Patch a Knowledge Base folder by ID.              |
| `KnowledgeBaseFolder_POST_ToSite.[java\\|sh]`   | Post a Knowledge Base folder to a site.           |
| `KnowledgeBaseFolder_PUT_ToSite.[java\\|sh]`    | Put a Knowledge Base folder by ID.                |
| `KnowledgeBaseFolders_GET_FromSite.[java\\|sh]` | Get a list of Knowledge Base folders from a site. |

The [API Explorer](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/consuming-rest-services) shows all of the `KnowledgeBaseArticle` and `KnowledgeBaseFolder` services and schemas and has an interface to try out each service.

\
