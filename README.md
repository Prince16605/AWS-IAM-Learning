````markdown
# 🔐 AWS IAM – Identity and Access Management

---

# 📌 Table of Contents

- [What is AWS IAM?](#-what-is-aws-iam)
- [Purpose of IAM](#-purpose-of-iam)
- [Types of IAM Policies](#-types-of-iam-policies)
- [AWS Managed Policies](#-aws-managed-policies)
- [Customer Managed Policies](#-customer-managed-policies)
- [Inline Policies](#-inline-policies)
- [Important AWS Managed Policies](#-important-aws-managed-policies)
- [Root User vs IAM Administrator](#-root-user-vs-iam-administrator)
- [MFA](#-mfa)
- [Inline Policy](#-inline-policy)
- [Policy vs Role](#-policy-vs-role)
- [IAM Real-World Example](#-iam-real-world-example)
- [IAM Interview Questions](#-iam-interview-questions)
- [IAM Quick Revision](#-iam-quick-revision)
- [IAM Revision Table](#-iam-revision-table)

---

# 🔐 What is AWS IAM?

**AWS IAM (Identity and Access Management)** is an AWS service used to securely control access to AWS resources.

IAM helps you control:

- 👤 Who can access AWS?
- 🔑 How can they authenticate?
- 🛡️ What actions can they perform?
- 📦 Which AWS resources can they access?

### Simple Architecture

```text
User
  ↓
IAM
  ↓
Authentication
  ↓
Authorization
  ↓
AWS Resources
````

---

# 🎯 Purpose of IAM

The main purpose of IAM is:

> **To securely manage identities and control access to AWS resources.**

IAM follows the principle of:

### 🔒 Least Privilege

Users and applications should receive only the permissions they actually need.

### Example

If a developer only needs to read files from S3:

```text
Developer
    ↓
IAM
    ↓
S3 Read Permission
```

The developer does not need full S3 permissions.

---

# 📜 Types of IAM Policies

There are three important types of IAM policies:

1. AWS Managed Policies
2. Customer Managed Policies
3. Inline Policies

```text
IAM Policies
     │
     ├── AWS Managed Policies
     │
     ├── Customer Managed Policies
     │
     └── Inline Policies
```

---

# ☁️ AWS Managed Policies

AWS Managed Policies are policies created and maintained by AWS.

### Important Point

AWS manages these policies, so AWS can update them when required.

### Examples

* `AmazonEC2ReadOnlyAccess`
* `AmazonEC2FullAccess`
* `AmazonS3ReadOnlyAccess`
* `AmazonS3FullAccess`

---

# 👤 Customer Managed Policies

Customer Managed Policies are policies created and managed by the AWS customer.

### Important Point

You have control over:

* Policy creation
* Policy versioning
* Policy updates
* Policy deletion

---

# 📝 Inline Policies

An Inline Policy is a policy directly embedded into a specific IAM identity.

It can be attached directly to:

* IAM User
* IAM Group
* IAM Role

### Important Point

Inline policies have a direct relationship with the identity they are embedded into.

```text
IAM User
    ↓
Inline Policy
    ↓
Specific Permissions
```

They are useful when permissions are intended to exist only with that particular identity.

---

# 🔑 Important AWS Managed Policies

## 1. AmazonEC2ReadOnlyAccess

Provides read-only access to EC2 resources covered by the policy.

### User can generally:

* View EC2 instances
* View instance information
* View configurations

### User cannot generally:

* Launch instances
* Terminate instances
* Modify resources covered by the policy

---

# 2. AmazonEC2FullAccess

Provides broad permissions for EC2 resources covered by the policy.

### Example Actions

* Launch EC2 instances
* Stop instances
* Start instances
* Terminate instances
* Modify EC2 resources

> Always check the current AWS managed policy definition because AWS can update managed policies.

---

# 3. AmazonS3ReadOnlyAccess

Provides read-only access to S3 resources covered by the policy.

### Example Actions

* List buckets
* View objects
* Download/read objects

The user cannot normally perform write/delete actions covered by the policy.

---

# 4. AmazonS3FullAccess

Provides broad S3 permissions covered by the policy.

### Example Actions

* Create/manage buckets
* Upload objects
* Download objects
* Delete objects
* Manage S3 resources covered by the policy

---

# 👑 Root User vs IAM Administrator

An AWS account has a **Root User**, which is the original account identity.

An IAM Administrator is an IAM identity that has administrator-level permissions through IAM policies.

## Comparison

| Root User                                  | IAM Administrator                      |
| ------------------------------------------ | -------------------------------------- |
| Original AWS account identity              | IAM identity                           |
| Has unrestricted account access            | Has permissions granted through IAM    |
| Should not be used for daily operations    | Suitable for regular administration    |
| Used for specific root-level account tasks | Used for managing AWS resources        |
| MFA should be enabled                      | MFA should be enabled                  |
| Credentials must be strongly protected     | Credentials must be strongly protected |

### Best Practice

> ❌ Do not use the AWS Root User for everyday AWS operations.

```text
AWS Account
     │
     ├── Root User
     │
     └── IAM Users / Roles
              ↓
          Permissions
```

---

# 🛡️ MFA

## What is MFA?

**MFA = Multi-Factor Authentication**

MFA adds an additional authentication factor to protect an AWS identity.

Instead of using only:

```text
Password
```

Authentication can require:

```text
Password
   +
MFA
   ↓
Access
```

---

# 🎯 Why MFA is Important?

MFA provides an additional security layer if a password is compromised.

### Example

If someone gets your password:

```text
Password ❌
     ↓
MFA Required
     ↓
Unauthorized Person Blocked
```

### Recommended

Enable MFA especially for:

* Root User
* Privileged IAM users
* Administrative identities

---

# 📝 Inline Policy

An Inline Policy is directly embedded into a specific IAM User, Group or Role.

### Example

```text
IAM User
   │
   └── Inline Policy
          │
          └── S3 Read Permission
```

The policy is directly associated with that identity.

### Use Case

Suppose one specific application user needs a special permission that should not be reused by other identities.

An inline policy can be used for that specific requirement.

---

# 🔄 Policy vs Role

Understanding the difference between **Policy** and **Role** is very important for AWS interviews.

| Policy                                    | Role                                                              |
| ----------------------------------------- | ----------------------------------------------------------------- |
| Defines permissions                       | Can be assumed by a trusted principal                             |
| Specifies Allow/Deny actions              | Provides temporary credentials when assumed                       |
| Written as a JSON policy document         | Contains permissions policies and a trust policy                  |
| Can be attached to users, groups or roles | Can be assumed by users, AWS services or other trusted principals |
| Answers: "What can this identity do?"     | Answers: "Who can assume this identity?"                          |

---

# 🧩 Real-World Example: EC2 Accessing S3

Suppose an EC2 application needs to read files from an S3 bucket.

### Recommended Architecture

```text
             EC2 Instance
                  │
                  ▼
              IAM Role
                  │
                  ▼
             IAM Policy
                  │
                  ▼
            Amazon S3
                  │
                  ▼
             Read Objects
```

### Why use a Role?

Instead of storing long-term AWS access keys on the EC2 server, the EC2 instance can assume an IAM role and obtain temporary credentials.

---

# 🔐 IAM Security Best Practices

### 1. Use Least Privilege

Give only the permissions required.

### 2. Enable MFA

Especially for privileged identities and the root user.

### 3. Avoid Root User for Daily Tasks

Use appropriate IAM identities instead.

### 4. Use IAM Roles for AWS Services

For example:

```text
EC2 → IAM Role → S3
```

### 5. Regularly Review Permissions

Remove permissions that are no longer required.

### 6. Avoid Unnecessary Long-Term Access Keys

Use temporary credentials and IAM roles where possible.

---

# 🎤 IAM Interview Questions

## Q1. What is AWS IAM?

AWS IAM is a service used to manage identities and control access to AWS resources.

---

## Q2. What are the types of IAM policies?

The three important types are:

1. AWS Managed Policies
2. Customer Managed Policies
3. Inline Policies

---

## Q3. What is the difference between AWS Managed and Customer Managed Policies?

AWS Managed Policies are created and maintained by AWS, while Customer Managed Policies are created and managed by the customer.

---

## Q4. What is an Inline Policy?

An Inline Policy is directly embedded into a specific IAM user, group or role.

---

## Q5. What is the difference between an IAM Policy and IAM Role?

A policy defines permissions, while a role is an identity that can be assumed by a trusted principal and provides temporary credentials.

---

# 🔁 IAM Quick Revision

```text
                    AWS IAM
                       │
        ┌──────────────┼──────────────┐
        │              │              │
     Policies        Users          Roles
        │
   ┌────┼────┐
   │    │    │
 AWS  Customer Inline
Managed 
```

---

# 📋 IAM Revision Table

| No. | Topic                   | Key Point                           |
| --: | ----------------------- | ----------------------------------- |
|   1 | IAM                     | Identity & Access Management        |
|   2 | AWS Managed Policy      | Managed by AWS                      |
|   3 | Customer Managed Policy | Managed by Customer                 |
|   4 | Inline Policy           | Embedded directly into identity     |
|   5 | EC2 Read Only           | Read-only EC2 access                |
|   6 | EC2 Full Access         | Broad EC2 access                    |
|   7 | S3 Read Only            | Read-only S3 access                 |
|   8 | S3 Full Access          | Broad S3 access                     |
|   9 | Root User               | Original AWS account identity       |
|  10 | IAM Administrator       | IAM identity with admin permissions |
|  11 | MFA                     | Additional authentication factor    |
|  12 | Policy                  | Defines permissions                 |
|  13 | Role                    | Assumable identity                  |
|  14 | Least Privilege         | Give only required permissions      |
|  15 | EC2 + Role              | Secure AWS service access           |

---

# 🏆 Key Takeaways

* 🔐 **IAM = Identity and Access Management**
* 📜 **Policy = Defines Permissions**
* ☁️ **AWS Managed Policy = Managed by AWS**
* 👤 **Customer Managed Policy = Managed by Customer**
* 📝 **Inline Policy = Directly embedded policy**
* 👑 **Root User = Original AWS account identity**
* 👨‍💻 **IAM Administrator = IAM identity with administrator permissions**
* 🛡️ **MFA = Additional Authentication Layer**
* 🔄 **Role = Assumable identity**
* 🔒 **Least Privilege = Minimum Required Permissions**
* ☁️ **EC2 Role → S3 = Common real-world pattern**

---

## 👨‍💻 Author

**Prince Vaghasiya**

### AWS IAM Learning & Hands-on Practice

```
```
