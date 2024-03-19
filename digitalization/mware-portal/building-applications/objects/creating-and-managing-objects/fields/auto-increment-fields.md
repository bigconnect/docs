# Auto-Increment Fields

Auto-Increment fields are configurable [read-only fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/using-read-only-fields) for uniquely identifying object entries in a human-readable format. For example, an IT ticketing system should generate a unique but meaningful identifier for each ticket, so users can refer to each entry easily.

| Prefix | Starting Value | Suffix | Example Values                                                               |
| ------ | -------------- | ------ | ---------------------------------------------------------------------------- |
| life-  | 2000           | -ray   | <p>life-2000-ray<br>life-2001-ray<br>life-2002-ray<br>…<br>life-3048-ray</p> |
| none   | 0              | none   | <p>0<br>1<br>2<br>…<br>256</p>                                               |
| FOO-   | 01             | none   | <p>FOO-01<br>FOO-02<br>FOO-03<br>…<br>FOO-12</p>                             |

To add an auto-increment field, first enable its release feature flag. Go to _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) → _Control Panel_ → _Instance Settings_ → _Feature Flags_. Open the _Release_ section and enable _Auto-Increment Field Type for Objects (LPS-196724)_.

You must configure the starting numeric value. Liferay adds `1` to this value for each subsequent object entry. Optionally, you can add a meaningful suffix and/or prefix to each numeric value.

![You must provide the initial value of an auto-increment field.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/auto-increment-fields/images/01.png)

When editing the field, you can change its label and configure how it’s searched.

![After creating an auto-increment field, you can edit the label and the searchable settings.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/fields/auto-increment-fields/images/02.png)

### [Exporting and Importing Entries with Auto-Increment Fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/auto-increment-fields#exporting-and-importing-entries-with-auto-increment-fields) <a href="#exporting-and-importing-entries-with-auto-increment-fields" id="exporting-and-importing-entries-with-auto-increment-fields"></a>

Object entries are imported and exported using the [Data Migration Center](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/data-migration-center), a [batch client extension](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/batch-client-extensions), or by calling the [batch engine’s import/export task](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/batch-engine-api-basics-exporting-data) endpoints.

Exporting entries with auto-increment fields preserves the values in the exported JSON. When importing entries that contain auto-increment fields, you must carefully consider whether the data is compatible with the object definition’s existing entries.

Imported entries must have both unique [External Reference Codes (ERCs)](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/using-external-reference-codes) and unique auto-increment values.

| Unique ERC | Unique Auto-Increment Value | Successful Import |
| ---------- | --------------------------- | ----------------- |
| ✔          | ✔                           | ✔                 |
| ✘          | ✔                           | ✘                 |
| ✔          | ✘                           | ✘                 |

Not following this rule leads to inconsistent results. If you import entries with unique ERCs but overlapping auto-increment values, only entries with unique values import successfully:

| <p>Existing Entries<br>ERC/Auto-Increment</p> | <p>Entries to Import<br>ERC/Auto-Increment</p>                                                          | <p>Import Result<br>ERC/Auto-Increment</p>                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| <p>11111/foo-01-bar<br>22222/foo-02-bar</p>   | <p>33333/foo-01-bar<br>44444/foo-02-bar<br>55555/foo-03-bar<br>66666/foo-04-bar<br>77777/foo-05-bar</p> | <p>11111/foo-01-bar<br>22222/foo-02-bar<br>55555/foo-03-bar<br>66666/foo-04-bar<br>77777/foo-05-bar</p> |

If you import entries that don’t follow the auto-increment format defined in the host system, only the values following the specified format import successfully:

| <p>Existing Entries<br>Valid Auto-Increment Values</p> | <p>Entries to Import<br>Auto-Increment Values</p>                          | <p>Import Result<br>Valid Auto-Increment Values</p>         |
| ------------------------------------------------------ | -------------------------------------------------------------------------- | ----------------------------------------------------------- |
| <p>foo-01-bar<br>foo-02-bar</p>                        | <p>foo-003-bar<br>foo-4-bar<br>life-05-ray<br>foo-04-bar<br>foo-05-bar</p> | <p>foo-01-bar<br>foo-02-bar<br>foo-04-bar<br>foo-05-bar</p> |

When you import entries, the highest numeric value in the system after the import becomes the value used to increment new field values.

If you import entries with no value in an auto-increment field, the system generates one according to the specified prefix/suffix format and uses the next numeric value in the series.

### [Using Auto-Increment Fields with APIs](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/fields/auto-increment-fields#using-auto-increment-fields-with-apis) <a href="#using-auto-increment-fields-with-apis" id="using-auto-increment-fields-with-apis"></a>

When you make a GET request to return object entries with an auto-increment field, the value is returned as a JSON string:

```json
"ticket" : "ticket-12"
```

To [add an object definition field](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/managing-objects-with-headless-apis) with the headless API, use a POST request body like this one, which defines this auto-increment field format: `foo-01-bar`, `foo-02-bar`, etc.

```json
{
   "businessType": "AutoIncrement",
   "label": {
      "en_US": "Able Ticket"
   },
   "name": "ableTicket",
   "objectFieldSettings": [
      {
         "name": "initialValue",
         "value": "01"
      },
      {
         "name": "prefix",
         "value": "foo-"
      },
      {
         "name": "suffix",
         "value": "-bar"
      }
   ],
   "required": false
}
```
