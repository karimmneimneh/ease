---
name: first-win
description: "Runs a beginner's first real Cowork task end to end as a draft, so nothing is sent, posted or scheduled. Use when the user says 'do my first Cowork task', 'help me do a real task in Cowork', 'let's try my first task' or has finished setup and wants a genuine result. It briefly asks what the user does over and over in their actual work, takes that single most-repeated task (never a demo or a toy example), gathers the real input and produces the output as a reviewable draft in the working folder or as a Gmail draft. It teaches the approval habit: it shows each step and waits for a yes, and never sends or posts on the user's behalf. Second of four Day One skills. It logs the win to the progress file with the user's approval, then points to the health-check skill ('check my Cowork setup'). If setup is not finished, it routes back to 'help me set up Cowork safely' first. It does not send, publish or schedule, and it does not build the full workspace system, which is the EASE OS plugin."
---

# EASE Day One - First win

The second step of EASE Day One. It takes one task the user already does over and over and runs it end to end as a draft, so they get a genuine result and learn the approval habit at the same time. Nothing is sent, posted, or scheduled: the output is a draft the user reviews and finishes themselves.

Boundary: this skill produces one real, draft-only result to build confidence and the review habit. A finished setup is its prerequisite. It does not send, publish, or schedule, and it does not build the full workspace system (routing maps, memory ceilings, a wide skill set). That is the EASE OS plugin, later on the journey.

---

## Step 0 - Operating rules (non-negotiable)

1. **Draft only.** The result is always something the user reviews and finishes: a new file in the working folder, or a Gmail draft. Never send an email, post anything, publish, make a purchase, schedule a task, or move, rename or delete any existing file. The Gmail connector cannot send by design, so a Gmail draft is safe; carry that same care to everything else.
2. **Propose first, then act.** Show what you are about to do and wait for an explicit yes before doing it. This approve-as-you-go rhythm is the habit the skill exists to teach.
3. **Real task, not a toy.** Use something the user genuinely does again and again, with input that exists right now. Never invent a demo or a sample task. If nothing real is to hand, pick a smaller real thing rather than a fake one.
4. **Guide, do not fake.** Some steps happen in the user's own screens: approving an action, opening a Gmail draft. Explain plainly and confirm; never claim to have done something only they can do.
5. **Never reset or overwrite.** Save new work into the outputs folder (or the working folder, if the user declined `outputs/`). Do not replace an existing file. If a task would replace something, show a before and after, get a separate yes, and copy the original to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) first.

---

## Step 1 - Check setup is done (read the progress file)

- **First, make sure the right folder is selected.** A new chat may not have a working folder selected yet. If no folder is selected in this chat, ask the user to select the folder Day One set up (the one holding `ease-day-one-progress.md`, or `cowork-day-one-progress.md` if they started before the kit was renamed) and wait until they have. If a folder is selected but has no progress file, ask whether this is the folder Day One set up before going further: if they set one up elsewhere, ask them to select that one instead. Only if they say they never set one up is setup missing.
- **A progress file from the earlier name.** This kit was called Cowork Day One before it was renamed, and its progress file was `cowork-day-one-progress.md`. If the working folder has that file and no `ease-day-one-progress.md`, it is the progress file: read it and write to it in place, never rename it and never create a second one. Its graduation line may say "Ready for Cowork OS" where a new file says "Ready for EASE OS"; treat the two as the same line.
- Read the `ease-day-one-progress.md` file in the working folder. If `rules.md` and `notes.md` are there, read them too and follow them throughout.
- If there is no progress file, or any setup step in it is still "not yet" ("declined" counts as answered; only the five table rows count as setup steps, not the notes under the table), setup comes first. Say so in one plain line and point the user to **"help me set up Cowork safely"**. Do not run a first task on a half-finished setup.
- If setup is done, read the "First win" section. If a win is already logged, tell the user, and offer either to run another task or to move on to **"check my Cowork setup"**. Do not silently redo a logged win.
- Otherwise, say in one line that setup looks complete and you are ready to find their first task, then continue.

---

## Step 2 - Find the one task that repeats

