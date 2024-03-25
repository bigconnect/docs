# Admin Console Enhancements

MWARE App Server has added many new features, many of which are accessible and configurable through the Admin Console.

Some of the enhancements in MWARE AS relate to the Admin Console itself and these are listed in the following pages:

[**Asadmin Recorder**](asadmin-recorder.md)

Using the Admin Console to configure resources in MWARE AS makes use of asadmin commands under the covers. Because of this fact, we are able to record all the asadmin commands that the Admin Console uses and output them to a file for later re-use.

[**Environment Warning**](environment-warning.md)

When working with MWARE AS, it is very common to have multiple environments for various purposes like test or QA. It can often be all too easy to accidentally make changes to the wrong environment, so the MWARE AS Web Administration console now allows you to add a configurable warning bar to give you an extra visual reminder of which environment you are working with.

[**Auditing Service**](auditing-service.md)

With the auditing enabled, you can collect information around the interactions with the Web Administration console and the _asadmin_ command-line tool usage. This can be used to keep track of changes and who made those changes to the MWARE AS configuration.

**Versioning**

The Administration Console provides a way to lock the configuration so you can make changes to the configuration while preventing other accounts from making changes during your edit session. When locking is enabled, you start the edit process by obtaining a lock. When you finish making changes, you save the changes. The changes do not take effect, however, until you activate them, distributing them to all server instances.

Changes can also be saved in Draft mode, without actually applying them. Every change of the configuration is recorded by the Auditing Service and you can choose to save your configuration anytime in Draft mode or to actually Publish the changes. Each save will store the configuration with a new version, so you can roll-back any time to a specific version.
