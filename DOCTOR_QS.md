---
schema_version: "0.1.0"
type: state
created:    # YYYY-MM-DDTHH:MM:SS±HH:MM
updated:    # YYYY-MM-DDTHH:MM:SS±HH:MM
tags: [doctor, questions, visits, providers]
---

# Doctor Questions & Visit Log

<!--
HOW TO USE THIS FILE:
When a health question occurs to you between appointments, add it to Queued Questions below.
Tag it with the date and which provider it's for. When you visit a provider, log the visit
using the template in the Visit Log section.

The DOCTOR_PREP skill reads this file to generate visit summaries. The more complete your
visit log, the better the prep output for future appointments.
-->

## Queued Questions

### For PCP (next visit)
<!-- Add questions as they come up. Include the date you thought of it. -->
<!-- - [ ] Should I get a cortisol panel given recent sleep disruption? (added YYYY-MM-DD) -->
<!-- - [ ] [Question about lab result or symptom pattern] (added YYYY-MM-DD) -->

### For [Specialist] (next visit)
<!-- Add a section for each specialist you see regularly. -->

## Visit Log

<!--
TEMPLATE — copy this block after each provider visit:

### YYYY-MM-DD — [Provider Name], [Visit Type]
- **Provider:** [Name, specialty]
- **Type:** [Annual physical, follow-up, urgent, specialist consult, telehealth, etc.]
- **Key findings:** [What was discussed, any diagnoses, physical exam findings]
- **Labs ordered:** [Any labs ordered, with expected turnaround]
- **Imaging ordered:** [Any imaging ordered]
- **Prescriptions:** [New or changed medications]
- **Referrals:** [Any specialist referrals]
- **Action items:** [What you need to do before next visit]
- **Follow-up:** [Next appointment date/timeframe]
- **Linked files:** [Path to relevant lab results, imaging reports, etc.]
-->
