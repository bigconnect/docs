# mod\_cgi

Any file that has the handler `cgi-script` will be treated as a CGI script, and run by the server, with its output being returned to the client. Files acquire this handler either by having a name containing an extension defined by the [`AddHandler`](https://httpd.apache.org/docs/2.4/mod/mod\_mime.html#addhandler) directive, or by being in a [`ScriptAlias`](https://httpd.apache.org/docs/2.4/mod/mod\_alias.html#scriptalias) directory.

For an introduction to using CGI scripts with Apache, see our tutorial on [Dynamic Content With CGI](https://httpd.apache.org/docs/2.4/howto/cgi.html).

When using a multi-threaded MPM under unix, the module `mod_cgid` should be used in place of this module. At the user level, the two modules are essentially identical.

For backward-compatibility, the cgi-script handler will also be activated for any file with the mime-type `application/x-httpd-cgi`. The use of the magic mime-type is deprecated.

[![Support Apache!](https://www.apache.org/images/SupportApache-small.png)](https://www.apache.org/foundation/contributing.html)

#### Topics

* [CGI Environment variables](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#env)
* [CGI Debugging](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#cgi-debug)

#### Directives

* [CGIScriptTimeout](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#cgiscripttimeout)
* [ScriptLog](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#scriptlog)
* [ScriptLogBuffer](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#scriptlogbuffer)
* [ScriptLogLength](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#scriptloglength)

#### Bugfix checklist

* [httpd changelog](https://www.apache.org/dist/httpd/CHANGES\_2.4)
* [Known issues](https://bz.apache.org/bugzilla/buglist.cgi?bug\_status=\_\_open\_\_\&list\_id=144532\&product=Apache%20httpd-2\&query\_format=specific\&order=changeddate%20DESC%2Cpriority%2Cbug\_severity\&component=mod\_cgi)
* [Report a bug](https://bz.apache.org/bugzilla/enter\_bug.cgi?product=Apache%20httpd-2\&component=mod\_cgi)

#### See also

* [`AcceptPathInfo`](https://httpd.apache.org/docs/2.4/mod/core.html#acceptpathinfo)
* [`Options`](https://httpd.apache.org/docs/2.4/mod/core.html#options) ExecCGI
* [`ScriptAlias`](https://httpd.apache.org/docs/2.4/mod/mod\_alias.html#scriptalias)
* [`AddHandler`](https://httpd.apache.org/docs/2.4/mod/mod\_mime.html#addhandler)
* [Running CGI programs under different user IDs](https://httpd.apache.org/docs/2.4/suexec.html)
* [CGI Specification](http://www.ietf.org/rfc/rfc3875)
* [Comment](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html#comments\_section)

### CGI Environment variables

The server will set the CGI environment variables as described in the [CGI specification](http://www.ietf.org/rfc/rfc3875), with the following provisions:

PATH\_INFOThis will not be available if the [`AcceptPathInfo`](https://httpd.apache.org/docs/2.4/mod/core.html#acceptpathinfo) directive is explicitly set to `off`. The default behavior, if `AcceptPathInfo` is not given, is that [`mod_cgi`](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html) will accept path info (trailing `/more/path/info` following the script filename in the URI), while the core server will return a 404 NOT FOUND error for requests with additional path info. Omitting the `AcceptPathInfo` directive has the same effect as setting it `On` for [`mod_cgi`](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html) requests.REMOTE\_HOSTThis will only be set if [`HostnameLookups`](https://httpd.apache.org/docs/2.4/mod/core.html#hostnamelookups) is set to `on` (it is off by default), and if a reverse DNS lookup of the accessing host's address indeed finds a host name.REMOTE\_IDENTThis will only be set if [`IdentityCheck`](https://httpd.apache.org/docs/2.4/mod/mod\_ident.html#identitycheck) is set to `on` and the accessing host supports the ident protocol. Note that the contents of this variable cannot be relied upon because it can easily be faked, and if there is a proxy between the client and the server, it is usually totally useless.REMOTE\_USERThis will only be set if the CGI script is subject to authentication.

This module also leverages the core functions [ap\_add\_common\_vars](https://ci.apache.org/projects/httpd/trunk/doxygen/group\_\_APACHE\_\_CORE\_\_SCRIPT.html#ga0e81f9571a8a73f5da0e89e1f46d34b1) and [ap\_add\_cgi\_vars](https://ci.apache.org/projects/httpd/trunk/doxygen/group\_\_APACHE\_\_CORE\_\_SCRIPT.html#ga6b975cd7ff27a338cb8752381a4cc14f) to add environment variables like:

DOCUMENT\_ROOTSet with the content of the related [`DocumentRoot`](https://httpd.apache.org/docs/2.4/mod/core.html#documentroot) directive.SERVER\_NAMEThe fully qualified domain name related to the request.SERVER\_ADDRThe IP address of the Virtual Host serving the request.SERVER\_ADMINSet with the content of the related [`ServerAdmin`](https://httpd.apache.org/docs/2.4/mod/core.html#serveradmin) directive.

For an exhaustive list it is suggested to write a basic CGI script that dumps all the environment variables passed by Apache in a convenient format

### CGI Debugging

Debugging CGI scripts has traditionally been difficult, mainly because it has not been possible to study the output (standard output and error) for scripts which are failing to run properly. These directives provide more detailed logging of errors when they occur.

#### CGI Logfile Format

When configured, the CGI error log logs any CGI which does not execute properly. Each CGI script which fails to operate causes several lines of information to be logged. The first two lines are always of the format:

`%% [time] request-line`\
`%% HTTP-status CGI-script-filename`

If the error is that CGI script cannot be run, the log file will contain an extra two lines:

`%%error`\
`error-message`

Alternatively, if the error is the result of the script returning incorrect header information (often due to a bug in the script), the following information is logged:

`%request`\
`All HTTP request headers received`\
`POST or PUT entity (if any)`\
`%response`\
`All headers output by the CGI script`\
`%stdout`\
`CGI standard output`\
`%stderr`\
`CGI standard error`\


(The %stdout and %stderr parts may be missing if the script did not output anything on standard output or standard error)

### CGIScriptTimeout Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description)     | The length of time to wait for more output from the CGI program                                        |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)               | `CGIScriptTimeout time[s\|ms]`                                                                         |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)             | `value of` [`Timeout`](https://httpd.apache.org/docs/2.4/mod/core.html#timeout) `directive when unset` |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)             | server config, virtual host, directory, .htaccess                                                      |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)               | Base                                                                                                   |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)               | mod\_cgi                                                                                               |
| [Compatibility:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Compatibility) | Available in version 2.4.59 and later.                                                                 |

This directive limits the length of time to wait for more output from the CGI program. If the time is exceeded, the request and CGI are terminated.

#### Example

```
CGIScriptTimeout 2
```

### ScriptLog Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Location of the CGI script error logfile                                                                                             |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `ScriptLog file-path`                                                                                                                |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host                                                                                                          |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Base                                                                                                                                 |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | [`mod_cgi`](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html), [`mod_cgid`](https://httpd.apache.org/docs/2.4/mod/mod\_cgid.html) |

The `ScriptLog` directive sets the CGI script error logfile. If no `ScriptLog` is given, no error log is created. If given, any CGI errors are logged into the filename given as argument. If this is a relative file or path it is taken relative to the [`ServerRoot`](https://httpd.apache.org/docs/2.4/mod/core.html#serverroot).

#### Example

```
ScriptLog logs/cgi_log
```

This log will be opened as the user the child processes run as, _i.e._ the user specified in the main [`User`](https://httpd.apache.org/docs/2.4/mod/mod\_unixd.html#user) directive. This means that either the directory the script log is in needs to be writable by that user or the file needs to be manually created and set to be writable by that user. If you place the script log in your main logs directory, do **NOT** change the directory permissions to make it writable by the user the child processes run as.

Note that script logging is meant to be a debugging feature when writing CGI scripts, and is not meant to be activated continuously on running servers. It is not optimized for speed or efficiency, and may have security problems if used in a manner other than that for which it was designed

### ScriptLogBuffer Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Maximum amount of PUT or POST requests that will be recorded in the scriptlog                                                        |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `ScriptLogBuffer bytes`                                                                                                              |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `ScriptLogBuffer 1024`                                                                                                               |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host                                                                                                          |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Base                                                                                                                                 |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | [`mod_cgi`](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html), [`mod_cgid`](https://httpd.apache.org/docs/2.4/mod/mod\_cgid.html) |

The size of any PUT or POST entity body that is logged to the file is limited, to prevent the log file growing too big too quickly if large bodies are being received. By default, up to 1024 bytes are logged, but this can be changed with this directive

### ScriptLogLength Directive

| [Description:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Description) | Size limit of the CGI script logfile                                                                                                 |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Syntax:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Syntax)           | `ScriptLogLength bytes`                                                                                                              |
| [Default:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Default)         | `ScriptLogLength 10385760`                                                                                                           |
| [Context:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)         | server config, virtual host                                                                                                          |
| [Status:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Status)           | Base                                                                                                                                 |
| [Module:](https://httpd.apache.org/docs/2.4/mod/directive-dict.html#Module)           | [`mod_cgi`](https://httpd.apache.org/docs/2.4/mod/mod\_cgi.html), [`mod_cgid`](https://httpd.apache.org/docs/2.4/mod/mod\_cgid.html) |

`ScriptLogLength` can be used to limit the size of the CGI script logfile. Since the logfile logs a lot of information per CGI error (all request headers, all script output) it can grow to be a big file. To prevent problems due to unbounded growth, this directive can be used to set an maximum file-size for the CGI logfile. If the file exceeds this size, no more information will be written to it.
