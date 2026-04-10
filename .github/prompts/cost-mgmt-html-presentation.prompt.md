Create an Advania-branded HTML presentation based on the provided CSV files that contain Azure PaaS resource recommendations.

## Use this presentation guidance
@.github\skills\presentation.md

## Objective
Analyze the provided CSV files and create an HTML presentation that explains Azure PaaS cost management findings for the specified customer.

## Required presentation inputs
Confirm the following before generating the final presentation:
- **Customer name**
- **Topic / title**
- **Audience**
- **Purpose**
- **Duration**
- **Language**
- **Key message / desired takeaway**

If any of these are missing, ask focused clarifying questions first instead of hardcoding them.

## Output requirement
- **Output format:** HTML

## Key message guidance
For each resource section in the data:
- visualize the current state clearly
- highlight what is operating within good performance ranges
- show what is over-dimensioned
- show what is under-dimensioned
- use any available `scale patterns` data to explain usage behavior throughout the day across the month-long data range

## Input expectations
- The source material is a set of CSV files containing Azure PaaS resource recommendations
- Some records may include a `scale patterns` property that represents calculated usage patterns over time

## Delivery guidance
- Base the presentation on the actual CSV findings, not generic Azure guidance
- Structure the content for the confirmed presentation duration
- Align the tone, level of detail, and framing to the confirmed audience and purpose
- Use the confirmed customer name, topic, and messaging rather than hardcoded values
- Organize the presentation so each major resource type or recommendation area is easy to follow
- Ask focused clarifying questions whenever required presentation information is missing
