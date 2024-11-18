# Redis

Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and streaming engine.

More information can be found in the [official Redis docs](https://redis.io/docs/about/).

### Connect

Add a data source and select Redis.

![](https://files.readme.io/a9c9734-Screenshot\_2022-09-02\_at\_15.55.33.png)

![](https://files.readme.io/ad48705-Screenshot\_2022-09-02\_at\_17.05.09.png)

You will then be prompted to provide the URL, port, username and password. The default database index is 0.

<figure><img src="https://files.readme.io/346b986-small-Screenshot_2023-05-10_at_09.45.17.png" alt=""><figcaption></figcaption></figure>

> ### 📘Connecting to a local docker instance
>
> If you are hosting Redis locally with docker, then use **host.docker.internal**

### Redis Command

Select `Redis Command` from the _Function_ dropdown. You will now be able to enter any valid Redis command into the query box below.

<figure><img src="https://files.readme.io/a761999-Screenshot_2022-09-02_at_17.01.49.png" alt=""><figcaption></figcaption></figure>

This is the recommended function as it is the most flexible. The basic commands can be found below.

For more information on the range of Redis data types and commands, click [here](https://redis.io/docs/data-types/).

#### Create

This is the equivalent of Redis' SET command. Enter a **key / string value** pair.

**Ttl** stands for _Time-to-live_ and is the number of seconds that the data structure will exist. Leave this blank if you want your key/value to never expire.

<figure><img src="https://files.readme.io/9cf3e6d-Screenshot_2022-09-02_at_16.02.14.png" alt=""><figcaption></figcaption></figure>

Only **strings** are supported by this function.

#### Read

This is the equivalent of Redis' GET command. Simply enter a **key** you want to get. The response will return the mapped value.

<figure><img src="https://files.readme.io/546ebe2-Screenshot_2022-09-02_at_16.07.40.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/f335d90-Screenshot_2022-09-02_at_16.07.57.png" alt=""><figcaption></figcaption></figure>

#### Delete

This is the equivalent of Redis' DEL command, except you can only delete a single **key** at a time.

<figure><img src="https://files.readme.io/7a41cbc-Screenshot_2022-09-02_at_16.15.12.png" alt=""><figcaption></figcaption></figure>
