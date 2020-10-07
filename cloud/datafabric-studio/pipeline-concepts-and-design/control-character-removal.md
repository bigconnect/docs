# Control Character Removal

You can use several stages to remove control characters - such as escape or end-of-transmission characters - from data. Remove control characters to avoid creating invalid records.

When DataFabric removes control characters, it removes ASCII character codes 0-31 and 127 with the following exceptions:

* 9 - Tab
* 10 - Line feed
* 13 - Carriage return

You can use the **Ignore Ctrl Characters** property in stages where it's available.



