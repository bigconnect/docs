# Client extensions

Client extensions extend the portal without using OSGi modules. You can deploy client extensions to any portal environment, whether you host it yourself or on Cloud. They are also the primary way to customize the portal with Cloud, together with built-in configurations like objects.

While other portal extensions like OSGi modules or themes modify or add functionality to the portal itself, client extensions interface only with the portal’s APIs. This loose coupling makes client extensions more flexible and easier to apply to environments after an upgrade, since they depend only on APIs, which rarely change between versions. You can also create client extensions using whatever programming languages or technologies you wish, because your client extensions run outside of the portal and integrate with objects, with no extra code required to connect them.

![Client extensions provide the key customization capabilities needed to meet specific business requirements, without modifying the portal itself.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/client-extensions/images/01.png)

With client extensions, you can

* Apply custom JavaScript or CSS, while avoiding dependencies on the portal’s code
* Embed applications hosted remotely into a custom element on a page
* Apply the same configurations or objects to multiple environments, by converting them into client extensions
* Call custom services or functions running outside of the portal (e.g., a REST endpoint), using the programming languages or technologies of your choice

### Types of Client Extensions <a href="#types-of-client-extensions" id="types-of-client-extensions"></a>

You use Workspace to develop and deploy Client extensions. Client extensions communicate with the portal via headless APIs.

These are the classifications of client extensions:

* Batch client extensions
* Configuration client extensions
* Front-end client extensions
* Microservice client extensions

#### Batch Client Extensions <a href="#batch-client-extensions" id="batch-client-extensions"></a>

Dev Feature

Batch client extensions provide data entities to your portal instance, such as object definitions or workflow definitions. Create batch client extensions by exporting data via the batch engine framework.

See Batch Client Extensions for more information on this specific type.

#### Configuration Client Extensions <a href="#configuration-client-extensions" id="configuration-client-extensions"></a>

Configuration client extensions provide specific configurations to change functionality within your portal instance.

They can be used together with other client extensions, such as microservice client extensions. For example, you can use an OAuth user agent client extension to authenticate client extensions that work with user-triggered action events.

See Configuration Client Extensions for more information on the specific types.

#### Front-end Client Extensions <a href="#front-end-client-extensions" id="front-end-client-extensions"></a>

Front-end client extensions provide resources to display on specific pages. Theme resources or custom HTML elements can now be provided in client extensions. Most front-end client extensions provide _static resources_ to your portal instance.

See Front-end Client Extensions for more information on the specific types.

#### Microservice Client Extensions <a href="#microservice-client-extensions" id="microservice-client-extensions"></a>

Microservice client extensions provide API endpoints to trigger within the portal (e.g., object or workflow actions). Once those action events call your API, you can run any function you want outside of the portal as a separate _microservice_.

See Microservice Client Extensions for more information on the specific types.

\
