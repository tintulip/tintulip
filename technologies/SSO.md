# SSO
AWS Single Sign-On `SSO` provides users with access to all of their assigned accounts and application from the user portal.These accounts already have the permissions set as they are assigned to a particular group that has customised security requirements.  When Control Tower was enabled it configured SSO to manage access to the accounts Control Tower created.

## Why we used SSO
IAM users have access and secret keys that need to be rotated manually, whereas SSO users are issued temporary credentials each session, this reduces the risk of human error. Also, AWS SSO allows users to have access to multiple accounts in an easier way, than having multiple IAM users which makes the potential attack surface quite large.


## What we did to set up SSO
Using the IAM user we created during the inital bootstrap process, we added SSO users using the AWS SSO page from the AWS console. Due to control tower being already set up only root and IAM users with the correct permissions can add SSO users. Below are the steps we took:

1. Clicked the `add user` button
2. Assigned them to `predefined groups`

## Setting up MFA for SSO
Under the settings section we configured `MFA` to ensure that when an SSO user signs in they are prompted to use MFA.Each user can now add and manage their own MFA device and able to authenticate with either an app or security keys.

## Suggestions
No longer need to create bootstrap IAM users when initialising account creation instead create AWS SSO users and assign them to groups.