---
name: interview-logger
description: Use when logging interview notes, recording interview outcomes, scheduling follow-ups, or updating application status after an interview round
---

# Interview Logger

## When to Use

Use this skill when the user:
- Just finished an interview and wants to log it
- Says: "log interview", "interview notes", "had my interview with X", "round N was today"
- Wants to schedule or record an upcoming interview
- Describes how an interview went ("it went well", "technical was hard")
- Wants to see notes from past interviews

## Overview

Creates interview documentation from templates, updates application status across all tracking files, and helps schedule follow-ups.

## File Conventions

- **Interview file:** `interviews/{company}-round{N}-{type}.md`
- **Interview types:** `hr-phone`, `technical`, `system-design`, `behavioral`, `final`
- **Template:** `templates/interview.md`
- **Date format:** `D/M/YYYY`

## Workflow

### Log a Completed Interview

1. **Collect info** (step by step):
   - Company name
   - Round number (auto-detect: count existing interview files for this company + 1)
   - Interview type (hr-phone / technical / system-design / behavioral / final)
   - Date and time
   - Format (video / phone / onsite)
   - Interviewer name(s)
   - Duration
   - How it went (brief summary)
   - Key notes: questions asked, responses, red flags, positive signals

2. **Create interview file:**
   - Copy `templates/interview.md` to `interviews/{company}-round{N}-{type}.md`
   - Fill in all provided details
   - Add questions asked and notes to appropriate sections

3. **Update application status:**
   - Read current status from `tracker.md`
   - Determine new status based on interview type:
     - `hr-phone` → 📞 Phone Screen
     - `technical` / `system-design` → 💻 Technical Interview
     - `behavioral` / `final` → 🏢 Onsite / Final Round
   - Update `tracker.md`: status emoji, Last Update date, notes
   - Move entry in `pipeline.md` to correct section
   - Add row to Status Log in application file: `| Date | Interview — Round N (type) | Notes |`

4. **Suggest next steps:**
   - Based on interview type, suggest likely next round
   - Suggest follow-up email timing
   - Flag any concerns from notes

5. **Show summary** and confirm before applying.

### Schedule an Upcoming Interview

1. **Collect:** company, round, type, date/time, format, interviewer
2. **Create interview file** with preparation sections filled, interview sections left blank
3. **Update tracker.md** notes column with upcoming interview info
4. **Show summary** and confirm.

## Interview Type Mapping

| Type | Status | Typical Next Round |
|------|--------|-------------------|
| hr-phone | 📞 Phone Screen | technical or system-design |
| technical | 💻 Technical Interview | system-design or behavioral |
| system-design | 💻 Technical Interview | behavioral or final |
| behavioral | 🏢 Onsite / Final Round | final or offer |
| final | 🏢 Onsite / Final Round | offer |

Use this mapping to suggest what comes next, but don't assume — let the user confirm.

## Guided Flow Pattern

Example prompt flow:
> "Which company?"
> "What round is this? (I see 2 previous interviews — so round 3?)"
> "What type? (hr-phone / technical / system-design / behavioral / final)"
> "How did it go? Tell me about it..."

Then show:
> **Ready to log interview:**
> - Create: `interviews/acme-round3-system-design.md`
> - Update: `tracker.md` → 💻 Technical Interview
> - Update: `pipeline.md` → move to Technical section
> - Update: `applications/acme-team-lead.md` → add Status Log entry
>
> Proceed?

## Edge Cases

- **No application exists yet:** Warn user, suggest creating application first or proceed anyway
- **Round number mismatch:** Auto-detect from existing files, let user override
- **Multiple interviews same day:** Use round number + type to differentiate
- **Interview went badly:** Note concerns, don't auto-update status downward — let user decide
