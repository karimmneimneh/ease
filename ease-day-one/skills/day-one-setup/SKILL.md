---
name: day-one-setup
description: "The guided on-ramp that takes a complete beginner from just-installed Claude Cowork to a safe, working setup, one approved step at a time. Use when the user says 'help me set up Cowork safely', 'EASE Day One', 'start EASE Day One', 'help me get set up on Cowork', 'walk me through setting up Cowork', 'I just installed Cowork, what now' or is a beginner starting from zero. It checks what already exists and continues where they left off; it never assumes a blank slate, never resets anything and never replaces a file without a backup and a yes. It routes install and connecting tools through Cowork's own onboarding rather than repeating those steps, then adds one tightly scoped folder, a minimal workspace and safe defaults (manual approvals, draft-not-send, nothing risky scheduled). First of four Day One skills; it hands on to the first-win skill. It does not build the full workspace system; that is the EASE OS plugin."
---

# EASE Day One - Setup

The spine of EASE Day One. It gets a beginner from just-installed to safe and working: tools connected, one tightly scoped folder, a minimal workspace, and safe defaults. It uses Cowork's own onboarding for the install and the connectors and adds the safety layer around them.

Boundary: this skill sets up a safe minimum. It does not build the full workspace system (routing maps, memory ceilings, workstations, a wide skill set). That is the EASE OS plugin, the destination the user graduates to later. Day One ends where EASE OS begins.

---

## Step 0 - Operating rules (non-negotiable)

1. **Propose first, then act.** Show each change and wait for an explicit yes. Never a blanket "apply all" that hides a write.
2. **Never reset, wipe, or overwrite.** Assume the user may already have some of this in place; only add what is missing. There is no start-fresh path. If a change would replace existing content, show a before and after, get a separate yes, and copy the original to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) first. Never remove the backup.
3. **State-aware and resumable.** Read the progress file first and continue from the first step still marked "not yet". Never redo a completed step.
4. **Impose nothing personal.** Do not set a spelling, tone, or style default. If the user wants standing preferences, ask and write only what they actually say.
5. **Guide, do not fake.** Some steps happen in Cowork's own screens: connecting a tool, choosing an approval mode, giving access to a folder. For those, explain plainly and confirm the user has done it. Never claim to have done something only they can do.
6. **Stay in your lane.** Set up the safe minimum only. If the user wants the full workspace system, that is EASE OS; point them to it rather than building it here.

---

## Step 1 - Check what already exists (read only, resume)

- If no folder is selected in this chat, or the selected folder has no progress file, ask whether they already set up a Day One folder in an earlier chat. If they did, ask them to select it as this chat's working folder and wait, then read its progress file. Never start a second setup beside a first one. **The one exception is moving house:** if their Day One folder turned out to be too broad (the health check flags this) and they want a new dedicated folder, offer to copy the progress file (under whichever of its two names it has), `rules.md` and `notes.md` into the new folder on a yes (if the new folder already holds a progress file under either name, or a `rules.md` or `notes.md`, keep it, copy no second progress file in, and ask before adding anything to it, under rule 2) (both folders need to be shared for the copy; the old one's access can be removed afterwards), leave the originals where they are, and continue from the progress file now in the new folder. If the old folder had an `outputs/` subfolder and the new one does not, set that row back to "not yet" in that file so the next step offers it again; a "declined" row stays declined.
- **A progress file from the earlier name.** This kit was called Cowork Day One before it was renamed, and its progress file was `cowork-day-one-progress.md`. If the working folder has that file and no `ease-day-one-progress.md`, it is the progress file: read it and write to it in place, never rename it and never create a second one. Its graduation line may say "Ready for Cowork OS" where a new file says "Ready for EASE OS"; treat the two as the same line.
- If a working folder has already been chosen in a previous session and a progress file (under either name) exists there, read it and continue from the first step marked "not yet". Confirm the done steps still hold; do not redo them. On a true first run no folder has been chosen yet, so there is nowhere to look and nothing to find: that is the normal case, not an error.
- If there is no progress file yet, treat this as a likely first run, but still look before assuming zero: are any tools already connected, is a working folder already chosen, are safe defaults already set, and does the chosen folder already hold instruction or notes files (a `rules.md`, `notes.md`, `CLAUDE.md`) or folder instructions? Note anything already in place so you skip it or build on it, never replace it.
- Tell the user in one or two plain lines where they are and what is left, then continue.

---

## Step 2 - Connect your tools (use Cowork's own onboarding)

Connectors come first, before anything clever, because most real tasks touch the user's email, calendar, or files.

