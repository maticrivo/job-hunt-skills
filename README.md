# Job Hunt Tracker

A markdown-based job hunt tracking system with 4 AI agent skills that manage your entire job search pipeline — applications, interviews, pipeline health, and progress reports.

Works with **OpenCode**, **Claude Code**, **Cursor**, **Gemini CLI**, **Codex**, and [40+ agents](https://github.com/vercel-labs/skills#supported-agents).

## What This Is

A set of skills that turn your AI agent into a job hunt assistant. Instead of manually updating spreadsheets and tracking docs, tell your agent:

- *"Add a new application for ACME as Team Lead"*
- *"Log my interview with Globex — round 3, system design"*
- *"What's stale in my pipeline?"*
- *"Give me my weekly report"*

The skills manage markdown files (tracker, pipeline, application docs, interview notes) and keep everything in sync.

## Install

```bash
# Install all tracking skills
npx skills add maticrivo/job-hunt-skills --all

# Install a specific skill
npx skills add maticrivo/job-hunt-skills --skill application-manager

# List available skills
npx skills add maticrivo/job-hunt-skills --list
```

## Skills

### application-manager
Manages the full lifecycle of job applications — adding new ones, updating status, closing/withdrawing. Creates files from templates and keeps `tracker.md` and `pipeline.md` in sync.

### interview-logger
Logs completed interviews, creates interview documentation, and automatically updates application status across all tracking files. Suggests follow-ups and next steps.

### pipeline-monitor
Scans your tracking files to detect stale applications, flag follow-up needs, and identify pipeline imbalances. Generates health reports with actionable suggestions.

### status-reporter
Generates comprehensive progress reports — weekly summaries, pipeline analytics, activity breakdowns, top prospects, and action items.

## File Structure

The skills expect (and help you create) this structure in your project:

```
your-project/
├── tracker.md                  # Master table of all applications
├── pipeline.md                 # Visual pipeline by stage
├── applications/               # One file per application
│   └── acme-team-lead.md
├── interviews/                 # Interview notes per round
│   └── acme-round1-technical.md
├── companies/                  # Company research
│   └── acme.md
└── offers/                     # Offer details
```

Each skill includes its own `resources/` directory with templates (e.g., `application-manager/resources/application.md`). These are installed automatically with the skill.

### Status Emojis

| Emoji | Meaning |
|-------|---------|
| 📝 | Applied |
| 📞 | Phone Screen |
| 💻 | Technical Interview |
| 🏢 | Onsite / Final Round |
| ✅ | Offer |
| ❌ | Rejected |
| 🚫 | Withdrawn |
| 👻 | Ghosted |

## Companion: Resume Skills

These tracking skills pair well with [Paramchoudhary/ResumeSkills](https://github.com/Paramchoudhary/ResumeSkills) for the resume and application writing side of the job search. Install both:

```bash
# Tracking (this repo)
npx skills add maticrivo/job-hunt-skills --all

# Resume optimization
npx skills add Paramchoudhary/ResumeSkills --all
```

Together they cover the full job search workflow: resume optimization → application tracking → interview management → progress reporting.

## How It Works

Each skill is a `SKILL.md` file with instructions your AI agent follows when activated. Skills are loaded on-demand — no context overhead until you use them.

Typical workflow:

1. **Add** an application with `application-manager`
2. **Log** interviews with `interview-logger`
3. **Check** what needs attention with `pipeline-monitor`
4. **Review** progress with `status-reporter`

## Contributing

PRs welcome. Skills are markdown files — easy to edit and improve.

## License

MIT
