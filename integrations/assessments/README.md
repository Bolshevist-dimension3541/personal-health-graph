# Standardized Health Assessments — Integration Guide

Scored results from validated clinical instruments: mental health screens (PHQ-9, GAD-7), sleep quality indices (PSQI), pain scales, quality-of-life measures, cognitive screens, and other standardized assessments administered by providers or self-reported.

## Why this matters

Standardized assessments produce comparable scores over time. A PHQ-9 score of 12 in January and 6 in June tells a clear story. These scores cross-reference powerfully with lab trends (cortisol, inflammatory markers), sleep data (wearable HRV, sleep efficiency), supplement changes, and protocol adjustments. Skills like PATTERN_DETECTION and HEALTH_MEMO use assessment trajectories to evaluate whether interventions are working.

## File naming

Name by instrument and date: `phq9_2026-03.md`, `gad7_2026-01.md`, `psqi_2025-12.md`

For assessments administered at a clinical visit, include the provider: `phq9_dr-lee_2026-03.md`

## Common instruments

### Mental Health

| Instrument | Full Name | Measures | Score Range | Frequency |
|-----------|-----------|----------|-------------|-----------|
| **PHQ-9** | Patient Health Questionnaire-9 | Depression severity | 0-27 (0-4 minimal, 5-9 mild, 10-14 moderate, 15-19 moderately severe, 20-27 severe) | Quarterly or at visits |
| **GAD-7** | Generalized Anxiety Disorder-7 | Anxiety severity | 0-21 (0-4 minimal, 5-9 mild, 10-14 moderate, 15-21 severe) | Quarterly or at visits |
| **PHQ-15** | Patient Health Questionnaire-15 | Somatic symptom severity | 0-30 (0-4 minimal, 5-9 low, 10-14 medium, 15-30 high) | As needed |
| **PCL-5** | PTSD Checklist for DSM-5 | PTSD symptom severity | 0-80 (33+ provisional PTSD diagnosis) | As clinically indicated |
| **MDQ** | Mood Disorder Questionnaire | Bipolar screening | Positive/Negative screen | Once, or as clinically indicated |
| **AUDIT** | Alcohol Use Disorders Identification Test | Alcohol use risk | 0-40 (8+ hazardous use) | Annually |
| **ASRS** | Adult ADHD Self-Report Scale | ADHD symptom screening | Part A (6 items) + Part B (12 items) | As clinically indicated |

### Sleep

| Instrument | Full Name | Measures | Score Range | Frequency |
|-----------|-----------|----------|-------------|-----------|
| **PSQI** | Pittsburgh Sleep Quality Index | Sleep quality (past month) | 0-21 (>5 poor sleep quality) | Monthly or quarterly |
| **ESS** | Epworth Sleepiness Scale | Daytime sleepiness | 0-24 (>10 excessive sleepiness) | Quarterly |
| **STOP-BANG** | STOP-BANG Questionnaire | Obstructive sleep apnea risk | 0-8 (0-2 low, 3-4 intermediate, 5-8 high risk) | Annually or as indicated |
| **ISI** | Insomnia Severity Index | Insomnia severity | 0-28 (0-7 none, 8-14 subthreshold, 15-21 moderate, 22-28 severe) | Monthly |

### Pain

| Instrument | Full Name | Measures | Score Range | Frequency |
|-----------|-----------|----------|-------------|-----------|
| **BPI** | Brief Pain Inventory | Pain severity and interference | Severity 0-10, Interference 0-10 | As needed |
| **NRS** | Numeric Rating Scale | Pain intensity | 0-10 | Per episode |
| **PCS** | Pain Catastrophizing Scale | Pain-related cognition | 0-52 | As clinically indicated |

### Quality of Life & Functional

| Instrument | Full Name | Measures | Score Range | Frequency |
|-----------|-----------|----------|-------------|-----------|
| **SF-36** | 36-Item Short Form Survey | Health-related quality of life | 8 subscales, 0-100 each | Annually |
| **EQ-5D** | EuroQol 5-Dimension | Health utility | 5 dimensions + VAS 0-100 | Annually |
| **WHO-5** | WHO Well-Being Index | General well-being | 0-25 raw (0-100 scaled; <13 raw suggests depression screening) | Monthly or quarterly |

### Cognitive

| Instrument | Full Name | Measures | Score Range | Frequency |
|-----------|-----------|----------|-------------|-----------|
| **MoCA** | Montreal Cognitive Assessment | Cognitive function | 0-30 (26+ normal) | Annually (40+) or as indicated |
| **MMSE** | Mini-Mental State Examination | Cognitive status | 0-30 (24+ normal) | As clinically indicated |

## How to record

Many of these assessments are administered during clinical visits — ask your provider for the scored result. Some (PHQ-9, GAD-7, PSQI, ESS, WHO-5) are freely available for self-administration, though clinical administration provides professional context.

For each assessment, use the template in `_TEMPLATE.md`. Record the total score, individual item responses if available, the date, who administered it, and any clinical notes.

## Cross-referencing with PHG

Assessment scores become powerful when correlated with other data:
- **PHQ-9 trend + cortisol trend + sleep HRV** — converging signals of stress response
- **GAD-7 spike + calendar density + symptom log** — identifying anxiety triggers
- **PSQI improvement + supplement change + wearable sleep data** — validating sleep interventions
- **WHO-5 trajectory + protocol changes** — measuring overall well-being impact of lifestyle modifications

The PATTERN_DETECTION, SYMPTOM_ANALYSIS, and HEALTH_MEMO skills all read from this directory when available.
