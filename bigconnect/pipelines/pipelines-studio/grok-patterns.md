# Grok Patterns

### &#x20;Defining Grok Patterns <a href="#ariaid-title2" id="ariaid-title2"></a>

You can use the grok patterns in this appendix to define the structure of log data.

You can use a single pattern or compose several patterns to define a larger pattern, or create a custom pattern.

When you define grok patterns in a Data Pipelines stage, you configure the following properties:

**Grok Pattern Definition**

Use to define a complex or custom grok pattern. You can use this property to define a pattern for a single grok pattern or to define multiple patterns for use within a larger pattern.

When configuring the pattern definition, state the pattern name and then the pattern description as follows:

```
<PATTERN NAME> <grok pattern>
<PATTERN NAME2> <grok pattern>
```

The following example defines several patterns, `MYHOSTTIMESTAMP`, `MYCUSTOMPATTERN` which expands upon `MYHOSTTIMESTAMP`, and `DURATIONLOG`:

```
MYHOSTTIMESTAMP %{CISCOTIMESTAMP:timestamp} %{HOST:host}
MYCUSTOMPATTERN %{MYHOSTTIMESTAMP} %{WORD:program}%{NOTSPACE} %{NOTSPACE}
DURATIONLOG %{NUMBER:duration}%{NOTSPACE} %{GREEDYDATA:kernel_logs}
```

**Grok Pattern**

Defines the actual grok pattern used to evaluate data. You can enter a predefined grok pattern, such as `%{COMMONAPACHELOG}`. Or, to define a custom grok pattern, you can use the patterns listed in this appendix or the patterns that you defined in the Grok Pattern Description property.

For example, after defining the patterns above in the Grok Pattern Description property, you can use the patterns to configure the Grok Pattern property as follows:

```
%{MYCUSTOMPATTERN} %{DURATIONLOG}
```

The following image shows the configuration example in the UI:

![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/GrokProperties.png)

### General Grok Patterns <a href="#ariaid-title3" id="ariaid-title3"></a>

You can use the following general grok patterns to define the structure of log data:

**USER**

%{USERNAME}

**USERNAME**

\[a-zA-Z0-9.\_-]+

**BASE10NUM**

(?\<!\[0-9.+-])(?>\[+-]?(?:(?:\[0-9]+(?:\\.\[0-9]+)?)|(?:\\.\[0-9]+)))

**BASE16FLOAT**

\b(?\<!\[0-9A-Fa-f.])(?:\[+-]?(?:0x)?(?:(?:\[0-9A-Fa-f]+(?:\\.\[0-9A-Fa-f]\*)?)|(?:\\.\[0-9A-Fa-f]+)))\b

**INT**

(?:\[+-]?(?:\[0-9]+))

**NONNEGINT**

\b(?:\[0-9]+)\b

**NUMBER**

(?:%{BASE10NUM})&#x20;

BASE16NUM (?\<!\[0-9A-Fa-f])(?:\[+-]?(?:0x)?(?:\[0-9A-Fa-f]+))

**POSINT**

\b(?:\[1-9]\[0-9]\*)\b

**WORD**

\b\w+\b

**NOTSPACE**

\S+

**SPACE**

\s\*

**DATA**

.\*?

**GREEDYDATA**

.\*

**QUOTEDSTRING**

(?>(?\<!\\\\)(?>"(?>\\\\.|\[^\\\\"]+)+"|""|(?>'(?>\\\\.|\[^\\\\']+)+')|''|(?>\`(?>\\\\.|\[^\\\\\`]+)+\`)|\`\`))

**UUID**

\[A-Fa-f0-9]{8}-(?:\[A-Fa-f0-9]{4}-){3}\[A-Fa-f0-9]{12}

### Date and Time Grok Patterns <a href="#ariaid-title4" id="ariaid-title4"></a>

You can use the following date and time grok patterns to define the structure of log data:

**MONTH**

\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b

**MONTHNUM**

(?:0?\[1-9]|1\[0-2])

**MONTHNUM2**

(?:0\[1-9]|1\[0-2])

**MONTHDAY**

(?:(?:0\[1-9])|(?:\[12]\[0-9])|(?:3\[01])|\[1-9])

**DAY**

(?:Mon(?:day)?|Tue(?:sday)?|Wed(?:nesday)?|Thu(?:rsday)?|Fri(?:day)?|Sat(?:urday)?|Sun(?:day)?)

