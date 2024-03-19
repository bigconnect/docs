# Client extensions

Client extensions extend Liferay without using OSGi modules. You can deploy client extensions to any Liferay environment, whether you host it yourself or on Liferay Cloud. They are also the primary way to customize Liferay with Liferay Cloud, together with built-in configurations like [Liferay objects](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects).

While other Liferay extensions like [OSGi modules](https://learn.liferay.com/web/guest/w/dxp/liferay-internals/fundamentals/module-projects) or [themes](https://learn.liferay.com/web/guest/w/dxp/site-building/site-appearance/themes/introduction-to-themes) modify or add functionality to Liferay itself, client extensions interface only with Liferay’s APIs. This loose coupling makes client extensions more flexible and easier to apply to environments after an upgrade, since they depend only on APIs, which rarely change between versions. You can also create client extensions using whatever programming languages or technologies you wish, because your client extensions run outside of Liferay and integrate with objects, with no extra code required to connect them.

![Client extensions provide the key customization capabilities needed to meet specific business requirements, without modifying Liferay itself.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/client-extensions/images/01.png)

With client extensions, you can

* Apply [custom JavaScript or CSS](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#front-end-client-extensions), while avoiding dependencies on Liferay’s code
* Embed applications hosted remotely into a [custom element](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions/understanding-custom-element-and-iframe-client-extensions) on a Liferay page
* Apply the same configurations or objects to multiple environments, by [converting them into client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#configuration-client-extensions)
* Call custom services or [functions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#microservice-client-extensions) running outside of Liferay (e.g., a REST endpoint), using the programming languages or technologies of your choice

### [Types of Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#types-of-client-extensions) <a href="#types-of-client-extensions" id="types-of-client-extensions"></a>

You use [Liferay Workspace](https://learn.liferay.com/web/guest/w/dxp/building-applications/tooling/liferay-workspace/what-is-liferay-workspace) to develop and deploy Client extensions. Client extensions communicate with Liferay via [headless APIs](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/using-liferay-as-a-headless-platform).

These are the classifications of client extensions:

* [Batch client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#batch-client-extensions)
* [Configuration client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#configuration-client-extensions)
* [Front-end client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#front-end-client-extensions)
* [Microservice client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#microservice-client-extensions)

#### [Batch Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#batch-client-extensions) <a href="#batch-client-extensions" id="batch-client-extensions"></a>

[Dev Feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#dev-feature-flags)

Batch client extensions provide data entities to your Liferay instance, such as [object definitions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/exporting-and-importing-object-definitions) or [workflow definitions](https://learn.liferay.com/web/guest/w/dxp/process-automation/workflow/introduction-to-workflow). Create batch client extensions by exporting data via the [batch engine framework](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/batch-engine-api-basics-exporting-data).

See [Batch Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/batch-client-extensions) for more information on this specific type.

#### [Configuration Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#configuration-client-extensions) <a href="#configuration-client-extensions" id="configuration-client-extensions"></a>

Configuration client extensions provide specific configurations to change functionality within your Liferay instance.

They can be used together with other client extensions, such as [microservice client extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#microservice-client-extensions). For example, you can use an OAuth user agent client extension to authenticate client extensions that work with user-triggered action events.

See [Configuration Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/configuration-client-extensions) for more information on the specific types.

#### [Front-end Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#front-end-client-extensions) <a href="#front-end-client-extensions" id="front-end-client-extensions"></a>

Front-end client extensions provide resources to display on specific pages. Theme resources or custom HTML elements can now be provided in client extensions. Most front-end client extensions provide _static resources_ to your Liferay instance.

See [Front-end Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/front-end-client-extensions) for more information on the specific types.

#### [Microservice Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions#microservice-client-extensions) <a href="#microservice-client-extensions" id="microservice-client-extensions"></a>

Microservice client extensions provide API endpoints to trigger within Liferay (e.g., object or workflow actions). Once those action events call your API, you can run any function you want outside of Liferay as a separate _microservice_.

See [Microservice Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/microservice-client-extensions) for more information on the specific types.

\
