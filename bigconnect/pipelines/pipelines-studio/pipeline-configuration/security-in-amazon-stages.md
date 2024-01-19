# Security in Amazon Stages

You can configure Amazon stages to use one of the following authentication methods to connect securely to Amazon Web Services (AWS):

Instance profileWhen BigConnect Pipelines runs on an Amazon EC2 instance that has an associated instance profile, BigConnect Pipelines uses the instance profile credentials to automatically authenticate with AWS.For more information about associating an instance profile with an EC2 instance, see the Amazon EC2 documentation.AWS access keysWhen BigConnect Pipelines does not run on an Amazon EC2 instance or when the EC2 instance doesn’t have an instance profile, you can authenticate using an AWS access key pair. When using an AWS access key pair, you specify the access key ID and secret access key to use.Note: When configuring an Amazon S3 stage or an Amazon SQS Consumer origin to access a public bucket, you can also configure the stage to connect anonymously using no authentication.

## Assume Another Role <a href="#ariaid-title2" id="ariaid-title2"></a>

When using instance profile or AWS access keys authentication, you can configure the Amazon stage to assume another IAM role.

For example, if the instance profile or the IAM user permissions do not grant access to write to Amazon S3 resources, you can configure the Amazon S3 destination to assume another role that does grant write access.

When an Amazon stage assumes a role, it temporarily gives up the instance profile or IAM user permissions and uses the permissions assigned to the assumed role. To assume a role, the stage calls the AWS STS AssumeRole API operation and passes the role to use. The operation creates a new session with the temporary credentials, as long as the following conditions are true:

* The IAM policy attached to the current principal - the IAM role or user - grants permission to assume the specified role.
* The IAM trust policy attached to the role to be assumed permits the current principal to assume it.

You can configure an Amazon stage to assume a role in the following ways:Assume a role with no restrictions

When configured to assume a role with no restrictions, any StreamSets user account that starts the pipeline can assume the role specified in the Amazon stage, as long as the IAM policies attached to the current principal and to the role to be assumed allow it.

For example, any Control Hub user who starts the job for the pipeline can assume the `finance` role when the IAM trust policy attached to the `finance` role allows the role to be assumed by the IAM role or user identified by the selected authentication method.

Assume a role using session tags to restrict role access. For increased security, you can configure a stage to assume a role and set session tags to restrict BigConnect Pipelines user accounts allowed to assume the role. When configured to set session tags, the stage passes the following session tag to the AWS STS AssumeRole API operation:

`streamsets/principal=<user>`

Where `<user>` is the name of the currently logged in BigConnect Pipelines user that starts the pipeline or the Control Hub user that starts the job for the pipeline.

AWS IAM verifies that the user account set in the session tag can assume the specified role. The IAM trust policy attached to the role to be assumed must allow the current principal permission to assume the role and must have constraints using IAM condition keys to limit the AssumeRole action based on the requested session tags.

For example, when the Control Hub user Joe starts the job for the pipeline, he can assume the `finance` role when the IAM trust policy attached to the `finance` role allows the user `joe` to assume the role. The Control Hub user Emily cannot assume the `finance` role because the trust policy attached to the `finance` role does not allow the user `emily` to assume the role.

To configure an Amazon stage to assume a role, you first must create the trust policy in AWS that allows the role to be assumed. Then, you configure the required stage properties in BigConnect Pipelines

### Create the Trust Policy <a href="#ariaid-title3" id="ariaid-title3"></a>

In AWS, create and attach a trust policy to the role to be assumed. The policy must allow other principals - IAM roles or users - to assume the role.

Important: You must also attach a policy to the principal that grants permission to the principal to assume another role. AWS IAM provides several methods of granting a principal access to assume another role. For details, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_roles\_create\_for-user.html).

The trust policy that you create for the role to be assumed depends on whether you want to allow stages to assume the role with or without restrictions:

Trust policy to assume the role with no restrictionsCreate and attach a trust policy to the role to be assumed that allows another IAM role or user to assume the role.For example, if using instance profile authentication, you might create the following policy where:

