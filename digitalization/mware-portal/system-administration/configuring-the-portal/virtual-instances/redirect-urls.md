# Redirect URLs

Your Liferay Instance can be configured to allow redirects to certain URLs or IP addresses. To configure, navigate to _Control Panel_ → _Instance Settings_ → _Content and Data_ → _Pages_ → _Redirect URLs_.

![Navigate to Redirect URLs under Instance Settings](https://resources.learn.liferay.com/images/dxp/latest/en/system-administration/configuring-liferay/virtual-instances/redirect-urls/images/01.png)

Under Security Mode, choose between _Domain_ or _IP_. Select _Domain_ if you want your Liferay Instance to redirect users to allowed domains. Select _IP_ if you want your Liferay Instance to redirect users to allowed IP addresses.

If you select _Domain_, add your allowed domains under _Allowed Domains_ and click _Save_. To allow redirects to any domain, remove all existing entries. Note, to allow redirects to subdomains, specify a domain with a leading _\*_.

If you select _IP_, add your allowed IP addresses under Allowed IPs and click _Save_. To allow redirect to any IP address, remove all existing entries.
