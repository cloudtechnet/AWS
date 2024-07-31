# What is IAM?

AWS Identity and Access Management (IAM) is a service that enables you to manage access to AWS services and resources securely. It allows you to create and manage AWS users and groups and use permissions to allow and deny their access to AWS resources.

### Users
In AWS IAM, a user is an entity that you create in AWS to represent a person or application that interacts with AWS resources. Users are necessary for providing credentials (passwords, access keys) that allow access to AWS. Each user has a unique name within an AWS account and can be given specific permissions directly or through group membership.

### MFA (Multi-Factor Authentication)
MFA is an additional layer of security used to protect your AWS environments by requiring users to present two or more separate forms of identification before they can access AWS resources. Typically, these forms include:
1. Something you know (password).
2. Something you have (MFA device, such as a hardware token or an MFA app on a smartphone).

This additional step helps ensure that only authorized users can access the system, even if their password is compromised.

### Groups
A group in AWS IAM is a collection of IAM users. You can create groups to manage permissions for multiple users at once. For example, you might create a group called "Developers" and grant it permissions to manage EC2 instances. Any user added to the Developers group would inherit those permissions. This approach simplifies management by allowing you to set permissions for many users in one place.

### Roles
Roles in AWS IAM are sets of permissions that you can assign to entities that are not users, such as AWS services, applications, or users from another AWS account. Roles allow these entities to assume certain permissions without having to create IAM users and distribute access keys. For instance, an EC2 instance can assume an IAM role to get temporary security credentials for accessing other AWS resources without embedding credentials in the code.

Roles are beneficial for:
- **Delegation:** Allowing AWS services to interact with each other.
- **Cross-account access:** Granting access to AWS resources in different accounts.
- **Federation:** Integrating with corporate identity providers to grant AWS access based on existing identities.

### Summary
- **Users:** Individual identities for people or applications interacting with AWS.
- **MFA:** Additional security layer requiring multiple forms of verification.
- **Groups:** Collections of users with shared permissions for easier management.
- **Roles:** Permissions assigned to entities other than users, enabling secure and scalable access management without distributing long-term credentials.
