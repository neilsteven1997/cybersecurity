# AWS Security - S3cret Santa
---

Programmatic access to Amazon Web Services (AWS) is facilitated through long-term credentials consisting of an Access Key ID and a 
Secret Access Key. These credentials, typically stored in the local file system, enable the AWS Command Line Interface (CLI) to 
authenticate requests. Verification of the currently active identity is performed using the Security Token Service (STS) 
`get-caller-identity` call, which returns the unique UserID, Account ID, and Amazon Resource Name (ARN) of the principal. This 
initial enumeration is critical for situational awareness within a cloud environment, as it confirms the specific user account under 
which subsequent actions will be executed.

Cloud security depends heavily on Identity and Access Management (IAM), which governs the relationships between users, groups, roles,
and policies. IAM users represent individual identities with persistent credentials, while IAM groups allow administrators to manage 
permissions for multiple users collectively. IAM roles provide a mechanism for granting temporary, elevated permissions to users or 
services. Unlike users, roles do not have long-term credentials and must be assumed to activate their associated permissions. Access 
control for these entities is defined by IAM policies, which are JSON documents specifying allowed or denied actions, the target 
resources, and optional conditions.

Enumeration of a compromised user’s permissions involves identifying both inline and managed policies. Inline policies are embedded 
directly within a single IAM identity, whereas managed policies can be attached to multiple entities. By querying the IAM service, 
an attacker can determine if a user has the `sts:AssumeRole` permission, which may allow lateral movement or privilege escalation. 
If a role is found with a trust policy allowing the current user to assume it, temporary credentials—including a Session Token—can 
be requested from STS. These temporary credentials must be exported as environment variables to authenticate as the target role.

Amazon Simple Storage Service (S3) is an object storage solution where data is organized into containers called buckets. 
Misconfigured IAM policies often grant excessive permissions to S3 resources, such as the ability to list all buckets or retrieve 
objects. Once a higher-privileged role is assumed, an attacker can enumerate bucket contents and exfiltrate sensitive files. 
Securing these environments requires the principle of least privilege, ensuring that users and roles only have the minimum 
permissions necessary for their functions, thereby limiting the impact of credential theft.

---
| Description | Code/Command |
| --- | --- |
| Retrieve current user identity details | `aws sts get-caller-identity` |
| List all IAM users in the account | `aws iam list-users` |
| List inline policies for a specific user | `aws iam list-user-policies --user-name [username]` |
| List managed policies attached to a user | `aws iam list-attached-user-policies --user-name [username]` |
| Identify groups associated with a user | `aws iam list-groups-for-user --user-name [username]` |
| View the content of a specific inline user policy | `aws iam get-user-policy --policy-name [policy] --user-name [username]` |
| List all available IAM roles | `aws iam list-roles` |
| List inline policies for a specific role | `aws iam list-role-policies --role-name [rolename]` |
| Retrieve specific inline role policy details | `aws iam get-role-policy --role-name [rolename] --policy-name [policy]` |
| Request temporary credentials to assume a role | `aws sts assume-role --role-arn [arn] --role-session-name [session]` |
| Set Access Key ID environment variable | `export AWS_ACCESS_KEY_ID="[key]"` |
| Set Secret Access Key environment variable | `export AWS_SECRET_ACCESS_KEY="[secret]"` |
| Set Session Token environment variable | `export AWS_SESSION_TOKEN="[token]"` |
| List all S3 buckets in the account | `aws s3api list-buckets` |
| List objects contained within a specific bucket | `aws s3api list-objects --bucket [bucketname]` |
| Download a specific file from an S3 bucket | `aws s3api get-object --bucket [bucketname] --key [filename] [localname]` |

---
### Key Takeaways - Use the Security Token Service (STS) to verify identity and obtain temporary session credentials.
* Distinguish between inline policies (identity-specific) and managed policies (reusable across entities) during enumeration.
* Leverage the `sts:AssumeRole` permission to transition from a low-privileged user to a more permissive role.
* Analyze IAM policy JSON documents to identify specific Actions, Resources, and Effects (Allow/Deny).
* Treat S3 buckets as high-value targets for sensitive data such as credentials, logs, and internal documentation.
* Implement the principle of least privilege to prevent lateral movement via role assumption.

---
>[!Note]
>### You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!



