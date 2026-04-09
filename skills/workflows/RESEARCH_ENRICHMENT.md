---
schema_version: "0.1.0"
type: skill
subtype: workflow
name: Research Enrichment
description: Validate and enrich PHG findings with primary scientific literature using Consensus, PubMed, Clinical Trials, and Scholar Gateway. Produces evidence-graded annotations.
reads:
  - reports/*.md (any prior analysis output)
  - Any PHG analysis output (HEALTH_MEMO, RISK_ASSESSMENT, SUPPLEMENT_REVIEW, MASTER_ANALYSIS, BASELINE_REPORT, SYMPTOM_ANALYSIS)
  - GENETICS.md
  - SUPPLEMENTS.md
  - PROFILE.md
connectors_required:
  - At least one research API (PubMed, Consensus, Scholar Gateway, or Clinical Trials)
connectors_optional:
  - PubMed / PMC (primary literature, systematic reviews)
  - Consensus API (synthesized evidence with consensus meters)
  - Clinical Trials API (active/recruiting trials)
  - Scholar Gateway (cross-platform academic search)
  - ICD-10 API (diagnostic code standardization)
trigger: after any PHG analysis skill produces findings, or on-demand for specific research questions
output: evidence-annotated findings with citations, confidence adjustments, and research gaps identified
saves_to: reports/research_enrichment_YYYY-MM-DD.md
---

# Workflow Skill: Research Enrichment

## Purpose
Take any finding, hypothesis, or recommendation produced by a PHG analysis skill and validate it against the primary scientific literature. This workflow converts "the data suggests X" into "the data suggests X, which is supported/contradicted/unaddressed by the following evidence." It's the difference between pattern recognition and evidence-based reasoning.

This workflow exists because PHG analysis skills identify patterns in *your* data. Those patterns may be real and important, but they're N=1 observations. Research enrichment connects your personal findings to population-level evidence, calibrating confidence appropriately.

## When to use
- After running MASTER_ANALYSIS, HEALTH_MEMO, or RISK_ASSESSMENT — to validate key findings
- After SUPPLEMENT_REVIEW — to verify evidence grades for each supplement
- After SYMPTOM_ANALYSIS — to check whether identified correlations have established biological mechanisms
- On-demand: "What does the literature say about [specific finding from my data]?"
- When preparing for a physician visit — to bring evidence supporting your observations

## Instructions for LLM

### Step 1: Identify claims to validate

From the source analysis, extract every claim that can be empirically validated. Categorize each:

**Type A — Supplement efficacy claims:**
- "[Supplement] at [dose] improves [biomarker/outcome]"
- Example: "Omega-3 at 2g/day reduces triglycerides"

**Type B — Gene-phenotype associations:**
- "[Variant] in [gene] is associated with [clinical implication]"
- Example: "MTHFR C677T heterozygous reduces folate metabolism efficiency"

**Type C — Lifestyle-outcome correlations:**
- "[Behavior/exposure] correlates with [health outcome]"
- Example: "Sleep < 6.5 hours is associated with elevated morning cortisol"

**Type D — Risk stratification claims:**
- "[Risk factor combination] places subject in [risk category]"
- Example: "APOE ε3/ε4 + LDL-C 145 mg/dL + family history = elevated cardiovascular risk"

**Type E — Mechanistic claims:**
- "[Pathway] explains the observed [phenomenon]"
- Example: "COMT Val/Met status explains differential response to methylated B vitamins"

### Step 2: Research each claim

For each extracted claim, query the available research APIs in this order:

**2A: Consensus API (if connected) — fast evidence check**
- Query: restate the claim as a research question
- Capture: consensus level, number of supporting/opposing papers, key citations
- Purpose: rapid triage — is this claim well-supported, contested, or unstudied?

**2B: PubMed — primary literature**
- Search strategy for each claim type:
  - **Supplement claims:** Search for `[supplement] AND [outcome] AND (systematic review OR meta-analysis OR randomized controlled trial)`. Filter to last 10 years. Prioritize Cochrane reviews.
  - **Genetic claims:** Search for `[gene/rsID] AND [phenotype]`. Check ClinVar classification. Look for GWAS replication studies.
  - **Lifestyle claims:** Search for `[exposure] AND [outcome] AND (cohort study OR meta-analysis)`. Note study size and population.
  - **Risk claims:** Search for the specific risk framework referenced (e.g., "Pooled Cohort Equations validation" or "ASCVD risk calculator accuracy").
  - **Mechanistic claims:** Search for `[pathway] AND [gene/enzyme] AND [effect]`. Look for both supporting and contradicting mechanisms.

- For each search, capture:
  - Best supporting citation (PMID, first author, year, journal, key finding)
  - Best contradicting citation (if any)
  - Study type hierarchy: systematic review > RCT > prospective cohort > cross-sectional > case report > mechanistic/in vitro
  - Sample size and population studied
  - Effect size (if reported)

**2C: Scholar Gateway (if connected) — broader search**
- Use for claims where PubMed results are sparse
- Captures preprints, conference proceedings, interdisciplinary research
- Particularly useful for emerging topics (e.g., novel genetic associations, new supplement research)

**2D: Clinical Trials API (if connected) — active research**
- For claims where evidence is mixed or emerging, search for active trials studying the question
- Capture: trial ID, phase, status, expected completion date, primary endpoints
- Purpose: identify whether better evidence is coming soon and from where

### Step 3: Grade each claim

Using the evidence gathered, assign an evidence grade to each claim:

| Grade | Criteria | Example |
|-------|----------|---------|
| **A — Strong** | ≥ 2 RCTs or a high-quality systematic review/meta-analysis with consistent findings. Effect size clinically meaningful. | Omega-3 (EPA ≥ 1.8g/day) reduces triglycerides by 20-30% (Cochrane review, multiple RCTs including REDUCE-IT) |
| **B — Moderate** | 1 RCT or multiple well-designed observational studies. Consistent direction of effect. Some limitations (sample size, surrogate endpoints, short duration). | MTHFR C677T TT genotype associated with 20-40% reduced enzyme activity and elevated homocysteine (multiple cohort studies, mechanistic evidence) |
| **C — Weak** | Observational evidence only, or mechanistic reasoning without human outcome data. Effect may be real but insufficient to be confident. | COMT Val/Met genotype influences subjective response to methylated B vitamins (mechanistic plausibility, limited clinical data, individual variation high) |
| **D — Insufficient** | No published evidence for this specific claim, or evidence is conflicting with no clear direction. | [Specific supplement] at [specific dose] improves [specific biomarker] in [specific genetic context] — too narrow a claim for existing literature |
| **E — Contradicted** | Published evidence actively contradicts the claim. | [Example of a common health myth that evidence doesn't support] |

**Confidence adjustment rule:** If a PHG analysis skill reported a finding with "high confidence" based on personal data, but the literature grade is C or D, the combined confidence should be noted as: "Strong personal data signal, limited population evidence." Conversely, if personal data is weak but literature is Grade A, note: "Strong population evidence, insufficient personal data to confirm individual applicability."

### Step 4: Identify research gaps

For claims graded C, D, or E, note:
- What specific study would resolve the uncertainty?
- Is there an active clinical trial addressing it? (from Step 2D)
- Is this a question the user's own data could help answer with more time? (e.g., "Continue tracking for 6 more months and re-run PATTERN_DETECTION")

### Step 5: Generate enrichment report

```
# Research Enrichment Report
**Source analysis:** [Name of the PHG skill output being enriched]
**Date:** [date]
**Research APIs queried:** [list connectors used]

---

## Evidence Summary

### Claims Validated (Grade A-B)
[For each claim: original statement, evidence grade, key citation(s), effect size if available, any caveats]

### Claims with Limited Evidence (Grade C)
[For each: original statement, what evidence exists, what's missing, whether the personal data signal is strong enough to justify continued action]

### Claims with Insufficient Evidence (Grade D)
[For each: original statement, why evidence is insufficient, what would resolve it, whether active trials exist]

### Claims Contradicted by Evidence (Grade E)
[For each: original statement, contradicting evidence with citations, recommended revision to the original finding]

---

## Confidence Adjustments
[Table showing: original finding, original confidence, literature grade, adjusted confidence, rationale]

| Finding | Original Confidence | Literature Grade | Adjusted Confidence | Rationale |
|---------|--------------------|------------------|--------------------|-----------| 
| | | | | |

---

## Active Clinical Trials of Interest
[Trials identified that are relevant to the user's profile or findings]

| Trial ID | Title | Phase | Status | Relevance | Expected Completion |
|-----------|-------|-------|--------|-----------|---------------------|
| | | | | | |

---

## Research Gaps
[Questions raised by the analysis that current literature doesn't answer]

---

## Updated Citations
[Complete list of all citations used, formatted for reference]

## Methodology Note
This enrichment report searched [X] research databases for [Y] claims extracted from [source analysis]. Search strategies prioritized systematic reviews and meta-analyses where available, with progressive expansion to RCTs, cohort studies, and mechanistic evidence. Claims were graded using a modified GRADE framework. All PMIDs cited were verified as existing publications.
```

## Important notes
- **Verify citations exist.** LLMs can hallucinate PMIDs and paper titles. If you have PubMed API access, verify each cited PMID resolves to a real paper. If you cannot verify, state "citation not independently verified" and provide enough detail (author, year, journal, title) for the user to verify manually.
- **Do not cherry-pick.** For each claim, report both supporting and contradicting evidence. The user needs the full picture, not confirmation of their existing beliefs.
- **Distinguish study populations.** A study conducted in elderly diabetic patients may not apply to a 30-year-old athlete. Note when the studied population differs meaningfully from the user's profile.
- **Effect sizes matter more than p-values.** A statistically significant but clinically trivial effect (e.g., supplement reduces LDL by 2 mg/dL, p < 0.05) should not receive the same weight as a clinically meaningful effect (reduces LDL by 25 mg/dL).
- **Absence of evidence ≠ evidence of absence.** Grade D means "we don't know," not "it doesn't work." Make this distinction explicit.
- **This workflow is iterative.** Run it after major analyses, but also whenever new evidence might change a prior conclusion. Science evolves — a Grade C claim today might become Grade A next year.
- **Respect the hierarchy.** A single well-designed RCT outweighs any number of mechanistic arguments, in vitro studies, or animal models. Conversely, a dozen poorly designed studies don't add up to one good one.
