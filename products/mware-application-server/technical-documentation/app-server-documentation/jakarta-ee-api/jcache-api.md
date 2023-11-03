# JCache API

This section covers how to use the JCache functionality in Payara Server.

|   | _JSR107 (JCache)_ is implemented in Payara Server by Hazelcast IMDG. |
| - | -------------------------------------------------------------------- |

### Accessing the Caching Provider and Cache Manager

To create a `Cache` instance you will need to access either a `CachingProvider` or a `CacheManager` instance at runtime. This can be done either declaratively via CDI injection or programmatically via JNDI lookup.

#### Using Injection

You can inject both `CachingProvider` and `CacheManager` instances in any CDI beans like this:

```java
import javax.cache.CacheManager;
import javax.cache.spi.CachingProvider;
import jakarta.inject.Inject;
...
@Inject
CacheManager manager;
@Inject
CachingProvider provider;
```

|   | Your WAR or JAR artifact must be an implicit or explicit bean archive. |
| - | ---------------------------------------------------------------------- |

#### Using JNDI

Both the `CachingProvider` and the `CachingManager` components can also be programmatically accessed through their JNDI names as well:

```java
import javax.cache.spi.CachingProvider;
import javax.cache.CacheManager;
...
Context ctx = new InitialContext();
CachingProvider provider = (CachingProvider) ctx.lookup("payara/CachingProvider");
CacheManager manager = (CacheManager) ctx.lookup("payara/CacheManager");
```

### Creating a Cache Instance

You can create a cache using either the `CacheManager.getCache` method, or by directly injecting the cache instance into a CDI bean.

#### Using Injection

Injecting a cache into a CDI bean is simple enough:

```java
import jakarta.inject.Inject;
import javax.cache.Cache;

@Inject
Cache cache;
```

The name of this cache will be the canonical name of the class it is created in.

|   | Caches created in this way will also have JMX statistics and management enabled. |
| - | -------------------------------------------------------------------------------- |

**Typed Cache Injection**

A typed cache can also be injected in the same manner:

```java
import jakarta.inject.Inject;
import javax.cache.Cache;
...
@Inject
Cache<Long, Property> cache;
```

|   | Both key and value types must be `Serializable` so that the cache can be properly instantiated at runtime. |
| - | ---------------------------------------------------------------------------------------------------------- |

#### Injecting a Custom Cache

You can determine the name and other attributes of a cache created through injection using the `@NamedCache` annotation.

For example, to inject a cache with a custom name and with JMX management enabled:

```java
import fish.payara.cdi.jsr107.impl.NamedCache;
import jakarta.inject.Inject;
import javax.cache.Cache;
...
@NamedCache(cacheName = "custom", managementEnabled = true)
@Inject
Cache cache;
```

|   | This annotation is part of the [Payara Platform Public API](https://docs.payara.fish/community/docs/Technical%20Documentation/Public%20API/Overview.html) |
| - | --------------------------------------------------------------------------------------------------------------------------------------------------------- |

If you only want to set the name of the cache but don’t want to depend on the `@NamedCache`, you can use the `@CacheDefaults` annotation on the bean class instead:

```java
import jakarta.inject.Inject;
import javax.cache.Cache;
import javax.cache.annotation.CacheDefaults;
import jakarta.enterprise.context.ApplicationScoped;
...
@ApplicationScoped
@CacheDefaults(cacheName = "custom")
public class CacheBean {
    ...
    @Inject
    Cache cache;
    ...
}
```

|   | Keep in mind that this solution only works if your bean has one injected cache only. If you are in a situation where you must inject more than one cache into the bean then consider using the `@NamedCache` annotation to avoid name collisions. |
| - | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### Using JCache Annotations

The Payara Platform supports cache definitions and operations on caches by using the standard set of annotations that are part of the JCache API:

`@CachePut`

Puts the specified key and value in the cache.

`@CacheRemove`

Removes an element from the cache that corresponds to the supplied key.

`@CacheDefaults`

Allows the configuration of defaults for `CacheResult`, `CachePut`, `CacheRemove`, and `CacheRemoveAll` at the class level.

`@CacheKey`

Marks a method parameter as the key of a cache.

`@CacheValue`

Marks a method parameter as the value of a cache key.
