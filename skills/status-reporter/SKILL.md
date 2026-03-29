---
name: status-reporter
description: Use when generating job hunt progress reports, weekly summaries, pipeline analytics, or asking how the job search is going overall
---

# Status Reporter

## When to Use

Use this skill when the user:
- Wants a summary of their job hunt progress
- Says: "weekly summary", "hunt status", "how's my job search?", "progress report", "dashboard"
- Asks for analytics or trends
- Wants to compare this week vs last week
- Needs a formatted report to share or review

## Overview

Reads tracker.md, pipeline.md, and interview files to generate comprehensive progress reports. Purely read-only — generates reports, doesn't modify files.

## Data Sources

- `tracker.md` — stats, active/closed applications, status
- `pipeline.md` — current pipeline stages
- `interviews/*.md` — interview history and dates
- `applications/*.md` — status logs for detailed timeline

## Report Sections

### 1. Summary Stats

Pull from `tracker.md` Stats section, supplemented with computed data:
- Total Applied
- Active Applications
- Offers / Rejections / Withdrawn
- Response Rate
- Interviews Completed
- Interviews Upcoming

### 2. Pipeline Breakdown

Count from `pipeline.md` sections:
```
📝 Applied:        N  (X%)
📞 Phone Screen:   N  (X%)
💻 Technical:      N  (X%)
🏢 Onsite:         N  (X%)
✅ Offers:         N  (X%)
```

### 3. Activity This Week

Count events from Status Logs in application files where date falls within current week:
- Applications submitted
- Interviews completed
- Status changes
- Follow-ups sent

### 4. Top Prospects

Rank active applications by signal strength:
- **Hot:** Recently interviewed, awaiting next steps, positive signals
- **Warm:** Active communication, interviews scheduled
- **Cool:** Applied, no movement
- **Cold:** 14+ days no update

### 5. Upcoming

From interview files with future dates:
- Interviews in next 7 days
- Follow-ups due
- Deadlines approaching

### 6. Action Items

Based on analysis:
- Applications needing follow-up
- Interviews to prepare for
- Status updates to log
- Offers to evaluate

## Output Format

```markdown
# Job Hunt Report — Week of D/M/YYYY

## Summary

| Metric | Count |
|--------|-------|
| Total Applied | 10 |
| Active | 10 |
| Interviews Completed | 8 |
| Upcoming Interviews | 3 |
| Offers | 0 |
| Rejections | 0 |
| Response Rate | 100% |

## Pipeline

| Stage | Count | % |
|-------|-------|---|
| 📝 Applied | 4 | 40% |
| 📞 Phone Screen | 2 | 20% |
| 💻 Technical | 4 | 40% |
| 🏢 Onsite | 0 | 0% |

## This Week's Activity

- **Applications:** 3 new (ACME, Globex, Initech)
- **Interviews:** 2 completed (Globex VP R&D, Initech Technical)
- **Status Changes:** Globex → Technical, Initech → Technical

## Top Prospects

1. 🔥 **ACME** — System Design on 31/3, strong momentum
2. 🔥 **Globex** — Met VP R&D, 3 days office, good signal
3. 🟡 **Initech** — Technical done, awaiting next steps
4. 🟡 **Umbrella** — Technical on 31/3, "not fully convinced" note

## Upcoming

| Date | Company | Event |
|------|---------|-------|
| 30/3 | Hooli | Phone Screen at 12:15 |
| 31/3 | ACME | System Design at 14:30 |
| 31/3 | Umbrella | Technical at 16:00 |
| 1/4 | Globex | HR Screening at 10:00 |

## Action Items

1. ☐ Prepare for ACME System Design (31/3)
2. ☐ Follow up with Globex (applied 25/3, no response)
3. ☐ Prepare for Umbrella technical (31/3)
4. ☐ Log Hooli phone screen after 30/3
```

## Report Variants

- **Quick status:** Just summary stats + upcoming (30 seconds)
- **Weekly report:** Full report as above
- **Deep dive:** Add per-application analysis from status logs

Ask user which variant they want, or default to weekly report.

## Edge Cases

- **No interviews yet:** Skip interview sections, note "no interviews yet"
- **First week:** Can't compare to last week, just show current state
- **All applications in same stage:** Note pipeline concentration risk
- **No active applications:** Report "pipeline empty — time to apply!"
