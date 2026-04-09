# Menstrual & Fertility Tracking — Integration Guide

Monthly cycle tracking data: period dates, flow, ovulation markers, cycle-specific symptoms, fertility indicators, and hormonal context. One file per month, similar to wearable daily summaries.

## Why this matters

Menstrual cycle data is one of the most information-dense health signals available. Cycle length variability, luteal phase duration, and cycle-specific symptoms correlate with thyroid function, stress response, metabolic health, nutritional status, and training load. When cross-referenced with labs (estradiol, progesterone, LH, FSH, AMH, DHEA-S, thyroid panel), wearable data (HRV, basal body temperature, resting heart rate), and supplement/protocol changes, cycle data provides a window into systemic health that most tracking systems analyze in isolation.

Skills like PATTERN_DETECTION, SYMPTOM_ANALYSIS, and HEALTH_MEMO use cycle data to contextualize symptoms, identify hormonal patterns, and evaluate intervention impact.

## File naming

Name by month: `2026-04.md`, `2026-05.md`

## What to track

### Core data (minimum viable tracking)
- Period start and end dates
- Flow level per day (light, moderate, heavy)
- Cycle length (calculated: day 1 to next day 1)

### Enhanced tracking
- **Ovulation markers:** LH test results (positive/negative + date), basal body temperature (BBT) shift, cervical mucus observations, ovulation pain (mittelschmerz)
- **Cycle-specific symptoms:** cramps, bloating, breast tenderness, mood changes, energy levels, headaches, acne, food cravings, sleep disruption
- **Luteal phase length:** days from ovulation to period start (if ovulation is tracked)

### Fertility-specific tracking
- Intercourse timing relative to fertile window
- Progesterone confirmation (blood draw or at-home test)
- Early pregnancy testing dates and results
- IVF/IUI cycle details (medications, monitoring, procedures)

## Data sources

### Cycle tracking apps
- **Clue** — Export: Settings → Account → Data export (CSV)
- **Flo** — Export: Profile → My Data → Request data (may take up to 30 days)
- **Natural Cycles** — Export: Settings → My Data → Download
- **Fertility Friend** — Export: Menu → Data → Export data

### Wearable integration
- **Oura Ring** — detects cycle phase via overnight temperature deviation; export includes temperature trend data
- **Apple Watch / HealthKit** — cycle tracking in Apple Health; export includes period logging and predictions
- **Ava Bracelet** — designed for fertility tracking; exports temperature, pulse rate, HRV, breathing rate, sleep

### Lab integration
Hormonal data relevant to cycle tracking lives in `LABS_HISTORY.md` and `integrations/labs/`. Key markers:
- **Estradiol (E2)** — peaks at ovulation, relevant throughout cycle
- **Progesterone** — confirms ovulation (drawn ~7 days post-ovulation)
- **LH / FSH** — pituitary hormones; LH surge triggers ovulation
- **AMH** — ovarian reserve marker (not cycle-dependent, but fertility-relevant)
- **DHEA-S, testosterone** — androgen markers relevant to PCOS evaluation
- **Thyroid panel (TSH, fT3, fT4)** — thyroid dysfunction is a common cause of cycle irregularity
- **Prolactin** — elevated levels can disrupt cycles

## Hormonal context by cycle phase

Understanding which phase you're in when labs are drawn or symptoms occur is essential for accurate interpretation:

| Phase | Approx. Days | Hormonal Profile | Common Symptoms |
|-------|-------------|------------------|-----------------|
| **Menstrual** | 1-5 | Low estrogen, low progesterone | Cramps, fatigue, mood dip |
| **Follicular** | 1-13 | Rising estrogen, low progesterone | Increasing energy, improved mood |
| **Ovulatory** | ~14 | Estrogen peak, LH surge | Peak energy, possible ovulation pain |
| **Luteal** | 15-28 | High progesterone, moderate estrogen | PMS symptoms, sleep changes, mood shifts |

Lab values — especially estradiol, progesterone, and LH — must be interpreted relative to cycle day. A progesterone of 0.5 ng/mL is normal on day 3 but indicates anovulation if drawn on day 21.

## Cross-referencing with PHG

- **Cycle length variability + thyroid labs** — irregular cycles are a common early sign of thyroid dysfunction
- **Luteal phase deficiency + progesterone levels** — short luteal phase (<10 days) may indicate insufficient progesterone
- **PMS severity + supplement changes** — magnesium, B6, vitex, and calcium interventions tracked in SUPPLEMENTS.md
- **Cycle-specific HRV patterns** — many wearables show measurable HRV variation across cycle phases; PATTERN_DETECTION can identify this
- **Training load + cycle phase** — exercise performance and recovery vary across phases; correlate with PROTOCOLS.md and wearable strain data
- **Acne patterns + androgen labs** — cyclical acne tracked in symptoms/ correlated with DHEA-S and testosterone in LABS_HISTORY.md