**YEAR**

(?>\d\d){1,2}

**HOUR**

(?:2\[0123]|\[01]?\[0-9])

**MINUTE**

(?:\[0-5]\[0-9])

**SECOND**

(?:(?:\[0-5]?\[0-9]|60)(?:\[:.,]\[0-9]+)?) TIME (?!<\[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?!\[0-9])

Note: 60 is a leap second in most time standards.

**DATE\_US**

%{MONTHNUM}\[/-]%{MONTHDAY}\[/-]%{YEAR}

**DATE\_EU**

%{MONTHDAY}\[./-]%{MONTHNUM}\[./-]%{YEAR}

**ISO8601\_TIMEZONE**

(?:Z|\[+-]%{HOUR}(?::?%{MINUTE}))

**ISO8601\_SECOND**

(?:%{SECOND}|60)

**TIMESTAMP\_ISO8601**

%{YEAR}-%{MONTHNUM}-%{MONTHDAY}\[T ]%{HOUR}:?%{MINUTE}(?::?%{SECOND})?%{ISO8601\_TIMEZONE}?

**DATE**

%{DATE\_US}|%{DATE\_EU}

**DATESTAMP**

%{DATE}\[- ]%{TIME}

**TZ**

(?:\[PMCE]\[SD]T|UTC)

**DATESTAMP\_RFC822**

%{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}

**DATESTAMP\_RFC2822**

%{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601\_TIMEZONE}

**DATESTAMP\_OTHER**

%{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}

**DATESTAMP\_EVENTLOG**

%{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}

### Java Grok Patterns <a href="#ariaid-title5" id="ariaid-title5"></a>

You can use the following Java-related grok patterns to define the structure of log data:

**JAVACLASS**

(?:\[a-zA-Z$\_]\[a-zA-Z$\_0-9]\*\\.)\*\[a-zA-Z$\_]\[a-zA-Z$\_0-9]\*

**JAVAFILE**

(?:\[A-Za-z0-9\_. -]+)

A space character is allowed to match special cases, such as Native Method or Unknown Source.

**JAVAMETHOD**

(?:(\<init>)|\[a-zA-Z$\_]\[a-zA-Z$\_0-9]\*)

**JAVASTACKTRACEPART**

%{SPACE}at %{JAVACLASS:class}\\.%{JAVAMETHOD:method}\\(%{JAVAFILE:file}(?::%{NUMBER:line})?\\)

The line number is optional in special cases, such as Native Method or Unknown Source.

### Log Grok Patterns <a href="#ariaid-title6" id="ariaid-title6"></a>

You can use the following log-related grok patterns to define the structure of log data:

**SYSLOGTIMESTAMP**

%{MONTH} +%{MONTHDAY} %{TIME} PROG (?:\[\w.\_/%-]+)

**SYSLOGPROG**

%{PROG:program}(?:\\\[%{POSINT:pid}\\])?

**SYSLOGHOST**

%{IPORHOST}

**SYSLOGFACILITY**

<%{NONNEGINT:facility}.%{NONNEGINT:priority}>

**SYSLOGBASE**

%{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource} %{SYSLOGPROG}:

**HTTPDATE**

%{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}

**QS**

%{QUOTEDSTRING}

**COMMONAPACHELOG**

%{IPORHOST:clientip} %{USER:ident} %{USER:auth} \\\[%{HTTPDATE:timestamp}\\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-)

**COMBINEDAPACHELOG**

%{COMMONAPACHELOG} %{QS:referrer} %{QS:agent}

**LOGLEVEL**

(\[Aa]lert|ALERT|\[Tt]race|TRACE|\[Dd]ebug|DEBUG|\[Nn]otice|NOTICE|\[Ii]nfo|INFO|\[Ww]arn?(?:ing)?|WARN?(?:ING)?|\[Ee]rr?(?:or)?|ERR?(?:OR)?|\[Cc]rit?(?:ical)?|CRIT?(?:ICAL)?|\[Ff]atal|FATAL|\[Ss]evere|SEVERE|EMERG(?:ENCY)?|\[Ee]merg(?:ency)?)

### Networking Grok Patterns <a href="#ariaid-title7" id="ariaid-title7"></a>

You can use the following networking-related grok patterns to define the structure of log data:

**MAC**

(?:%{CISCOMAC}|%{WINDOWSMAC}|%{COMMONMAC})

**CISCOMAC**

(?:(?:\[A-Fa-f0-9]{4}\\.){2}\[A-Fa-f0-9]{4})

**COMMONMAC**

(?:(?:\[A-Fa-f0-9]{2}:){5}\[A-Fa-f0-9]{2})

**WINDOWSMAC**

(?:(?:\[A-Fa-f0-9]{2}-){5}\[A-Fa-f0-9]{2})

**HOST**

%{HOSTNAME}

**HOSTNAME**

\b(?:\[0-9A-Za-z]\[0-9A-Za-z-]{0,62})(?:\\.(?:\[0-9A-Za-z]\[0-9A-Za-z-]{0,62}))\*(\\.?|\b)

**HOSTPORT**

%{IPORHOST}:%{POSINT}

**IPORHOST**

(?:%{HOSTNAME}|%{IP})

**IP**

(?:%{IPV6}|%{IPV4})

**IPV6**

(((\[0-9A-Fa-f]{1,4}:){7}(\[0-9A-Fa-f]{1,4}|:))|((\[0-9A-Fa-f]{1,4}:){6}(:\[0-9A-Fa-f]{1,4}|((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3})|:))|((\[0-9A-Fa-f]{1,4}:){5}(((:\[0-9A-Fa-f]{1,4}){1,2})|:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3})|:))|((\[0-9A-Fa-f]{1,4}:){4}(((:\[0-9A-Fa-f]{1,4}){1,3})|((:\[0-9A-Fa-f]{1,4})?:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3}))|:))|((\[0-9A-Fa-f]{1,4}:){3}(((:\[0-9A-Fa-f]{1,4}){1,4})|((:\[0-9A-Fa-f]{1,4}){0,2}:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3}))|:))|((\[0-9A-Fa-f]{1,4}:){2}(((:\[0-9A-Fa-f]{1,4}){1,5})|((:\[0-9A-Fa-f]{1,4}){0,3}:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3}))|:))|((\[0-9A-Fa-f]{1,4}:){1}(((:\[0-9A-Fa-f]{1,4}){1,6})|((:\[0-9A-Fa-f]{1,4}){0,4}:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3}))|:))|(:(((:\[0-9A-Fa-f]{1,4}){1,7})|((:\[0-9A-Fa-f]{1,4}){0,5}:((25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)(\\.(25\[0-5]|2\[0-4]\d|1\d\d|\[1-9]?\d)){3}))|:)))(%.+)? IPV4 (?\<!\[0-9])(?:(?:25\[0-5]|2\[0-4]\[0-9]|\[0-1]?\[0-9]{1,2})\[.]\(?:25\[0-5]|2\[0-4]\[0-9]|\[0-1]?\[0-9]{1,2})\[.]\(?:25\[0-5]|2\[0-4]\[0-9]|\[0-1]?\[0-9]{1,2})\[.]\(?:25\[0-5]|2\[0-4]\[0-9]|\[0-1]?\[0-9]{1,2}))(?!\[0-9])

