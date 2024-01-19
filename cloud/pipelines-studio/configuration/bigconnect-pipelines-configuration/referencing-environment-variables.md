# Referencing Environment Variables

You can reference an environment variable in the BigConnect Pipelines configuration file, `$SDC_CONF/sdc.properties`, as follows: `${env("<environment variable>")}`.

For example, you want to display text in the BigConnect Pipelines UI next to the StreamSets logo indicating whether BigConnect Pipelines is running in a development, test, or production environment. You create an environment variable named ENVIRONMENT and populate the variable with the appropriate value: development, test, or production.

You set the value of the `ui.header.title` property in `sdc.properties` as follows:

```
ui.header.title=${env("ENVIRONMENT")}
```

You can also use this format to define runtime properties both in the BigConnect Pipelines configuration file or in separate runtime properties files.
