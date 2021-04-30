# Using Github Actions to set up CI/CD 

## Prerequisites

In order to set up the CI/CD pipeline there was `four components` that was needed:
    
1. S3 Bucket for storing terraform state.
2. IAM user with programmatic access.
3. IAM role for the IAM user to assume.
4. Defining Github Secrets with IAM user access and secret Keys.

These were created manually via the Amazon console. 

## Defining the pipeline
To ensure that only members of team dali are able to trigger the pipeline, in the settings of the repository in the `Branches` section there is `Restrict who can push to matching branches` and editied to allow specific memebers to push.

When a commit is pushed to the main branch the pipeline will run a series of steps. The access and secret keys are pulled from `GitHub secrets`and placed within environment variables. 

The selected actions match certain specified criteria which are defined in the settings, which are actions created by GitHub and actions in the marketplace created by verified creators.The action permissions are set under the actions section in the specified repository settings.    

## tfsec
In order to use tfsec the pipeline definition had to be modified. This was done by including the tfsec action with the specified version.Both errors and warnings break the pipeline and exceptions can be made within the code. As the action is currently not verified we had to set the organisations settings to allow all actions. Overall, using tfsec produced meaningful reports therefore is good to include within the pipeline.

## checkov
Checkov is a similar tool to tfsec. It requires the path of the directory containing the terraform files. It checks against multiple policies and some are similar to tfsec. Also allows to customise policies to be enforced. Checkov also allows the exclusion of policies by suppressing a check. This is done by putting a comment within the specific line of code that the issue was raised. The comment contains a `check id`, this is the specifc check that the code is violating and a `suppression comment`. Checkov applies different checks than tfsec and therefore, it is useful using tfsec alongside checkov.

## Manual Approval
In order to enable required approvers to review deployments, GitHub requires the repository to either be public or to have a GitHub enterprise license. This would have allowed a set of people to review pending jobs and either approve or reject a deployment.

## Future Considerations
Access and Secrets keys are static and need to be rotated every 90 days.