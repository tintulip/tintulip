# Trusted Pipeline

A trusted pipeline reduces a blast radius as it involves having a secondary pipeline in a highly secure environment. Therefore, when a malicious user makes changes to the pipeline code in the `CI` and pushes those changes up, it will go through secondary checks such as `Open Policy Agent` (OPA) and manual approvals by trusted users in `AWS CodePipeline` .


## Integrating CodePipeline with GitHub

CodePipeline can be integrated with Github actions, an IAM user that assumes a role which is able to trigger the trusted pipeline on  `AWS CodePipeline`. An alternative way is via a `webhook`, this initiates code pipeline when a change is made to a github repository.


 ## Policy-as-code

Policies-as-code are rules to be applied in the trusted pipeline to detect misconfigurations within Terraform files. These set of rules govern the behaviour of a software service and allows decoupling between the policies and the software service which allows the policy to be updated independently from the software and build them at scale. The rules are written in [rego](https://www.openpolicyagent.org/docs/latest/policy-language/) and use OPA (open policy agent) for the policy engine.

 ### Conftest

 [conftest](https://www.conftest.dev/) is a utility that helps in writing tests against structured configuration data and relies upon the Rego language from OPA for policies.

 ### Additional Source

 In the trusted pipeline, the [policy-as-code source code](https://github.com/tintulip/policies-as-code) is added as an additional source. It can be changed independently to the infra-code and the pipeline can execute a common set of tests from the policy-as-code repo against any infrastructure repository.