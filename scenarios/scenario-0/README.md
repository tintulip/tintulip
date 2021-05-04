# Scenario 0 - CLA's licensing guidance website

## Context

As we get TinTulip started, we agreed in initial kickoff meetings that we'll aim at creating the smallest possible experimental lab so that we can start trialling collaboration between Blue and Red teams early, and provide some scenario for the initial creation and setup of accounts in view of a more complex delivery.

## Backstory

Creative Licensing Agency is getting started planning their digital services.

They want to bootstrap an AWS account and some SaaS tooling to deliver digital services quickly, cheaply and securely.
They want to publish some guidance on licensing while they get the organisation ready to deliver more complex digital services. For now there are 2 teams allocated to work for the CLA:
- Team Dalí: a platform/infrastructure team
- Team Mozart: a web design team, with no AWS expertise

In this scenario, Team Dalí (Platform) will provide some infrastructure and pipeline access for Team Mozart (Web) to publish CLA's website.

![scenario 0 architecture diagram](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-0/CLA-scenario-0-target.png "scenario 0 architecture diagram")

## Threat model on first proposal, 20/04/2021

Workshop run on 20/04/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1618910618512/25bc1a6f0ed2f1cc6df5272f154ea1a2fce9036a)

![scenario 0 threat modelling](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-0/CLA-scenario-0-threat-modelling.png "threat model of scenario 0")

### Assumptions & areas not covered

- Team Dalí owns the GitHub organisation
- We assume the only teams at the moment are Dalí and Mozart, and no other access for e.g. security audit or pipeline approvals is in consideration for now
- There are no specified controls in pipelines and calling out good controls to add is in scope for this threat model
- We are actually planning to run Control Tower so the end result will have more basic controls in place that what represented in this scenario.

### Threats identified

with voting score in (brackets)

- Data operations on buckets are not captured by default, leading to tampering of statebucket being undetected (2)
- roles assumed are not least privilege and allow doing more than they should (2)
- Team Dalí can do anything to tamper with the website bucket (2)
- AWS credentials of administrators can be leaked/stolen (2)
- npm dependency with malicious code (1)
- Force push changes history of a repo (1)
- compromise of third party actions or tools in the pipeline (1)
- team Dalí can alter DNS to redirect to somehting malicious, or replace the website bucket with malicious content (1)
- using permissions of the lower-privilege pipeline to obtain access of the higher infrastructure pipeline (1)
- credentials in pipelines can be exposed (intentionally or not) by pushing code that access them (1)
- github administrators can access and leak secrets from the pipelines (1)
- Lack of logs, or logs not looked at makes it so admin misbehaviour is not caught (1)
- access logs (containing IP addresses!) are not kept confidential
- High volume of events is used to hide tracks in the event logs
- Data operations on buckets are not captured by default, leading to tampering of website being undetected
- teams can push a change that allow a TLS certificate to be used (verification token in - page)
- Team Dalí deleting logs (AWS)
- making a change in DNS to allow a TLS certificate to be used or CNAMEs to be created
- github administrators can override secrets with invalid ones, breaking pipelines

## Threat model on updated proposal with privileged access alternatives, 27/04/2021

Workshop run on 27/04/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1619449981497/2414a7100aa903bf465ccaf9409cf1c77f81dcc0)

![updated scenario 0 threat modelling](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-0/CLA-scenario-0-updated-threat-modelling.png "threat model of updated scenario 0")

This time we bring two alternatives for study:
- any privileged operation happens via protected device and in inherently manual (top diagram)
- semi-privileged operations happen via temporary credentials from engineer laptops, anything more privileged via protected device and manual instead (bottom diagram)

We opened the session introducing the differences in the two approaches and reasoning, then focused on the second option.

### Assumptions & areas not covered

As previous week, plus:

- Control Tower has now been run and is represented in the diagram
- 2 people in team Dalí have access to a "protected device", and have separate github/AWS identities that are to be used exclusively on the protected device
- the protected device only allows browser based interactions (modelling around a Rosa device)
- we are not considering artifact promotion or staging environments at the moment

### Highlights of approaches

#### privileged development tasks from protected device

Any cloud development operation that is required to initialise and restrict access to the website pipeline stage is done from a protected device.
This addresses threats raised last week around the suggested "high privilege infra pipeline", trading off fast feedback, automation, automated checks and reliability for resistance to external threats and key exposure.
With the current assumptions this implies changes are performed using the AWS Console.
Reproducing the same steps for e.g. introducing a new pipeline, or updating pipeline permissions to reflect changing requirements, is subject to human error and toil.

#### privileged development tasks from unprotected device with temporary credentials

Any cloud organisation configuration operation such as controlling Control Tower or creating new Accounts is done from a protected device.
Cloud development operations required to initialise and restrict access to the website pipeline stage is done from an unprotected device using temporary credentials obtained via SSO.
This improves on the "high privilege infra pipeline" by eliminating the long-lived credential that needs be stored in the pipeline. This trades off reliability and automated checks for limited resistance to key exposure.
This allows to maintain automation reliability on arbitrary cloud development tasks withing an Account, but aan infra engineer with a compromised device or evil intention would still be able to run arbitrary terraform as well as ignore any recommended pre-apply checks.

