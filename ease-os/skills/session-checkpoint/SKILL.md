---
name: session-checkpoint
description: "Saves an unfinished session as one checkpoint file and a short resume prompt, so the work carries on in a fresh chat without replaying the whole conversation, then picks it back up later. Use when the user says 'pause this', 'park this', 'checkpoint this', 'save the session', 'I need to stop here', 'resume [thread]', 'pick up [thread]', 'what have I paused', 'close [thread]' or 'I forgot to save [session]'. For a session that has to stop halfway; at a natural end, session-audit closes the session instead. Nothing is written until the user has seen it and said yes, and the index and any checkpoint are backed up before every edit. Never sends or posts anything; the only task it can create is a reminder the user has approved, and that reminder only reminds."
---

# Session Checkpoint

**Working folder check (before any write).** EASE OS lives in the user's working folder. If a file this skill would write to is missing, first check that this chat has a working folder selected and that it is the one EASE OS was set up in (it holds `about-me/` and a root `CLAUDE.md`). If no folder is selected, or the selected one has neither, ask the user to select the folder they set this kit up in (the one holding `about-me/` and `CLAUDE.md`) before offering to create anything, so nothing is written somewhere that will not be there next chat. Only if they confirm this is the right folder does the skill's own missing-file rule apply.

Stops a session halfway and leaves behind everything a fresh chat needs to carry on, and nothing it does not: one checkpoint file, one row in the checkpoint index and one short resume prompt.

**Why it helps.** A long chat gets slower and its work gets worse, because every reply carries everything said before it. The same task picked up in a fresh chat from a tight brief does better work. That only holds if the resume prompt is short and names exactly what to read, and what not to. That part is the heart of this skill.

---

## When to run

- "pause this", "park this", "checkpoint this", "save the session", "I need to stop here", "stop here and save it" or any moment a session has to end with work unfinished.
- "resume [thread]", "pick up [thread]" or the resume prompt itself.
- "what have I paused", "list my checkpoints", "close [thread]".
- "I forgot to save [session]", "recover yesterday's session".

**Not for:**
- The end of a session whose work is done. That is session-audit ("audit this session"), which lists what is left open and catches what the session taught. This skill is for the unnatural middle.
- A planned job that spans several chats and has a plan in `outputs/plans/`. It resumes from its plan file and the short next-step prompt that `CLAUDE.md` describes. Checkpoint it only when it has to stop in the middle of a step, and then the checkpoint carries only what the plan does not.

---

## Where things live

| Path | What it holds |
|---|---|
| `outputs/checkpoints/[thread-slug].md` | One file per open thread. The slug is short, plain and stays the same on every pause of that thread |
| `outputs/checkpoints/00-index.md` | One row per thread under `## Open` or `## Closed` |
| `outputs/checkpoints/_closed/` | Checkpoints whose work is finished. Moved here, never deleted |

If the folder or the index does not exist yet, propose creating them as part of the first checkpoint's approval (Step 5). The index starts as exactly this:

```markdown
# Checkpoints

## Open
| Thread | Project | Paused | Next action | File |
|---|---|---|---|---|

## Closed
| Thread | Project | Paused | Next action | File |
|---|---|---|---|---|
```

**Changing an existing file.** Every change is shown first, as the exact before and after, and applied only on a yes. Rewriting a checkpoint backs it up first as `[file].backup-[date]` (add the time if a backup from the same day already exists; never overwrite a backup). The index (`00-index.md`) and a checkpoint file are each backed up the same way before any edit to them, a new row, a Status change, a row moved between sections or a cell update included, because the kit backs up anything it replaces. Nothing in this folder is ever deleted: a row that no longer applies moves to `## Closed`, and a file moves to `_closed/`.

---

## Modes

| Mode | Fires on | What it does |
|---|---|---|
| **PAUSE** | The default | Captures the live session, shows the checkpoint, writes it on a yes and hands over the resume prompt |
| **RESUME** | "resume [thread]", or the resume prompt | Reads one checkpoint, runs the freshness check, then continues at its next action |
| **LIST** | "what have I paused", "close [thread]" | Shows the open threads and offers to close any that are already done, or closes the one named |
| **RESCUE** | "I forgot to save [session]" | Rebuilds a checkpoint from a past chat |

---

