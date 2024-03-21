# Channel Defaults

After creating a business account, you can set channel defaults for it. This can improve customer experience by reducing the time it takes to create orders and help account managers tailor Commerce behavior for specific accounts. You can set these channel defaults:

* Billing Address
* Shipping Address
* Delivery Terms and Conditions
* Payment Terms and Conditions
* Shipping Options
* Price Lists
* Discounts
* Currencies
* Payment Methods
* Channel Account Managers

Depending on user roles and permissions, you can change the defaults of an order during checkout. You can also set permissions so users can only place orders with the channel defaults.

### Understanding Eligibility and Channel Defaults <a href="#understanding-eligibility-and-channel-defaults" id="understanding-eligibility-and-channel-defaults"></a>

Channel defaults work in conjunction with eligibility. This means if an order is not eligible for the set default, Liferay uses the next highest priority value that is eligible. For some channel defaults, you can override this behavior to apply the default regardless of eligibility.

For example, if Discount A is the channel default for a business account but only Order Type X is eligible for Discount A, Liferay does not apply Discount A to orders of a different type. Instead, Liferay applies the next highest priority discount for which the order is eligible, if any exists. However, if you override eligibility, Liferay applies Discount A to all account orders, regardless of eligibility.

## Setting Channel Defaults <a href="#setting-channel-defaults" id="setting-channel-defaults"></a>

Users with the necessary account management permissions can set channel defaults for business accounts.

1. Open the _Global Menu_ ( ![Global Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-applications-menu.png) ) and navigate to _Control Panel_ → _Accounts_.
2. Select a business account and go to the _Channel Defaults_ tab.

Here you can set the following defaults.

NOTE

If you set defaults for individual channels and all other channels, the individual defaults takes priority if eligible. Version 2024.Q1+`There are two new permissions to control user access to view and manage channel defaults. A user with the`Accounts > Account Entry: View Channel Defaults`permission can only view the defaults under the Channel Defaults tab. Users with the`Accounts > Account Entry: Manage Channel Defaults permission can view and edit channel defaults.

### Default Billing and Shipping Addresses <a href="#default-billing-and-shipping-addresses" id="default-billing-and-shipping-addresses"></a>

1. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) under Billing Addresses or Shipping Addresses.
2. Select _All Channels_ or an individual channel.
3. Select an address.
4.  Click _Save_.

    ![Set default billing and shipping addresses for all or individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/01.png)

NOTE

Only users with account management permissions can manage channel defaults. However, account users can set their default billing and shipping addresses for all channels via the Account Details tab.

### Default Terms and Conditions <a href="#default-terms-and-conditions" id="default-terms-and-conditions"></a>

1. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) under Delivery Terms and Conditions or Payment Terms and Conditions.
2. Select _All Channels_ or an individual channel.
3.  (Optional) Enable the _Override Eligibility_ toggle to override eligibility set for the terms and conditions.

    IMPORTANT

    Payment terms are linked to a payment method and delivery terms are linked to a shipping option. When there are multiple eligible terms and conditions for a payment method or shipping option, overriding eligibility sets the selected terms and conditions as the default, regardless of eligibility. See Understanding Eligibility and Channel Defaults to learn more.
4. Select a _term_.
5.  Click _Save_.

    ![Set default terms and conditions for all or individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/02.png)

### Default Shipping Options <a href="#default-shipping-options" id="default-shipping-options"></a>

The default shipping value is determined by existing priority settings for the channel. If other shipping options are available, you can select one of them as the default:

1. Go to _Shipping Options_ and click _Edit_ for the desired channel.
2. Select a shipping option.
3.  Click _Save_.

    ![Set a default shipping option for individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/03.png)

### Default Price Lists and Discounts <a href="#default-price-lists-and-discounts" id="default-price-lists-and-discounts"></a>

1. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) under Price Lists or Discounts.
2. Select _All Channels_ or an individual channel.
3. (Optional) Enable the _Override Eligibility_ toggle to override existing eligibility criteria configured for price lists.
4. Select a _price list_ or _discount_.
5.  Click _Save_.

    ![Set a default price list or discount for all or individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/04.png)

### Default Currencies <a href="#default-currencies" id="default-currencies"></a>

1. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) under Currencies.
2. Select _All Channels_ or an individual channel.
3.  Select a _currency_.

    IMPORTANT

    This sets the default channel currency for the account during navigation only. Product pages and the catalog use the currency set here. The currency set for the channel at the time of creation is not modified.
4.  Click _Save_.

    ![Set a default currency for all or individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/05.png)

### Default Payment Methods <a href="#default-payment-methods" id="default-payment-methods"></a>

The default payment method is determined by existing priority settings for the channel. If other payment methods are available, you can select one of them as the default:

1. Go to _Payment Methods_ and click _Edit_ for the desired channel.
2. Select a payment method.
3.  Click _Save_.

    ![Set a default payment method for individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/06.png)

### Default Channel Account Managers <a href="#default-channel-account-managers" id="default-channel-account-managers"></a>

1. Click _Add_ ( ![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png) ) under Channel Account Managers.
2.  Select _All Channels_ or an individual channel.

    TIP

    You can assign multiple channel account managers to the same channel.
3.  Select a user.

    All users with a regular role having the `Accounts > Account Entry: Manage Available Accounts via User Channel Rel` permission appear in the channel account manager drop-down. See Understanding Channel Account Managers for more information.
4.  Click _Save_.

    ![Select default channel account manager for all or individual channels.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/07.png)

#### Understanding Channel Account Managers <a href="#understanding-channel-account-managers" id="understanding-channel-account-managers"></a>

When selecting default Channel Account Managers, only users with a regular role that have the `Accounts > Account Entry: Manage Available Accounts via User Channel Rel` permission appear. You can use this permission to grant users access to accounts without requiring them to be account members or organization account managers.

![Users with this permission appear in the channel account manager dropdown menu.](https://resources.learn.liferay.com/images/dxp/latest/en/users-and-permissions/accounts/channel-defaults/setting-channel-defaults/images/08.png)

By itself, this permission does not grant access to the Accounts application, or to view and update individual accounts. It also does not grant allow managing orders or setting channel defaults for price lists and other Commerce entities. If you want Channel Account Managers to perform these administrative actions, you must grant them the necessary permissions for each application (e.g., Accounts, Orders, Price Lists). See Channel Defaults Permissions Reference for more information.

NOTE

If you use organizations, users can continue to manage the organization accounts they have permission to manage along with accounts for which they are default Channel Account Managers.

## Channel Defaults Permissions Reference

Setting channel defaults for an account ensures that addresses, shipping options, terms and conditions, and payment methods are pre-selected for checkout. This can provide a smoother checkout experiences and faster order creation.

Only users with specific permissions can change these defaults at checkout. Users without these permissions do not see the associated checkout steps.

### Checkout Flow Permissions <a href="#checkout-flow-permissions" id="checkout-flow-permissions"></a>

Users with the following permissions can change an account’s billing address, payment methods, and shipping options during checkout.

#### Billing Addresses <a href="#billing-addresses" id="billing-addresses"></a>

| Permissions                                                            | Description                                                                                                             |
| ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Commerce > Order Management > Commerce Orders > View Billing Address   | View an account’s billing address during checkout and in these pages: Order Summary, Placed Orders, and Pending Orders. |
| Control Panel > Accounts > Accounts > Account Entry > Manage Addresses | View, add, and edit an account’s addresses during checkout.                                                             |

#### Payment Methods <a href="#payment-methods" id="payment-methods"></a>

| Permissions                                                                           | Description                                                                           |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Commerce > Order Management > Commerce Orders > Manage Commerce Order Payment Methods | Change an account’s payment methods based on the channel’s available payment methods. |

When one or more payment methods are active for the channel, the highest priority eligible payment method is pre-selected. With this permission, you can choose an alternative payment method. Otherwise, this checkout step does not appear.

#### Shipping Options <a href="#shipping-options" id="shipping-options"></a>

| Permissions                                                                            | Description                                                                             |
| -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Commerce > Order Management > Commerce Orders > Manage Commerce Order Shipping Options | Change an account’s shipping options based on the channel’s available shipping options. |

When one or more shipping options are active for the channel, the highest priority eligible option is pre-selected. With this permission, you can choose an alternative shipping option. Otherwise, this checkout step does not appear.

\
\
