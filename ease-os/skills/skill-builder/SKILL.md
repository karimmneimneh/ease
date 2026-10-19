---
name: skill-builder
description: "Turns something the user keeps doing the same way into a reusable skill of their own, built so it stays easy to change: the method goes in the skill and anything personal goes in a file the skill reads. Use when the user says 'turn this into a skill', 'make this a skill', 'build a skill for [task]', 'I keep doing this', 'build my next skill' or picks an idea from the Skill ideas table. Checks first whether an existing skill already covers the job, asks a few questions one at a time, drafts the skill for approval, saves the draft and a ready-to-upload zip in the working folder and records it in the registry. Proposal-only: writes nothing without a yes, never installs anything itself and never sends, posts or deletes anything."
---

# Skill builder

A skill is a written method Claude follows whenever a phrase calls for it. This skill helps the user build their own, the same way the skills in this kit are built: the steps live in the skill, and anything personal (their voice, their lists, their clients, their templates) lives in a file in their folder that the skill reads. Built that way, a skill stays short, is easy to change and could be shared without giving anything away.

---

## Step 0 - Operating rules (non-negotiable)

1. **Show, then save.** Every file is shown in full and written only on a yes. If the user edits it, show the new version and ask again.
2. **Never overwrite.** Before replacing any file, back it up. For a file inside `outputs/skills/[skill-name]/`, and for its `.zip`, the backup goes to `outputs/skills/backups/[skill-name]/[file].backup-[today's date]`, outside the skill's folder so it never ends up inside the upload zip. Every other file (a data file, `about-me/custom-workflows.md`) is backed up beside itself as `[file].backup-[today's date]`, like everywhere else in the kit. If a backup name is taken, add the time, `[file].backup-[date]-[HHMM]`; never overwrite a backup.
3. **The user installs, not this skill.** Installing a skill happens in Cowork's own screens. This skill prepares everything and gives the steps; it never claims a skill is installed.
4. **Safe by design.** Every skill this builds must itself show before it writes, never send or post without the user's yes, never delete what the user wrote, back up any file before replacing it and create scheduled tasks that only remind. Bake those lines into every draft (Step 4).

---

## Step 0.5 - Working folder check

If no folder is selected in this chat, ask the user to select the folder they set EASE OS up in and wait. The draft is saved there, so it must be the folder they use every time.

---

## Step 1 - Consolidate before creating

Before designing anything, check whether the job is already covered:

- the skills available in this session (their names and descriptions);
- the `Custom skills` table in `about-me/custom-workflows.md`, if it exists;
- `about-me/disabled-skills.md`, if it exists (a skill the user switched off may already do this).

If an existing skill covers most of the job, say which one and recommend using it, or adding the missing step to the user's own copy of it, instead of building a second skill that competes for the same phrases. Build a new one only if the user still wants it.

---

## Step 2 - Capture the method

Work from the conversation where possible: if the user just did the task with Claude, draft the steps from what happened and ask them to confirm. Otherwise ask, one question at a time (AskUserQuestion where available):

1. What should the skill be called, and what would you say to start it? (two or three phrases)
2. What goes in: what do you give it, or what does it read?
3. What are the steps, in order?
4. What comes out, and where should it go?
5. What must it never do?

Keep the user's own words for the steps.

---

## Step 3 - Split the method from the data

Go through the answers and sort every specific into one of two piles:

- **Method:** steps, checks and the order they come in. This goes in the skill.
- **Data:** anything personal or anything that will change: names of people or clients, a voice or a list of banned words, prices, templates, a checklist the user will keep editing. This goes in a file in the working folder, and the skill names that file and reads it when it runs.

Use the user's existing files wherever they fit (for example `about-me/writing-rules.md` for voice, a project's `memory.md` for project facts). Propose a new data file only when nothing existing fits, and put it beside what it relates to (`about-me/` for anything about the user, `projects/[name]/` for one project).

Then run the share test on the draft method: could someone in a different line of work read it and learn nothing about the user? If a line only makes sense because of who the user is, move that line to the data file.

---

## Step 4 - Draft the skill

Draft `SKILL.md` in this shape:

```
---
name: [skill-name]
description: "[What it does in one sentence]. Use when the user says '[phrase 1]', '[phrase 2]' or '[phrase 3]'. [What it reads.] [What it never does.]"
---