## Step 0: Check the index against the files. Every mode.

Most paused threads finish outside a resume: the user sends the message themselves, or another chat finishes the work. The checkpoint then says open while the work is done, and a later resume starts on a finished thread. So this step reads first, and any fix it finds is a proposal applied only on a yes.

**What counts.** A checkpoint is a `.md` file directly in `outputs/checkpoints/`, other than `00-index.md`. Read only each checkpoint's header block (the lines down to `## Objective`) plus its `## Pause history`; the rest waits until a mode needs it. Ignore every `*.backup-*` file. Read `_closed/` only to resolve Closed rows, or to rebuild a missing index (§ Edge cases). Any other file in the folder is mentioned in one line and left alone; where it belongs is not this skill's call.

**Look for:**

| Finding | Proposed fix |
|---|---|
| A file whose Status says closed, still in the folder or under `## Open` | Close it (RESUME § Closing a thread) |
| An Open row that points at no file | Search the working folder for the file name. Found elsewhere: propose moving it back. Truly gone: propose moving the row to `## Closed` with the File cell set to `file missing YYYY-MM-DD`. Never delete the row |
| An open file with no row | Propose a row built from the file's own fields |
| Open for more than 14 days, measured from the oldest `## Pause history` line | Ask whether to close it, keep it or re-scope it |
| Paused four times or more | The same question. A thread paused that often is not paused; it is blocked on something its checkpoint should name |

Say the result in one line, including when it is clean. If the folder does not exist yet, say "no checkpoints yet" and carry on. If checkpoints exist but `00-index.md` does not, see § Edge cases. **If the index exists but cannot be read or does not have the two sections, say so in one line: that is an unknown, never a clean result.** Propose fixes as one numbered list and apply only the ones the user says yes to, under the change rule above.

---

## Step 1: Gather

Work from the conversation already in this chat; reading it again costs nothing.

Then check memory against the record, because recall over a long session drops things, and the thing most likely dropped is a decision taken early and not mentioned since. Re-read any file the session itself wrote: those are the lasting trace of what was decided. On a session long enough that the early part is hazy, and only if this chat can read back over its own earlier turns, read a bounded slice (about the last 40 turns, or back to the start of this thread if shorter), never the whole thing.

---

## Step 2: Compress

This is the real work. Keep, in this order:

1. **The objective in one sentence.** What this thread produces, and for whom.
2. **Decisions taken, each with its reason.** A decision without its reason gets argued again.
3. **What was rejected, and why.** Otherwise the fresh chat proposes it again, as if new.
4. **Files created or changed**, one line each on what changed.
5. **Anything already sent, posted, published or changed outside the working folder.** The most important item: a fresh chat redrafting a message that already went out is the worst thing this skill can cause, and nothing would warn anyone.
6. **The exact next action, as an instruction.** "Draft section 4 of the brief" is an instruction; "continue the brief" is a topic.
7. **Open questions, each with an owner**: the user, someone else they named or Claude.
8. **What to read on resume, and what not to**, by file and section. If the work has a plan in `outputs/plans/`, list the plan here and do not copy its steps.
9. **The paused-at time** and the freshness checks the resume must run.
10. **What the resume needs to be set up**: the same working folder, plus any tool, connection or setting the work depends on.

Drop: tool output, search results and folder listings; old drafts (keep one line under item 3 saying a draft existed and why it was replaced); dead ends (one rejection line); Claude's own explanations; and **anything already written in a workspace file. Point at it, never copy it**, because the copy is the one that ends up wrong.

The test for every line: would a fresh chat do something different without it? If not, cut it.

---

## Step 3: Check every pointer

A pointer to something the fresh chat cannot reach is worse than none, because the handoff looks complete and fails quietly at the other end. For every file, link or fact the checkpoint points at:

- **Inside the working folder, or a link or ID that lives elsewhere** (a shared document, a web page): it survives. Point at it.
- **Only in this chat** (a file attached to the chat, a temporary folder, something said but never saved): it is gone next chat. Propose saving the file into the working folder first, or write the fact into the checkpoint itself.
- **If the checkpoint will be handed to someone else**, use paths inside the working folder rather than full computer paths, and write in anything they cannot open.

Say in one line how many pointers were checked and what, if anything, needs saving first. Silence here looks the same as skipping it.

