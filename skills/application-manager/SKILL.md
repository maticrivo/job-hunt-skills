---
name: application-manager
description: Use when adding, updating, or closing job applications, or when application files need to be created from templates and tracking files need to be synchronized
---

# Application Manager

## When to Use

Use this skill when the user:
- Wants to add a new job application
- Needs to update application status (e.g., "mark ACME as rejected")
- Wants to close or withdraw from an application
- Says: "new application", "add job", "close this one", "update status", "withdraw from X", "mark X as rejected/ghosted"
- Asks to create company or application files

## Overview

Manages the full lifecycle of job applications: creating files from templates, keeping tracker.md and pipeline.md in sync, and closing applications properly.

## File Conventions

All paths relative to repo root. Follow these conventions exactly:

- **Application:** `applications/{company}-{role-slug}.md` (lowercase, hyphens, slugify role)
- **Company:** `companies/{company}.md` (lowercase, hyphens)
- **Template sources:** `templates/application.md`, `templates/company.md`
- **Date format:** `D/M/YYYY` (e.g., `27/3/2026`)
- **Status emojis:** 📝 Applied · 📞 Phone Screen · 💻 Technical Interview · 🏢 Onsite · ✅ Offer · ❌ Rejected · 🚫 Withdrawn · 👻 Ghosted

## Workflows

### Add Application

1. **Collect info** (ask step by step):
   - Company name
   - Role title
   - Date applied (default: today)
   - Source (LinkedIn / referral / company site / recruiter)
   - Notes (optional)

2. **Check/create company file:**
   - If `companies/{company}.md` doesn't exist → create from `templates/company.md`
   - Replace `{Company}` with actual name

3. **Create application file:**
   - Copy `templates/application.md` to `applications/{company}-{role-slug}.md`
   - Replace `{Company}` and `{Role}` placeholders
   - Fill in Date Applied, Source, and any provided notes

4. **Update tracker.md:**
   - Add row to Active Applications table: `| Company | Role | Date | 📝 Applied | Date | Notes |`

5. **Update pipeline.md:**
   - Add entry under "📝 Applied" section
   - Update `_Last updated: D/M/YYYY_` at bottom

6. **Update stats** in tracker.md:
   - Increment Total Applied, Active
   - Recalculate Response Rate

7. **Show summary** of all changes and confirm before applying.

### Update Status

1. **Find application** in tracker.md Active table
2. **Collect:** new status, notes about the update
3. **Update tracker.md:**
   - Change Status column emoji
   - Update Last Update date
   - Update Notes column
4. **Update pipeline.md:**
   - Remove from current section
   - Add to new section (Applied → Phone Screen → Technical → Onsite → Offers)
5. **Update application file:**
   - Add row to Status Log table: `| Date | Event | Notes |`
6. **Show summary** and confirm.

### Close Application

1. **Find application** in tracker.md
2. **Collect:** outcome (Rejected / Withdrawn / Ghosted), notes
3. **Update tracker.md:**
   - Remove from Active Applications table
   - Add to Closed Applications table with outcome
   - Update stats (decrement Active, increment Rejections or keep as-is)
4. **Update pipeline.md:**
   - Remove entry entirely
   - Update last updated date
5. **Update application file:**
   - Add final entry to Status Log
6. **Show summary** and confirm.

## Guided Flow Pattern

For all operations, follow this pattern:

1. **Ask** for required info one piece at a time
2. **Draft** all file changes and show to user
3. **Confirm** — wait for user approval
4. **Apply** — make all changes
5. **Verify** — read back what changed

Example prompt flow:
> "What company is this for?"
> "What's the role title?"
> "When did you apply? (press Enter for today)"
> "Any notes to add?"

Then show:
> **Ready to add application:**
> - Create: `applications/acme-team-lead.md`
> - Update: `tracker.md` (add row to Active)
> - Update: `pipeline.md` (add to Applied section)
> - Update: stats (+1 applied)
>
> Proceed?

## Edge Cases

- **Company already exists:** Skip company file creation, just create application
- **Multiple roles at same company:** Create separate application files with distinct role slugs
- **Role slug conflicts:** Append a descriptor (e.g., `acme-team-lead-backend` vs `acme-team-lead-frontend`)
- **Moving from Applied to Offer:** Also consider adding to Offers section in pipeline.md with special note
