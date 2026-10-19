---
name: new-project-setup
description: "Scaffolds a new project folder pair in the Cowork workspace. Use when the user says 'start a new project for [name]', 'create a new project for [name]', 'start a new project called [X]', 'set up project [Y]', 'I want to track work on [Z]' or describes a new piece of work that doesn't fit any existing project. Creates matching folders in projects/[name]/ and outputs/[name]/, generates a blank CLAUDE.md and memory.md and asks which workstation it belongs to, showing what it will create before creating it. It also adds a row to up to three files the user already has (their root CLAUDE.md routing map, about-me/memory.md and the workstation's memory.md), showing each row and waiting for a yes before writing it. Follows the structure rules in root CLAUDE.md."
---

# New Project Setup

**Working folder check (before any write).** EASE OS lives in the user's working folder. If a file this skill would write to is missing, first check that this chat has a working folder selected and that it is the one EASE OS was set up in (it holds `about-me/` and a root `CLAUDE.md`). If no folder is selected, or the selected one has neither, ask the user to select the folder they set this kit up in (the one holding `about-me/` and `CLAUDE.md`) before offering to create anything, so nothing is written somewhere that will not be there next chat. Only if they confirm this is the right folder does the skill's own missing-file rule apply.

One-shot scaffold for a new project in the Cowork workspace. Creates the folder pair and the blank files, and ties the project into the right workstation.

---

## Step 1: Confirm the project

Ask the user, in one short message:

1. **Project slug.** Short, lowercase, hyphenated (e.g., `acme-application`, `client-q3-review`). This is the folder name.
2. **One-line goal.** What the project is for, in plain English.
3. **Which workstation does it belong to?** A workstation is a grouping of related projects. Offer the workstations that already exist in this workspace, plus these common shapes:
   - **An area of their work or life** (for example "home", "studies", "the business"): a workstation for a group of related projects.
   - **Work with one person or organisation.** A relationship they manage over time; it nests under whichever workstation it belongs to.
   - **Client work.** Paid work for a specific client; if there's no workstation yet, propose creating one.
   - **Other.** The user names it; consider whether a new workstation is warranted.
   - **None.** Keep the project on its own, with no workstation. This is fine, and the usual choice for someone just starting out.
4. **Outcome target.** One sentence on what success looks like.

Don't proceed until all four answers are given. If the user's request already covers some, only ask for the missing pieces.

---

## Step 2: Create the folders

**First, show what you will create and get a yes.** List the new folders and files (all new, nothing existing is touched) in one short message and wait for the user's yes before creating anything.

**Then check the slug is free (hard precondition).** If `projects/[slug]/` or `outputs/[slug]/` already exists, stop: do not create, write or overwrite anything. Tell the user the slug is already taken and ask for a different one, so an existing project is never overwritten with blank scaffolding. Only continue once the slug is free.

Two folders, mirroring slugs:

```
projects/[slug]/
outputs/[slug]/
```

