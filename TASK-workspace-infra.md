# TASK: Build Workspace Data Infrastructure

## Objective
Set up the data layer for Zephon's command center. Everything runs locally in the workspace directory. No external services, no paid tools.

## 1. Directory Structure

Create the following under your workspace:

- data/calendar.json — Meetings, earnings dates, deadlines
- data/pipeline.json — Kanban task tracking
- data/sources.db — SQLite source/reference database
- files/inbox/ — New files to process
- files/active/ — Work in progress
- files/review/ — Awaiting Ernest's review
- files/archive/ — Completed work
- templates/telegram-briefing.md — Standard Telegram output format
- agents/greg/ — Future: Greg's workspace

## 2. Calendar (calendar.json)

JSON array of events. Zephon manages this — adds, updates, removes entries.

Each entry has these fields:
- id: unique string
- title: event name
- date: YYYY-MM-DD
- time: HH:MM
- timezone: UTC
- type: one of earnings, meeting, deadline, reminder
- company: ticker or name (null if not company-specific)
- notes: free text for context
- recurring: boolean
- alert: how far ahead to remind (e.g. "1d", "2h")

Types:
- earnings — Company earnings calls and report dates
- meeting — Internal or external meetings
- deadline — Report due dates, quarterly deck deadlines
- reminder — Proactive nudges from Zephon

## 3. Pipeline / Kanban (pipeline.json)

JSON array tracking all tasks across the team.

Each task has these fields:
- id: unique string
- title: task description
- status: one of backlog, active, review, done
- assignee: zephon, greg, or ernest
- priority: high, medium, or low
- created: YYYY-MM-DD
- updated: YYYY-MM-DD
- due: YYYY-MM-DD or null
- workstream: one of coverage, inventory-deck, adi-satellite, news, ai-strategy
- notes: free text
- files: array of file paths
- source_refs: array of source IDs from sources.db

Statuses:
- backlog — Identified but not started
- active — Currently being worked on
- review — Output ready, awaiting Ernest's review
- done — Completed and delivered

## 4. Sources Database (sources.db)

SQLite database with two tables.

Table "sources" columns:
- id (TEXT PRIMARY KEY)
- title (TEXT NOT NULL)
- type (TEXT NOT NULL) — one of: earnings_call, sec_filing, research_note, expert_call, news, technical_paper, conference, internal
- company (TEXT) — ticker or name, NULL if industry-wide
- date (TEXT NOT NULL) — YYYY-MM-DD
- author (TEXT)
- url (TEXT)
- file_path (TEXT)
- summary (TEXT) — 2-3 sentence summary
- tags (TEXT) — comma-separated
- added_by (TEXT DEFAULT 'zephon')
- added_at (TEXT DEFAULT datetime('now'))
- verified (INTEGER DEFAULT 0) — 0 = unverified, 1 = Ernest confirmed

Table "citations" columns:
- id (TEXT PRIMARY KEY)
- source_id (TEXT NOT NULL, FOREIGN KEY references sources.id)
- claim (TEXT NOT NULL) — the specific datapoint or claim
- context (TEXT) — where this was used
- cited_at (TEXT DEFAULT datetime('now'))

## 5. Telegram Briefing Template

Save as templates/telegram-briefing.md with this format:

[HEADLINE]
Why it matters:
- Point 1 (concise)
- Point 2 (concise)
- Point 3 if needed

Next action or source link

## 6. Initialization

After creating everything:
- Populate calendar.json with known upcoming earnings dates for all FAF coverage companies: NVIDIA, AMD, Broadcom, MediaTek, Marvell, Skyworks, Qorvo, UMC, GlobalFoundries, Intel, TSMC, Credo, ARM, and ADI
- Create pipeline.json with Ernest's current workstreams as initial tasks (reference USER.md for the list)
- Initialize sources.db with both tables created
- Confirm all paths exist and are readable/writable

## 7. Validation

Run and report:
- Directory structure is correct
- calendar.json is valid JSON with earnings dates populated
- pipeline.json is valid JSON with initial tasks
- sources.db has both tables created
- templates/telegram-briefing.md exists

Report back when done.