### Threats identified

We transposed most threats from the previous round, with their suggested controls noted nearby.

A set of threats we highlighted are around the fact that even with a protected device, we need to consider the infrastructure between that device and AWS/GitHub and what AWS/GitHub do to prevent it from happening.
We then focused on the fact that the "privileged development tasks" essentially allow executing arbitrary terraform even with limited-time credentials.
We also acknowledged that there is a set of "bootstrap" administrative and high-privileged steps involved in getting to a point where further "day to day" development can happen.

We discussed what area of focus we want from the scenario rather than threats, calling out:
- The assumptions we made around what a protected device can or cannot do are too close to Rosa and not widely applicable.
  - The potential creation of a secure VDI/Console environment to allow automation to be executed from a protected device is not resusable enough to be worth pursuing.
- How to make the bootstrap both automated and resistant to tampering (e.g. supply chain attacks) so that it can form a trust anchor is the second most interesting finding we can pursue.
- How to make day to day infrastructure changes secure, assuming a bootstrap has been done securely is the most inresting finding we can pursue.

## Threat model on pipeline design with privileged workloads in AWS, 04/05/2021

Workshop run on 04/05/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1620122393386/8160d3bb4a45c969fdae1bd5dcd23ff6a289a530)

![threat model of team Dalí's pipeline with privileged workloads on the AWS side](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-0/CLA-scenario-0-team-dali-pipeline-threat-modelling.png "threat model of team Dalí's pipeline with privileged workloads on the AWS side")

This time we focus on the pipelines and suggest a change of design mainly motivated by the threat of a compromised internal user.
From some reseach (see [this issue](https://github.com/actions/runner/issues/458)) it's evident that any GH Actions workflow can be tampered with by any legitimate committer to the repository. In our threat model a legitimate committer can be compromised so GH Actions by itself is not tamper resistant.
The design we suggest takes inspiration from [this article from Gruntwork.io](https://gruntwork.io/guides/automations/how-to-configure-a-production-grade-ci-cd-setup-for-apps-and-infrastructure-code/), extending their threat model to match ours. Some highlights:

- GitHub Actions is now considered a lower security side than AWS and does not have permission to perform write operations into AWS.
- We introduce a loosely defined "Applier workload" that resides on the AWS side and enforces trustable policy controls and applied privileged write operations
- We maintain, for the sake of learning exercise, that the GH Actions pipeline maintains read access to AWS to enable better development experience to team Dalí

### Assumptions & areas not covered

- We intentionally removed from the diagram anything related on how to initialise this structure, so that the focus is on the ongoing operation of this system rather than its initialisation
- The ReadOnly role given to the pipeline needs more permissions that the AWS-managed ReadOnly policy - it also needs to read bucket contents to perform a `terraform plan`!
- About the "Applier workload":
  - is isolated in its own VPC with egress to GH and AWS APIs only.
  - its logic is not influenced by the GH repo it applies
  - exposes some form of endpoint to process a git commit - this is restricted to a specific, preconfigured repository.
  - runs on ephemeral compute (e.g. ECS Fargate or lambdas or CodePipeline - not EC2)
  - can apply policy-based decisions on the outcome of a `terraform plan` or other tools (preconfigured)
  - has a way to ask operators for approval - be that with email and a confirmation email or slack integrations.

### Threats identified and mitigations discussed

with voting score in (brackets)

- can turn versioning off and delete the bucket (3)
- Requesting a build can be spoofed / is there authentication to the worklload (3)
- can use exposed readonly keys to expose secrets from the TFstate (2)
- can use tf output to leak taskRole credentials (2)
- can find vulnerabilities in the AWS setup just by looking at the terraform (1)
- request deploy of previous TF configuration (1)
- request can trigger a vulnerability in the workload request endpoint if there is not enough validation (1)
- privilege escalation via terraform plan (1)
- approving own changes (1)
- can find vulnerabilities in the AWS setup by looking around the environment
- access logging on TF statebucket
- Triggering a lot of requests can overwhelm workload from picking up real  requests
- can use to expose secrets from the TFstate
- Triggering a lot of approvals can spam people and have approvals go in email blocklist

#### mitigations for the pipeline tampering with statebuckets

Mainly, introducing account isolation between the "Applier workload" and Production resources created is the best solution long-term as it scales well with the evolution of complexity of operations that the pipeline will perform.

- account boundary to separate "build" buckets from "built" buckets (and other infra resources)
- explicit deny on deletions and versioning disable
- multiple levels of deny - SCP, bucket policy
- attempts to delete buckets can be part of the rejection logic of pipeline

#### mitigations for spoofing requests to applier workload request endpoint

Introduce some form of authentication. We did not call extra assumptions on this so it's hard to make any further observation.