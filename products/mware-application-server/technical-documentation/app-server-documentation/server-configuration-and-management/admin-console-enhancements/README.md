# Admin Console Enhancements

MWARE App Server has added many new features, many of which are accessible and configurable through the Admin Console.

Some of the enhancements in Payara Server relate to the Admin Console itself and these are listed in the following pages:

[Asadmin Recorder](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/Admin%20Console%20Enhancements/Asadmin%20Recorder.html)

Using the Admin Console to configure resources in Payara Server makes use of asadmin commands under the covers. Because of this fact, we are able to record all the asadmin commands that the Admin Console uses and output them to a file for later re-use.

[Environment Warning](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/Admin%20Console%20Enhancements/Environment%20Warning.html)

When working with Payara Server, it is very common to have multiple environments for various purposes like test or QA. It can often be all too easy to accidentally make changes to the wrong environment, so the Payara Server Web Administration console now allows you to add a configurable warning bar to give you an extra visual reminder of which environment you are working with.

[Auditing Service](https://docs.payara.fish/community/docs/Technical%20Documentation/Payara%20Server%20Documentation/Server%20Configuration%20And%20Management/Admin%20Console%20Enhancements/Auditing%20Service.html)

With the auditing enabled, you can collect information around the interactions with the Web Administration console and the _asadmin_ command-line tool usage. This can be used to keep track of changes and who made those changes to the Payara configuration.
