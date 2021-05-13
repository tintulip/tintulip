# Using Github Actions to set up CI/CD

## Prerequisites

In order to set up the CI/CD pipeline there was `four components` that was needed:

1. S3 Bucket for storing terraform state.
2. IAM user with programmatic access.
3. IAM role for the IAM user to assume.
4. Defining Github Secrets with IAM user access and secret Keys.

Initially these components were created manually via the amazon console, then used [terraformer](https://github.com/GoogleCloudPlatform/terraformer) to convert it to Infrastructure-as-Code(IaC). This is stored in the [bootstrap-pipeline](https://github.com/tintulip/bootstrap-pipeline) repository. Using temporary admin credentials we ran the terraform code on our laptop to make and apply changes.

## Defining the pipeline
To ensure that only members of team dali are able to trigger the pipeline, in the settings of the repository in the `Branches` section there is `Restrict who can push to matching branches` and editied to allow specific members to push.

When a commit is pushed to the main branch the pipeline will run a series of steps. The access and secret keys are pulled from `GitHub secrets`and placed within environment variables.

The selected actions match certain specified criteria which are defined in the settings, which are actions created by GitHub and actions in the marketplace created by verified creators.The action permissions are set under the actions section in the specified repository settings.

## tfsec
In order to use tfsec the pipeline definition had to be modified. This was done by including the tfsec action with the specified version.Both errors and warnings break the pipeline and exceptions can be made within the code. As the action is currently not verified we had to set the organisations settings to allow all actions. Overall, using tfsec produced meaningful reports therefore is good to include within the pipeline.

## checkov
Checkov is a similar tool to tfsec. It requires the path of the directory containing the terraform files. It checks against multiple policies and some are similar to tfsec. Also allows to customise policies to be enforced. Checkov also allows the exclusion of policies by suppressing a check. This is done by putting a comment within the specific line of code that the issue was raised. The comment contains a `check id`, this is the specifc check that the code is violating and a `suppression comment`. Checkov applies different checks than tfsec and therefore, it is useful using tfsec alongside checkov.

## Manual Approval
In order to enable required approvers to review deployments, GitHub requires the repository to either be public or to have a GitHub enterprise license. This would have allowed a set of people to review pending jobs and either approve or reject a deployment.

## Pipeline test for principle of least privilege
There were various tools that inspected IAM policies and ensured that it had least privilige permissions.

### IAM parliment
[IAM parliment](https://github.com/duo-labs/parliament) is a linting library that reviews policies and looks for problems with those policies. There are several tools implemented using this library. However, `IAM parliment` only checks for issues using JSON syntax and not IAM policies using the `hcl` syntax.

### IAM policy Simulator
The [IAM policy simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) is a tool that allows you to test if a user has the permissions to do an action associated with a particular service. This makes it easier to check if a user has the correct permissions however, as it is only accessible in the `IAM management console` can not use it as test in the pipeline.

### Tf-parliment
[Tf parliment](https://github.com/rdkls/tf-parliament) implements `IAM parliment` but runs against terraform files. It checks the `IAM policies` validity against the latest `AWS IAM specifications`. However, it is currently immature and several issues exist that means it is currently not fit to be used within a pipeline. One of the issues that exists involves a false positive when referring to objects within a bucket. As this tool becomes more developed, it could be integrated within the pipeline.

### Policy sentry
[Policy sentry](https://policy-sentry.readthedocs.io/en/latest/) generates least privilege IAM policies according to resources and access levels. This tool generates the recommended actions and policies required depending on the use case instead of trying to hand craft it. However, this tool can not be used as a pipeline test but it is helpful to allow users to create least privilige IAM polices which can be stored as policy as code.

## Future Considerations
Access and Secrets keys are static and need to be rotated every 90 days.