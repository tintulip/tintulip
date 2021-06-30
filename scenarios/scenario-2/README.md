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