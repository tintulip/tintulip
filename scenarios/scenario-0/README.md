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

![scenario 0 architecture diagram](https://github.com/tintulip/documentation/raw/main/scenarios/scenario-0/CLA-scenario-0-target.png "scenario 0 architecture diagram")

## Threat model

Workshop run on 20/04/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1618910618512/25bc1a6f0ed2f1cc6df5272f154ea1a2fce9036a)

![scenario 0 threat modelling](https://github.com/tintulip/documentation/raw/main/scenarios/scenario-0/CLA-scenario-0-threat-modelling.png "threat model of scenario 0")

### Assumptions & areas not covered

- Team Dalí owns the GitHub organisation
- We assume the only teams at the moment are Dalí and Mozart, and no other access for e.g. security audit or pipeline approvals is in consideration for now
- There are no specified controls in pipelines and calling out good controls to add is in scope for this threat model
- We are actually planning to run Control Tower so the end result will have more basic controls in place that what represented in this scenario.

### Threats identified

with voting score in (brackets)

- Data operations on buckets are not captured by default, leading to tampering of statebucket being undetected (2)
- roles assumed are not least privilege and allow doing more than they should (2)
- Team Dali can do anything to tamper with the website bucket (2)
- AWS credentials of administrators can be leaked/stolen (2)
- npm dependency with malicious code (1)
- Force push changes history of a repo (1)
- compromise of third party actions or tools in the pipeline (1)
- team Dali can alter DNS to redirect to somehting malicious, or replace the website bucket with malicious content (1)
- using permissions of the lower-privilege pipeline to obtain access of the higher infrastructure pipeline (1)
- credentials in pipelines can be exposed (intentionally or not) by pushing code that access them (1)
- github administrators can access and leak secrets from the pipelines (1)
- Lack of logs, or logs not looked at makes it so admin misbehaviour is not caught (1)
- access logs (containing IP addresses!) are not kept confidential
- High volume of events is used to hide tracks in the event logs
- Data operations on buckets are not captured by default, leading to tampering of website being undetected
- teams can push a change that allow a TLS certificate to be used (verification token in - page)
- Team Dali deleting logs (AWS)
- making a change in DNS to allow a TLS certificate to be used or CNAMEs to be created
- github administrators can override secrets with invalid ones, breaking pipelines
