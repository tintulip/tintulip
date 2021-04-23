# 1. Using Control Tower

Date: 2021-04-23

## Status

Proposed.

## Context
AWS Control Tower provides an easy way to set up multiple accounts structure within organisations using well architectured practices. 

## Decision

Go through the process of setting up Control Tower, using an admin account within the root account.

## Consequences

* Easier to create multiple accounts.
* However, less visibilty with what AWS control Tower manages.
* In order to enroll account requires to set up the root email of that account and SSO user which need to be different people.

