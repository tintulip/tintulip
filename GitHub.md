# GitHub Organisation

These are security features that have been set throughout a GitHub organisation to help avoid security incidents.

## Enforcing MFA

[two-factor authentication](https://github.com/organizations/tintulip/settings/security) can be enabled by checking the checkbox under `organization security` section though anyone who does not have it already enabled will be removed.

## Dependabot

[dependabot](https://github.com/organizations/tintulip/settings/security_analysis) can be enabled through the `security & analysis` section for new repositories by checking the checkbox. For existing repositories, clicking on the 'enable all' button will enable this feature. Private repositories within an organization need to explicitly grant Dependabot access.

## Branch Protection

Branch protection is done on a repository-by-repository basis. For each repository, it requires creating a branch protection rule (found within the `branches` section under settings). The protection rules currently select `require signed commits` and `include administrators`. To prevent force pushes, the `allow force pushes` checkbox is unselected.