- Ask, in plain language, what the user finds themselves doing again and again in their work. Keep it to one or two short questions, not a survey.
- If they are unsure, offer a few everyday examples to jog them, framed generically: writing the same kind of email reply, turning rough notes into a short document, summarising a long file, pulling scattered information into one place, or drafting a checklist you reuse. Let them name their own; the examples are only a prompt.
- Pick the single most-repeated task that can be finished as a draft. If their answer is naturally a "send" or "post" action, keep the task but move the finish line: Claude drafts it, the user sends or posts it themselves.
- Confirm the real input is available now: a file in the folder, something they can paste, or a thread in a connected account. If the input is not to hand, choose a smaller real task rather than a hypothetical one.
- State the task back in one line and get a yes before starting.

---

## Step 3 - Set it up as a draft, never a send

- Decide where the draft will live and say so plainly: a new file in the `outputs/` folder (or straight in the working folder, if they declined `outputs/`) for a document, spreadsheet or summary; a Gmail draft for an email (the Gmail connector drafts only, it cannot send, so an email finishes as a safe draft).
- A file in the working folder needs the Claude desktop app installed and open on that computer; from web or mobile it works only while the app is open there. A Gmail draft works either way.
- If the task needs a tool that is not connected, say so and walk the user through connecting it in Cowork's own screen (Customize in the left sidebar, then Connectors), rather than working around it. Connecting is something only they can do, so wait until they confirm it is connected, then carry on with this task. If they would rather not connect it, pick a different real task that uses what is already connected.
- Confirm the plan in one or two lines before doing the work.

---

## Step 4 - Do the task, one approved step at a time

- Work through the task approve-as-you-go: show what you are about to do, do it once the user says yes, then show the result. This is the rhythm that keeps them safe once the training wheels are off, and it mirrors Cowork's own advice to approve manually when a task is new or hard to undo.
- Keep everything as a draft. **Before saving anything, show the finished draft in full in the chat** (the text of the document or email, or for a spreadsheet its columns and first rows) and ask whether to save it. Only on a yes, save it as a new file in the `outputs/` folder (or the working folder), or create the Gmail draft. If they want changes, make them and show it again. Do not send, post, publish or schedule.
- If the natural next step would send or publish, stop and hand it back. The user does that part themselves, after they have read it.

---

## Step 5 - Log the win (with your yes)

- Fill in the "First win" section of the progress file: the task in one line, and today's date. Use the template below.
- Show the exact change and get a yes before writing it. Never write silently.
- This is a small append. It does not touch the setup steps or the graduation flag; the health-check skill sets graduation later. In the same write, update the file's "Last updated" date and, unless the Graduation section already reads "Ready for EASE OS: yes", its "What is left" line ("run the health check: check my Cowork setup").
- If `notes.md` exists, offer, as a separate item, to add one dated line to it recording what the task was and where the draft lives, so the notes file starts to grow with real work. Write it only on its own yes; skipping is fine.

---

## Step 6 - Report, then point to the check

- Give a short report, three lines at most: what was produced, where the draft sits (the `outputs/` folder, the working folder or the Gmail drafts), and the reminder that the user reviews and finishes it themselves.
- Close with the journey breadcrumb, in these words: "That is your first real task, done as a draft. Next, say **'check my Cowork setup'** to make sure everything is safe." Then add the shared map line: "Day One has four steps: 1 set up safely, 2 your first task, 3 check your setup, 4 move up to EASE OS. You are on step 2."

---

## What this skill does not do

- Does not send, post, publish, make purchases, or schedule anything. Every result is a draft the user finishes.
- Does not invent a demo or sample task. It uses a real, repeated task with real input, or a smaller real one.
- Does not run on a half-finished setup. It routes back to **"help me set up Cowork safely"** first.
- Does not reset, overwrite, move, rename or delete existing files. It saves new drafts into the outputs folder (or the working folder) and backs up before any approved replacement.
- Does not set the graduation flag or build the full workspace system. Those are the health-check skill and the EASE OS plugin.

---

## Templates

Used as the source when writing to the progress file. Write only after showing the change and getting a yes. Where a template says `[today's date]`, use the real date.

### First win block (written into ease-day-one-progress.md at Step 5)

On the first win, replace the two placeholder lines in the existing "First win" section, leaving the heading and its note as they are. **On any later win, add a new pair of lines underneath rather than replacing what is there**, so the record of an earlier win is never lost. The section becomes a short running list, newest at the bottom:

```
- Task: [one line describing the real task done as a draft]
- Logged on: [today's date]
```
