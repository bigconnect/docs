# Export and import apps

Use the methods below to move an App from one installation to another or to back up the design and data in your App. Admins and Developers can export any app as a JSON blob text file.

The exported file will include all DB data, data connector configurations, query configs, and builder data for all your screens.

> ### 🚧Security
>
> When exporting a production app, be vigilant about whom you share the app export files with.\
> Are you confident you won't leak database credentials, user identity information, authentication keys, or other sensitive information?
>
> We also recommend that you make use of the password-based app export encryption.

### Export an application

To export an app from the Apps dashboard with just a few clicks, select the three dots next to the app that you'd like to export:

<figure><img src="https://files.readme.io/353e850720f457c4def61192643eaf1e319c2e1c87d12cc89d78e3f9b13d6aef-Screenshot_2024-10-24_at_16.06.16.png" alt=""><figcaption></figcaption></figure>

This will bring up a few options to choose from:

* **Duplicate** will create a copy of your app
* **Export latest edited** will export the development version of your app, including any changes since your last publish
* **Export latest published** will export only the last published version of your app
* **Delete** will delete your app\


After choosing an export option, you will be given the option to _Export rows from internal tables_. This will include or exclude all the data rows from your internal DB tables in the export; however, data source **configuration and credentials will still be exported**.

\


<figure><img src="https://files.readme.io/1129a3f732b109d75a92a100879edd9b7084e079adad86f02f8800b6294a02f4-Screenshot_2024-10-24_at_16.13.08.png" alt=""><figcaption></figcaption></figure>

In addition to that, you'll be presented with the option _Encrypt my export_ which will allow you to add a password to encrypt your export:

![](https://files.readme.io/cb1c545715e98ced631677be6fb0b5022c869e8c90fd92f6e0cd735a74f49c39-Screenshot\_2024-10-24\_at\_16.17.08.png)

Alternatively, you can click on the app you'd like to export, navigate to the **Settings** section, select Import/Export, and follow the same steps to download a copy.

\


<figure><img src="https://files.readme.io/cd865319196f5cbddf686669c710e9448aa9a676b7854dfa8879c3fafa2a971a-Screenshot_2024-10-24_at_16.29.26.png" alt=""><figcaption></figcaption></figure>

> ### 👍Environment variables
>
> By using Environment variables, you can exclude secrets and datasource credentials from your app exports.

### Import an application

You can create an app by importing an app export file.

Within the _Apps_ screen, click the **Create new app** button. From here, click on the **Import app** button:

![](https://files.readme.io/129750f-Screenshot\_2022-06-14\_at\_16.03.04.png)

This will present a modal, into which you can drag the app export file, and click **Import app** to finish.

![](https://files.readme.io/9144850-Screenshot\_2022-06-14\_at\_16.04.33.png)

> ### 👍Backups
>
> App backups can be imported in the same way you would import a manual export.
>
> The benefit of using backups is that they will automatically be created when publishing your app.

### Import an application and retain your users table

If you need to retain your users and data tables, but override your screens and automations, you can import your screens and datasources within your application settings.

<figure><img src="https://files.readme.io/7e5ab9a-Screenshot_2023-09-27_at_12.21.16.png" alt=""><figcaption></figcaption></figure>
