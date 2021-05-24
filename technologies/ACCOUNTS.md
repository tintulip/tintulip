# Account Structure

As part of scenario 1, two new accounts have been added to the default [accounts](https://docs.aws.amazon.com/controltower/latest/userguide/how-control-tower-works.html) in our case log-archive, audit, production and root accounts.

## Builder Account

Builder account is owned by the governance team, and is a highly secure environment for deploying infrastructure into production environment. No other team should have admin access to the account, but all can still view the account.

## Sandbox Account

Sandbox account is owned by team Dali, and is a test environment for platform teams to test infrastructure changes before they are released to production. Resources withing this environment are expect to be regularly destroyed.