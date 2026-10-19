---
name: ease-setup
description: "Safely optimises a Cowork workspace for the EASE OS kit (sets up what is missing and upgrades what is already there) by comparing the kit against what the user already has. Use when the user says 'optimise my workspace', 'optimize my workspace', 'optimise my Cowork', 'upgrade my workspace', 'compare the kit to what I have' or runs this after installing the kit. Always assumes the user already has a system; never resets or wipes anything. Inventories what exists, preserves everything good, proposes only genuine improvements (missing files or missing sections) for the user's approval item by item and never deletes or overwrites anything without an explicit yes and a backup."
---

# EASE Setup

Compares this kit against whatever the user already has, preserves everything that is theirs and proposes only improvements, for approval. It never assumes a blank slate, never resets and never deletes.

## Step 0 - Operating rules (non-negotiable)

1. **Always assume the user already has a Cowork system.** Do NOT ask "fresh or existing". Do NOT offer any option that could reset or reinitialise their workspace. There is no "start fresh" path.
2. **Preserve by default.** The user's existing files and content always win. Your normal actions are: (a) ADD a file or folder they are missing, or (b) ADD a missing section into one of their files. Both only after they approve. The one exception is (c): replacing existing content, which happens only when the user asks for it, under rule 3.
3. **Never delete, reset or overwrite.** If a proposed change would replace any of their existing content, you must show a full before/after, get an explicit yes for that specific item and first copy the original to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup). You never remove the backup.
4. **Additive, not destructive.** Enhancements add what is missing; they never reorder, rewrite or strip the user's own content. Before proposing any section, check it does not contradict a rule already in their file; if it does, keep theirs and mention the difference instead of adding a second, conflicting rule.
5. **Everything is shown and approved item by item.** No blanket "apply all" that includes a replacement. The user sees exactly what each change does before it happens.
6. **Check impact before proposing any change to what exists.** Before proposing a change that touches their existing structure or an existing file, first look at what depends on the current state: other skills (their `reads:`), scheduled tasks (their prompts may reference paths) and files that point at it. Tell the user in plain terms what the change touches and confirm it is additive and will not break those dependencies. If a change could affect something they rely on, flag that clearly and let them decide. Never propose a structural change without saying how it affects what they already have. Adding a brand-new file or folder they were missing has no impact and is always safe to offer.

## Step 0.5 - Make sure a working folder is selected

EASE OS lives in the user's working folder, so check that this chat has one selected before doing anything else. If none is selected, stop and ask the user to select the folder they want EASE OS to live in (a new, dedicated folder is best; if they do not have one, suggest creating one, for example "Claude Work" in Documents, in Finder or File Explorer). Tell them to select that same folder whenever they start a chat. If the selected folder is a broad location (a whole Desktop, Documents, Downloads or home folder), say in one line that the kit would scatter its files through everything in there, and recommend creating or picking a dedicated folder instead; continue in the broad folder only if the user says so explicitly. Continue only once a folder is selected. Write nothing before this.

## Step 1 - Inventory what they already have (read only)

