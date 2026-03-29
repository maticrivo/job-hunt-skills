---
name: pipeline-monitor
description: Use when checking for stale applications, reviewing follow-up needs, scanning for ghosted applications, or assessing pipeline health
---

# Pipeline Monitor

## When to Use

Use this skill when the user:
- Wants to know what needs attention
- Says: "what's stale?", "any follow-ups needed?", "who haven't I heard from?"
- Asks about pipeline health or distribution
- Wants to identify ghosted applications
- Needs proactive alerts about application deadlines

## Overview

Scans tracker.md, pipeline.md, and interview files to detect stale applications, flag follow-up needs, and identify pipeline issues. Read-only by default — only modifies files if explicitly requested.

## Analysis Steps

### 1. Stale Detection

Read `tracker.md` Active Applications table. For each row:
- Parse "Last Update" date
- Compare to today's date
- Flag rules:
  - **Applied status + 14+ days no update** → 👻 Potential Ghosted
  - **Any status + 7+ days no update** → ⚠️ Stale — follow-up suggested
  - **Applied status + 7-13 days** → 📋 Monitor — follow up at 14 days

### 2. Upcoming Deadlines

Scan `interviews/` directory for files with future dates in the Interview Info section. List any interviews scheduled within the next 7 days.

### 3. Pipeline Distribution

Count applications per stage from `pipeline.md`:
```
📝 Applied:        N
📞 Phone Screen:   N
💻 Technical:      N
🏢 Onsite:         N
✅ Offers:         N
```

Flag imbalances:
- Too many in "Applied" with no movement → suggest follow-ups
- All in early stages → pipeline may stall soon
- Multiple in "Technical/Onsite" → busy period, prioritize prep

### 4. Follow-Up Suggestions

For each stale application, suggest action:
- **Applied + 7-14 days:** Send follow-up email (reference `resources/follow-up.md`)
- **Applied + 14+ days:** Consider marking as Ghosted
- **Phone Screen + 7+ days:** Follow up on next steps
- **Technical + 7+ days:** Check on timeline

## Output Format

```markdown
## Pipeline Health Report

**Date:** 27/3/2026
**Total Active:** 10

### 🚨 Needs Attention

| Company | Status | Last Update | Days Stale | Action |
|---------|--------|-------------|------------|--------|
| ACME | 📝 Applied | 25/3/2026 | 2 | Monitor |
| Globex | 📞 Phone Screen | 26/3/2026 | 1 | OK |

### 📅 Upcoming

| Company | Event | Date/Time |
|---------|-------|-----------|
| ACME | System Design | 31/3 at 14:30 |
| Globex | Technical | 31/3 at 16:00 |

### 📊 Distribution

- 📝 Applied: 4
- 📞 Phone Screen: 2
- 💻 Technical: 4
- 🏢 Onsite: 0

### 💡 Suggestions

1. Follow up with ACME (applied 25/3, no response yet)
2. Prepare for ACME system design on 31/3
3. Check Globex status (technical done, awaiting next steps)
```

## Guided Flow

This skill is primarily read-only. When follow-ups are suggested:
1. Show the report
2. Ask if user wants to generate a follow-up email (using `resources/follow-up.md`)
3. Ask if any statuses need updating (hand off to `application-manager` if so)

## Edge Cases

- **No Last Update date:** Treat as applied date if available, otherwise flag as unknown
- **Missing files:** Skip gracefully, note which files couldn't be read
- **Interview files without dates:** Note but don't crash
- **All applications recent:** Report "all clear, nothing stale"
