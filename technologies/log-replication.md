# Cross account Log Replication

Control Tower creates a log archive account for central logging when configuring a landing zone. In order to have all the logs replicated from other accounts into `log-archive`, we followed this [guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-config-for-kms-objects.html#replication-kms-cross-acct-scenario). The following resources have been created: 

- `S3 bucket` for application logs in the `Log-archive` account.
- `IAM` role with the permission to replicate objects from the workload account.
- `KMS key` for object encryption during replication with the required permission for the source and destination account. 

## Log Encryption
Using the default `AWS Kms key` for encryption resulted in cross account replication errors. This was because we could not grant permission to the role access to the `kms key`, more information provided [here]("https://www.reddit.com/r/aws/comments/cgw8ob/need_help_configuring_kms_key_policy_to_work_with/"). Therefore, we issued our own `kms` key for server side encryption on the destination bucket. 

