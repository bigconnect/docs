# Install methods

When getting started with Mware Application Development Framework, you have two options:

1. Use Cloud - we host your apps. This is the easier, faster option. No docs are required to get started.
2. Self-host - you host your apps. You can find further instructions below.

### Host with Cloud

The onboarding process is simple, minimal, and takes seconds.&#x20;

### Self-host&#x20;

You can self-host Mware Application Development Framework in under 5 minutes. We understand a lot of users need to host their apps on their own systems. This is why we've put a lot of effort into simplifying the self-hosting process. You only have to set up your instance **once**. Once your instance is up and running, other members of your team can build and interact with Mware Application Development Framework simply by accessing the URL that you have hosted your instance on.

> ### 🚧Self-hosting is not for everyone
>
> When diving into the world of self-hosting, be sure to know what you're doing. Self-hosting means you have to understand how servers work, implement security, maintain the servers, and figure out what's wrong when something goes wrong. If you do not understand this or don't have the time to dive into understanding all of this, we highly recommend the cloud-hosted solution.

To read more about self-hosting, check the Docker, Kubernetes and DigitalOcean pages.

### Unsupported Install Methods

While Mware Application Development Framework offers a fair bit of flexibility with deployment options, there are some methods that are currently unsupported.

#### AWS App Runner

AWS App Runner is a fully managed container application service offered by Amazon Web Services (AWS).

At the time of writing, we **do not** recommend using AWS App Runner to host Mware Application Development Framework. It requires storage for the backend database and object store. Without persistent storage your data will be lost after each restart.

When Amazon introduce Elastic File System (EFS) support for AWS App Runner we will test and update this documentation.
