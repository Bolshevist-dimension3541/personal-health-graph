---
schema_version: "0.1.0"
type: skill
subtype: workflow
name: Physician Report
description: Generate a polished, provider-ready health report from PHG analysis and deliver it via Notion, Gamma, or Word — with automated follow-up reminders.
reads:
  - PROFILE.md
  - LABS_HISTORY.md
  - GENETICS.md
  - SUPPLEMENTS.md
  - PROTOCOLS.md
  - DOCTOR_QS.md
  - symptoms/*.md
  - journal/*.md
  - integrations/labs/*.md
  - integrations/genetics/*.md
  - integrations/wearable_daily/*.md
  - integrations/imaging/*.md
connectors_required:
  - At least one output connector (Notion, Gamma, Word, or PowerPoint)
connectors_optional:
  - Apple Reminders (follow-up scheduling)
  - ICD-10 API (diagnostic code references)
  - NPI Registry (provider credential verification)
  - Google Calendar or Fantastical (appointment context)
prerequisite_skills:
  - Run DOCTOR_PREP first for visit-specific reports
  - Run HEALTH_MEMO first for comprehensive overview reports
  - Run RISK_ASSESSMENT first for risk-focused reports
trigger: before a medical appointment or when sharing health data with a provider
output: formatted report in Notion/Gamma/Word + follow-up reminders
saves_to: reports/physician_report_YYYY-MM-DD.md (also .docx, .pptx, or Notion/Gamma)
---

# Workflow Skill: Physician Report

## Purpose
Transform the raw analytical output of PHG skills into a polished, professional document that you can share with a physician, specialist, health coach, or any provider — delivered in the format most useful for the context. This workflow bridges the gap between "I have all this data" and "my doctor can actually use this in a 15-minute appointment."

## When to use
- Before a doctor appointment: generate and deliver a pre-visit brief
- When onboarding a new provider: generate a comprehensive health overview
- When seeking a second opinion: prepare a complete case file
- When sharing results with a health coach or advisor
- When you need a printed or digital document for medical records

## Instructions for LLM

### Step 1: Determine report type and audience

**Report types (choose based on context):**

| Type | Source Skill | Best For |
|------|-------------|----------|
| Visit Brief | DOCTOR_PREP | Upcoming appointment with established provider |
| Full Overview | HEALTH_MEMO | New provider onboarding, second opinion, annual review |
| Risk Report | RISK_ASSESSMENT | Cardiology, endocrinology, or preventive medicine visit |
| Baseline Summary | BASELINE_REPORT | First visit with any provider, establishing care |

**Audience considerations:**
- **PCP / Internal Medicine:** Full breadth, moderate depth. Include screening gaps and preventive care.
- **Cardiologist:** Emphasize lipid trends, cardiovascular genetics (APOE, Lp(a) genetics, 9p21), imaging (CAC), ASCVD risk calculation, wearable cardiovascular metrics.
- **Endocrinologist:** Emphasize metabolic panel trends, HbA1c, thyroid, hormones, genetic metabolic variants, body composition.
- **Psychiatrist / Neurologist:** Emphasize pharmacogenomics (CYP2D6, CYP2C19), neurological genetic variants, sleep data, symptom patterns.
- **Functional / Integrative Medicine:** Full depth on everything — these providers typically engage with the complete dataset including genetics, supplements, and wearable data.
- **Health coach / Nutritionist:** Emphasize protocols, supplements, dietary context, symptom-lifestyle correlations, wearable trends.

### Step 2: Generate the base report

Run the appropriate source skill (DOCTOR_PREP, HEALTH_MEMO, RISK_ASSESSMENT, or BASELINE_REPORT) if it hasn't been run recently. Use its output as the foundation.

**Enrich with connector data (if available):**

- **ICD-10 API:** Look up ICD-10 codes for all conditions in PROFILE.md. Include them in the report — providers need these for billing and documentation, and having them ready saves time.
- **NPI Registry:** Verify the receiving provider's credentials and specialty. Tailor the report accordingly.
- **Google Calendar / Fantastical:** Check the appointment details — time, location, visit type — to include in the report header.

### Step 3: Format for delivery platform

#### Option A: Notion
Create a Notion page with:
- **Page title:** "Health Report — [Patient Name] — [Date] — [Provider/Purpose]"
- **Page icon:** 🏥 or relevant emoji
- **Sections** using Notion headings (H1, H2, H3)
- **Callout blocks** for critical findings (medication interactions, significantly abnormal labs, urgent items)
- **Tables** for lab values, supplement lists, risk scores
- **Toggle blocks** for detailed appendices (the provider can expand if they want depth)
- **Linked databases** for lab history or supplement tracking (if the user maintains Notion databases)

Set the page to "sharable via link" so it can be sent to the provider or printed from the browser.

#### Option B: Gamma
Generate a Gamma presentation with:
- **Slide 1:** Patient overview (demographics, conditions, medications — from PROFILE.md)
- **Slide 2:** Key findings / reason for visit
- **Slides 3-6:** One slide per major finding or section (lab trends with visual charts, genetic highlights, risk scores)
- **Slide 7:** Supplement stack (current, with evidence basis)
- **Slide 8:** Questions for provider (from DOCTOR_QS.md)
- **Slide 9:** Appendix — full lab table, genetic variant list

Gamma's strength is visual presentation. Use it when you want the provider to see trends graphically — lab trajectories, wearable trend charts, risk score visualizations.

#### Option C: Word document
Generate a Word document using formal medical document conventions:
- **Header:** Patient name, DOB, date of report, report type
- **Sections** with numbered headings
- **Tables** for structured data (labs, supplements, risk scores)
- **Footer:** "Generated from Personal Health Graph. Not a clinical document."
- **Print-ready** formatting (standard margins, readable font, proper page breaks)

Use Word when the provider prefers paper documents, when the report needs to be faxed (yes, still happens), or when it needs to be uploaded to a patient portal.

#### Option D: PowerPoint
Generate slides following the same structure as Gamma Option B, but in native PowerPoint format for maximum compatibility.

### Step 4: Set follow-up reminders (if Apple Reminders connected)

After generating and delivering the report, create reminders for:

**Pre-appointment:**
- "Review health report before appointment" — 1 day before
- "Bring [specific item] to appointment" — morning of (e.g., medication list, insurance card, raw genetic data file on USB)

**Post-appointment:**
- "Update PHG from appointment notes" — day of appointment (trigger MEETING_TO_PROTOCOL workflow)
- "Check for provider's after-visit summary" — 2-3 days after
- "Schedule follow-up tests ordered" — 1 week after (if applicable)
- "Follow up on referral" — 2 weeks after (if referral was mentioned)

### Step 5: Generate delivery summary

```
## Physician Report — Delivery Summary
**Report type:** [Visit Brief / Full Overview / Risk Report / Baseline Summary]
**Audience:** [Provider name, specialty]
**Delivered via:** [Notion page / Gamma presentation / Word document / PowerPoint]
**Link:** [URL or file path]

### Report Contents
- [List of sections included]
- [Key findings highlighted]
- [ICD-10 codes included: Yes/No]

### Reminders Set
- [List each reminder with date]

### Before the appointment, verify:
- [ ] Report is accessible (test the link or print a copy)
- [ ] Provider information is correct
- [ ] All recent lab results are included
- [ ] Medication list is current
- [ ] Questions for provider are prioritized
```

## Important notes
- **Include a disclaimer.** Every generated report should include a footer or note: "This report was generated from a Personal Health Graph using AI-assisted analysis. It is intended to support — not replace — clinical evaluation. All data should be verified against primary sources."
- **Don't overwhelm the provider.** A 15-minute appointment cannot absorb a 20-page report. For visit briefs, target 2-3 pages or 5-6 slides. Use appendices and toggle blocks for depth. Lead with what matters most.
- **Respect data sensitivity.** If sharing via Notion link, consider who might access it. Use share settings appropriately. For printed documents, remind the user to treat them as medical records.
- **Verify genetic interpretations.** If the report includes genetic data, include a note that consumer genotyping results should be confirmed by clinical-grade testing before making medical decisions.
- **Offer both digital and print.** Some providers engage better with a printed page they can annotate. Others prefer a link they can review on their screen. When possible, prepare both options.
