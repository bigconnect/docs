# SSO Maintenance Mode

> ### Self Host Only
>
> Maintenance mode is available in self host only. If you are experiencing lock out in cloud please contact support.

### Enabling Maintenance Mode

Set the following environment variable for your installation:

```
ENABLE_SSO_MAINTENANCE_MODE=1
```

#### Rules for Admin SSO Users

When enabled the following rules will apply:

* Request password reset emails will be sent.
* Password reset using email is enabled.
* Login using password is enabled for users who were previously a password user, or set one using the method above

Any non-admin SSO user will be prevented from performing password actions.
