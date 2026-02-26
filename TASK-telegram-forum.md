# TASK: Set Up Telegram Forum Command Center

## Objective
Transform Telegram from a single chat into an organized command center using a Forum Supergroup. Each topic becomes a dedicated workspace with isolated context. This replaces the need for a web dashboard in the short term.

## Overview

Ernest will create the Telegram forum group and add you (@ZephonBot) manually. Once added, you will configure the topics, update OpenClaw config for topic routing, set up cron jobs for proactive messaging, and test everything end-to-end.

## Step 1: Ernest Creates the Forum Group (Manual)

Ernest has already completed this step. The forum group exists and you have been added as admin.

## Step 2: Zephon Discovers Group and Topic IDs

1. Check the OpenClaw gateway logs for incoming messages from the forum group
2. Extract the group ID (will be a negative number like -100XXXXXXXXXX) and each topic ID (integer)
3. Record them in a new workspace file: data/telegram-forum.json

Schema for telegram-forum.json:
{
  "groupId": "-100XXXXXXXXXX",
  "groupName": "Arete FAF Command Center",
  "topics": {
    "pipeline": { "id": 0, "label": "Pipeline", "purpose": "Task status, pipeline updates, kanban queries" },
    "news": { "id": 0, "label": "News", "purpose": "Daily news briefings, breaking news alerts" },
    "research": { "id": 0, "label": "Research", "purpose": "Deep analysis requests and output" },
    "calendar": { "id": 0, "label": "Calendar", "purpose": "Earnings dates, meetings, deadlines" },
    "alerts": { "id": 0, "label": "Alerts", "purpose": "Proactive notifications, reminders, flagged items" },
    "general": { "id": 0, "label": "General", "purpose": "Catch-all, ad hoc questions, admin" }
  }
}

Commit this file to the workspace.

## Step 3: Configure OpenClaw for Forum Group

Update the OpenClaw config to handle the forum group properly:

1. Set requireMention to false for this group (so you respond to all messages without needing @ZephonBot)
2. Ensure each topic creates an isolated session
3. Keep DM access working as before (nothing changes for direct messages)

Use the openclaw config CLI to apply these settings. The group config goes under channels.telegram.groups.

## Step 4: Set Up Topic-Specific Behavior

Each topic should have a focused context:

Pipeline - When messaged here, read data/pipeline.json and respond with task status. Support commands like: "what's active?", "move X to review", "add task: [description]". Update pipeline.json when tasks are moved or created.

News (Future: Greg's territory) - For now, Zephon handles this. Focus on semiconductor news and industry updates. Use the telegram-briefing.md template for all posts.

Research - Deep analysis requests. Responses can be longer and more detailed. Always cite sources, link to sources.db entries. If analysis requires multiple steps, acknowledge receipt and provide updates.

Calendar - Read data/calendar.json. Support: "what's coming up this week?", "next earnings?", "add meeting: [details]". Update calendar.json when events are added or modified.

Alerts - Primarily for proactive outbound messages. Earnings reminders (1 day before), deadline warnings, breaking news flags, pipeline items stuck in review.

General - Catch-all for anything that doesn't fit the other topics.

## Step 5: Set Up Cron Jobs for Proactive Messaging

Configure these scheduled jobs:

Morning Brief (Daily, 06:30 SAST / 04:30 UTC, weekdays only) - Post to Calendar topic: today's scheduled events, deadlines within 3 days, pipeline items needing attention. Use telegram-briefing.md template.

Earnings Reminder (Daily check, 06:00 UTC) - Post to Alerts topic: check calendar.json for earnings calls within next 24 hours. Only post if there is actually an upcoming event.

Weekly Pipeline Review (Monday, 07:00 SAST / 05:00 UTC) - Post to Pipeline topic: tasks completed last week, current active tasks, items stuck in backlog or review for more than 5 days, suggested priorities for the week.

## Step 6: Test Everything

After setup:
1. Post a test message in each topic and confirm you respond with topic-appropriate context
2. Verify DMs still work normally
3. Confirm topic IDs are correctly recorded in telegram-forum.json
4. Verify cron jobs are registered
5. Manually trigger one cron job to test the output format

## Step 7: Update SOUL.md

Add a section documenting the Telegram forum structure: the forum group layout, topic purposes, which topics are inbound vs outbound, reference to telegram-forum.json, and note that Greg will take over News when onboarded.

## Deliverables

Report back with:
1. telegram-forum.json populated with all topic IDs
2. OpenClaw config updated for forum group
3. Cron jobs registered and listed
4. Test results from each topic
5. SOUL.md updated with forum documentation
6. Git commits for all workspace file changes
