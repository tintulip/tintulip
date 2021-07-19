# Trusted Pipeline

A trusted pipeline reduces a blast radius as it involves having a secondary pipeline in a highly secure environment. Therefore, when a malicious user makes changes to the pipeline code in the `CI` and pushes those changes up, it will go through secondary checks such as `Open Policy Agent` (OPA) and manual approvals by trusted users in `AWS CodePipeline` .


## Integrating CodePipeline with GitHub

CodePipeline can be integrated with Github actions, an IAM user that assumes a role which is able to trigger the trusted pipeline on  `AWS CodePipeline`. An alternative way is via a `webhook`, this initiates code pipeline when a change is made to a github repository.


## Policy-as-code

Policies-as-code are rules to be applied in the trusted pipeline to detect misconfigurations within Terraform files. These set of rules govern the behaviour of a software service and allows decoupling between the policies and the software service which allows the policy to be updated independently from the software and build them at scale. The rules are written in [rego](https://www.openpolicyagent.org/docs/latest/policy-language/) and use OPA (open policy agent) for the policy engine.

### Conftest

[conftest](https://www.conftest.dev/) is a utility that helps in writing tests against structured configuration data and relies upon the Rego language from OPA for policies.

### Semgrep

[semgrep](https://semgrep.dev/) is a lightweight static analysis tool to help find bugs and enforce coding standards. Rules can either be sourced from the [rules registry](https://semgrep.dev/explore) or be [custom-created](https://semgrep.dev/docs/writing-rules/overview/). These rules can be ran against several supported languages such as Java, Go and Python as well as use pattern matching against generic languages such as Terraform. As semgrep uses pattern matching, there is a lot of flexibility in tuning rules and the rules themselves are written have a YAML syntax.

### tfsec

[tfsec](https://tfsec.dev/) is a static analysis security scanner specifically for Terraform. In general the checks that are performed by tfsec are good and it should definitely run in the pipeline. However, the checks can be [ignored by modifying the source code](https://github.com/aquasecurity/tfsec#ignoring-warnings). There are some cases where a rule is flagged but needs to be ignored as there is a valid use case. [Custom rules](https://tfsec.dev/docs/custom-checks/) can also be written for tfsec, however it is based on the attributes within terraform blocks of code and can be useful in some cases.

### Additional Source

In the trusted pipeline, the [policy-as-code source code](https://github.com/tintulip/policies-as-code) is added as an additional source. It can be changed independently to the infra-code and the pipeline can execute a common set of tests from the policy-as-code repo against any infrastructure repository.