Look at the workspace without changing anything:
- Does `CLAUDE.md` exist, and which of these sections does it already have: a session-start list (and whether it names `about-me/how-claude-works-with-me.md` and `about-me/verification-rules.md`), a routing-map table (and whether it has the "turn this into a skill", "add this to my inbox" and "pause this" rows), memory-ceiling guidance, a safety rule (and whether it includes the line about never changing `CLAUDE.md` without showing the change first and the line about approving copy on its full text), a workspace-folders description, a working-preferences section (and whether it includes the pointer to `about-me/how-claude-works-with-me.md`, a rule for big jobs that span several chats, a sparring-partner line and a weekly-review line; note any line in it about reply length or style that could disagree with that file) and a continuous-learning section (and whether it includes a session-end close that offers an audit or a next-step handoff prompt, the routes to `about-me/how-claude-works-with-me.md` and `about-me/verification-rules.md` and the line that offers a skill when something repeats)?
- Which `about-me/` files exist (`about-me.md`, `how-claude-works-with-me.md`, `writing-rules.md`, `verification-rules.md`, `memory.md`, `custom-workflows.md`, `disabled-skills.md`), and does each have real content or is it empty/absent? Note whether `about-me.md` has Areas of work and Goals sections and whether `custom-workflows.md` has a Skill ideas table.
- Which folders exist: `outputs/`, `workstations/`, `projects/`, `inbox/`, `library/`, `_archive/`?
- Does the folder hold files from the EASE Day One starter kit (`rules.md`, `notes.md` and a progress file named `ease-day-one-progress.md`, or `cowork-day-one-progress.md` if it was made before the kit was renamed from Cowork Day One)? If so, read them. Their safety rules and notes are the user's own content: in Step 2, propose carrying each rule into the matching `CLAUDE.md` section (Safety, Working preferences) and each note into `about-me/memory.md` as an Enhance item, showing the exact text (apply the kit's one test for "full" first: if the file's line count plus the notes waiting would reach or pass its ceiling, carry over only those that fit and suggest "tidy my memory for about-me/memory.md" for the rest), and skip any rule the kit's template already states, any line starting "(example" or "- (example", any italic placeholder line (one starting with "_(") and the kit's own pointer lines ("New notes now go to", "These rules now live in"). Leave the Day One files themselves in place. Then offer, as two separate items, to add one line at the top of `notes.md` ("New notes now go to about-me/memory.md") and one at the top of `rules.md` ("These rules now live in CLAUDE.md; edit them there"), so the user knows which file is live and the two copies cannot drift apart unnoticed; add each only on its own yes.
- Among the skills currently available to you, are any named the same as this kit's skills (ease-setup, workspace-hygiene, memory-router, new-project-setup, inbox-triage, correspondence-log, disabled-skill-check, sparring-partner, session-audit, session-checkpoint, consolidate-memory, orientation, personalise, output-sharpen, skill-builder, goals-review)? Note them; you will not change them.

## Step 2 - Compare, component by component

For each thing this kit provides, classify it as exactly one of:
- **Keep yours** - they already have it with real content that covers what the kit's version does, or it is a kit template already in place (blank or template-only, perhaps from an earlier run). Preserve it untouched; filling a blank about-me, how-claude-works-with-me or writing-rules file is the personalise skill's job (Step 5), not this table's.
- **Add** - they are missing it entirely. Propose adding it (from the Templates section).
- **Enhance** - they have the file, but it lacks a useful section the kit's template has, or a single line the kit relies on inside a section they already have (for example the sparring-partner line in Working preferences), shown in full and added on its own yes, or lines the user's own Day One files hold (Step 1) (for example, their `CLAUDE.md` has no routing-map table, or their `writing-rules.md` has no self-description-guardrail heading). Propose adding only that missing section, or those missing lines into the matching existing section, into their file.

How to judge: you do NOT silently decide the user's version is worse. Preserve theirs by default. Only call something an improvement when the kit genuinely adds a capability or a structure the user's file is missing. If theirs already has equivalent content in a different shape, keep theirs and say so. If a line they have disagrees with a kit file they are about to receive (for example a Working preferences line saying "keep answers short" next to a reply-length choice in `about-me/how-claude-works-with-me.md`), keep theirs and name the difference in one line, so they can decide which one stands. When unsure, preserve and merely mention the option.

Same-named skills: if the user already has a skill with the same name as one of this kit's, do not touch either. Flag it: both will be available, and the user can disable whichever they prefer. If they come from Cowork OS, this kit's earlier name, say so and suggest uninstalling it instead, as Step 4b does. You cannot judge which is better, so you defer to them.

## Step 3 - Show the comparison and get approval

Present one clear table:

| Component | What you have | What this kit adds | Recommendation |
|---|---|---|---|

If every component is already "Keep yours" (for example on a re-run), say so in one line, skip the table and go straight to the offer in Step 5.

Recommendation is one of: Keep yours, Add (missing), Enhance (add a missing section). For every Add and Enhance, show the exact content you would create or the exact section you would append, so the user sees it before approving. Then ask the user which to apply (AskUserQuestion with the proposed items, or a short numbered list they pick from). Apply only what they approve.

