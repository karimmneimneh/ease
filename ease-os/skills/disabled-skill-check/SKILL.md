---
name: disabled-skill-check
description: "Checks whether a disabled or deleted skill exists for a task before improvising. Triggers when: the current task would clearly benefit from a specialised skill, no enabled skill in the available list matches it and Claude is about to improvise. Reads about-me/disabled-skills.md, scans for a fit and surfaces the match before proceeding."
---

# Disabled Skill Check

Before rebuilding work that a skill already does, check whether that skill is simply off.

---

## When this skill activates

All three conditions must be true:

1. The current task would clearly benefit from a specialised skill (not general writing or analysis).
2. No enabled skill in the available list matches the task.
3. About to improvise, i.e., proceeding without a dedicated skill.

If all three are true, do not proceed yet. Run this check first.

---

## Step 1: Read the data source

Read `about-me/disabled-skills.md`. If the file does not exist, there is nothing switched off to find: carry on with the task silently and say nothing about it.

This file lists every skill that exists but is currently unavailable, in two states:

- **DISABLED**: installed but switched off. Re-enable via the Skills UI (toggle). Fast.
- **DELETED**: removed entirely. Reinstall the plugin that provided it (Customize, then Plugins). Slightly slower.

The runtime does not expose either state. Disabled and deleted skills simply do not appear in the available list. This file is the only way to know they exist.

---

## Step 2: Scan for a fit

Scan both tables for any skill whose "What it does" (or, for deleted skills, "What it did") entry matches the current task.

Match on purpose, not on name. A task about briefing a photo shoot matches a `photography-brief` skill even if the word "brief" wasn't used.

---

## Step 3: Surface the match

If a relevant skill is found, stop and tell the user before proceeding:

> "There's a [disabled / deleted] skill that fits this: `<skill-name>`. Want to [enable it / reinstall it]?"

Include one line on what the skill does and what state it is in (disabled = fast toggle, deleted = reinstall the plugin that provided it).

Wait for a response before proceeding. If the user says yes, they will enable or reinstall the skill and then restart the task in a new session. If the user says no or to proceed anyway, improvise as normal.

---

## Step 4: If no match is found

If no disabled or deleted skill fits the task, proceed with the task as normal without mentioning this check.

Do not announce that you ran the check and found nothing, as that adds noise. Only surface findings, not empty scans.

---

## What this skill does not do

- Does not activate on routine tasks that clearly have no dedicated skill (e.g., scheduling a post, editing a sentence).
- Does not load `about-me/disabled-skills.md` at session start. Load on demand only, when all three trigger conditions are met.
- Does not attempt to replicate the disabled skill's logic. Surfaces the option and waits.
- Does not suggest enabled skills that are already visible in the available list.
