# Autosave

See [Database Support](https://doc.orbeon.com/form-runner/persistence/db-support) for the detail of which persistence layers and Orbeon Forms versions support the autosave feature.

### See also <a href="#see-also" id="see-also"></a>

* [Blog post](https://blog.orbeon.com/2013/10/autosave.html).

### Enabling autosave <a href="#enabling-autosave" id="enabling-autosave"></a>

The following conditions must be met for autosave to happen:

* The user must be logged in.
* The user must have update permissions (if permissions are enabled for the form).
* The form mode must be `new` or `edit`.
* The persistence layer must support autosave and have the `oxf.fr.persistence.$name.autosave` property set to `true` (`true` by default for the built-in relational databases).
* The autosave delay (set with `oxf.fr.detail.autosave-delay`) must be greater than `0` (which is the case by default).

_NOTE: Form Builder doesn't yet support autosave as of Orbeon Forms 2019.1._

### Disabling autosave <a href="#disabling-autosave" id="disabling-autosave"></a>

If the following conditions above are met, autosave will be enabled. In this case, if you wish to disable autosave, you can set the `oxf.fr.detail.autosave-delay` to 0:

Copy

```
<property 
    as="xs:integer" 
    name="oxf.fr.detail.autosave-delay.*.*" 
    value="0"/>
```

You can as usual disable it for specific forms only, for example:

Copy

```
<property 
    as="xs:integer" 
    name="oxf.fr.detail.autosave-delay.acme.order" 
    value="0"/>
```

### How autosave works <a href="#how-autosave-works" id="how-autosave-works"></a>

When autosave is enabled and you are an authenticated user, form data is automatically saved as _drafts_ in the background as you enter and modify form data. This reduces the chance that you will lose data if something goes wrong and you haven't explicitly saved the data.

#### Summary page <a href="#summary-page" id="summary-page"></a>

The summary page shows the drafts, on separate lines and clearly marked as such. From the summary page, users can click on a draft to open it, or select it to then delete it, assuming they have the permission to do so.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC1GlCjdLk-HR9h28%252Fautosave-summary.png%3Fgeneration=1528743356359783%26alt=media\&width=768\&dpr=4\&quality=100\&sign=5c0d77fa0cd250eaa837b8bd21a538a272951c33bc7fb78ea82cb71eb27ca9cf)

Summary page

#### Edit page <a href="#edit-page" id="edit-page"></a>

If users edit a form for which there is a draft, they will be asked whether they want to open the saved data, or start from the autosaved draft.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-Lt6yUHWbW3tVI39SKCk%252F-LEkC1GnY57wSjjjKTl4%252Fautosave-open.png%3Fgeneration=1573165643884742%26alt=media\&width=768\&dpr=4\&quality=100\&sign=fe22692e87fb942d9b59c11ff873569d253f82e09344ad0f20dc58b09056dc5c)

Edit page

#### New page <a href="#new-page" id="new-page"></a>

If users started filling out a new form, but didn't save the data, if starting to fill out a new form later, they will be asked whether they wish to start from scratch, or from one of the drafts saved earlier. In this case, the prompt will be different whether there is just one draft for new data, or multiple drafts available, as in the latter case, users will need to select which draft they want to use.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-Lt6zvh8A2L44WJhx1Rl%252F-LEkC1GpyWHNLyGTdWHe%252Fautosave-new-single.png%3Fgeneration=1573166027980225%26alt=media\&width=768\&dpr=4\&quality=100\&sign=9769ffbbd0ab276851d71c258eb48caad7f5ae505f244e4f7cd5fac8c512e8f1)

New page, single draft

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-Lt6zvh8A2L44WJhx1Rl%252F-LEkC1GrwbixOy966GZI%252Fautosave-new-multiple.png%3Fgeneration=1573166027878815%26alt=media\&width=768\&dpr=4\&quality=100\&sign=735683bb53e4e19e2b2abb74ac2ed6a791a196d15fa010c1c90b6c9ef9b467c3)

New page, single draft

When multiple drafts are available, choosing the "View autosaved drafts" button takes you to the form's Summary page in a special mode where only the relevant drafts are visible:

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC1Gt-zm\_-XDbgH71%252Fautosave-summary-drafts-only.png%3Fgeneration=1528743348532878%26alt=media\&width=768\&dpr=4\&quality=100\&sign=bbf156d8c0a0887336fc59da2fdc5c49d4cb151a84f788a244966d80b1645928)

Summary page with drafts only

### Configuration <a href="#configuration" id="configuration"></a>

#### With Orbeon Forms 4.4 and newer <a href="#with-orbeon-forms-4.4-and-newer" id="with-orbeon-forms-4.4-and-newer"></a>

You don't need to do anything special to use this feature.

#### Properties <a href="#properties" id="properties"></a>

Copy

```
<property
  as="xs:integer"
  name="oxf.fr.detail.autosave-delay.*.*"
  value="5000"/>
```

The value of `oxf.fr.detail.autosave-delay` sets the maximum time, in milliseconds, after which a change done by the user will be autosaved. If 0 or negative, autosaving is disabled. Out of the box, this property is set to `5000`, that is 5 seconds. If your server experiences a lot of load, you might want to make that value higher to reduce the number of requests made by Orbeon Forms to your database.

The following property enables or disable autosave for a given persistence provider, as autosave requires support from the persistence provider. For database support, see [Database Support](https://doc.orbeon.com/form-runner/persistence/db-support).

Copy

```
<property
  as="xs:boolean"
  name="oxf.fr.persistence.*.autosave"
  value="false"/>
```
