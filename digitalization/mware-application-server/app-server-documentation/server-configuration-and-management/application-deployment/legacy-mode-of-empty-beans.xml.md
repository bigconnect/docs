# Legacy mode of empty beans.xml

Starting with CDI 4.0, bean archives with empty beans.xml have discovery mode `annotated`. It is possible to activate the legacy mode option to switch this back to `all` discovery mode.

### Enable legacy mode of empty beans.xml

The legacy mode of empty beans.xml can be enabled using the following mechanisms:

**Via asadmin CLI**

Using the asadmin CLI it is possible to enable legacy mode for a single application or for ALL deployed applications.

**Enabling for a single application**

The legacy mode can be enabled for a single application with the `deploy` command. Add the `emptyBeansXmlModeALL=true` property to the `--properties` argument’s value of the `deploy` command.

Here’s an example of enabling development mode for an application named `application.war`:

```shell
asadmin> deploy --properties emptyBeansXmlModeALL=true myapplication.war
```

Legacy mode is configured on a per-configuration basis using the `create-system-properties` on the `fish.payara.deployment.emptyBeansXmlModeALL` property like this:

```bash
asadmin create-system-properties fish.payara.deployment.emptyBeansXmlModeALL=true
```
