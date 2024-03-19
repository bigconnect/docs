# Making Unauthenticated Requests

By default, Liferay DXP restricts API access for the sake of security, requiring authentication in order to get a valid response. However, in certain cases it may make sense to open up an API for guest access.

Making an API request without passing credentials generally leads to an error response:

```log
curl localhost:8080/o/headless-admin-user/v1.0/sites/20122
```

```log
{
  "message" : "Access denied to com.liferay.headless.admin.user.internal.resource.v1_0.SiteResourceImpl#getSite"
}
```

If you need access to an API without credentials, then grant unrestricted access via a Service Access Policy.

### [Relaxing API Restrictions via Service Access Policy](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/making-unauthenticated-requests#relaxing-api-restrictions-via-service-access-policy) <a href="#relaxing-api-restrictions-via-service-access-policy" id="relaxing-api-restrictions-via-service-access-policy"></a>

You can use [Service Access Policies](https://learn.liferay.com/web/guest/w/dxp/installation-and-upgrades/securing-liferay/securing-web-services/setting-service-access-policies) to define API access at a granular level.

1. Visit your site with your browser at `http://localhost:8080`.
2.  Sign in using the default credentials:

    **User Name:** `test@liferay.com` **Password:** `test`
3. Go to Global Menu → Control Panel → Security → Service Access Policy.
4. Click _Add_ (![add](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
5. Give the policy a descriptive name. It makes sense to call it something like “SITE\_API\_GUEST\_ACCESS”.
6. Switch the _Enabled_ toggle to enable the policy.
7. Switch the toggle labeled _Default_ to apply the policy to unauthenticated requests as well as authenticated requests.
8. Give the policy a localized title, like _Grant Guest access to the Site API_.
9. Click _Switch to Advanced Mode_ at the bottom.
10. Copy and paste the method signature from the error message above: `com.liferay.headless.admin.user.internal.resource.v1_0.SiteResourceImpl#getSite`.
11. Click _Save_.

NOTE

Make sure to use Liferay’s external headless API classes instead of Liferay’s internal service layer classes in your policies.

![Service Access Policies define rules for access to APIs.](https://resources.learn.liferay.com/images/dxp/latest/en/headless-delivery/consuming-apis/making-unauthenticated-requests/images/01.png)

Your Service Access Policy is now active, granting Guests access to the Sites API.

### [Confirming Access](https://learn.liferay.com/web/guest/w/dxp/headless-delivery/consuming-apis/making-unauthenticated-requests#confirming-access) <a href="#confirming-access" id="confirming-access"></a>

Now that the Service Access Policy change is applied, the API call that previously failed now succeeds:

```log
curl localhost:8080/o/headless-admin-user/v1.0/sites/20122
```

```log
{
  "availableLanguages" : [ "en-US" ],
  "description" : "",
  "friendlyUrlPath" : "/guest",
  "id" : 20122,
  "key" : "Guest",
  "membershipType" : "open",
  "name" : "Guest",
  "parentSiteId" : 0,
  "sites" : [ ]
}
```

You can apply this pattern to open access to any headless REST API. Leverage the flexibility of Service Access Policies to make this as granular as you need.

IMPORTANT

Service Access Policies can’t discriminate between GraphQL APIs as easily as Headless REST APIs, since all the APIs are consolidated under a single endpoint. So while opening up GraphQL access like this may work in some cases, it isn’t generally recommended, because you might accidently grant access to too much data.
