# Scenario definition beyond 1 - Workshop 22/06/2021

We used the weekly threat modelling slot on June 22 to brainstorm and prioritise scenarios rather than individual threats, and reserved half the time to discuss what is worth exploring and feasibility for the next 5 weeks.
This is the board at the end of session:

![Scenario definition session](https://github.com/tintulip/tintulip/raw/main/scenarios/scenarios/scenarios-definition-22-June-2021.png "scenario definition beyond 1")

The top 6 scenarios that emerged are, in order from most voted:
- Assume some bad terraform gets through the pipeline and deployed. What is the blast radius?
- What if Rob from Red team was an evil developer on the Platform team? Add control to infra pipeline
- Assume some bad code gets through the pipeline and into the web-application. What is the blast radius?
- Assume the app pipeline compromised, can it escalate to take over the infra pipeline by virtue of being in the same account?
- assume can tamper with database migrations, what is the blast radius? can exfil happen?
- What if one of the development team add an evil dependency? How can we detect that? Before deployment to prod?

In summary, we all agreed that attacking the infrastructure pipelines is not only the most interesting scenarios we can explore next, but also feasible both in sense of work required to support from the blue team as well as availability of red team people to attack these specific scenarios. With 5 weeks ahead we have high confidence in being able to cover 3 scenarios, aiming at covering all 6 of these if time allows.