If any item is a true replacement of existing content (rare, only when the user explicitly wants the kit's version instead), show a full before/after, get a separate explicit yes and back up the original to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) before changing it.

## Step 4 - Apply only what was approved

- Create approved missing files and folders from the Templates section.
- Append approved missing sections to existing files, leaving all their existing content in place and in order.
- For any approved replacement: back up first, then apply. Never delete the backup.

## Step 4b - Flag improvements across their wider system (read-only, advisory)

Beyond the kit's own files, take one read-only look across the user's wider Cowork and FLAG (never change) anything the kit could improve:
- **Same-named skills.** If any skill already available to the user shares a name with one of this kit's skills, list them. Both coexist; the user decides which to keep enabled. Change neither. Skills from the earlier version of this kit are covered by the next bullet instead.
- **The earlier version of this kit.** If skills from a plugin named Cowork OS (or cowork-os) are also available, say in one line that it is this kit's earlier name, that the two answer to the same phrases, and suggest uninstalling it (Customize > Plugins > Uninstall). Change nothing.
- **Scheduled tasks.** If the scheduled-tasks tool is available, list the user's tasks read-only. If any are not recorded in a registry (`about-me/custom-workflows.md`), flag that recording new tasks there keeps them documented going forward. Do not modify, pause or delete any task.
- **Structure and MD files.** Note any session-start or routing structure the kit adds that theirs lacks (from Step 2).

Present all of this as suggestions only, never actions. For a full review of their whole workspace (files, memory, scheduled tasks, skills), recommend they run "audit my workspace": the workspace-hygiene skill reports first and changes nothing unless they approve each fix.

## Step 5 - Report, then offer to make it theirs

Show three lists: **Added** (new files/folders), **Enhanced** (sections added, with approval), **Kept untouched** (everything preserved). Confirm in one line that nothing was deleted, reset or overwritten.

Then, if `about-me/about-me.md`, `about-me/how-claude-works-with-me.md` or `about-me/writing-rules.md` is still blank or template-only (the Background or Goals section of `about-me.md`, or Your choices in `how-claude-works-with-me.md`, still holds its placeholder line, one containing `_(`), and those files now exist, offer to make the workspace theirs now with the personalise skill ("make it mine"): a short interview, one question at a time and every question skippable, that fills those files in the user's own words, shows each file before saving and ends with a block to paste into Cowork's Global instructions so the essentials apply in every chat. Ask first; skipping is fine. If they say yes, hand over to the personalise skill. If that skill is not available in this session, say so in one line and point them to the three files to fill by hand whenever they like. If they skip, tell them they can say "make it mine" any time.

End the report with the same line either way: EASE OS lives in this folder, so select this same folder as the working folder whenever you start a chat; then bring a real piece of work (a plan, a draft, some research) and say "remember this" when something matters; the system gets more useful with every real task. Then mention "audit my workspace" as the full health check to run any time: it reports first and changes nothing without their yes on each fix.

## What this skill does not do

- Never asks "fresh or existing" and never offers a reset path.
- Never deletes, resets or overwrites the user's content; replacements require explicit approval and a backup, and the original is never removed.
- Enhancements are additive only. It never invents the user's personal content: filling the profile is the personalise skill's job, which writes only what the user says, shown for approval before saving; if they skip it, the scaffolding stays blank for them to fill.

---

## Templates (the kit's versions - used as the source when ADDING a missing file, or to show the section you propose adding)

### CLAUDE.md
```
# CLAUDE.md: Root
_Last updated: [date]_

Your workspace constitution. Keep it lean; detail lives in the files it points to.

## Session start: read these first
1. `about-me/about-me.md`: who you are, your goals, your audience.
2. `about-me/how-claude-works-with-me.md`: how replies and decisions work.
3. `about-me/writing-rules.md`: how you want things written.
4. `about-me/verification-rules.md`: how to check something is true before acting on it or calling it done.
5. `about-me/memory.md`: index of active projects and recent context.

## Routing map: load on demand
Add a row whenever you create a project or a workstation.

| Task | Load |
|---|---|
| _(example)_ Work on project X | `projects/x/CLAUDE.md` |
| Capturing a thought between sessions | say "add this to my inbox: [thought]" (inbox-triage shows the line, then appends it to `inbox/inbox.md`) |
| Stopping a session with work unfinished | say "pause this" (session-checkpoint saves a checkpoint in `outputs/checkpoints/` and a resume prompt); "resume [thread]" picks it up |
| Something you keep doing the same way | say "turn this into a skill" (skill-builder drafts it into `outputs/skills/`) |
| A new project not listed above | invoke `new-project-setup`, then add its row here |

## Memory ceilings
`about-me/memory.md` 50 lines; `workstations/*/memory.md` 150; `projects/*/memory.md` 200. When a file is full, say "tidy my memory" (consolidate-memory archives the older entries and backs the file up before it changes anything). Never raise the ceiling.

## Workspace folders
`about-me/` who you are and how you work; `workstations/` groups of projects; `projects/` one folder per project; `inbox/` quick capture; `outputs/` everything generated, including `outputs/skills/` for skills you build; `library/` reference material; `_archive/` files moved out of the way instead of deleted. All outputs go in `outputs/`, never the root.

## Safety: non-negotiable
Never delete, send or publish anything without your explicit OK. When in doubt, show and ask.
Never change this file without showing the exact change first and getting a yes.
Anything that will be sent or published is approved on its full text, never on a summary of it.

## Continuous learning
When you correct, refine or redirect during a session, treat it as a signal, not a one-off. Offer to save it to the closest-scoped file: a reply or decision preference to `about-me/how-claude-works-with-me.md`, a writing or voice preference to `about-me/writing-rules.md`, a rule about checking things to `about-me/verification-rules.md`, an identity or goal change to `about-me/about-me.md`, a global rule to `about-me/memory.md`, a workstation rule to that workstation's `memory.md`, a project fact to that project's `memory.md`, a skill you have switched off to `about-me/disabled-skills.md`. Surface the signal rather than waiting to be asked, and write it only once the user says yes. If the user asks for the same thing three times, or a session does the same steps twice, offer to turn it into a skill ("turn this into a skill"). At the end of a session with real work, offer the right close in one line: for a single or open-ended task, offer a quick session audit ("Want me to run a quick session audit before we close?"); for a multi-step task still in progress, offer a choice between the audit (A) and a next-step prompt for a fresh session (B) that points at the plan file and names the next item. The session-audit skill runs the audit and catches anything missed.

## Working preferences
- How replies and decisions should work lives in `about-me/how-claude-works-with-me.md`. Change it there, or say "make it mine".
- **Big jobs that span several chats.** For anything too big for one sitting, keep a written plan in `outputs/plans/[plan-name].md` (the goal, the steps and which are done), created at the start. When a chat ends mid-job, finish with a short prompt for the next chat: the step to do next and the plan file to read. Ordinary tasks just get done in one go.
- Keep a sparring partner's eye on the work: when you see a real risk, a weak assumption or a better option, say so in one line (the sparring-partner skill), and stay quiet when there is nothing worth flagging.
- Once a week, say "weekly review" to check the work against the goals in `about-me/about-me.md`.
- These are sensible defaults; edit them to match how you work.
```

### about-me/about-me.md
```
# About Me
_Last updated: [date]_

## Background
_(A few lines on your work and history.)_

## Current situation
_(What you are focused on right now.)_

## Audience
_(Who your work is for.)_

## Tools I use
_(e.g. Gmail, Drive, Docs. Default outputs to formats that work in these.)_

## Areas of work
_(The two to five main areas you want help with, e.g. clients, job search, a side project.)_

## Goals (next three months)
_(One to three things you most want to move forward, and how you will know each worked. The goals-review skill reads this.)_
```

### about-me/how-claude-works-with-me.md
```
# How Claude works with me
_Last updated: [date]_

How replies and decisions should work. Read at the start of every session. Say "make it mine" to fill in Your choices or change any default.

## Your choices
- Reply length: _(short / medium / detailed)_
- Decisions: _(a multiple-choice question with your recommendation first / one clear recommendation / the options side by side)_
- Small choices inside a task (wording, layout, order): _(ask me / make the call and tell me)_

## Kit defaults (keep, change or delete any)
_These came with the kit. "make it mine" walks through them one by one. Nothing here overrides the Safety section of `CLAUDE.md`: anything that changes a file, sends, publishes or deletes still needs my yes._
1. Answer first. The result goes in the first line; background only if I need it.
2. Recommend, do not just list. When there are options, say which you recommend and why. Give me a full menu only for big or hard-to-undo choices.
3. One decision per question, with your recommended option first.
4. Do not narrate the work. Tell me what came out of it, not every step taken.
5. Show the full text. Anything I will send or publish, show me the complete text, not a description of the changes.
6. Finish the task I asked for in one go, and offer the obvious follow-on fixes rather than leaving them unsaid.
7. When only I can do a step (a setting, a sign-in, an approval), give me numbered steps, one action each, with any text to paste in a copy box and how I will know it worked.
8. If what I ask conflicts with a rule in these files, tell me both in one line and let me choose before acting.
9. Before showing me a message I will send to someone, read it as that person and fix anything they would find unclear or could not act on.
10. Say what you could not check, in one line.
```

### about-me/verification-rules.md
```
# Checking before trusting
_Last updated: [date]_

How to know something is true before acting on it or telling me it is done. Read at the start of every session. These are kit defaults: keep, change or delete any ("make it mine" walks through them), and add your own as you learn them; the session audit offers to.

1. Check the real thing, not the note about it. A log, a summary or an old message records what someone believed. Open the actual file, page or folder before saying it is so.
2. "No record" is not "no". If nothing is written down, say "not on record" rather than "it did not happen".
3. A search that finds nothing proves nothing until it can find something. Before trusting an empty result, try the same search on something known to be there.
4. Run the fix before believing it. A change that looks right is a guess until the result has been checked.
5. Read the whole document, not the preview. A title, a snippet or someone's summary is a claim about a document, not a read of it. Say which parts were read.
6. Take today's date from the clock, never from the conversation.
7. A rule written for one case is not a rule for every case. Before applying an old "this cannot be done" to something new, check again.
8. A yes on wording is not a check of the facts. Verify any fact before asking me to approve text that contains it.
9. Never say "done everywhere" until each place has been checked. List the places first, then check each one.
```

### about-me/writing-rules.md
```
# Writing Rules
_Last updated: [date]_

How you want things written. The single source of truth for voice and style. Skills that produce copy read this file.

## Tone
_(e.g. plain English, direct, warm, short sentences.)_

## Spelling and formatting
_(e.g. British or American spelling; tables vs prose.)_

## Words and phrases to avoid
_(Anything you never want to see. Skills check against this.)_

## Self-description guardrail
_(Labels you never want applied to yourself, e.g. "expert", "guru". Leave blank if none.)_

## Examples
_(A short sample that sounds like you. Optional but powerful.)_
```

### about-me/memory.md
```
# Memory
_Last updated: [date]_

Index of active projects and recent context. Read at session start. Soft ceiling 50 lines, one or two sentences per entry, always dated.

## Active projects
| Project | Status | Path | Workstation |
|---|---|---|---|
| _(example)_ | | | |

## Global rules and preferences
_(Standing rules that apply everywhere.)_

## Decisions and context
_(Dated decisions worth remembering.)_
```

### about-me/custom-workflows.md
```
# custom-workflows.md
_Last updated: [date]_

A registry of every custom workflow: skills installed, scheduled tasks running and ideas for skills worth building. Add a row to the Scheduled tasks table whenever you create one.

## Custom skills
| Skill | Trigger phrases | What it does | Source |
|---|---|---|---|
| _(example)_ | | | |

## Scheduled tasks
| Task ID | Schedule | What it does | Source |
|---|---|---|---|
| _(add a row when you create a scheduled task)_ | | | |

## Pending reinstall
| Skill | Source lines | Why it matters | Date logged | Status |
|---|---|---|---|---|
| _(rows added by workspace-hygiene when a skill goes missing)_ | | | | |

## Skill ideas
| Idea | How often | Steps it follows | Date logged | Status |
|---|---|---|---|---|
| _(rows added by personalise; say "turn this into a skill" to build one)_ | | | | |
```

### about-me/disabled-skills.md
```
# disabled-skills.md
_Last updated: [date]_

Skills that exist but are switched off, and skills that were deleted. disabled-skill-check reads this before improvising. When you switch a skill off, say "remember I switched off [skill]" and it gets a row here.

## Disabled
| Skill | What it does | Why disabled | How to re-enable |
|---|---|---|---|
| _(example)_ | | | |

## Deleted
| Skill | What it did | Why removed |
|---|---|---|
| _(example)_ | | |
```

### inbox/inbox.md
```
# Inbox
_Append-only quick capture. Cleared by the inbox-triage skill._

One line per thought. Run "triage my inbox" to file everything.

The file starts empty on purpose. Add your first line whenever a thought turns up.
```

### inbox/triaged-archive.md
```
# Triaged archive
_A log of inbox items that have been filed, and where they went. Written by inbox-triage._

| Captured | Triaged | Item (full text) | Type | Destination | Notes |
|---|---|---|---|---|---|
| _(rows added by inbox-triage)_ | | | | | |

## Discard log
| Captured | Triaged | Item (full text) | Reason |
|---|---|---|---|
| _(rows added by inbox-triage)_ | | | |
```
