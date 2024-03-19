# Beta Features and Feature Flags

### [Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#feature-flags) <a href="#feature-flags" id="feature-flags"></a>

DXP 7.4 U69+/GA69+

Some features in Liferay are hidden behind _feature flags_. Using feature flags, you have control over new functionality introduced into your Liferay instance. New and experimental features can be adopted and validated more effectively with feature flags.

To access and manage feature flags, open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Instance Settings_. Click _Feature Flags_ under _Platform_. There are three types of feature flags available:

1. [Beta feature flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags)
2. [Release feature flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags)
3. [Dev feature flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#dev-feature-flags)

![You can use feature flags to control the visibility of features.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/feature-flags/images/01.png)

The documentation for a beta feature includes a badge to set it apart:

![The documentation for a beta feature includes a badge.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/feature-flags/images/02.png)

### [Beta Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags) <a href="#beta-feature-flags" id="beta-feature-flags"></a>

Beta features are functional features or capabilities undergoing business validation. These are experimental and require careful consideration before usage. Beta features may be deprecated after a limited time period or converted into a [release feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags). You can give feedback on beta features through Liferay’s [Community Slack](https://liferay.dev/chat).

To enable a beta feature,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Instance Settings_.
2. Click _Feature Flags_ under Platform, then click _Beta_.
3. All beta features appear. Toggle to enable or disable a feature.

You can also use the search bar to find beta features, sort them using the Sort (![Sort](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-sort2.png)) button, or filter them based on their status.

![You can search, sort, or filter beta features.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/feature-flags/images/03.png)

### [Release Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags) <a href="#release-feature-flags" id="release-feature-flags"></a>

Release features are fully functional, but remain hidden behind a flag for a limited time. The flag allows you to control the visibility of a new feature and enable it when ready.

To enable a release feature,

1. Open the _Global Menu_ (![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png)) and navigate to _Control Panel_ → _Instance Settings_.
2. Click _Feature Flags_ under Platform, then click _Release_.
3. All release features are listed. Toggle to enable or disable a feature.

You can also use the search bar to find release features, sort them using the Sort (![Sort](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-sort2.png)) button, or filter them based on their status. The flag for release features is removed after a period of time, so monitor the release notes for your Liferay update.

![You can search, sort, or filter release features.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/feature-flags/images/04.png)

### [Dev Feature Flags](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#dev-feature-flags) <a href="#dev-feature-flags" id="dev-feature-flags"></a>

Dev feature flags are for features and capabilities in active development, and you must not enable them in production. Only enable dev features for testing.

To enable a theoretical dev feature flag with the code `XXX-000000` using a portal property, add this to `portal-ext.properties`:

```log
feature.flag.XXX-000000=true
```

To enable a theoretical dev feature flag with the code `XXX-000000` using an environment variable, add this to your configuration:

```log
LIFERAY_FEATURE_PERIOD_FLAG_PERIOD__UPPERCASEX__UPPERCASEX__UPPERCASEX__MINUS__NUMBER0__NUMBER0__NUMBER0__NUMBER0__NUMBER0__NUMBER0_=true
```

WARNING

Never use dev features in production. Instabilities in Liferay are expected side effects of enabling dev feature flags.

### [Out-of-the-Box Beta Features](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#out-of-the-box-beta-features) <a href="#out-of-the-box-beta-features" id="out-of-the-box-beta-features"></a>

Liferay 2023.Q4+/GA102+

As described above, beta features are experimental and require careful consideration before usage. Beta features may be deprecated after a limited time period or converted into a [release feature](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#release-feature-flags). You can give feedback on beta features through Liferay’s [Community Slack](https://liferay.dev/chat).

Some beta features are added behind a [beta feature flag](https://learn.liferay.com/web/guest/w/dxp/system-administration/configuring-liferay/feature-flags#beta-feature-flags).

As of Liferay 2023.Q4, some beta features are enabled out-of-the-box, but are marked with a tag in the UI to highlight their beta status (![Beta](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-beta-feature.png)).

![Some beta features are included out-of-the-box, clearly marked in the UI.](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/feature-flags/images/05.png)

\
