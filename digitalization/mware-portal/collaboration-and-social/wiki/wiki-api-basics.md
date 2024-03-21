# Wiki API Basics

Headless Delivery application provides REST services for the [Wiki](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/wiki/getting-started-with-wikis) application. With these services, you can add Wiki nodes and pages, list their information, modify their content, or remove them altogether. Here you’ll call those services using cURL commands and Java classes.

### Preparing Tutorial Resources <a href="#preparing-tutorial-resources" id="preparing-tutorial-resources"></a>

Before proceeding with the tutorial, first set up a clean Docker container and prepare the provided tutorial code for use.

#### Docker Container <a href="#liferay-docker-container" id="liferay-docker-container"></a>

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.112-ga112
```

Sign in to the portal at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Once started, retrieve the Site ID. To find your Site ID, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), and go to _Configuration_ → _Site Settings_ → _Site Configuration_.

![Find the Site ID under Site Configuration settings.](https://resources.learn.liferay.com/images/dxp/latest/en/collaboration-and-social/wiki/developer-guide/wiki-api-basics/images/01.png)

#### Tutorial Code <a href="#tutorial-code" id="tutorial-code"></a>

This tutorial provides sample code to demonstrate the Headless API. This code includes both sample cURL and Java files for use throughout the tutorial.

Run the following command to download and unzip the [sample code](https://resources.learn.liferay.com/dxp/latest/en/collaboration-and-social/wiki/developer-guide/liferay-q8u2.zip):

```bash
curl https://resources.learn.liferay.com/dxp/latest/en/collaboration-and-social/wiki/developer-guide/liferay-q8u2.zip -O
```

```bash
unzip liferay-q8u2.zip
```

While the cURL scripts come ready for use, you must manually compile the Java source files before you can run them. To do this, go to the project’s `java` folder, and run the `javac` command.

```bash
cd liferay-q8u2/java
```

```bash
javac -classpath .:* *.java
```

Note that the project includes the `com.liferay.headless.delivery.client.jar` file as a dependency. You can find client JAR dependency information for all REST applications in the API explorer in your installation at `/o/api`.

The provided code includes APIs for the `WikiNode` and `WikiPage` services. See Tutorial Code Reference for a list of all included sample APIs.

IMPORTANT

The provided code uses basic authentication for demonstration purposes. For production, you should authorize users with OAuth2.

### Calling the Sample WikiNode APIs <a href="#calling-the-sample-wikinode-apis" id="calling-the-sample-wikinode-apis"></a>

In this exercise, you can use either the cURL commands or Java classes to call the `WikiNode` APIs. The following output examples correspond to the cURL commands, which slightly differ from the output for the provided Java classes.

1.  Navigate to the `curl` or `java` folder in the `liferay-q8u2` project.

    **For cURL:**

    ```bash
    cd liferay-q8u2/curl
    ```

    **For Java:**

    ```bash
    cd liferay-q8u2/java
    ```
2.  Execute `WikiNode_POST_ToSite` using your Site ID as a parameter. This creates a new Wiki node in the specified Site.

    **For cURL:**

    ```bash
    ./WikiNode_POST_ToSite.sh {site-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DsiteId={site-id} WikiNode_POST_ToSite
    ```

    The terminal displays the complete schema for the newly created Wiki node. The provided API calls only define the `description` and `name` fields for the new node.
3.  Copy the node’s ID for use with the following GET, PUT, and DELETE methods.

    ```json
    {
      ...
      "description" : "Foo",
      ...
      "id" : 38405,
      "name" : "Able Node",
      ...
    }
    ```
4.  Execute `WikiNodes_GET_FromSite` using the Site ID for its parameter. This returns a list of all Wiki nodes in the specified Site.

    **For cURL:**

    ```bash
    ./WikiNodes_GET_FromSite.sh {site-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DsiteId={site-id} WikiNodes_GET_FromSite
    ```

    NOTE

    All DXP/Portal instances come with a default Wiki node called `Main`. This node is shown in the above output, along with any nodes you’ve created.
5.  Execute `WikiNode_PUT_ById` using the Wiki node ID for its parameter. This replaces the details of the specified Wiki node with the details provided in the API call.

    **For cURL:**

    ```bash
    ./WikiNode_PUT_ById.sh {wiki-node-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiNodeId={wiki-node-id} WikiNode_PUT_ById
    ```

    ```json
    {
      ...
      "description" : "Bar",
      ...
      "id" : 38405,
      "name" : "Baker Node",
      ...
    }
    ```
6.  Execute `WikiNode_DELETE_ById` using the same Wiki node ID for its parameter. This deletes the specified node.

    **For cURL:**

    ```bash
    ./WikiNode_DELETE_ById.sh {wiki-node-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiNodeId={wiki-node-id} WikiNode_DELETE_ById
    ```
7.  Execute `WikiNode_GET_ById` using the same Wiki node ID for its parameter. This returns the details for the specified node if it exists.

    **For cURL:**

    ```bash
    ./WikiNode_GET_ById.sh {wiki-node-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiNodeId={wiki-node-id} WikiNode_GET_ById
    ```

    Since you deleted the node in the preceding step, it returns the following message.

    ```json
    {
      "status" : "NOT_FOUND",
      "title" : "No WikiNode exists with the primary key 38405"
    }
    ```

### Calling the Sample WikiPage APIs <a href="#calling-the-sample-wikipage-apis" id="calling-the-sample-wikipage-apis"></a>

You can use either the cURL commands or Java classes to call the `WikiPage` APIs. The following output examples correspond to the cURL commands, which differ slightly from the output for the provided Java classes.

1.  Navigate to the `curl` or `java` folder in the `liferay-q8u2` project.

    **For cURL:**

    ```bash
    cd liferay-q8u2/curl
    ```

    **For Java:**

    ```bash
    cd liferay-q8u2/java
    ```
2.  Execute `WikiNode_POST_ToSite` to create a node for your Wiki page.

    **For cURL:**

    ```bash
    ./WikiNode_POST_ToSite.sh {site-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DsiteId={site-id} WikiNode_POST_ToSite
    ```

    NOTE

    Since Wiki pages are stored in Wiki nodes, at least one node must exist before you can create any Wiki pages.
3.  Copy the node’s ID from the output for use with the `WikiPage_POST_ToNode` call.

    ```json
    {
      ...
      "description" : "Foo",
      ...
      "id" : 38504,
      "name" : "Able Node",
      ...
    }
    ```
4.  Execute `WikiPage_POST_ToNode` using the above node ID for its parameter. This creates a new Wiki page for the specified node.

    **For cURL:**

    ```bash
    ./WikiPage_POST_ToNode.sh {wiki-node-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiNodeId={wiki-node-id} WikiPage_POST_ToNode
    ```
5.  Copy the page’s ID for use with the `WikiPage_POST_ToParent` call.

    ```json
    {
      ...
      "content" : "Foo",
      ...
      "description" : "Able Page",
      "encodingFormat" : "text/x-wiki",
      ...
      "headline" : "Able Page",
      "id" : 38506,
      ...
    }
    ```
6.  Execute `WikiPage_POST_ToParent` using the above Wiki page ID for its parameter. This creates a child page for the specified Wiki page.

    **For cURL:**

    ```bash
    ./WikiPage_POST_ToNode.sh {wiki-page-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DparentWikiPageId={wiki-page-id} WikiPage_POST_ToParent
    ```

    ```json
    {
       ...
       "content" : "Foo",
       ...
       "description" : "Charlie Page",
       "encodingFormat" : "text/x-wiki",
       ...
       "headline" : "Charlie Page",
       "id" : 38510,
       ...
    }
    ```
7.  Execute `WikiPages_GET_FromNode` using the Wiki node ID for its parameter. This returns a list of all Wiki pages added to the specified node, which includes both of the newly created Wiki pages.

    **For cURL:**

    ```bash
    ./WikiPages_GET_FromNode.sh {wiki-node-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiNodeId={wiki-node-id} WikiPages_GET_FromNode
    ```
8.  Execute `WikiPages_GET_FromParent` using the Wiki parent page ID for its parameter. This returns any existing child pages for the specified Wiki page.

    **For cURL:**

    ```bash
    ./WikiPages_GET_FromParent.sh {wiki-page-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DparentWikiPageId={wiki-page-id} WikiPages_GET_FromParent
    ```
9.  Execute `WikiPage_PUT_ById` using either of the above Wiki page IDs for its parameter. This replaces the original page’s content with the content defined in the API call.

    **For cURL:**

    ```bash
    ./WikiPage_PUT_ById.sh {wiki-page-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiPageId={wiki-page-id} WikiPage_PUT_ById
    ```

    ```json
    {
      "content" : "Bar",
      ...
      "encodingFormat" : "text/x-wiki",
      ...
      "headline": "Baker Page",
      "id" : 38515,
      ...
    }
    ```
10. Execute `WikiPage_DELETE_ById` using the desired Wiki page ID for its parameter. This deletes the specified Wiki page.

    **For cURL:**

    ```bash
    ./WikiPage_DELETE_ById.sh {wiki-page-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiPageId={wiki-page-id} WikiPage_DELETE_ById
    ```
11. Execute `WikiPage_GET_ById` using the deleted Wiki page’s ID for its parameter. This returns the details for the specified page if it exists.

    **For cURL:**

    ```bash
    ./WikiPage_GET_ById.sh {wiki-page-id}
    ```

    **For Java:**

    ```bash
    java -classpath .:* -DwikiPageId={wiki-page-id} WikiPage_GET_ById
    ```

    Since the page was deleted in the preceding step, you see the following message.

    ```json
    {
      "status" : "NOT_FOUND",
      "title" : "No WikiNode exists with the primary key 38515"
    }
    ```

### Tutorial Code Reference <a href="#tutorial-code-reference" id="tutorial-code-reference"></a>

The provided sample code includes cURL scripts and Java classes for the following `WikiNode` and `WikiPage` APIs.

#### [Sample WikiNode APIs](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/wiki/developer-guide/wiki-api-basics#sample-wikinode-apis) <a href="#sample-wikinode-apis" id="sample-wikinode-apis"></a>

| Service  | HTTP Method | HTTP Endpoint                     | Description                                                                                                               |
| -------- | ----------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| WikiNode | `POST`      | `/v1.0/sites/[siteId]/wiki-nodes` | Creates a new Wiki node in the specified Site using the details provided in the API call                                  |
| WikiNode | `GET`       | `/v1.0/sites/[siteId]/wiki-nodes` | Returns a complete list of all Wiki nodes in the specified Site; results can be paginated, filtered, searched, and sorted |
| WikiNode | `GET`       | `/v1.0/wiki-nodes/[wikiNodeId]`   | Returns details for the specified node entity                                                                             |
| WikiNode | `PUT`       | `/v1.0/wiki-nodes/[wikiNodeId]`   | Replaces the specified node’s details with those provided in the API call                                                 |
| WikiNode | `DELETE`    | `/v1.0/wiki-nodes/[wikiNodeId]`   | Deletes the specified node and returns a 204 if the operation succeeds                                                    |

#### Sample WikiPage APIs <a href="#sample-wikipage-apis" id="sample-wikipage-apis"></a>

| Service  | HTTP Method | HTTP Endpoint                                    | Description                                                                                                                                |
| -------- | ----------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| WikiPage | `POST`      | `/v1.0/wiki-nodes/[wikiNodeId]/wiki-pages`       | Creates a new Wiki page in the specified node using the details provided in the API call                                                   |
| WikiPage | `POST`      | `/v1.0/wiki-pages/[parentWikiPageId]/wiki-pages` | Creates a new Wiki child page for the specified Wiki page using the details provided in the API call.                                      |
| WikiPage | `GET`       | `/v1.0/wiki-nodes/[wikiNodeId]/wiki-pages`       | Returns a complete list of all Wiki pages added to the specified Wiki node; results can be paginated, filtered, searched, and sorted       |
| WikiPage | `GET`       | `/v1.0/wiki-pages/[parentWikiPageId]/wiki-pages` | Returns a complete list of all Wiki child pages added to the specified Wiki page; results can be paginated, filtered, searched, and sorted |
| WikiPage | `GET`       | `/v1.0/wiki-pages/[wikiPageId]`                  | Returns details for the specified Wiki page entity                                                                                         |
| WikiPage | `PUT`       | `/v1.0/wiki-pages/[wikiPageId]`                  | Replaces the specified page’s details with those provided in the API call                                                                  |
| WikiPage | `DELETE`    | `/v1.0/wiki-pages/[wikiPageId]`                  | Deletes the specified page and returns a 204 if the operation succeeds                                                                     |

IMPORTANT

When using POST and PUT methods for Wiki pages, you must define both the `headline` and `encodingFormat` fields. The `headline` field sets the page’s main title, while the `encodingFormat` field determines the page’s media format (e.g., HTML, BBCode, etc.).

### Examining the Sample cURL Scripts <a href="#examining-the-sample-curl-scripts" id="examining-the-sample-curl-scripts"></a>

The following are representative examples of the tutorial’s cURL commands.

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/wiki-nodes" \
	--data-raw '
		{
			"description": "Foo",
			"name": "Able Node"
		}' \
	--header "Content-Type: application/json" \
	--request "POST" \
	--user "test@liferay.com:learn"
```

#### [`WikiPage_POST_ToNode.sh`](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/wiki/developer-guide/wiki-api-basics#wikipage-post-tonodesh) <a href="#wikipage-post-tonodesh" id="wikipage-post-tonodesh"></a>

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/wiki-nodes/${1}/wiki-pages" \
	--data-raw '
		{
			"content": "Foo",
			"encodingFormat": "text/x-wiki",
			"headline": "Able Page"
		}' \
	--header "Content-Type: application/json" \
	--request "POST" \
	--user "test@liferay.com:learn"
```

#### [`WikiPages_GET_FromNode.sh`](https://learn.liferay.com/web/guest/w/dxp/collaboration-and-social/wiki/developer-guide/wiki-api-basics#wikipages-get-fromnodesh) <a href="#wikipages-get-fromnodesh" id="wikipages-get-fromnodesh"></a>

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/wiki-nodes/${1}/wiki-pages" \
	--user "test@liferay.com:learn"
```

### Examining the Sample Java Classes <a href="#examining-the-sample-java-classes" id="examining-the-sample-java-classes"></a>

The following are representative examples of the tutorial’s Java commands.

#### `WikiNode_POST_ToSite.java` <a href="#wikinode-post-tositejava" id="wikinode-post-tositejava"></a>

```java
public class WikiNode_POST_ToSite {

	/**
	 * java -classpath .:* -DsiteId=1234 WikiNode_POST_ToSite
	 */
	public static void main(String[] args) throws Exception {
		WikiNodeResource.Builder builder = WikiNodeResource.builder();

		WikiNodeResource wikiNodeResource = builder.authentication(
			"test@liferay.com", "learn"
		).build();

		WikiNode wikiNode = wikiNodeResource.postSiteWikiNode(
			Long.valueOf(System.getProperty("siteId")),
			new WikiNode() {
				{
					description = "Foo";
					name = "Dog Node";
				}
			});

		System.out.println(wikiNode);
	}

}
```

#### `WikiPage_POST_ToNode.java` <a href="#wikipage-post-tonodejava" id="wikipage-post-tonodejava"></a>

```java
public class WikiPage_POST_ToNode {

	/**
	 * java -classpath .:* -DwikiNodeId=1234 WikiPage_POST_ToNode
	 */
	public static void main(String[] args) throws Exception {
		WikiPageResource.Builder builder = WikiPageResource.builder();

		WikiPageResource wikiPageResource = builder.authentication(
			"test@liferay.com", "learn"
		).build();

		WikiPage wikiPage = wikiPageResource.postWikiNodeWikiPage(
			Long.valueOf(System.getProperty("wikiNodeId")),
			new WikiPage() {
				{
					content = "Foo";
					encodingFormat = "text/x-wiki";
					headline = "Dog Page";
				}
			});

		System.out.println(wikiPage);
	}

}
```

#### `WikiPages_GET_FromNode.java` <a href="#wikipages-get-fromnodejava" id="wikipages-get-fromnodejava"></a>

```java
public class WikiPages_GET_FromNode {

	/**
	 * java -classpath .:* -DwikiNodeId=1234 WikiPages_GET_FromNode
	 */
	public static void main(String[] args) throws Exception {
		WikiPageResource.Builder builder = WikiPageResource.builder();

		WikiPageResource wikiPageResource = builder.authentication(
			"test@liferay.com", "learn"
		).build();

		Page<WikiPage> page = wikiPageResource.getWikiNodeWikiPagesPage(
			Long.valueOf(System.getProperty("wikiNodeId")), null, null, null,
			Pagination.of(1, 2), null);

		System.out.println(page);
	}

}
```
