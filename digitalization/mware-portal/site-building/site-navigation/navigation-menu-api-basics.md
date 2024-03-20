# Navigation Menu API Basics

MWARE Portal REST APIs provide services for the portal’s navigation menus. You can create and edit navigation menus with the API. Start by seeing an example of adding a new navigation menu.

### Adding a Navigation Menu <a href="#adding-a-navigation-menu" id="adding-a-navigation-menu"></a>

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/dxp:2023.q4.0
```

Sign in to the portal at http://localhost:8080 using the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps:

1.  Download and unzip Categories and Vocabulary API Basics.

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/site-building/site-navigation/developer-guide/liferay-p7s4.zip -O
    ```

    ```bash
    unzip liferay-p7s4.zip
    ```
2. Find your site’s ID. You’ll use this in different service calls below.
3.  Use the cURL script to add a new navigation menu to your Site. On the command line, navigate to the `curl` folder. Execute the `NavigationMenu_POST_ToSite.sh` script with your site ID as a parameter.

    ```bash
    ./NavigationMenu_POST_ToSite.sh 1234
    ```

    The JSON response shows a new navigation menu has been added:

    ```json
    {
      "creator" : {
        "additionalName" : "",
        "contentType" : "UserAccount",
        "familyName" : "Test",
        "givenName" : "Test",
        "id" : 20129,
        "name" : "Test Test"
      },
      "dateCreated" : "2021-09-09T21:41:31Z",
      "dateModified" : "2021-09-09T21:41:31Z",
      "id" : 40131,
      "name" : "Foo",
      "navigationMenuItems" : [ ],
      "siteId" : 20125
    }
    ```
4.  Go to the Navigation Menus application by navigating to _Administration Menu_ → _Site Builder_ → _Navigation Menus_. See that a new navigation menu has been added.

    ![See that a new Navigation Menu has been added.](https://resources.learn.liferay.com/images/dxp/latest/en/site-building/site-navigation/developer-guide/navigation-menu-api-basics/images/01.png)
5.  The REST service can also be called using the Java client. Navigate out of the `curl` folder and into the `java` folder. Compile the source files with the following command:

    ```bash
    javac -classpath .:* *.java
    ```
6.  Run the `NavigationMenu_POST_ToSite` class with the following command. Replace the `siteId` value with your site’s ID:

    ```bash
    java -classpath .:* -DsiteId=1234 NavigationMenu_POST_ToSite
    ```

### Examine the cURL Command <a href="#examine-the-curl-command" id="examine-the-curl-command"></a>

The `NavigationMenu_POST_ToSite.sh` script calls the REST service with a cURL command.

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/navigation-menus" \
	--data-binary '
		{
			"name": "Foo"
		}' \
	--header "Content-Type: application/json" \
	--request "POST" \
	--user "test@liferay.com:learn"
```

Here are the command’s arguments:

| Arguments                                                                      | Description                                         |
| ------------------------------------------------------------------------------ | --------------------------------------------------- |
| `-H "Content-Type: application/json"`                                          | Indicates that the request body format is JSON.     |
| `-X POST`                                                                      | The HTTP method to invoke at the specified endpoint |
| `"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/navigation-menus"` | The REST service endpoint                           |
| `-d "{\"name\": \"Foo\"}"`                                                     | The data you are requesting to post                 |
| `-u "test@liferay.com:learn"`                                                  | Basic authentication credentials                    |

NOTE

Basic authentication is used here for demonstration purposes. For production, you should authorize users via OAuth2. See Using OAuth2 to Authorize Users for a sample React application that uses OAuth2.

The other cURL commands use similar JSON arguments.

### Examine the Java Class <a href="#examine-the-java-class" id="examine-the-java-class"></a>

The `NavigationMenu_POST_ToSite.java` class adds a navigation menu by calling the navigation menu related service.

```java
public static void main(String[] args) throws Exception {
	NavigationMenuResource.Builder builder =
		NavigationMenuResource.builder();

	NavigationMenuResource navigationMenuResource = builder.authentication(
		"test@liferay.com", "learn"
	).build();

	NavigationMenu navigationMenu =
		navigationMenuResource.postSiteNavigationMenu(
			Long.valueOf(System.getProperty("siteId")),
			new NavigationMenu() {
				{
					name = "Foo";
				}
			});

	System.out.println(navigationMenu);
}
```

This class invokes the REST service using only three lines of code:

| Line (abbreviated)                                                                     | Description                                                                                   |
| -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| `NavigationMenuResource.Builder builder = ...`                                         | Gets a `Builder` for generating a `NavigationMenuResource` service instance.                  |
| `NavigationMenuResource navigationMenuResource = builder.authentication(...).build();` | Specifies basic authentication and generates a `NavigationMenuResource` service instance.     |
| `NavigationMenu navigationMenu = navigationMenuResource.postSiteNavigationMenu(...);`  | Calls the `navigationMenuResource.postSiteNavigationMenu` method and passes the data to post. |

Note that the project includes the `com.liferay.headless.delivery.client.jar` file as a dependency. You can find client JAR dependency information for all REST applications in the API explorer in your installation at `/o/api`.

NOTE

The `main` method’s comment demonstrates running the class.

The other example Java classes are similar to this one, but call different `NavigationMenuResource` methods.

IMPORTANT

See NavigationMenuResource for service details.

Below are examples of calling other `NavigationMenu` REST services using cURL and Java.

### Get Navigation Menus from Site <a href="#get-navigation-menus-from-site" id="get-navigation-menus-from-site"></a>

You can list a site’s navigation menus by executing the following cURL or Java command. As above, replace `1234` with your site’s ID.

#### NavigationMenus\_GET\_FromSite.sh <a href="#navigationmenus-get-fromsitesh" id="navigationmenus-get-fromsitesh"></a>

Command:

```bash
./NavigationMenus_GET_FromSite.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/sites/${1}/navigation-menus" \
	--user "test@liferay.com:learn"
```

#### NavigationMenus\_GET\_FromSite.java <a href="#navigationmenus-get-fromsitejava" id="navigationmenus-get-fromsitejava"></a>

Command:

```bash
java -classpath .:* -DsiteId=1234 NavigationMenus_GET_FromSite
```

Code:

```java
public static void main(String[] args) throws Exception {
	NavigationMenuResource.Builder builder =
		NavigationMenuResource.builder();

	NavigationMenuResource navigationMenuResource = builder.authentication(
		"test@liferay.com", "learn"
	).build();

	Page<NavigationMenu> page =
		navigationMenuResource.getSiteNavigationMenusPage(
			Long.valueOf(System.getProperty("siteId")),
			Pagination.of(1, 2));

	System.out.println(page);
}
```

The site’s `NavigationMenu` objects are listed in JSON.

### Get a Navigation Menu <a href="#get-a-navigation-menu" id="get-a-navigation-menu"></a>

Get a specific navigation menu with the following cURL or Java command. Replace `1234` with the navigation menu’s ID.

TIP

Use `NavigationMenus_GET_FromSite.[java|sh]` to get `NavigationMenu` IDs.

#### NavigationMenu\_GET\_ById.sh <a href="#navigationmenu-get-byidsh" id="navigationmenu-get-byidsh"></a>

Command:

```bash
./NavigationMenu_GET_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/navigation-menus/${1}" \
	--user "test@liferay.com:learn"
```

#### NavigationMenu\_GET\_ById.java <a href="#navigationmenu-get-byidjava" id="navigationmenu-get-byidjava"></a>

Command:

```bash
java -classpath .:* -DnavigationMenuId=1234 NavigationMenu_GET_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	NavigationMenuResource.Builder builder =
		NavigationMenuResource.builder();

	NavigationMenuResource navigationMenuResource = builder.authentication(
		"test@liferay.com", "learn"
	).build();

	System.out.println(
		navigationMenuResource.getNavigationMenu(
			Long.valueOf(System.getProperty("navigationMenuId"))));
}
```

The `NavigationMenu` fields are listed in JSON.

### Put a Navigation Menu <a href="#put-a-navigation-menu" id="put-a-navigation-menu"></a>

Do a complete overwrite of an existing navigation menu with the following cURL and Java commands. Note, replace `1234` with your navigation menu’s ID.

#### NavigationMenu\_PUT\_ById.sh <a href="#navigationmenu-put-byidsh" id="navigationmenu-put-byidsh"></a>

Command:

```bash
./NavigationMenu_PUT_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/navigation-menus/${1}" \
	--data-binary '
		{
			"name": "Bar"
		}' \
	--header "Content-Type: application/json" \
	--request "PUT" \
	--user "test@liferay.com:learn"
