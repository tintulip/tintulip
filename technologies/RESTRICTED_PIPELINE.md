# Configuring less privileged pipeline

To ensure that the pipeline was less priviliged, a less privileged role was assumed. This role is allowed to do limited `S3 bucket` operations on a specified bucket. The team (`Mozart`) that pushes code to the repository has read only permissions on `AWS` and does not have the ability to change the infrastructure. Team `Dali` on the otherhand has set up the bucket and the role for team `Mozart`. They have placed the access and secret key within the repository secrets.

## Sharing of Access and Secret Key securely
The access and secret key was created programtically (`using terraform`) and the encrypted secret key was outputted in the `infra-pipeline`. A  `pgp key` (belonging to a member of `team Dali`) was used to decrypt the `secret key` and this was manually placed within the github repository secrets by a memeber of team `Dali` (`platform team`).

## Role permissions
The policy attached to the assumed role only has limited permissions (read, update and delete objects) applied to a specific `S3 bucket`. When using [configure AWS credentials](https://github.com/aws-actions/configure-aws-credentials) GitHub action, there are two actions required in the trust policy (`sts:AssumeRole`, `sts:TagSession`) which allows a user to assume that role. 

## Dependabot 
Dependabot was [configured as code](https://docs.github.com/en/code-security/supply-chain-security/configuration-options-for-dependency-updates) using a `YAML` file within the `.github` directory. This monitors a project dependencies such as `npm` or `github actions` and issues a pull request when a new version of a dependency is detected. This is also useful for detecting when there are security updates for vulnerable dependencies. Dependabot also alerts reviewers when there is a dependency update. 

## Less privilege team (Team Mozart)
Members of team Mozart have write access to the repository but must continue to sign commits and are not able to force push to the main branch. They are also referenced as reviewers for any pull requests and any approvals that are required. However, team Mozart are able to modify the pipeline definition despite not knowing the secrets to defined in the repository.  