If the workstation also needs a folder (rare, only if it's a brand new workstation), create that too at `workstations/[name]/` with a `memory.md` (Step 6 shows its first row). Confirm the workstation creation explicitly with the user before proceeding, because workstations are a bigger structural commitment.

---

## Step 3: Write `projects/[slug]/CLAUDE.md`

Use this template, filled in from Step 1:

```markdown
# CLAUDE.md: [Project Name]
_Last updated: YYYY-MM-DD_

Project-level brief. Voice rules in `about-me/writing-rules.md`. Workstation context in `workstations/[workstation]/memory.md` (if the project belongs to a workstation).

---

## Read in this order

| For | Load |
|---|---|
| Voice and tone (always) | `about-me/writing-rules.md` |
| Workstation context (if any) | `workstations/[workstation]/memory.md` |
| Project memory | `projects/[slug]/memory.md` |

---

## What this project is

[One-paragraph description from Step 1.]

**Outcome target:** [from Step 1]

---

## Standing rules for this project

- [Add as the user corrects, refines or locks decisions during sessions.]
- **Never publish, send or schedule** without the user's explicit sign-off (root CLAUDE.md safety rules).
- **Apply the workspace voice rules strictly** (`about-me/writing-rules.md`).

---

## Files in this folder

| File | Purpose |
|---|---|
| `CLAUDE.md` | This file |
| `memory.md` | Project state, decisions, rolling log |
```

---

## Step 4: Write `projects/[slug]/memory.md`

Use this template:

```markdown
# memory.md: [Project Name]
_Project-only log._

_Soft ceiling: the one for project memory in the root CLAUDE.md (200 lines by default). When full, say "tidy my memory"._

---

## Active state

- Project created YYYY-MM-DD
- Workstation: [workstation name]
- Goal: [from Step 1]

---

## Open items

- [ ] [The user's first action item, if known]

---

## Project log

| Date | Entry |
|---|---|
| YYYY-MM-DD | Project created. [Brief note on initial context.] |

---

## Contact log

_Append-only. Dated entries for calls, emails and messages with anyone outside the workspace. Newest at the bottom._
```

---

## Step 5: Update the root pointer index

Edit `about-me/memory.md`, "Active projects" table. Add a row:

```
| [Project Name] | Active, just created | `projects/[slug]/` | [workstation, or a dash if none] |
```

If `about-me/memory.md` is full (the kit's one test for "full": the file's line count plus the entry waiting to be saved would reach or pass its ceiling), say so when you show the row and suggest "tidy my memory for about-me/memory.md"; the row is still added on a yes, because an index row is too important to drop. If `about-me/memory.md` has no "Active projects" table yet, add the table header first, then the row. If the file does not exist, create it. **Show the user the exact row and wait for a yes before writing it.** Showing is only half of the kit's promise; the promise is that nothing changes in a file without both the showing and the yes. If they decline, write nothing and note the decline so Step 8 does not undo it.

---

## Step 6: Update the workstation memory

Edit `workstations/[workstation]/memory.md`, "Active projects" table. Add the same row. If that file is full (the kit's one test: the file's line count plus the entry waiting to be saved would reach or pass its ceiling), say so when you show the row and suggest "tidy my memory for [file]"; the row is still added on a yes, as in Step 5. **Show the user the exact row and wait for a yes before writing it**, exactly as in Steps 5 and 7. This step writes into a file the user already owns, so it carries the same gate as every other write in this kit; there is no such thing as a change too small to ask about. If they decline, write nothing and note the decline so Step 8 does not undo it.

If the project has no workstation (the user chose to keep it loose), skip this step and its Step 8 check entirely. If the `workstations/[workstation]/` folder or its `memory.md` does not exist yet, **say so and confirm creating them before you do** - Step 2 treats a new workstation as "a bigger structural commitment" and this step must not quietly create one the user never agreed to. On a yes, create the folder and a `memory.md` containing an "Active projects" table, then add the row. Never assume the workstation already exists, and never create one silently.

---

## Step 7: Update root routing map (mandatory final step)

This is the step that is easiest to skip, and skipping it is the single most common cause of decay: a project gets created without a routing row, so the workspace never learns to load it. The fix is mechanical, and this step now blocks the skill from completing.

Ask the user, in one short message:

> What task descriptions or trigger phrases should make Claude load this project? Give me 1 to 2 short phrases the way they'd appear in the routing table (for example, a named outreach campaign, or a specific client engagement with its scope in brackets).

Do not auto-generate this from the project name. The routing column is what Claude scans when deciding what to load, so the user's phrasing matters.

Once the user answers, work out the edit to `CLAUDE.md`: find the routing-map table (match it by intent, whatever its exact heading, e.g. "Routing map" or "Routing Map"). If the user's `CLAUDE.md` has no routing-map table at all, the edit is to append a new "## Routing map" section with a two-column table first, then add the row. **If the user has no `CLAUDE.md` at all**, which is normal on a brand-new workspace before the setup skill has run, say so and offer to create one holding just the routing map. Tell them plainly that a routing-map-only file is a partial constitution: it carries no session-start list, no memory ceilings and no safety rules, and running "optimise my workspace" fills in the rest. Repeat that in the closing message, so a user who created the stub and stopped is not left believing their workspace is fully set up. The row goes in the existing format:

```
| [the user's trigger phrase] | `projects/[slug]/CLAUDE.md` |
```

**Show the user the exact row and wait for a yes before writing it.** `CLAUDE.md` is the workspace constitution, so this edit is approved on its own and never rolled into a blanket "do the rest".

If the project belongs to a workstation that already has a row, place the new row directly below the workstation row. Otherwise place it before the catch-all "A new project not listed above" row if one exists, else at the end of the table.

**Exit gate.** The skill cannot complete until one of the following is true:

1. The user has approved the row and it has been added to root `CLAUDE.md`, confirmed via Read, or
2. The user has declined or deferred it in any form ("skip routing for now", "not now", "leave it" or a plain no), in which case log it in `projects/[slug]/memory.md` under Open items: `- [ ] Add routing row for [slug] to root CLAUDE.md (deferred by the user YYYY-MM-DD)`, shown to the user and written on their yes, and move on. If they do not want even that line, leave it out.

Declining is a complete answer, not a failure to answer. Only silence leaves the gate open: if the user has not responded either way, ask the trigger-phrase question once more before proceeding to Step 8, then take a second non-answer as a defer and log it as above. Never write the row to get past this gate.

---

## Step 8: Confirm

Verify each artefact before announcing:

- `projects/[slug]/` exists and contains `CLAUDE.md` and `memory.md`
- `outputs/[slug]/` exists
- Row added to `about-me/memory.md` Active projects table (or the user declined it at Step 5)
- Row added to `workstations/[workstation]/memory.md` Active projects table (or the user declined it at Step 6, or the project has no workstation)
- Row added to root `CLAUDE.md` Routing Map (or deferral logged per Step 7 exit gate)

If any of the five is missing because its step was skipped, go back and run that step as written, including its question: show the exact row and write it only on a yes. **If the user declined that item, leave it alone and say so in the summary.** This step exists to catch a step that was skipped, never to reverse a decision the user made. A row the user said no to stays un-written. Then tell the user:

> Created `projects/[slug]/` and `outputs/[slug]/`. CLAUDE.md and memory.md scaffolded. Linked under [workstation]. Routing added to root CLAUDE.md as "[trigger phrase]". Ready when you are.

If routing was deferred, say so explicitly:

> Created `projects/[slug]/` and `outputs/[slug]/`. CLAUDE.md and memory.md scaffolded. Linked under [workstation]. **Routing not yet added.** Logged as an open item in project memory per your request.

---

## Edge cases

- **Project name conflicts with existing folder** -> ask the user for a different slug.
- **Project doesn't fit any workstation** -> propose creating a new workstation OR keeping it loose under projects/ with no workstation tie. Don't auto-create workstations.
- **Project is for a recurring activity (e.g., a monthly recurring feature)** -> don't create a project; that pattern already lives at the workstation level. Confirm with the user.
- **Project is a one-off output** (single document, quick task) -> don't create a project. Save it in `outputs/` (in a subfolder if the workspace's own folder rules ask for one).
