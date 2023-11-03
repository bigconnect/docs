# JAX-WS Enhancements

This section covers the extra functionality relating to the JAX-WS implementation provided in MWARE AS.

### Additional Scanning Locations

MWARE AS scans for specific JAX-WS files in locations further to those specified in the **JAX-WS** specification. Additional directories and sources are scanned are documented here.

#### `jax-ws-catalog.xml`

The JAX-WS 2.2 specification states that the `jax-ws-catalog.xml` file should be searched for in the `META-INF` directory of a deployed `WAR` file relative to the classpath. Since the classpath of a WAR can be found in `/WEB-INF/classes/`, this means that this file is expected at `/WEB-INF/classes/META-INF/jax-ws-catalog.xml` (from the root of the `WAR`). MWARE AS also searches for this file at `/WEB-INF/jax-ws-catalog.xml`.
