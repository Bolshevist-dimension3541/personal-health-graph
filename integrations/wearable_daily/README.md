# Wearable Data — Integration Guide

## What goes here
Daily summaries from wearable devices: WHOOP, Oura Ring, Garmin, Apple Watch, Fitbit, Eight Sleep, etc. One file per month containing daily entries.

## How to structure a wearable summary file

Name files by month: `2026-01.md`, `2026-02.md`, etc.

```markdown
---
schema_version: "0.1.0"
type: integration
tier: summary
source: "[Device] (via [method: app export, Terra API, manual])"
period: YYYY-MM
sync_frequency: daily
tags: [wearable, device-name, YYYY-MM]
---

# Wearable Daily — [Month Year]

## Device: [WHOOP 4.0 / Oura Gen 3 / Garmin / Apple Watch / etc.]

### YYYY-MM-DD
- **Sleep:** [duration] hrs | HRV: [value] ms | Recovery: [score]%
- **Resting HR:** [value] bpm
- **Strain/Activity:** [score or calories]
- **Steps:** [count]
- **Notes:** [anything notable — travel, illness, alcohol, missed wearing]

### YYYY-MM-DD
...
```

## Field reference by device

### WHOOP
- Sleep: duration, efficiency, latency, disturbances, sleep stages
- Recovery: recovery score (%), HRV (ms RMSSD), resting HR, SpO2, skin temp
- Strain: day strain (0-21), calories

### Oura Ring
- Sleep: total sleep, efficiency, latency, REM/deep/light, HRV
- Readiness: readiness score, HRV balance, body temperature deviation
- Activity: activity score, steps, calories, training frequency

### Apple Watch
- Sleep: duration, stages (if enabled)
- Heart: resting HR, HRV, walking HR
- Activity: move/exercise/stand rings, steps, workouts

### Garmin
- Sleep: duration, sleep score, body battery
- Heart: resting HR, HRV status, stress score
- Activity: steps, intensity minutes, training status/load

## How to get your data

### Manual daily logging
Simplest approach: each morning, open your wearable app and transcribe yesterday's key metrics.

### Export from app
Most wearables allow CSV export. Store in `raw/wearable/` and summarize here.

### Terra API (programmatic)
If you have a Terra API integration, it can write directly to these files.

### Apple Health export
Settings → Health → Export All Health Data → store the .xml in `raw/healthkit/`

## Critical: HRV measurement differences

**HRV values from different devices are NOT directly comparable.**

- **WHOOP and Oura Ring** report HRV as **RMSSD** (Root Mean Square of Successive Differences) — a time-domain measure of parasympathetic activity, typically measured during sleep.
- **Apple Watch** reports HRV as **SDNN** (Standard Deviation of NN intervals) — a broader measure of total autonomic variability, measured in short daytime samples.
- **Garmin** reports an "HRV Status" score derived from overnight RMSSD but presented as a proprietary composite.

RMSSD and SDNN measure different things and produce different numbers. RMSSD is typically lower than SDNN for the same individual. **Do not compare a WHOOP HRV of 45ms to an Apple Watch HRV of 45ms — they are not equivalent.**

**Best practice:** Pick one device as your canonical HRV source and use it consistently. Note the measurement method (RMSSD vs. SDNN) in your wearable files. If you switch devices, annotate the transition date and expect a baseline shift.

## Notes
- Daily logging is the most valuable habit in Personal Health Graph. Even 30 seconds of transcribing your sleep score and HRV builds a dataset that powers every analysis skill.
- If you miss a day, leave a gap. Don't backfill from memory.
- The symptom analysis and pattern detection skills heavily depend on this data.
