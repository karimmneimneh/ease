---
name: health-check
description: "A re-runnable safety check for an EASE Day One setup, and the gate to graduating into EASE OS. Use when the user says 'check my Cowork setup', 'is my Cowork setup safe', 'run the Cowork health check' or wants to confirm things are safe before moving on. It reads the setup against a short checklist grounded in how Cowork works: one tightly scoped working folder, each connected tool passing a read test, work kept as drafts only, no scheduled task that sends or is hard to undo, a small plugin set and every skill triggering from plain language. It gives a plain pass-or-fix list and never changes anything automatically. Third of four Day One skills. When setup is complete, every check passes and a first win is logged, it records the graduation flag with the user's yes and points to the graduate skill ('am I ready for EASE OS'). If setup has not been done, it routes back to 'help me set up Cowork safely'. It reports and never fixes on its own; the full workspace audit is the EASE OS plugin."
---

# EASE Day One - Health check

A re-runnable safety check for an EASE Day One setup. It reads the setup against a short safe-setup checklist and gives a plain pass-or-fix list: what is safe, and what to tighten. It changes nothing on its own; it only reports, and, with the user's yes, records when they are ready to graduate or saves them a copy of the check.

Boundary: this skill checks and reports. It never fixes anything automatically, and never sends, deletes, or reschedules. It is the light safety gate before graduating to the EASE OS plugin, not the full workspace audit that EASE OS itself provides.

---

## Step 0 - Operating rules (non-negotiable)

1. **Read and report, do not change.** This is an audit. It never edits files, connectors, tasks, or settings on its own. It writes in two places only, both with an explicit yes: the graduation lines in the progress file (Step 4, setting them or setting them back), and a saved copy of the check as a new file, if the user asks for one (Step 3).
2. **Propose fixes, never auto-apply.** For anything that needs tightening, describe the fix in plain language and let the user decide. Never fix it for them.
3. **Guide, do not fake.** Some checks live in the user's own Cowork screens: scheduled tasks, installed plugins, skill toggles. Where you cannot see something directly, guide the user to look and take their answer. Never mark a check passed without evidence. Two kinds of answer are the user's call, not a failure, and neither holds up graduation: a connected tool they chose not to have read in check 2 (mark it "not tested (your choice)"), and a broad folder, a scheduled task or a plugin they have been told about plainly and decide to keep because they rely on it (mark it "kept (your choice)", with the risk named in one line).
4. **Re-runnable.** This skill is meant to be run again any time. It never assumes it is the first run and never resets anything.

---

## Step 1 - Read the progress file

- **First, make sure the right folder is selected.** A new chat may not have a working folder selected yet. If no folder is selected in this chat, ask the user to select the folder Day One set up (the one holding `ease-day-one-progress.md`, or `cowork-day-one-progress.md` if they started before the kit was renamed) and wait until they have. If a folder is selected but has no progress file, ask whether this is the folder Day One set up before going further: if they set one up elsewhere, ask them to select that one instead. Only if they say they never set one up is setup missing.
- **A progress file from the earlier name.** This kit was called Cowork Day One before it was renamed, and its progress file was `cowork-day-one-progress.md`. If the working folder has that file and no `ease-day-one-progress.md`, it is the progress file: read it and write to it in place, never rename it and never create a second one. Its graduation line may say "Ready for Cowork OS" where a new file says "Ready for EASE OS"; treat the two as the same line.
- Read `ease-day-one-progress.md` in the working folder.
- If there is no progress file, setup has not been done. Say so in one line and point to **"help me set up Cowork safely"**. There is nothing to check yet.
- Note whether every setup step in the progress file is marked done ("declined" counts as done for any row; the notes under the table are not setup steps) and whether a first win is logged. Both feed the graduation gate in Step 4. Read `rules.md` too, if it is there, so the check follows the user's own rules.

---

## Step 2 - Run the six safety checks

Work through each check and mark it pass or fix. Each one reflects how Cowork actually works.