# [Skill name]

[Two lines: what it is for.]

## Read first
- [data file path]: [what it holds]

## Steps
1. ...

## Safety
- Show anything it would write and save it only on the user's yes.
- Never send, post or publish anything; drafts only.
- Never delete anything the user wrote.
- Back up any file before replacing it, as `[file].backup-[date]` (add the time if that name exists; never overwrite a backup).
- A scheduled task it creates may only remind; it never edits, pauses or deletes an existing task.

## What this skill does not do
- ...
```

The name rules, because an upload fails without them: the `name` is lowercase words joined by hyphens, and it must match the folder name exactly. The description is a single line in double quotes, under 1,000 characters, with no angle brackets, and it names the phrases that start the skill: Claude decides when to use a skill from its description alone.

Then check the draft twice before showing it. **One job:** if the steps do two different jobs, propose two skills. **Read it cold:** read the draft as a fresh session that has never seen this conversation; any step that only makes sense with this conversation's context gets rewritten until it stands on its own.

Draft the data file too, if Step 3 created one: the headings and any content the user gave, with blank lines for the rest. Never fill it with invented content.

---

## Step 5 - Show and save

Show the full `SKILL.md`, then the data file. On a yes to each:

1. Write the skill to `outputs/skills/[skill-name]/SKILL.md` and the data file to its chosen path (backup rule if either exists).
2. Offer to make the upload file: a fresh zip of the `outputs/skills/[skill-name]/` folder, saved as `outputs/skills/[skill-name].zip`, with the folder itself at the top of the zip. If that zip already exists, back it up first (rule 2) and build a new one rather than adding to it. Make it only on a yes, and if code execution is not available, give the user the one step to make it themselves (on a Mac, right-click the folder and choose Compress; on Windows, right-click and choose Send to, then Compressed folder).
3. Offer to record it. If `about-me/custom-workflows.md` exists, show one row for its `Custom skills` table (skill, trigger phrases, what it does and `outputs/skills/[skill-name]/SKILL.md (not uploaded yet)` as the source) and add it on a yes. If the skill came from the `Skill ideas` table, show that row with its Status changed to `built [today's date]` and change it on the same yes, backing the file up first. Never remove a row.

---

## Step 6 - Install and try it

Give the user the steps, because only they can install a skill:

1. In Cowork, open Customize in the left menu, then Skills.
2. Click "+", then "+ Create skill", then "Upload a skill".
3. Choose `outputs/skills/[skill-name].zip`.
4. Open a new chat, select the same working folder and type one of the skill's phrases. You will know it worked when Claude says it is using the skill.

If it does not start: check the skill is switched on under Customize, then Skills; check that code execution is switched on in Settings, then Capabilities; and try naming it directly ("use my [skill-name] skill to..."). If the description is too vague, offer to sharpen it and rebuild the zip.

Once it works, say so in this chat: offer to change the registry row's "(not uploaded yet)" to "uploaded [today's date]", shown first and written on a yes, with the file backed up first. Never remove the row. To change the skill later, edit the file in `outputs/skills/`, then upload it again. To change only its data (a list, a template, a voice rule), edit the data file: no re-upload needed. That is the point of the split.

---

## What this skill does not do

- Does not install, switch on or switch off any skill. It prepares the files and gives the steps.
- Does not build a second skill for a job an existing one already does without saying so first.
- Does not put personal content inside a skill; it goes in a data file the skill reads.
- Does not write anything without showing it first, and backs up anything it would replace.
- Does not send, post, publish or delete anything.