### Path Grok Patterns <a href="#ariaid-title8" id="ariaid-title8"></a>

You can use the following path grok patterns to define the structure of log data:

**PATH**

(?:%{UNIXPATH}|%{WINPATH})

**UNIXPATH**

(?>/(?>\[\w\_%!$@:.,\~-]+|\\\\.)\*)+ TTY (?:/dev/(pts|tty(\[pq])?)(\w+)?/?(?:\[0-9]+))

**WINPATH**

(?>\[A-Za-z]+:|\\\\)(?:\\\\\[^\\\\?\*]\*)+ URIPROTO \[A-Za-z]+(\\+\[A-Za-z+]+)?

**URIHOST**

%{IPORHOST}(?::%{POSINT:port})?

**URIPATH**

(?:/\[A-Za-z0-9$.+!\*'(){},\~:;=@#%\_\\-]\*)+ #URIPARAM \\?(?:\[A-Za-z0-9]+(?:=(?:\[^&]\*))?(?:&(?:\[A-Za-z0-9]+(?:=(?:\[^&]\*))?)?)\*)?

**URIPARAM**

\\?\[A-Za-z0-9$.+!\*'|(){},\~@#%&/=:;\_?\\-\\\[\\]]\*

**URIPATHPARAM**

%{URIPATH}(?:%{URIPARAM})?

**URI**

%{URIPROTO}://(?:%{USER}(?::\[^@]\*)?@)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?