* `<account_id>` is the ID of your AWS account.
* `<role_name>` is the name of the role permitted to assume this role. Enter the name of the role included in the instance profile associated with the Amazon EC2 instance where the execution engine - BigConnect Pipelines - runs.

```
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Sid": "",
     "Effect": "Allow",
     "Principal": {
       "AWS": "arn:aws:iam::<account_id>:role/<role_name>"
     },
     "Action": [
       "sts:AssumeRole"
     ]
   }
 ]
}
```

If using AWS access keys authentication, create a similar trust policy. However, for the principal, specify the ARN of the IAM user permitted to assume this role. Enter the name of the IAM user that owns the access keys used to authenticate with AWS. For example:

```
...
"Principal": {
       "AWS": "arn:aws:iam::<account_id>:user/<user_name>"
},
...
```

Trust policy to assume a role using session tags to restrict role accessCreate and attach a trust policy to the role to be assumed that allows the IAM role or user to assume the role, uses session tags, and restricts the session tag values to specific StreamSets user accounts.For example, if using instance profile authentication, you might create the following policy where:

* `<account_id>` is the ID of your AWS account.
* `<role_name>` is the name of the role permitted to assume this role. Enter the name of the role included in the instance profile associated with the Amazon EC2 instance where the execution engine - BigConnect Pipelines - runs.
* `<user1>` and `<user2>` are BigConnect Pipelines, user accounts allowed to assume this role. To specify a Control Hub user account, use the required naming convention: `<user ID>@<organization ID>`. For example, `joe@MyCompany`.

```
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Sid": "",
     "Effect": "Allow",
     "Principal": {
       "AWS": "arn:aws:iam::<account_id>:role/<role_name>"
     },
     "Action": [
       "sts:AssumeRole",
       "sts:TagSession"
     ],
     "Condition": {
       "StringEquals": {
         "aws:RequestTag/streamsets/principal": ["<user1>", "<user2>"]
       },
        "Null": {
          "aws:RequestTag/streamsets/principal": "false"
        }
     }
   }
 ]
}
```

If using AWS access keys authentication, create a similar trust policy. However, for the principal, specify the ARN of the IAM user permitted to assume this role. Enter the name of the IAM user that owns the access keys used to authenticate with AWS. For example:

```
...
"Principal": {
       "AWS": "arn:aws:iam::<account_id>:user/<user_name>"
},
...
```

For more information about creating an IAM trust policy, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies.html).

### Configure Stages to Assume a Role <a href="#ariaid-title4" id="ariaid-title4"></a>

After you create and attach a trust policy to the role to be assumed, you can configure Amazon stages to assume the role.

1. On the primary tab of the Amazon stage, select AWS Keys or Instance Profile for the Authentication Method property.
2. Select Assume Role.
3.  Configure the following properties:

    | Assume Role Property | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
    | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | Role ARN             | <p>Amazon resource name (ARN) of the role to assume, entered in the following format:</p><p><code>arn:aws:iam::&#x3C;account_id>:role/&#x3C;role_name></code></p><p>Where <code>&#x3C;account_id></code> is the ID of your AWS account and <code>&#x3C;role_name></code> is the name of the role to assume. You must create and attach an IAM trust policy to this role that allows the role to be assumed.</p>                                                                                                                       |
    | Role Session Name    | Optional name for the session created by assuming a role. Overrides the default unique identifier for the session.                                                                                                                                                                                                                                                                                                                                                                                                                    |
    | Session Timeout      | <p>Maximum number of seconds for each session created by assuming a role. The session is refreshed if the pipeline continues to run for longer than this amount of time.</p><p>Set to a value between 3,600 seconds and 43,200 seconds.</p>                                                                                                                                                                                                                                                                                           |
    | Set Session Tags     | <p>Sets a session tag to record the name of the currently logged in Data Pipelines Studio or Transformer user that starts the pipeline or the Control Hub user that starts the job for the pipeline. AWS IAM verifies that the user account set in the session tag can assume the specified role.</p><p>Select only when the IAM trust policy attached to the role to be assumed uses session tags and restricts the session tag values to specific user accounts.</p><p>When cleared, the connection does not set a session tag.</p> |
