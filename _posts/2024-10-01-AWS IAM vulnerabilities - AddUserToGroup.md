
---
title: AWS:IAM vulnerabilities - AddUserToGroup
tags: 
categories: AWS
---

W3lc0m3 to this post focusing in AWS:IAM vulnerabilities. For these tests, i will be using the [IAM Vulnerable Lab](https://github.com/BishopFox/iam-vulnerable) created by Bishop Fox.

This lab provides common IAM misconfigurations and privilege escalation risks in AWS environments.

In this post, we’ll dive into the **iam:AddUserToGroup** action, how it can be exploited, and how to keep safe.

![3ceddd5c84e686c8273936d24b1ed03d.png](/assets/img/screenshots/aws1/3ceddd5c84e686c8273936d24b1ed03d.png)

# iam:AddUserToGroup Policy

The **iam:AddUserToGroup** policy allows users to add other users to an IAM group. This can lead to a user with low privileges assign itself to a group with higher privileges. For example, a user who can execute iam:AddUserToGroup on an administrative group could potentially gain full access to AWS resources.

## How to mitigate?

The key to mitigating this is to limit who can execute this action, apply strict conditions, and monitor IAM activity closely.

An example policy:

- The policy grants permission to the AdminGroup to use the iam:AddUserToGroup action.
- The policy explicitly denies everyone else from using this action.
 \
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:AddUserToGroup",
      "Resource": [
        "arn:aws:iam::account-id:group/AdminGroup"
      ]
    },
    {
      "Effect": "Deny",
      "Action": "iam:AddUserToGroup",
      "Resource": "*"
    }
  ]
}

```

## Automated recon

With [my automated tool](https://github.com/jvs3c/iam_enumerator) you can easily search for and exploit this permission in a single step, saving you time and avoiding manual checks:

![98d94749b59e7f0ec9121f935ed619c4.png](/assets/img/screenshots/aws1/98d94749b59e7f0ec9121f935ed619c4.png)

## Manual Recon

However, if we want to do things manually, let’s go over the process step-by-step using the AWS CLI.

### Step 1: Enumerate Users

First, let’s list all the IAM users in our AWS account. This will give us an overview of all users, and from there, we can pinpoint the specific user we want to manage:

`aws iam list-users`

Once we have our list of users, we can get detailed information about any specific user by running the following command:

`aws iam get-user --user-name <username>`

![9f4ad2a6357bdf57c44472c2ebb97a5f.png](/assets/img/screenshots/aws1/9f4ad2a6357bdf57c44472c2ebb97a5f.png)

### Step 2: Checking User Policies

We’ll want to check both types of policies associated with an IAM user: inline policies and attached policies

**Inline Policies**: These policies are directly embedded in the user. We can list them using:

`aws iam list-user-policies --user-name`

**Attached Policies**: These are standalone policies attached to the user. We can list them using:

`aws iam list-attached-user-policies --user-name <username>`

In this specific scenario we can only find Attached policies, no inline policies set for this user

![f50e69c7f95615b46228ccb9519ca222.png](/assets/img/screenshots/aws1/f50e69c7f95615b46228ccb9519ca222.png)

To view the details of an attached IAM policy, we first need to retrieve the default version ID of the policy by running

`aws iam get-policy --policy-arn <policy-arn>`

![daaa6705872caf35b8d4a7eaca3f81c1.png](/assets/img/screenshots/aws1/daaa6705872caf35b8d4a7eaca3f81c1.png)

The previous command will return metadata, including the `"DefaultVersionId"`. Now lets use this version ID to fetch the full policy document

`aws iam get-policy-version --policy-arn <policy-arn> --version-id <version-id>`

![4633066d924077da15babbe187d4e9e6.png](/assets/img/screenshots/aws1/4633066d924077da15babbe187d4e9e6.png)

After reviewing the policies, we proceed to list and modify group memberships because the iam:AddUserToGro up action is a potential privilege escalation risk.

### Step 3: Escalate privileges

To identify any potentially privileged groups, we list all the groups in the account

`aws iam list-groups`

![57452946d160f39969b6d28a7de170aa.png](/assets/img/screenshots/aws1/57452946d160f39969b6d28a7de170aa.png)

In this scenario, we find a group named `privesc-sre-group`, which could have administrative or elevated privileges based on its name.

To demonstrate the vulnerability, let’s add the user privesc13-AddUserToGroup-user to the privesc-sre-group:

`aws iam add-user-to-group --group-name <group_name> --user-name <user_name>`

![849cc53324d720c216bff5eb3bf88c95.png](/assets/img/screenshots/aws1/849cc53324d720c216bff5eb3bf88c95.png)

After adding the user to the group, we need to verify that the group membership has been successfully applied and that the user has inherited the group’s privileges. To check this, we can list the groups the user is now part of

`aws iam list-groups-for-user --user-name privesc13-AddUserToGroup-user`

![0f2dbae1960addfa4b37c072ee7f8802.png](/assets/img/screenshots/aws1/0f2dbae1960addfa4b37c072ee7f8802.png)

This command will display all groups the user belongs to, including the newly added `privesc-sre-group`

<span style="color: #242424;">Th4nkY0u f0r r34d1ng! :D</span>

&nbsp;

&nbsp;