---

## Step 4: Completeness check

Before showing anything, confirm in one line that each of the ten items in Step 2 is either present or marked not applicable. Item 5 is never skipped: if nothing went out, the checkpoint says so in words. Where an item cannot be confirmed either way, say so rather than guess.

---

## Step 5: Show it, then write it on a yes

**First, settle which file this is.** Look for `[thread-slug].md` in `outputs/checkpoints/` and in `_closed/`. If it exists and this chat did not come in through RESUME of that thread, ask whether it is the same thread: in `outputs/checkpoints/` that makes it a re-pause (below), and in `_closed/` it goes to RESUME § Reopening. If it is a different thread, choose a different slug. Never write over a checkpoint because the names matched.

Show the whole checkpoint in a code block, the index row it adds or changes (with the before and after for a change) and the resume prompt (Step 6). Write nothing until the user says yes.

- **First pause of a thread:** create the file from the template below, with one `## Pause history` line for this pause, and add its row under `## Open` (the index is backed up first, under the change rule in § Where things live).
- **Another pause of the same thread:** read the file as it is now (the user or another chat may have changed it since this chat last saw it) and back it up, then **rewrite** it from the template, keeping its lines unless this chat has superseded them and showing the user anything the rewrite would drop. Never append. One of each heading and one next action; this pause adds one line to `## Pause history`; earlier decisions and rejections merge into the live lists, and superseded ones are removed. Pause count is the file's own count plus one. Status is a state (`open`, or `closed YYYY-MM-DD`), not a story. A file over about 10 KB has been appended to rather than rewritten: compress it again. Update the row's Paused and Next action cells under the change rule.
- **A closed thread that is being picked up again:** RESUME § Reopening.

Then read the file back from the working folder to confirm it is there. If it could not be written, say so plainly and do not hand over a resume prompt that points at it.

---

## Step 6: The resume prompt, and a reminder if there is a date

The prompt is a pointer, never a payload. Show it in a code block:

```
Resume [thread]: read outputs/checkpoints/[thread-slug].md, run its freshness check, then continue at its next action.
```

Tell the user to start a new chat with the same working folder selected and paste it, or simply say "resume [thread]".

If the user names a day to pick it up and a scheduled-tasks tool is available, offer one reminder. Show:

- its name ("Resume [thread] reminder");
- its date and time (if they named only a day, ask what time suits them);
- its full instructions, which are only: `Tell the user: time to pick up [thread]. Start a new chat in your EASE OS folder and say "resume [thread]". Do nothing else: read no files and ask no questions.`;
- if `about-me/custom-workflows.md` exists, the row it would add to its `Scheduled tasks` table. If that file does not exist, say the task will not be recorded in a registry.

Create the task and the row only on a yes. With no date, or no scheduled-tasks tool, the index row keeps the thread visible; suggest a calendar reminder if they want one.

Close in one line: the file, the index row and that the prompt is ready. Then stop.

---

## RESUME mode: the freshness check

**Find the thread.** Match the user's words against the thread names and slugs under `## Open`. If exactly one matches, use it. If several match or none do, show the LIST output and ask which one. If the only match is a closed thread, offer to reopen it (§ Reopening) or show the list.

Then run this before any work continues. A checkpoint written on Tuesday and opened on Friday describes Tuesday.

1. If the thread's file is missing, or it is closed and the user did not reopen it, stop and say so with its current path.
2. Read the checkpoint and note its paused-at time.
3. Open only the files it names under `## Read on resume`.
4. **Check the checkpoint itself.** Open every file and section the next action cites and confirm it says what the checkpoint claims. A checkpoint is written at the tired end of a long session, which is when a reference goes in wrong.
5. Check what the work depends on for change since the paused-at time: the files under `## Freshness check` (check the modified date where the tools show it; otherwise compare their content against what the checkpoint says), newer entries in `projects/[Project]/memory.md` or `projects/[Project]/contact-log.md`, where [Project] is the checkpoint's Project field (skip this when it is none), and anything the user has said since.
6. **Say what moved and what it changes.** If nothing moved, say that too.
7. If something moved enough to change the next action, say so and let the user redirect before carrying on.

Then continue at the next action. A resumed chat that stops again without finishing stays open: re-pause it, or leave it.