```

#### NavigationMenu\_PUT\_ById.java <a href="#navigationmenu-put-byidjava" id="navigationmenu-put-byidjava"></a>

Command:

```bash
java -classpath .:* -DnavigationMenuId=1234 NavigationMenu_PUT_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	NavigationMenuResource.Builder builder =
		NavigationMenuResource.builder();

	NavigationMenuResource navigationMenuResource = builder.authentication(
		"test@liferay.com", "learn"
	).build();

	NavigationMenu navigationMenu =
		navigationMenuResource.putNavigationMenu(
			Long.valueOf(System.getProperty("navigationMenuId")),
			new NavigationMenu() {
				{
					name = "Bar";
				}
			});

	System.out.println(navigationMenu);
}
```

### Delete a Navigation Menu <a href="#delete-a-navigation-menu" id="delete-a-navigation-menu"></a>

Delete an existing navigation menu with the following cURL and Java commands. Note, replace `1234` with your navigation menu’s ID.

#### NavigationMenu\_DELETE\_ById.sh <a href="#navigationmenu-delete-byidsh" id="navigationmenu-delete-byidsh"></a>

Command:

```bash
./NavigationMenu_DELETE_ById.sh 1234
```

Code:

```bash
curl \
	"http://localhost:8080/o/headless-delivery/v1.0/navigation-menus/${1}" \
	--request "DELETE" \
	--user "test@liferay.com:learn"
```

#### NavigationMenu\_DELETE\_ById.java <a href="#navigationmenu-delete-byidjava" id="navigationmenu-delete-byidjava"></a>

Command

```bash
java -classpath .:* -DnavigationMenuId=1234 NavigationMenu_DELETE_ById
```

Code:

```java
public static void main(String[] args) throws Exception {
	NavigationMenuResource.Builder builder =
		NavigationMenuResource.builder();

	NavigationMenuResource navigationMenuResource = builder.authentication(
		"test@liferay.com", "learn"
	).build();

	navigationMenuResource.deleteNavigationMenu(
		Long.valueOf(System.getProperty("navigationMenuId")));
}
```

The API Explorer lists all of the `NavigationMenu` services and schemas and has an interface to try out each service.
