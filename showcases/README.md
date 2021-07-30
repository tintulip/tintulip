# Showcase 1 - 21/04/2021

First showcase!

Blue Team started configuring the AWS account on previous Friday and in the 3 days since the team focused on drafting an architecture and backlog for "Scenario 0" as well as configuring GitHub accounts and Threat Modelling.

## Accounts set up

We have set up the following:

- [Slack workspace](https://tintulip.slack.com)
- [GitHub organisation](https://github.com/tintulip)
- [AWS Account](https://048191938814.signin.aws.amazon.com/console)

With this the team is ready to go!

## GitHub setup

The organisation is available at [https://github.com/tintulip](https://github.com/tintulip). Other than code, it hosts the [Blue Team's kanban board](https://github.com/tintulip/tintulip/projects/1) using GH Projects as well as the [knowledgebase](https://github.com/tintulip/tintulip) that we'll be accumulating as we play out Scenarios. For now you can find the [description of Scenario 0](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-0) and the outputs from the threat modelling session we ran on it, as well as documentation on how we [bootstrapped the AWS account](https://github.com/tintulip/tintulip/blob/main/technologies/bootstrap-owners.md) and [GitHub organisation](https://github.com/tintulip/tintulip/blob/main/GitHub.md).

The other repository available for now is [`bootstrap-users`](https://github.com/tintulip/bootstrap-users), containing the Terraform scripts we used for initial bootstrap of AWS administrators.

## AWS setup

We started an AWS account that will become the main account of an AWS Organisation. In generating the account we assumed that we know how to initialise the root token via a Rosa device, and let the real execution out of scope. We did enable MFA on it (as everyone should!) but used a virtual one - we called out further detailing of this process as we move ownership of the account to Cabinet Office in the next two weeks.

We instead focused on how to move from that initial token to a set of administrator users that can take the setup forward. What we came up are a [set of manual bootstrap steps](https://github.com/tintulip/tintulip/blob/main/technologies/bootstrap-owners.md) that include running a [small piece of Terraform code](https://github.com/tintulip/bootstrap-users). What we get out of this is:

- 3 "administrators"
- any privileged is granted via an `AssumeRole`
- new IAM users cannot perform any action without setting up MFA
- MFA is required to `AssumeRole`
- initial passwords are encrypted with individual GPG keys for secure initial sharing

## ScoutSuite report

For good measure, we ran a [ScoutSuite report](https://github.com/tintulip/tintulip/tree/main/scoutsuite) on our initial setup.

While there are many flags raised about lack of some controls (that we simply have not worked on yet), a few findings from the IAM category raised good discussion points:
- Administrators having both Console login credentials AND Access Keys is strongly discouraged
- Root token can be better managed (hardware MFA, redundancy)

It has been pointed out that there are techinical solutions that can be looked at to reduce risk around highly privileged administrator access and we'll be following up on this.

## Scenario 0 threat modelling

We ran our [initial threat modelling session on Scenario 0](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-0#threat-model) the day before showcase. The session ran using timeboxed STRIDE to brainstorm threats. Due to lack of time, we didn't discuss suggested mitigations during the session but produced a set of initial suggestions instead and move the followup conversation to the [#threat-modelling](https://app.slack.com/client/T01UTMNBF9P/C01UQQWSHAN/thread/C01UR9LE47N-1619017494.005100) channel in Slack.

We identified a whole set of threats which details you can find from the links above. Among these, we identified some major points that we decided to raise for prioritisation and steering of the scenario itself, specifically:
- Administrator seem to be overly privileged, and in our fictional organisation there is no model for audit and oversight
- Running infrastructure pipelines from GitHub Actions requires potentially privileged Access Keys that are subject to leak and theft

## Next steps

Next priorities, actually already in progress:
- Configuring security controls for the GitHub organisation
- Configuring AWS Control Tower
- Configure the infrastructure pipelines to start delivering the Creative Licensing Agency's service

## Prioritisation

We reserved the last part of the meeting to discuss next priorities. The current working assumptions cover building out Scenario 0 including:
- infrastructure and web pipelines in GitHub Actions
- Well-known or "out of the box" security controls enabled
- continuing with the assumption that administrator have high levels of privilege, for "break glass" scenarios.

The main decision points were whether continuing with this assumptions or:
- revisit the decision of using GHA for pipelines
- revisit the assumptions and backstory around administrator access
- revisit the decision of adopting out-of-the-box controls such as Control Tower

We ultimately decided to keep working towards current assumptions, and take a look at refining technical solutions for administrator access.

## Discussion highlights

### Are we focussing on Security or Delivery?

The main highlight from discussion is clarity around scope of the scenarios and how much security are we baking into the build versus how much of the build represents a realistic service delivery that has to balance security with delivery speed, and how much attack surface we leave intentionally for the Red Team to explore.

As part of this we clarified that the intention was, for Scenario 0, "to represent a newly formed organisation that is getting up to speed and wants to deliver an MVP fast, but is aware that many security controls are readily available and it's best to adopt them early". We called out a new Tradeoff Slider to represent which side of the spectrum we'll be leaning towards in the future: "focus on Security" vs "focus on Delivery".

### Is the root token in scope?

Root token has a minimal set of controls applied, and we called more details on it out of scope as we know that it can be generated on secure devices.

We are up for revisiting this and dedicate some efforts in this area as we change ownership of the token and AWS setup from ThoughtWorks to Cabinet Office.

### What is the boundary between AWS-native automation and Infrastructure as Code

A significant chunk of risk identified in threat modelling comes from the high-privilege pipeline that we initially accounted for. In reality we expect that running Control Tower will give us a set up in which we'll rely less on Terraform and highly privileged tooling executing outside the AWS environment and rely on AWS-native tooling instead, integrating Infrastructure as Code only within security boundaries and not around them.

# Showcase 2 - 28/04/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20April%2028.pdf).

In this showcase we present initial progress on:
- GitHub Organisation Security Controls ([read more](https://github.com/tintulip/tintulip/blob/main/GitHub.md#github-organization))
- AWS Control Tower ([read more](https://github.com/tintulip/tintulip/tree/main/technologies#aws-control-tower))
- Introducing the first Infrastructure as Code pipeline ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/PIPELINE.md#using-github-actions-to-set-up-cicd))

## Threat Modelling

We present an [updated Threat Model for scenario 0](https://github.com/tintulip/tintulip/blob/main/scenarios/scenario-0/README.md#threat-model-on-updated-proposal-with-privileged-access-alternatives-27042021).

## Prioritisation

Main point of the discussion is that we want to focus on how to improve attack resistance of the longer temporal attack surface rather on low-frequency or one-off operations.

# Showcase 3 - 05/05/2021

Showcase canceled due to shorter week and work being in flight rather than completed.
A replacement deck with updates is available [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20May%205.pdf).

We update progress on:
- Replacing "owner" IAM users with SSO ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/SSO.md))

## Threat Modelling

We present a [different approach to pipelines running privileged AWS operations](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-0#threat-model-on-pipeline-design-with-privileged-workloads-in-aws-04052021).

# Showcase 4 - 12/05/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20May%2012.pdf).

Since last showcase we present completion of scenario 0. CLA's website is now available [at this link](https://d2euivgmcq1yyp.cloudfront.net/).

Individual items we worked on:
- Pipeline for website infrastructure ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/pipeline.md))
- Pipeline for website content delivery ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/restricted-pipeline.md))
- Security tooling for IaC pipelines ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/pipeline.md))
- Publishing CLA's website ([read more](https://github.com/tintulip/website-infra#cloudfront-considerations))

Everything developed so far is either covered with Terraform code or documented as a manual step.

## Threat Modelling

We present a high level of the findings for previous week's threat modelling to gather feedback.

General consensus is that there is a lot of interest in the "develop low, deploy high" pattern that we are going towards but can easily become a never-ending rabbithole of controls.

## Prioritisation

We present our next suggested priorities as such:

1. Scenario 0 is "well architected": GuardDuty, SCPs, centralised logs, GH actions controls
2. Trustable pipelines: From Threat Modelling - current pipelines are not tamper-resistant.
3. Scenario 1: Build towards CLA's "Apply for a Creative License" service

And agree that the sequence makes sense, but:
- we should carefully timebox 1.: some controls are important but it can become an infinite rabbithole of not-so-interesting, well-documented controls.
- as mentioned above, there is a lot of interest in 2. and where it will bring us. We agreed to put the structure for it in place following existing publications as a reference but not spend more on that, as we believe we will discover more as we progress
- with the above 2 in place we accellerate towards building Scenario 1

### Scenario 1 proposal

We make initial suggestions for Scenario 1 backstory, processes, organisational aspects and technology ([read more](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-1#scenario-1---clas-apply-for-a-creative-license-service)).
Feedback indicates consensus on the suggestions made and seems to lean towards the ECS option for the runtime platform, although more feedback will be collected via xGov Slack.

Some technology highlights are:
- focus on service technology choices that promote accessibility (e.g. SSR, Progressive Enhancement) as:
  - it is a critical requirement for gov.uk services
  - some architectural tradeoffs made for this have security implications (e.g. backend-frontend interactions)
- strong preference towards RDS as opposed as self-managing DBs

## GDPR and CLA's services

We touch upon the GDPR puzzle and we conclude that it warrants a dedicated conversation. Main points:
- do we add some affordance to act on SARs and Right to Erasure as part of Scenario 1? We believe this changes the attack surface and it worth considering.
- Right to Erasure might not be relevant as it implies usage of Consent as the legal base for processing and it is not generally the case for Government services.
- The actual data in CLA's service will be fake - do we need both a real and a fictional versions of privacy policies and data processing playbooks?

## The data provenance puzzle

As we are approaching the introduction of data, an interesting point for future consideration will be data provenance - as data is siphoned out from services via integration, reporting, APIs and more, how do we track origin and properties of a piece of data, so that later we can e.g. detect sprawl of sensitive information, perform audits efficiently and apply pseudonymisation/anonymisation where necessary?

# Showcase 5 - 19/05/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20May%2019.pdf).

Since last showcase we covered the "improve security controls" timebox (minus a story in flight). CLA's website is available [at this link](https://d2euivgmcq1yyp.cloudfront.net/).

Individual items we worked on:
- GuardDuty ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/control-tower.md#guardduty))
- Service Control Policies ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/control-tower.md#service-control-policies-scps))
- Extending Control Tower ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/control-tower.md#region))
- Restricting GitHub Actions ([read more](https://github.com/tintulip/tintulip/tree/main/technologies/GitHub.md#actions-restrictions))

Everything developed so far is either covered with Terraform code or documented as a manual step.

## Prioritisation

We confirm the order of next priority in line with previous week.

1. Trustable pipelines:
  From Threat Modelling - current pipelines are not tamper-resistant.
  Build CD pipeline in AWS (high side) to enforce security controls.
  Limited to groundwork to make it work.
2. Scenario 1:
  Build towards CLA's "Apply for a Creative License" service.
  - Will capture user details to apply for a license
  - Java service using SSR, Docker container in ECS, RDS database
  - GDPR considerations out of scope - option for future scenario

# Showcase 6 - 26/05/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20May%2026.pdf).

Since the last showcase we covered various security controls applied to the AWS accounts. We also started on the groundwork for the trusted pipeline including the architecture and setting up the required accounts for it.

Individual items we worked on:
- GuardDuty in all regions ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/control-tower.md#enable-guardduty-on-multiple-regions))
- Cross account log replication ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/log-replication.md))
- Groundwork for trusted pipeline ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/accounts.md))

Everything developed so far is either covered with Terraform code or documented as a manual step.

## Threat Modelling

We present a high level of the findings for previous week's threat modelling to gather feedback.

We took a [closer look at the "develop low, deploy high" pattern that we are going towards and especially focusing on the "deployment" account](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-1#threat-model-on-trusted-pipeline-for-scenario-1-25052021).

## Prioritisation

We confirm the order of next priority in line with previous week.

1. Trustable pipelines:
  Continue moving the tradeoff slider towards building our own delivery
  and laying the foundation for trusted pipelines.
  Question about security checks in the trusted pipeline: can it be done whilst still maintaining multiple deployments per day and what would be the impact on productivity?
2. Scenario 1:
  Build towards CLA's "Apply for a Creative License" service.

General consensus was that the red team would start attacking a well built senario 1 within a few weeks.

# Showcase 7 - 02/06/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20June%202.pdf).

The blue team is now preparing for red team's start by adding a dynamic workload to the environment and finishing the trusted pipelines.

Individual items we worked on:
- Running Terratest in the Sandbox ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/terraform.md#terratest))
- Setting up CodePipeline ([read more](https://github.com/tintulip/tintulip/blob/main/technologies/trusted-pipeline.md))
- Network setup

## Prioritisation

We confirm that the top priority is to get a basic dynamic workload for Scenario 1 ready.

# Showcase 8 - 09/06/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20June%209.pdf).

Blue team deployed a basic service for Scenario 1 and will follow up adding a database to it. The main focus is on logistics to enable Red team
to start such as accounts and permissions.

Red team started testing against AWS best practices and presents some initial findings!

Individual items we worked on:
- Configure IaC pipeline
- Configure webapp pipeline
- Build & Deploy skeleton webapp (view it [here](https://www.tintulip-scenario1.net/greeting))
- Setup public-facing webapp networking
- Initial kickoff with Red Team
- First Red Team findings!

## Prioritisation

We will finish adding a database to Scenario 1 and remediate findings from various automated tools we adopted, as well as the finding reported by the Red team.

# Showcase 9 - 23/06/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20June%2023.pdf).

The CLA's environment has been undergoing testing for Scenario 1. As well as, planning the scenarios for the next 5 weeks for the Red team. 

Individual items we worked on:
- Restricting Workload Egress
- Team Mozart(Web) can now access AWS
- Add RDS to the Workload
- Apply for a creative license(view it [here](https://www.tintulip-scenario1.net/addUser))
- Policy as code checks for Terraform

## Prioritisation

We confirm that the order of priority of testing for the Red team for the next 5 weeks are :

1. Scenario 2: Assume some bad code gets through the pipeline and into the web-application. What is the blast radius?

2. Scenario 3: Assume some bad terraform gets through the pipeline and deployed. What is the blast radius?

3. Scenario 4: What if Rob from Red team was an evil developer on the Platform team? Add control to infra pipeline

# Showcase 10 - 30/06/2021

You can find the deck presented [here](https://github.com/tintulip/tintulip/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20June%2030.pdf).

The Red team is testing Scenario 2 and produced findings. The Blue team is looking at improving strength of controls for Scenario 3.

Individual items Blue team worked on:
- Restricting Workload Egress(With VPC endpoints)
- Use KMS for statebuckets

Individual items Red team worked on:
- Data Exfiltration
- Remote Access


## Prioritisation

We confirm that the order of testing for the Red team is in line with the previous week. The order of priority for the Blue team is as follows:

1. Add SCPs to prevent AWS-based database exfils (e.g backups to external accounts).
2.  Introduce anomaly detection in preproduction application testing.
3. Limit egress from builder to a SaaS artifact repository.

# Showcase 11 - 07/07/2021

You can find the deck presented [here](Add link).

The blue team is improving security controls in the Builder account and the Red team is continuing with testing scenario 2 and 3. 

Individual items we worked on:
- Preventing Data Exfil with [read more](https://github.com/tintulip/cla-organisation/tree/main/service-control-policies#deny-rds-backup-to-go-to-external-accounts)
- Web Application Firewall [read more](https://github.com/tintulip/cla-organisation/tree/main/service-control-policies#deny-rds-backup-to-go-to-external-accounts)

## Prioritisation

We confirm that the order of testing for the Red team is in line with the previous week. The order of priority for the Blue team is as follows:

1. Limit egress from builder to a SaaS artifact repository.
2. Introduct technical controls around two pairs of eyes.
3. Introduce anomaly detection in preproduction application testing.

# Showcase 12 - 21/07/2021

You can find the deck presented [here](Add link).

Scenario 2 and 3 have been undergoing more testing by the Red team and the Blue team has been improving security controls in the Builder account. 

Individual items we worked on:
- Network Restrictions for Builder Account [read more](https://github.com/tintulip/tintulip/blob/main/technologies/network-firewall.md#network-firewall)
- Read-only access for web team
- Checking for malicious code

## Prioritisation

We confirm the order of priority for the Blue team is:

1. Complete Artifactory PoC.
2. Introduce a control for supply chain attacks.
3. Clean up code and documentation deliverables.

# Showcase 12 - 28/07/2021

You can find the deck presented [here](Add link).

Since last showcase Red team has finished testing Scenario 2 and 3 and has now begun testing Scenario 4. The Blue team has also finished improving controls and is now writing the final report and reviewing existing artifacts.

Individual items we worked on:
- Use Artifactory to pull images and packages
- Ensure all repos have branch protection
- Add pipeline checks for vulnerable dependancies

## Prioritisation

As it's the last showcase the Blue team priority will be working on producing a final writeup. 