1. **One tightly scoped working folder.** Confirm Claude is pointed at a single dedicated folder, not a broad location like the whole Desktop or Documents. Claude can only read and write where it is pointed, so a tight folder is the core safety habit. Then ask whether they have granted Claude any OTHER folder, and which. A folder granted earlier is invisible to this check but just as real. Other folders are fine when each is a deliberate, dedicated folder they meant to share: that passes. What fails is broad access (a whole Desktop, Documents, Downloads or home folder) or a folder they had forgotten about. To fix one, tell them which folder to stop pointing Claude at and to remove its access in the same place they granted it; let them decide. Never ask them to give up a folder they rely on. If the broad folder is the Day One folder itself, explain the move in this order: create a dedicated folder, select it, and say "help me set up Cowork safely", which offers to copy the three Day One files across (the old folder must still be shared at that moment); only after the copy is done do they remove the old folder's access. If they would rather keep the folder they have, mark the check "kept (your choice)".
2. **Each connected tool passes one read test.** Say first what this involves and get a yes: for every tool they have connected, one safe, read-only action to confirm it works, which means reading a little of their actual email, calendar or files. Name the tools before touching any of them, and skip any the user would rather leave alone. Then do one read per approved tool: search recent emails, list the next few calendar events, or open a document from the drive. Only test tools that are actually connected. A tool that cannot complete a simple read needs reconnecting in Customize.
3. **Work exists as drafts only.** Confirm the work produced so far lives as drafts: files in the outputs folder (or the working folder), or Gmail drafts. If the progress file's Global instructions line says anything other than "yes", mention once that pasting it there makes the rule apply in every chat; if its approval-mode line shows anything other than Manually approve, mention once that Manual is the safer choice while they are new. Both are tips, not fails. The Gmail connector cannot send by design, and nothing should have been posted or published. Fix if anything is set up to send or post on its own, unless it is a scheduled task the user has already chosen to keep in check 4 ("kept (your choice)"), which passes here too.
4. **Nothing risky on a schedule.** Review the scheduled tasks (the Scheduled area in the left sidebar). Confirm none send messages, make purchases, or do anything hard to undo; safe read-only summaries are fine. Creating a scheduled task always needs approval, so there should be no surprises. If a task looks risky, say which one and why, and tell the user how to pause or delete it themselves. Never pause or delete one for them.
5. **A small, understood plugin set.** Check how many plugins are installed (Customize, then Plugins). A small set from sources the user trusts is safer and simpler than a large one. This kit and EASE OS are expected, so do not flag them. If Cowork Day One or Cowork OS (the earlier names of these two kits) is still installed beside them, suggest removing the old one: the two answer to the same phrases. Mark it fix until the old one is removed, or "kept (your choice)" if they decide to keep it. For anything else the user does not recognise, tell them where to remove it and let them decide (a plugin they choose to keep is "kept (your choice)" and passes); the habit worth keeping is installing only from people they trust.
6. **Each skill triggers from plain language.** Confirm the Day One skills answer to their plain-language phrases. They also need Cowork's code execution and file creation settings switched on (Settings, then Capabilities, on Pro and Max; on Team and Enterprise plans an admin controls this in Organization settings): without them the skills show greyed out and nothing happens when they are named. If a skill does not trigger: check it is toggled on in Customize, check that setting is on, and name the skill explicitly in the request.

---

## Step 3 - Give the pass-or-fix list

- Present a short list: one line per check, marked pass or fix. For each fix, one plain sentence on what to do. No jargon.
- Keep it scannable. This is a quick health read, not a report.
- Save nothing by default; the list is the deliverable. If the user wants a copy, show the file name and save it as a new file, `health-check-[today's date].md`, in the `outputs/` folder (or the working folder, if they declined `outputs/`). If a file of that name already exists, add the time to the name; never replace an earlier copy.

---

## Step 4 - The graduation gate

- Graduation needs three things together: every setup row is done or declined, every check in Step 2 passes (a check marked "not tested (your choice)" or "kept (your choice)" counts as passing), and a first win is logged in the progress file. If a setup row is still "not yet", say which and point to **"help me set up Cowork safely"**; do not flip the flag.
- **If all three are true:** tell the user they are ready. Show the exact change to the "Graduation" section of the progress file (Ready for EASE OS: yes, with today's date), get a yes, then write it using the template below. Then offer the next step by name: "You are ready. Next, say **'am I ready for EASE OS'** to move up to the EASE OS plugin."
- If the progress file is the earlier-named `cowork-day-one-progress.md`, say in one line that Cowork OS in its notes is EASE OS's earlier name.
- **If the checks pass but no first win is logged:** say what is left in one line, and point to **"do my first Cowork task"**. Do not flip the flag.
- **If any check needs a fix:** list the fixes and note that graduation waits until they pass. Do not flip the flag.
- Never flip the graduation flag without all three conditions met and an explicit yes. When the user is not ready, report in chat and write nothing, with one exception: **if the flag already reads yes from an earlier run and a check now fails,** show the change setting it back ("Ready for EASE OS: not yet", with what is left) and write it on a yes, so the file never says ready when the setup is not. If they say no, tell them plainly that the file will still say ready until the check passes.
- Whichever branch applies, close with the shared map line so the user always knows where they are: "Day One has four steps: 1 set up safely, 2 your first task, 3 check your setup, 4 move up to EASE OS. You are on step 3."

---

## What this skill does not do

- Does not fix anything automatically. It reports; the user decides.
- Does not send, delete, publish, or reschedule anything.
- Does not change any setting, connector, or task on the user's behalf.
- Does not write to the progress file except the graduation lines, and only with a yes.
- Does not replace the full EASE OS workspace audit. It is a light safety check, not that.

---

## Templates

Used as the source when writing to the progress file. Write only after showing the change and getting a yes. Where a template says `[today's date]`, use the real date.

### Graduation block (written into ease-day-one-progress.md at Step 4)

Written only on a full pass with a first win logged. Replace the two placeholder lines in the existing "Graduation" section, leaving the heading and its note as they are:

```
- Ready for EASE OS: yes ([today's date])
- What is left: nothing, you are ready. Say "am I ready for EASE OS".
```