- Point the user to Cowork's own setup rather than repeating it: open Customize in the left sidebar (open the Cowork tab first), then Connectors, and connect the tools they actually work in. If Cowork's own guided setup is offering to connect tools, let it do that job; this skill does not replace it.
- Ask which tools they use day to day (for example email, calendar, files) and connect only those. Fewer tools is safer and simpler. On a Team or Enterprise plan, an admin may need to enable a connector (such as the Google ones) before it appears; if so, say that plainly rather than troubleshooting.
- Set one expectation now, because it shapes the whole safety habit: the Gmail connector can search, read and create drafts, but it cannot send email. Every email is reviewed and sent by the user. Carry that same care to everything else.
- **Ask, before going further, whether they are on the Claude desktop app.** A working folder on their computer needs the Claude desktop app installed and open on that computer; web and mobile can reach that folder only while the desktop app is open there. If they do not have the desktop app, say so plainly before going any further: connecting their tools works anywhere, but everything from Step 3 on needs the Claude desktop app, which they download from claude.com. Finish connecting the tools with them, then stop there. Tell them what is done, what is waiting, and that saying "help me set up Cowork safely" again on the desktop app picks up exactly where they left off. Do not start Step 3, and do not send them on to the first task: without a working folder there is no progress file, and the later steps all read it. Never walk them into Step 3 and let them discover the problem there.
- When the tools they need are connected, note the step as done and continue. If they choose not to connect anything for now, that is fine: note it as "declined (none connected yet)". It is recorded when the progress file is created in Step 3.

---

## Step 3 - One tightly scoped working folder

