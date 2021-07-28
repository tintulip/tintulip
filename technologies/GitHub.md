# GitHub Organization

These are security features that have been set throughout a GitHub organization to help avoid security incidents. There is an [article by GitHub](https://docs.github.com/en/organizations/keeping-your-organization-secure) that helps to explain how to keep the organization secure.

## Enforcing MFA

[two-factor authentication](https://github.com/organizations/tintulip/settings/security) can be enabled by checking the checkbox under `organization security` section though anyone who does not have it already enabled will be removed.

## Dependabot

[dependabot](https://github.com/organizations/tintulip/settings/security_analysis) can be enabled through the `security & analysis` section for new repositories by checking the checkbox. For existing repositories, clicking on the 'enable all' button will enable this feature. Private repositories within an organization need to explicitly grant Dependabot access.

## Branch Protection

Branch protection is done on a repository-by-repository basis. For each repository, it requires creating a branch protection rule (found within the `branches` section under settings). The protection rules currently select `require signed commits` and `include administrators`. To prevent force pushes, the `allow force pushes` checkbox is unselected.

Branch protection is automatically set for all repository when the [github stack](https://github.com/tintulip/cla-organisation/tree/main/github) is ran. It will query all repositories and update branch protection for the `main` branch.

## Audit Log

The [audit log](https://docs.github.com/en/organizations/keeping-your-organization-secure/reviewing-the-audit-log-for-your-organization) within GitHub allows admins to review the actions performed by members of an organization.

## Secret Scanning

GitHub scans repositories for known types of secrets (e.g. AWS Access/Secret Keys) to prevent fraudulent use of secrets that were committed accidentally. It is enabled automatically for public repositories, however for a private repository within an organization, an [advanced security license](https://docs.github.com/en/github/getting-started-with-github/about-github-advanced-security) is required.

## Semmle

[Semmle](https://semmle.com) is a code analysis platform to help find zero-day vulnerabilities and automate variant analysis. [LGTM](https://lgtm.com/) helps development teams to identify vulnerabilities early by enabling automatic code reviews. The service is free for open-source, but requires commercial products for private repositories.

## Actions restrictions

Which actions are allowed in pipelines can introduce risks in the supply chain.
GH offers a few options out of the box to restrict which actions are allowed to run. The option we are after is "Allow select actions" that allows to restrict to:
- actions defined in public repositories in the TinTulip organisation
  - private repositories not supported at this time, roadmap item: https://github.com/github/roadmap/issues/74
- actions published by GH
- actions published by verified creators

We make this choice under the assumption that GitHub applies some form of due diligence on actions published, and requires verified creators to demonstrate due diligence as well (although the recent incident from Codecov.io, which is a verified creator, introduces significant doubt on this).

The "Allow select actions" option also allows to specify a safelist of actions, but with our subscription plan it only applies to public repositories.

## Creating a new repository

These are the steps followed to create a new repository:

- Branch protection rule `Require signed commits` and `Include administrators` on `main` branch.
- Allow write access and alerts to the team
