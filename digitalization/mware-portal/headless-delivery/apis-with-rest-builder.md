# APIs with REST Builder

REST Builder is a code generation tool that makes it easy for you to take your local APIs and make them available on the web. It uses the [OpenAPI Specification](https://www.openapis.org/) to generate REST and [GraphQL](https://graphql.org/) APIs.

REST Builder uses the configuration you define in `rest-config.yaml` and `rest-openapi.yaml` files to generate most of the code necessary for your API to work all at once. You configure class names and where to put the code, and REST Builder generates all of the necessary files. Then you add your implementation logic.

You run REST Builder using the Gradle task `buildREST` from your `impl` module in a workspace. Once configured, it generates all the scaffolding code, interfaces, and even resource classes ready for implementation.

## Producing and Implementing APIs with REST Builder

With REST Builder, you can define the API you want to build, and REST Builder provides the framework and endpoints for you.

### Deploy an Example REST API <a href="#deploy-an-example-rest-api" id="deploy-an-example-rest-api"></a>

To see REST Builder in action, you can deploy an example API that retrieves a dummy product by its ID in a catalog. Once you understand how this simple example works, you can create APIs for your own applications.

Start a new portal instance by running

```bash
docker run -it -m 8g -p 8080:8080 liferay/portal:7.4.3.112-ga112
```

Sign in to the portal at [http://localhost:8080](http://localhost:8080/). Use the email address _test@liferay.com_ and the password _test_. When prompted, change the password to _learn_.

Then, follow these steps:

1.  Download and unzip the `.zip` archive containing the [Acme Foo API](https://resources.learn.liferay.com/dxp/latest/en/headless-delivery/apis-with-rest-builder/liferay-r3b2.zip):

    ```bash
    curl https://resources.learn.liferay.com/dxp/latest/en/headless-delivery/apis-with-rest-builder/liferay-r3b2.zip -O
    ```

    ```bash
    unzip liferay-r3b2.zip
    ```
2.  Build and deploy the example:

    ```bash
    ./gradlew deploy -Ddeploy.docker.container.id=$(docker ps -lq)
    ```

    NOTE

    This command is the same as copying the deployed jars to /opt/liferay/osgi/modules on the Docker container.
3.  Confirm the deployment in the Docker container console for both the `api` and `impl` bundles:

    ```log
    STARTED com.acme.headless.r3b2.api_1.0.0
    STARTED com.acme.headless.r3b2.impl_1.0.0
    ```
4. Log into your DXP instance and navigate to the _Global Menu_ ( ![Global Menu icon](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) → _Control Panel_ → _Gogo Shell._
5.  In the Gogo Shell prompt, type the following command:

    ```log
    jaxrs:check
    ```

    The page lists all of the installed JAX-RS bundles, including the newly deployed API, `Liferay.Headless.R3B2`. The API is now deployed and ready for you to call.

    ![The newly deployed API (named Liferay.Headless.R3B2) is listed as a result from the command and is ready to use.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/apis-with-rest-builder/producing-and-implementing-apis-with-rest-builder/images/01.png)
6.  Test the API by running the following command from your terminal, substituting a number between 1 and 3 for `{fooId}`:

    ```bash
    curl -u 'test@liferay.com:learn' "http://localhost:8080/o/headless-r3b2/v1.0/foo/{fooId}"
    ```

    The query returns a corresponding product’s ID, name, and description wrapped in a JSON object:

    ```json
    {
      "description": "Universal truth must be transcendental.",
      "id": 1,
      "name": "Truth"
    }
    ```

Congratulations, you’ve successfully deployed and used a new REST API.

Now that you’ve seen an API generated with REST Builder, it’s time to understand how it works.

### Initial Setup <a href="#initial-setup" id="initial-setup"></a>

Begin by creating `impl` and `api` modules in your workspace project. Your `impl` module’s `build.gradle` file must install and apply REST Builder as a plugin:

```log
buildscript {
    dependencies {
        classpath group: "com.liferay", name: "com.liferay.gradle.plugins.rest.builder", version: "1.1.32"
    }

    repositories {
        maven {
            url "https://repository-cdn.liferay.com/nexus/content/groups/public"
        }
    }
}

apply plugin: "com.liferay.portal.tools.rest.builder"

dependencies {
	compileOnly group: "com.liferay.portal", name: "release.portal.api"
	compileOnly project(":headless-r3b2-api")
}
```

Your `build.gradle` files in both modules must also declare dependencies on the portal release.

### YAML configuration <a href="#yaml-configuration" id="yaml-configuration"></a>

Your first step is to create the REST Builder configuration files. In the `impl` module’s root folder, add two files: `rest-config.yaml` and `rest-openapi.yaml`. These files must contain all the information necessary for REST Builder to generate the scaffolding code for your API.

#### Add REST Builder Configuration <a href="#add-rest-builder-configuration" id="add-rest-builder-configuration"></a>

REST Builder configuration belongs in the `rest-config.yaml` file. It defines the following fields:

`apiDir`: your Java source code folder

`apiPackagePath`: the starting Java package path where REST Builder generates code across all modules

`baseURI`: the context URL for all APIs in this project

`className`: the Java class name for the root resource class (used by JAX-RS)

`name`: the JAX-RS name of the API

Define these fields using this structure:

```yaml
apiDir: "../headless-r3b2-api/src/main/java"
apiPackagePath: "com.acme.headless.r3b2"
application:
    baseURI: "/headless-r3b2"
    className: "HeadlessR3B2Application"
    name: "Liferay.Headless.R3B2"
author: "Jonah the son of Amittai"
clientDir: "../headless-r3b2-client/src/main/java"
testDir: "../headless-r3b2-test/src/testIntegration/java"
```

#### Add an Information Block to the OpenAPI Configuration <a href="#add-an-information-block-to-the-openapi-configuration" id="add-an-information-block-to-the-openapi-configuration"></a>

Next, open the `rest-openapi.yaml` file to begin configuring your APIs.

The first section to add is the information block:

```yaml
info:
    description:
        "API to return a Foo."
    license:
        name: "Apache 2.0"
        url: "http://www.apache.org/licenses/LICENSE-2.0.html"
    title: "Headless R3B2"
    version: v1.0
openapi: 3.0.1
```

IMPORTANT

The `version` field defined here becomes part of the URL when your API paths are exposed within your portal instance.

#### Define the Necessary Schemas <a href="#define-the-necessary-schemas" id="define-the-necessary-schemas"></a>

Next, in the `components` block, define schemas for your entities. REST Builder uses what you define here to create corresponding Java beans to represent these entities.

Define a `schema` block for each entity you want to represent:

```yaml
components:
    schemas:
        Foo:
            properties:
                description:
                    type: string
                id:
                    format: int64
                    type: integer
                name:
                    type: string
            type: object
        Goo:
            properties:
                description:
                    type: string
                fooId:
                    format: int64
                    type: integer
                id:
                    format: int64
                    type: integer
                name:
                    type: string
```

In this example, a schema called `Foo` represents the important data for the use of this API. The `Goo` entity is linked to `Foo` by use of the `fooId`. See the [OpenAPI specification](https://swagger.io/docs/specification/data-models/data-types/) for a list of supported data types for schemas.

Your schema definition determines the names of the classes REST Builder generates, including the scaffolding and templates in the resources files. Because the above schemas are called `Foo` and `Bar`, the implementation logic belongs in the `FooResourceImpl` and `GooResourceImpl` classes.

#### Define Your APIs <a href="#define-your-apis" id="define-your-apis"></a>

Finally, add the `paths` block. This must include all APIs that you plan to implement with REST Builder. Here’s a small snippet of the paths block:

```yaml

paths:
    "/foo":
        get:
        # operations for get and post go here. See the project for full source code.
        # ...

    "/foo/{fooId}":
        get:
            operationId: getFoo
            # ...

            responses:
                200:
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/Foo"
                        application/xml:
                            schema:
                                $ref: "#/components/schemas/Foo"
        # Place other operations, such as get, patch, and put here. See the project for full source code.

    "/foo/{fooId}/goos":
        get:
            operationId: getFooGoosPage
            # This is the relationship between Foos and Goos.
            # Place your get and post operations here.
            # ...

    "/goo/{gooId}":
        delete:
            operationId: deleteGoo

            # Place operations on other entities as needed.
```

TIP

You can add paths for different kinds of requests, including `get`, `post`, `put`, `patch`, and `delete`.

The path (`foo/{fooId}`) specifies that this API (`getFoo`) can be reached by appending the path string to the end of the URL (which also includes the `baseURI` and `version` values from your `rest-config.yaml` file). For instance, this example API is accessed via the full URL: `localhost:8080/o/headless-r3b2/v1.0/foo/{fooId}`.

The value you substitute for `fooId` is used as the parameter with the matching name.

Each path has a `responses` block beneath the `parameters` block (and within the `get` block) that defines at least the response for a successful call (indicated by a `200` response).

This `responses` block specifies that a successful call returns a `Product`. The string `#/components/schemas/Foo` references the schema defined earlier in the same file, allowing REST Builder to use the `Foo` schema as the return type for this API.

Lastly, add a `tags` definition for this path, beneath the `responses` block:

```yaml
tags: ["Foo"]
```

The tag specifies information that is added to the generated documentation when REST Builder annotates your scaffolding code. The tag name should reflect the name of your schema.

See the `rest-openapi.yaml` file you downloaded earlier for a complete reference.

There’s also a `Goo` object to show how you might do relationships: Goos are related to Foos in the sense that they are associated with a `fooId`.

### Run REST Builder <a href="#run-rest-builder" id="run-rest-builder"></a>

Now that you have added all of the configuration necessary for REST Builder to do most of the work, run the following command from within your `impl` module to run the `buildREST` Gradle task:

```bash
../gradlew buildREST
```

REST Builder uses your configuration and populates both your `api` and `impl` classes with scaffolding code, as well as the Java classes where you can add your implementation logic.

GraphQL endpoint code and JAX-RS application code are both generated in the `graphql` and `jaxrs` packages, respectively. Your own API implementation is added into the appropriate `*ResourceImpl` class in the `resource` package.

### Add Your Implementation Logic <a href="#add-your-implementation-logic" id="add-your-implementation-logic"></a>

The last step is to define the logic for each API you have defined. Within your `impl` module, find the Java resource class where your implementation goes, based on the schema name you defined in `rest-openapi.yaml` (in this example, `FooResourceImpl.java` and `GooResourceImpl.java`).

TIP

The location of the class for your implementation depends on the value you defined for `apiPackagePath` in your `rest-config.yaml` file. Follow that path and then navigate into `internal/resource/<version>/` within it. If you used the same path as this example, then the file is located within `src/main/java/com/acme/headless/r3b2/internal/resource/v1_0/`.

The implementation class (`[SchemaName]ResourceImpl`) is located beside the base class (`Base[SchemaName]ResourceImpl`). Open the implementation class. Since this is just an example, this implementation uses a pre-populated `HashTable`, and the `getFoo` method returns the product from the `HashTable` with the matching `fooId`. See `FooResourceImpl.java` in the project for the full implementation.

```java
	@Override
	public Foo getFoo(Integer fooId) {
		return _foos.get(fooId);
	}
```

This method overrides the base method defined in the base class (`Base[SchemaName]ResourceImpl`), which is defined using special JAX-RS annotations.

You can add any business logic to complete the request. REST Builder only creates a default constructor for the object you defined in your schema. This example business logic creates an object and adds values to it (based on how you defined its `parameters` in `rest-openapi.yaml`):

```java
   Foo foo1 = new Foo() {
      {
         description = "Universal truth must be transcendental.";
         id = 1L;
         name = "Truth";
      }
   };
```

The `Goo` logic is similar, except in this case multiple `Goo`s are returned because `Foo` objects can contain multiple `Goo`s. When returning a collection of objects, you must use a pagination-friendly object called a `Page`:

```java
	@Override
	public Page<Goo> getFooGoosPage(Long fooId) {
		List<Goo> goos = new ArrayList<>();

		for (Goo goo : _goos.values()) {
			if (Objects.equals(fooId, goo.getFooId())) {
				goos.add(goo);
			}
		}

		return Page.of(goos);
	}
```

### Add a GraphQL Namespace <a href="#add-a-graphql-namespace" id="add-a-graphql-namespace"></a>

The portal supports namespace configuration for your GraphQL applications. You can use namespaces to group queries, mutations, and types for a given application. Specifying namespaces also allows you to use duplicate schemas in different applications.

INFO

See Consuming GraphQL APIs for more information on using GraphQL for applications.

To add and use a GraphQL namespace, follow these steps:

1. Open the `rest-config.yaml` file in your application’s `impl` module.
2.  Add the `graphQLNamespace` field with the desired namespace name.

    ```yaml
    graphQLNamespace: "<namespace name>"
    ```
3.  Add the namespace a level below the desired query or mutation objects.

    ```graphql
    query{
        <namespace name> {
            ...
        }
    }
    ```
4. Execute REST Builder to publish all designated queries and mutations in the namespace.

You can also access namespaces in your GraphQL extensions with the `GraphQLContributor` interface. Implement the `getGraphQLNamespace` method to return the desired namespace name.

```java
public class MyGraphQLContributor implements GraphQLContributor {
  ...

  @Override
  public String getGraphQLNamespace() {
    return "<namespace name>";
  }

  ...
}
```

Namespaces are disabled by default in GraphQL extensions.

### Conclusion <a href="#conclusion" id="conclusion"></a>

Congratulations! You now know the basics of implementing a new API with REST Builder and have added a new API to DXP.
