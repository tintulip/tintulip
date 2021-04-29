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

![scenario 0 architecture diagram](https://github.com/documentation/documentation/raw/main/scenarios/scenario-0/CLA-scenario-0-target.png "scenario 0 architecture diagram")

## Threat model on first proposal, 20/04/2021

Workshop run on 20/04/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1618910618512/25bc1a6f0ed2f1cc6df5272f154ea1a2fce9036a)

![scenario 0 threat modelling](https://github.com/documentation/documentation/raw/main/scenarios/scenario-0/CLA-scenario-0-threat-modelling.png "threat model of scenario 0")

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

![updated scenario 0 threat modelling](https://github.com/documentation/documentation/raw/main/scenarios/scenario-0/CLA-scenario-0-updated-threat-modelling.png "threat model of updated scenario 0")

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