**Closing a thread.** When the work is finished, propose the close in one approval, showing each change: Status set to `closed YYYY-MM-DD`, the file moved to `_closed/` (if `_closed/` already holds that name, add the date to the moved file's name) and its row moved to `## Closed` with the File cell updated to the new path. The file and the index are backed up first under the change rule in § Where things live. All of it on one yes. Any chat that finishes the work a checkpoint names should propose this; session-audit lists such threads at the close, and Step 0 and LIST catch the rest.

**Reopening.** To pick up a closed thread, propose moving the file back from `_closed/` under its original slug (if an open file already holds that name, ask first), setting Status to `open` and moving its row back to `## Open`, on one yes, with the same backups first. Then resume as normal.

---

## LIST mode

Run Step 0, then show each Open row: thread, project, paused date and next action. Give the Step 0 findings in one line. Then ask whether any listed thread is already done, and offer its close (§ Closing a thread). Change nothing without a yes.

---

## RESCUE mode

For a session that ended without a checkpoint.

1. Only if this chat can search or open past chats. If it cannot, say so and ask the user to paste the part that matters.
2. Confirm which chat it is before reading it; reading the wrong one in full is pure waste.
3. Read it bounded (the part that matters, not the whole thing), then run Steps 2 to 6 as normal.
4. **Mark anything inferred as "(reconstructed)".** A past chat shows what was said, not always what was concluded, so the resume treats a reconstructed line as a claim to check, not a decision taken.
5. **Set Paused at to when the past chat ended, marked (reconstructed)**, never to now, so the freshness check covers everything that changed since.

---

## Edge cases

- **The session holds two unrelated pieces of unfinished work.** Offer two checkpoints, one per thread, each with its own slug and row. Never merge them into one file.
- **Checkpoint files exist but `00-index.md` does not.** Propose recreating the index from the template in § Where things live: one `## Open` row per checkpoint in `outputs/checkpoints/` and one `## Closed` row per file in `_closed/`, each built from its own fields, all in one approval.
- **There is no next action yet** (the work stopped before anything was decided). A checkpoint still helps if there is an open question to settle first: make the next action `Settle: [question] (owner: [name])`. If there is not even that, say so and ask whether to write it anyway.
- **The user asks to delete a checkpoint.** This skill never deletes. Offer the close (it moves to `_closed/`), and tell them they can delete the file themselves if they want it gone, along with any `[file].backup-*` copies.

---

## Checkpoint file format

```markdown
# Checkpoint: [thread name]

**Project:** [project name, or "none"]
**Paused at:** YYYY-MM-DD HH:MM (the user's local time)
**Pause count:** N
**Status:** open | closed YYYY-MM-DD

## Objective
One sentence. What this thread produces, and for whom.

## Where it stopped
Two or three sentences, no more.

## Decisions taken
- [decision], because [reason]

## Rejected, do not re-propose
- [option], because [reason]

## Files created or changed
- `path`: what changed

## Already done outside the working folder
- [sent / posted / published / changed]: what, where, when. Nothing here gets redone.
- (If nothing: "Nothing. Nothing went out.")

## Next action
One instruction, not a topic.

## Open questions
- [question] (owner: [user / named person / Claude])

## Read on resume
Read: `path` § section
Do not read: [files, with the reason]

## Freshness check
Check for change since the paused-at time: [named files or places]

## Resume set-up
[Same working folder; any tool, connection or setting the work needs]

## Pause history
- YYYY-MM-DD HH:MM: [one line: what that pause did and where it stopped]
```

---

## What this skill does not do

- Does not write anything before the user has seen it and said yes.
- Does not send, post or publish anything. It only writes files in the working folder.
- Does not delete anything. A finished checkpoint moves to `_closed/`, a row that no longer applies moves to `## Closed` and the index and any checkpoint are backed up before every edit.
- Does not write over an existing checkpoint because its name matched.
- Does not run the session close. That is session-audit, and running both on one ending would leave two records of the same session.
- Does not save rules or preferences. A correction made during the session goes through "remember this" (memory-router) or session-audit.
- Does not create any scheduled task except one reminder the user has seen in full and approved, and that reminder only reminds. It never changes or deletes an existing task.
- Does not replace a planned job's plan file; it points at it.
