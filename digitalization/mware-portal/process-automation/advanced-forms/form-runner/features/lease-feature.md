# Lease Feature

When the lease feature is enabled, Form Runner prevents multiple users from concurrently editing the same form instance. When a first user, let's call him Homer, opens a form instance for editing, Homer is assigned a _lease_ on that form instance for a given duration, say 10 minutes. The lease gets automatically extended when Homer updates the form, say by changing the value of a field, or when Homer clicks on a button to request a lease extension (more on this below).

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252F-LEkBmMpnaxnftLGDglW%252F-LEkC7VG7d2d7b5BB-Zr%252Flease-own.png%3Fgeneration=1528743366151426%26alt=media\&width=768\&dpr=4\&quality=100\&sign=a7e4d937fa6c410d45e7721b1e0a204d0cdecfc21a481273d34df1997eaddedb)

Message showing the user owns the lease

If a second user, let's call her Marge, opens the same form instance for editing while Homer has a lease, Marge will be told she can't edit this form right now, has Homer has a lease. She can click on the first button to try to acquire the lease again, or \[SINCE Orbeon Forms 2020.1] on the second to view data in read-only mode.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F796a281d9bf8f165432e8ae588cbdebf08ca8242.png%3Fgeneration=1590697209104507%26alt=media\&width=768\&dpr=4\&quality=100\&sign=4cf2e2220fb1a8139677f76129ecde742e59c948c7ce5727e3ae7522f8e2617e)

Message showing another user owns the lease

The lease given to Homer will end either because it expires without being renewed, or because Homer clicked on a button to explicitly relinquish the lease. At this point, Homer can click on the first button to try to acquire the lease again, or \[SINCE Orbeon Forms 2020.1] on the second to view the data in read-only mode. \[SINCE Orbeon Forms 2021.1.5 and 2022.1] As the "Show in read-only mode" button does a page navigation, and as page navigations aren't supported by the JavaScript or Java embedding API, that button is hidden in those cases. You can override this behavior through the `oxf.fr.detail.button.lease.show-in-view-mode.visible.*.*` property.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-LEkBiSDvlXs3VWiO0Zu%252Fsync%252F3ec3a275ea7c39705e17d405ebffa0f3406fe89d.png%3Fgeneration=1590726929960892%26alt=media\&width=768\&dpr=4\&quality=100\&sign=b9931ef173d5a486cd273efc38051fdbf0380c282f5c42a71f19354f1d8469a3)

Message showing the user relinquished the lease

### Enabling the lease feature <a href="#enabling-the-lease-feature" id="enabling-the-lease-feature"></a>

By default, the lease feature is disabled. It is enabled when the following conditions are met:

* You've set the property `oxf.fr.detail.lease.enabled.*.*` to `true`, as shown below (the default is `false`, so you do need to set this property).
* The user is authenticated.
* The implementation of the persistence API used by the current app/form supports the lease feature. As of Orbeon Forms 2018.2, this is the case of all the built-in implementations of the persistence API for relational databases, but not of the implementation of the persistence API for eXist.

If the lease feature is enabled, when non-authenticated users try to edit the data, they receive receive a [403 Forbidden](https://en.wikipedia.org/wiki/HTTP\_403).

Copy

```
<property 
    as="xs:boolean" 
    name="oxf.fr.detail.lease.enabled.*.*"
    value="true"/>
```

### Configure lease duration and thresholds <a href="#configure-lease-duration-and-thresholds" id="configure-lease-duration-and-thresholds"></a>

When users get a lease, by default its duration is of 10 minutes. You can change this default by setting the property below. The timeout can be set for all the form in a given app by replacing the second `*` by the name of that app, or even more specifically for a specific form of that app by also replacing the third `*` by the name of that form.

The first `*` can be replaced by a role name if you want the duration of the lease to be higher for users having that role. If in a given situation multiple properties apply to the current form and current user, then the highest duration is used. For instance, say you leave the default to 10 minutes, but add a property `oxf.fr.detail.lease.duration.admin.*.*` set to 20 minutes. Then users with the role `admin` will get 20 minutes, while other users will get 10 minutes.

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.detail.lease.duration.*.*.*"
    value="10"/>
```

As mentioned earlier, when user change values in the form, the lease gets automatically renewed. However, minimize hits to the database this isn't done during the first minute after the lease was first obtained or renewed. If you wish to even even further minimize hits to the database, you can increase the value of the value of the following property.

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.detail.lease.renew-threshold.*.*"
    value="1"/>
```

Two minutes before the lease expires, Form Runner will tell users that the lease is about to expire, and offer the option to renew the lease. If you wish users to be warned earlier, you can increase the value of the following property.

Copy

```
<property
    as="xs:integer"
    name="oxf.fr.detail.lease.alert-threshold.*.*"
    value="2"/>
```

### Hiding buttons <a href="#hiding-buttons" id="hiding-buttons"></a>

Most of the buttons typically shown at the bottom of the form when editing data, such as "Save" or "Submit", shouldn't be shown when users don't currently own the lease on the document. However, other buttons, such as "Home" or "Summary" that are only used for navigation, can be shown even when users don't currently own the lease.

To discriminate between those cases, you can use the `fr:owns-lease-or-none-required()` XPath function. By default, buttons are only shown if users own the lease or no lease is required; you could change this default by modifying the value of the following property, but we don't recommend you do so.

Copy

```
<property
    as="xs:string"
    name="oxf.fr.detail.button.*.visible.*.*"
    value="fr:owns-lease-or-none-required()"/>
```

If you have a button, that should be shown even if users don't own the lease, you'll want to add the following property:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.detail.button.[button-name].visible.*.*"
    value="true()"/>
```

If you have a button displayed upon some condition being true, that is for which you have defined a `oxf.fr.detail.button.[button-name].visible.*.*` property, if that button shouldn't be displayed when users don't own the lease, you'll want to add `and fr:owns-lease-or-none-required()` to your XPath expression:

```
<property
    as="xs:string"
    name="oxf.fr.detail.button.[button-name].visible.*.*"
    value="
        [your conditional XPath expression]
        and fr:owns-lease-or-none-required()
    "/>
```
