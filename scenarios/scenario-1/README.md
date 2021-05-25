# Scenario 1 - CLA's "apply for a creative license" service

## Context

Scenario 1 will expand the lab created in scenario 1 introducing the software delivery of a full-fledged service and introducing the database of "highly sensitive OFFICIAL" information that will constitute the main target for future attacks.

## Backstory

Creative Licensing Agency expanded the team and onboarded more people for their engineering organisation, and are ready to start delivering their licensing service.
They want to build and deliver a service that allows the public to provide their contact details to apply for a creative license.
Operations are still not ready to start so this service would allow for early application for a Creative License, but no processing will be performed yet.

With the new staff available, they can also split team Dalí (Platform) and introduce team `TBD` (Governance) team. While team Dalí's responsibility focuses on development of platform products, team `TBD` focuses on administration and audit of the infrastructure and setting guardrails for service delivery.

## Proposal

Scenario 1 is still in definition. The following is a proposal of how we envision it to look like.

[Mural board](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1620642892400/995e5203e0061b21d048d5796e1cb027afc92129) (requires access).

### Application technologies

We would like to use:
- Java + Spring Framework
- TypeScript + React
- Postgresql

We believe this choice of stack is common enough to give a good representation of real cases, and the Blue Team has familiarity with all of them.

### Deployment technologies

We would like to keep the "build own - use SaaS" tradeoff slider towards "use SaaS" and look at AWS-managed option for serivce deployment.
This means using RDS for database and choose one of the following application architectures:

#### Dockerised service on ECS

Java webapp serving its own frontend, running as a Docker container on ECS Fargate.

- common architectural choice
- optionally run backend and frontend as API + static frontend instead

#### Lambda architecture

Static frontend, the backend is implemented with AWS Lambda functions.

- can take away some attack surface from Docker and a long-running service
- increasingly popular architecture choice
- Blue Team has low confidence in building this

#### Kubernetes service

Java webapp service its own frontend, running as a Kubernetes service in EKS Fargate.

- main boundary between Platform and Application would move to the Kubernetes API instead of the AWS API, potentially worth exploring
- high build cost, the Kubernetes ecosystem adds a lot of complexity

## Threat model on trusted pipeline for scenario 1, 25/05/2021

Workshop run on 25/05/2021 on [this mural board (prior access required)](https://app.mural.co/t/thoughtworksclientprojects1205/m/thoughtworksclientprojects1205/1621936615268/5477c54ae1a38dc019e398db14d5416f6f746ef0)

![Trusted Pipeline threat modelling](https://github.com/tintulip/tintulip/raw/main/scenarios/scenario-1/CLA-scenario-1-trust-pipeline-0.png "threat model of trusted pipeline")

### Threats identified

- Can find vulnerabilities just by looking at the terraform.
- Request can trigger a vulnerability in the workload request endpoint if there is not enough validation.
- Request deploy previous tf configuration.
- Requesting a build can be sppofed but that would need AWS(MFA) and github creds.
- Triggering a lot of requests can overwhelm the builder from picking up real requests.
- Trigger a lot of approval can spam people and hav approval in email blocklist.
- Approving own changes if spoofed AWS (MFA) and Github.
- Approval of dangerous changes.
- Can use to expose secrets from tf state.
- Attempt to delete buckets can be rejected.
- Can find vulnerabilities just by looking at the AWS setup in the environment.
