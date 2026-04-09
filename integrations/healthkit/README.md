# Apple HealthKit — Integration Guide

## What goes here
Aggregated daily summaries from Apple Health. This captures data that flows through HealthKit from multiple sources: Apple Watch, iPhone sensors, third-party apps, manual entries.

## How to structure a HealthKit summary file

Name files by month: `2026-01.md`, `2026-02.md`, etc.

```markdown
---
schema_version: "0.1.0"
type: integration
tier: summary
source: "Apple HealthKit"
period: YYYY-MM
sync_frequency: daily
tags: [healthkit, apple-health, YYYY-MM]
---

# HealthKit Daily — [Month Year]

### YYYY-MM-DD
- **Steps:** [count]
- **Distance:** [miles/km]
- **Flights climbed:** [count]
- **Active calories:** [kcal]
- **Resting calories:** [kcal]
- **Exercise minutes:** [min]
- **Stand hours:** [hrs]
- **Resting HR:** [bpm]
- **Walking HR avg:** [bpm]
- **HRV (SDNN):** [ms]
- **SpO2:** [%]
- **Respiratory rate:** [breaths/min]
- **Weight:** [lbs/kg] (if tracked)
- **Body fat %:** [%] (if tracked)
- **Blood pressure:** [sys/dia] (if tracked)
- **Blood glucose:** [mg/dL] (if tracked)
- **Mindful minutes:** [min]
```

## How to export from Apple Health

### Full export (for raw/ directory)
1. Open Health app on iPhone
2. Tap your profile picture (top right)
3. Scroll down → "Export All Health Data"
4. This creates a large .xml file — store in `raw/healthkit/`

### Daily summary (for this directory)
Transcribe key daily metrics from the Health app's summary view, or use a shortcut/automation to extract them.

## Notes
- HealthKit aggregates data from many sources. If you wear both an Apple Watch and a WHOOP, some metrics (like HR) will have overlapping data. Pick one as your canonical source for each metric.
- The full HealthKit export can be very large (100MB+). It's useful as an archive but too detailed for daily analysis. The summary files here are what skills read.
