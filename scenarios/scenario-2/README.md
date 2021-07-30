# Scenario 2 - Tampering with CLA's webapp code

## Context

On top of the environment built for Scenario 1, Scenario 2 asks the question:

> Assume some bad code gets through the pipeline and into the web-application. What is the blast radius?

## Threat model on detail of pipelines, 29/06/2021

Workshop run on 29/06/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1624958214729/9fc9ac5d43574de15c95264a27bace55859852cb)

In this session we spent less time brainstorming threats, which we mainly discussed previously, and decided to focus the conversation on controls instead.

![Pipeline zoom in threat modelling](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-2/threat-modelling-29-06-2021-networking-in-builder.png "Pipeline zoom in threat modelling")

### Main controls discussed

- Reproducible builds can detect and prevent artifact tampering.
  - this can be at a source code level - does not necessarily need to be at a binary artifact level
- Layer 7 network controls for Builder sound good but even better to have an artifact repository as a funnel between Builder and the internet.
  - This allows to act as a point for dependency review and compliance auditing, and once a "good" artifact is stored it prevents future compromise to propagate.
  - Still TOFU (Trust On First Use) - so integrating some form of tool or process to review remains important
- Usage of preproduction environments as sandboxes to detect anomalies
  - It is normal to run some form of smoke or journey tests in preproduction environment to exercise the application code and validate that it works as expected.
    Connecting this to security testing, the intuition is that exercising application code would also potentially exercise malicious functionality in it.
    Applying detective controls to detect such behaviour and prevent further artifact promotion if anything is detected can be a way to preventing propagation of malware.
  - Some malware remains dormant for some time - hard to catch with this!
  - Similarly, synthetic testing against production and production can help detecting issues quickly
- SCPs remain an excellent way to prevent data exfiltration based on cross-account usage of AWS services

## Threat model on code commits, 19/07/2021

Workshop run on 29/06/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1626707907874/d2d6ec91d2a3b1e85d0460dc78e2ba52881bbaf6?sender=fmeden1147)

The conversation was focused on threats and controls around code checkins and the review process. Some highlight of the conversation are that we concluded good engineering practices (small and frequent changes, small simple components, readable code) are believed to have a positive impact on code review and threfore on security. For a code review to be effective, we believe developers need to know how to revire specifically for security and have an awareness of attacking techniques used.
On the technical controls side, good PAM for GitHub can make spoofing identities and bypassing code reviews significantly harder. Administrators with the ability of disabling branch protections remain a risk.

### Main threats and controls discussed

#### Threats
- platform engineer with enough privilege can disable branch protection
  - implies that GH admins are as powerful as AWS admin
- developer merges without checking
- PR triggers some actions and exfils any secrets available in the environemnt
- PR triggers some actions running e.g. testing, linters or other authenticated tooling and leverages this context for escalation
- attacker bypasses code review process
- timing on PR? update to PR
- attacker spoofs code review
- branch protection disabled by accident
- Use of a 'process bypass' which exists for hotfixes
- branch protection not enabled on a new repo
- attacker commits obfuscated malicious code - tricks other reviewers into approving by leveraging authority/ experience
- Multi-commit PR with malicious change buried eg in a 'revert' commit
- Developer-to-developer attack via malicious PR
- Homoglyph attack to include malicious dependency
- developers refuse to do PRs as its "not CI"

#### Controls

- control: branch protection, PRs
- Commit signing with GPG
- Branch protection in GH
- tooling to spot sneaky stuff - revert commits - make a warning
- MFA of Github
- Well defined standard around configuration repos
- Good PAM for Github - SSO with AWS?
- Good engineering
- sandboxing
- Hardening endpoints
- review commits for security before running
- Small well understood components
- Guideline of how to spot malicous changes
- Awareness of what attackers are trying to
- Training for developers
- introducing SQL injection
- Deliberately picking more readable tools (but only for simple things)
- Small commits

