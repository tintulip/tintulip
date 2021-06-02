# Trusted Pipeline

A trusted pipeline reduces a blast radius as it involves having a secondary pipeline in a highly secure environment. Therefore, when a malicious user makes changes to the pipeline code in the `CI` and pushes those changes up, it will go through secondary checks such as `Open Policy Agent` (OPA) and manual approvals by trusted users in `AWS CodePipeline` .


## Integrating CodePipeline with GitHub

CodePipeline can be integrated with Github actions, an IAM user that assumes a role which is able to trigger the trusted pipeline on  `AWS CodePipeline`. An alternative way is via a `webhook`, this initiates code pipeline when a change is made to a github repository. 


 