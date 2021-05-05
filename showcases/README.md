# Showcase 0 - 21/04/2021

First showcase!

Blue Team started configuring the AWS account on previous Friday and in the 3 days since the team focused on drafting an architecture and backlog for "Scenario 0" as well as configuring GitHub accounts and Threat Modelling.

## Accounts set up

We have set up the following:

- [Slack workspace](https://documentation.slack.com)
- [GitHub organisation](https://github.com/documentation)
- [AWS Account](https://048191938814.signin.aws.amazon.com/console)

With this the team is ready to go!

## GitHub setup

The organisation is available at [https://github.com/documentation](https://github.com/documentation). Other than code, it hosts the [Blue Team's kanban board](https://github.com/documentation/documentation/projects/1) using GH Projects as well as the [knowledgebase](https://github.com/documentation/documentation) that we'll be accumulating as we play out Scenarios. For now you can find the [description of Scenario 0](https://github.com/documentation/documentation/tree/main/scenarios/scenario-0) and the outputs from the threat modelling session we ran on it, as well as documentation on how we [bootstrapped the AWS account](https://github.com/documentation/documentation/blob/main/BOOTSTRAP.md) and [GitHub organisation](https://github.com/documentation/documentation/blob/main/GitHub.md).

The other repository avaialble for now is [`bootstrap-users`](https://github.com/documentation/bootstrap-users), containing the Terraform scripts we used for initial bootstrap of AWS administrators.

## AWS setup

We started an AWS account that will become the main account of an AWS Organisation. In generating the account we assumed that we know how to initialise the root token via a Rosa device, and let the real execution out of scope. We did enable MFA on it (as everyone should!) but used a virtual one - we called out further detailing of this process as we move ownership of the account to Cabinet Office in the next two weeks.

We instead focused on how to move from that initial token to a set of administrator users that can take the setup forward. What we came up are a [set of manual bootstrap steps](https://github.com/documentation/documentation/blob/main/BOOTSTRAP.md) that include running a [small piece of Terraform code](https://github.com/documentation/bootstrap-users). What we get out of this is:

- 3 "administrators"
- any privileged is granted via an `AssumeRole`
- new IAM users cannot perform any action without setting up MFA
- MFA is required to `AssumeRole`
- initial passwords are encrypted with individual GPG keys for secure initial sharing

## ScoutSuite report

For good measure, we ran a [ScoutSuite report](https://github.com/documentation/documentation/tree/main/scoutsuite) on our initial setup.

While there are many flags raised about lack of some controls (that we simply have not worked on yet), a few findings from the IAM category raised good discussion points:
- Administrators having both Console login credentials AND Access Keys is strongly discouraged
- Root token can be better managed (hardware MFA, redundancy)

It has been pointed out that there are techinical solutions that can be looked at to reduce risk around highly privileged administrator access and we'll be following up on this.

## Scenario 0 threat modelling

We ran our [initial threat modelling session on Scenario 0](https://github.com/documentation/documentation/tree/main/scenarios/scenario-0#threat-model) the day before showcase. The session ran using timeboxed STRIDE to brainstorm threats. Due to lack of time, we didn't discuss suggested mitigations during the session but produced a set of initial suggestions instead and move the followup conversation to the [#threat-modelling](https://app.slack.com/client/T01UTMNBF9P/C01UQQWSHAN/thread/C01UR9LE47N-1619017494.005100) channel in Slack.

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

# Showcase 1 - 28/04/2021

You can find the deck presented [here](https://github.com/documentation/documentation/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20April%2028.pdf).

In this showcase we present initial progress on:
- GitHub Organisation Security Controls ([read more](https://github.com/documentation/documentation/blob/main/GitHub.md#github-organization))
- AWS Control Tower ([read more](https://github.com/documentation/documentation/tree/main/technologies#aws-control-tower))
- Introducing the first Infrastructure as Code pipeline ([read more](https://github.com/documentation/documentation/blob/main/technologies/PIPELINE.md#using-github-actions-to-set-up-cicd))

## Threat Modelling

We present an [updated Threat Model for scenario 0](https://github.com/documentation/documentation/blob/main/scenarios/scenario-0/README.md#threat-model-on-updated-proposal-with-privileged-access-alternatives-27042021).

## Prioritisation

Main point of the discussion is that we want to focus on how to improve attack resistance of the longer temporal attack surface rather on low-frequency or one-off operations.

# Showcase 2 - 05/05/2021

Showcase canceled due to shorter week and work being in flight rather than completed.
A replacement deck with updates is available [here](https://github.com/documentation/documentation/raw/main/showcases/Tin%20Tulip%20Showcase%20-%20May%205.pdf).

We update progress on:
- Replacing "owner" IAM users with SSO ([read more](https://github.com/documentation/documentation/tree/main/technologies/SSO.md))

## Threat Modelling

We present a [different approach to pipelines running privileged AWS operations](https://github.com/tintulip/tintulip/tree/main/scenarios/scenario-0#threat-model-on-pipeline-design-with-privileged-workloads-in-aws-04052021).
