# Landing page

The Landing page is organized in cards which provides quick access to:

* Quick links, including Form Builder and the Administration page
* Your published forms
* Your in-progress Form Builder forms
* Demo forms

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-dbdbb11fc2846f92dbd98014bc6568a32ef780bb%252Flanding.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=5ddc5575bb448fff9ff1ef3c9673dc605e8f6ed571b73c405afe88d7c736a5e9)

The Landing page

Some cards directly list content, including the list of published forms and the list of in-progress Form Builder forms.

You can configure whether you want to have a particular card on the Landing page. For example, you can hide the demo forms for production deployment.

All Form Runner and Form Builder navigation bars now provide a direct link to the Landing page. This can be disabled if not desired.

![](https://doc.orbeon.com/\~gitbook/image?url=https:%2F%2F4129616727-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-LEkBiSDvlXs3VWiO0Zu%252Fuploads%252Fgit-blob-4cd9c0c78a35a1261084f294a308682c529d6985%252Fsummary-navigation.png%3Falt=media\&width=768\&dpr=4\&quality=100\&sign=8d064667df9dbe8091720bee8cf30d3872a93012c8a4e947070f7dedbffbce64)

Landing page navigation

### Configuration properties <a href="#configuration-properties" id="configuration-properties"></a>

The following properties control which cards are shown on the Landing page:

Copy

```
<property
    as="xs:string"
    name="oxf.fr.landing.cards"
    value="quick-links published-forms form-builder-forms demo-forms"/>
```