- If the user does not have a folder yet, walk them through making one first: in Finder (Mac) or File Explorer (Windows), create a new folder somewhere easy to find, for example "Claude Work" inside Documents. Then they choose that folder where Cowork asks for a working folder when they start a task.
- Recommend one dedicated folder for Claude to work in, not a broad location like the whole Desktop or the whole Documents folder. **If the selected folder is a broad location (a whole Desktop, Documents, Downloads or home folder), do not set up there:** explain in one line why, and ask them to create or pick a dedicated folder and select it instead. A tight folder is the simplest safety habit there is: Claude can only read and write where the user points it, and giving access to a folder is a deliberate, approved choice.
- Inside that folder, propose an `outputs/` subfolder for finished work, so drafts and generated files have a tidy home. Create it only once the user says yes.
- Now check whether a progress file already exists in the chosen folder, under either name (`ease-day-one-progress.md`, or `cowork-day-one-progress.md` from the kit's earlier name). If one does, read it and resume from it (Step 1); never create a second one. If neither does, create `ease-day-one-progress.md` from the template below. Show its contents, get a yes, then write it (if they say no, explain in one line that the later steps read this file to know where they are, and that Day One can pause here until they are ready), marking "Working folder" as done with today's date and "Tools connected" as done (or as "declined (none connected yet)" if they chose to connect nothing). Mark "Outputs folder" done if they said yes to it; if they declined, mark it "declined" with today's date. Declined is a complete answer, not an unfinished step: finished work then saves straight into the working folder, and no later step asks again or blocks on it. This file is what makes the whole setup resumable across sessions.

---

## Step 4 - A minimal workspace: rules that stay put, notes that grow

Keep this small. Two homes, nothing more:

If the folder already holds a `rules.md` or `notes.md`, keep it: propose adding only the real lines the template has and theirs lacks (never the template's example or placeholder lines), shown in full, each on its own yes. If it holds a `CLAUDE.md` or folder instructions, leave them alone: Day One adds nothing that changes them.

- **A short rules file.** Create `rules.md` in the working folder from the template below. It holds a few standing rules that should not change: the safety rule (never send, post, publish or delete without my OK; draft, do not send) plus any one or two plain preferences the user names. Keep it to standing rules only. A step-by-step procedure does not belong here; that is what a skill is for.
- **A notes file that grows.** Create `notes.md` in the working folder from the template below, as a running log of decisions and context that is added to as the work goes. This is the changing memory. Cowork does not remember between sessions the way chat does and does not read these files on its own: first-win reads both and health-check reads `rules.md` when they run, and EASE OS picks them up later if the user moves on to it.
- **Make the safety rule live everywhere (recommended).** Cowork only applies a rule in every session if it sits in Cowork's Global instructions (Settings, then Cowork, then Global instructions, on the desktop app). Draft the exact line or two from the Safety section of `rules.md` (or, if they declined `rules.md`, from the Safety lines in its template below) for them to paste, and say plainly that this paste is what makes the rule apply in every chat; `rules.md` is the written copy. That screen is one only they can edit, so offer it, never claim it is done, and ask them to confirm once they have pasted it. Record the answer in the progress file's "Safety rule pasted into Global instructions" line, on the same yes as the Minimal workspace update: "yes" once they confirm they pasted it, "later" if they want to do it another time, or "declined". This line is a note, not a setup step: it never blocks the next step.

Show both files in full, then ask for each one separately: `rules.md` first, written on its yes, then `notes.md`. Two files are two changes, and Step 0's rule is a yes per change, not a yes per step. If the user says "both, go ahead" once they have seen both, that is a yes on each and needs no second ask. If they decline one or both files, that is a complete answer too: record the row as "declined ([which file])" and move on; no later step asks again or blocks on it. Then show the progress update ("Minimal workspace: done, [today's date]", or the declined wording, plus the Global instructions line) and write it on a yes.

---

## Step 5 - Safe defaults

Set the habits that keep a beginner safe. These are Cowork choices, so explain each in plain language and confirm the user has set it:

- **Manual approvals.** Recommend the "Manually approve" mode from the mode selector in the chat box, so Claude pauses and asks before each action and the user chooses allow or deny. This matches Cowork's own advice to approve manually when using something new or doing anything hard to undo. The mode can be changed any time. If they prefer to keep a different mode, respect that: say in one line that Manual is the safer choice while they are new, and note their choice in the progress file.
- **Draft, do not send.** Reviewing and sending things yourself is the habit. The Gmail connector already cannot send; carry the same care to messages, posts, and purchases.
- **Nothing risky on a schedule.** Do not set up any scheduled task that sends messages, makes purchases, or is hard to undo. Day One schedules nothing itself. If the user ever wants a scheduled task, keep it to safe, read-only summaries.

Then show the one-line progress update ("Safe defaults: done, [today's date]", plus the approval mode they chose) and write it on a yes.

---

## Step 6 - Report, then point to the first real task

- Show the user the updated progress file and get a yes, then write it so every completed step reads done (or declined), keeping the date each step was actually finished; only steps finished today carry today's date. In the same write, update the file's "Last updated" date and the Graduation section's "What is left" line ("log a first win").
- Give a short report, three lines at most: what is now connected, where the working folder sits and what is in it (`outputs/` if they chose it, `rules.md`, `notes.md`), and which safe defaults are set.
- Close with the journey breadcrumb, in these words: "You are set up safely. Next, say **'do my first Cowork task'** to do one real task as a draft." Then add the shared map line that every Day One skill closes with, so the user always knows where they are: "Day One has four steps: 1 set up safely, 2 your first task, 3 check your setup, 4 move up to EASE OS. You are on step 1."

If setup was already complete when this skill ran (every table row done or declined), do not re-run the steps or rewrite the progress file. If the user came back to connect another tool, or to change an earlier answer (a declined step, the approval mode, or the Global instructions line once they have pasted it), help with just that, show the one-line change to the progress file and write it on a yes, then return them to what they were doing. Say so in one line, and point the user to the next unstarted step on the journey instead: "do my first Cowork task" if no win is logged yet, otherwise "check my Cowork setup".

---

## What this skill does not do

- Does not install Cowork, sign the user in, or connect a tool for them. Cowork's own onboarding does that; this skill routes them to it and adds the safety layer.
- Does not build the full workspace system (routing map, memory ceilings, workstations, the wider skill set). That is the EASE OS plugin, the destination the user graduates to.
- Does not send, post, publish, delete, or schedule anything. It proposes; the user approves.
- Does not reset or overwrite. It checks what exists and adds only what is missing, backing up anything it is ever asked to replace.
- Does not impose a spelling, tone, or style default. It writes only the preferences the user gives.

---

## Templates

Used as the source when creating each file. Create a file only after showing it to the user and getting a yes. Where a template says `[today's date]`, use the real date.

### ease-day-one-progress.md

```
# EASE Day One - progress

_This file tracks your Day One setup so nothing has to be redone. The Day One skills read it and update it, always with your yes first. You can read it any time; you should not need to edit it by hand._

_Created: [today's date]. Last updated: [today's date]._

## Setup progress

_Only the five rows of this table are setup steps. The lines under it are notes and never hold up the next step._

| Step | Status | Date |
|---|---|---|
| 1. Tools connected (or declined) | not yet | |
| 2. Working folder | not yet | |
| 3. Outputs folder (or declined) | not yet | |
| 4. Minimal workspace (rules and notes, or declined) | not yet | |
| 5. Safe defaults | not yet | |

- Approval mode: not set yet
- Safety rule pasted into Global instructions: not yet

## First win

_Your first real task, done as a draft. Filled in by the first-win skill ("do my first Cowork task")._

- Task: not logged yet
- Logged on:

## Graduation

_You are ready for EASE OS once setup is a full pass and a first win is logged. Set by the health-check skill ("check my Cowork setup")._

- Ready for EASE OS: not yet
- What is left: finish setup, then log a first win
```

### rules.md

```
# My rules

_Standing rules for Claude. Keep them short and stable. A step-by-step procedure does not belong here; that is what a skill is for._

## Safety

- Never send, post, publish or delete anything without my explicit OK.
- Draft, do not send. Show me first; I review and send it myself.

## My preferences

_(Optional. Add a line or two only if you want to, for example: keep answers short and in plain English. Leave blank if you are not sure yet.)_
```

### notes.md

```
# Notes

_A running log that grows as the work does. Cowork does not remember between sessions the way chat does, so anything worth keeping goes here or in my rules._

- (example, delete this line when you add your first note) [today's date] Decided to keep all finished drafts in one place.
```
