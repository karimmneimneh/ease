---
name: memory-router
description: "Routes any 'remember this' moment to the correct file layer before writing. Triggers on: 'remember this', 'remember X', 'save this', 'note this for next time', 'keep this', 'I want to remember', 'make a note', 'noting this' and equivalents, and also when the user locks a rule ('from now on', 'never again', 'this is a hard rule', 'make this permanent') even without a memory word. Not for filing a pasted message or email thread into a project's record, which is the correspondence-log skill. Identifies the correct scope (global, workstation, project, voice, identity), resolves which file to write to and enforces the no-duplication principle. Writes to the determined location and confirms in chat. Does not write before routing is resolved and the user has said yes to the exact entry."
---

# Memory Router

**Working folder check (before any write).** EASE OS lives in the user's working folder. If a file this skill would write to is missing, first check that this chat has a working folder selected and that it is the one EASE OS was set up in (it holds `about-me/` and a root `CLAUDE.md`). If no folder is selected, or the selected one has neither, ask the user to select the folder they set this kit up in (the one holding `about-me/` and `CLAUDE.md`) before offering to create anything, so nothing is written somewhere that will not be there next chat. Only if they confirm this is the right folder does the skill's own missing-file rule apply.

Every "remember this" has exactly one right home. This skill finds it before anything is written.

---

## Step 1: Trigger recognition

This skill activates on any of the following phrases or clear equivalents:

- "remember X"
- "save this"
- "note this for next time"
- "noting this"
- "keep this"
- "I want to remember"
- "make a note"
- "don't forget this"
- "add this to memory"
- Context emerging in conversation that clearly should persist (a decision, a preference, a project update)

**This skill also activates when the user locks a rule, even if no memory word appears.** A rule someone means to be permanent and a rule they ask you to save are the same thing; only the wording differs. Treat any of these as a trigger:

- "this is a hard rule"
- "this should apply from now on" / "from now on..."
- "never again" / "never do this again"
- "make this permanent" / "this is permanent"
- "standing rule" / "non-negotiable"
- a plain always or never instruction meant to last: "always check the date first", "never schedule anything on a Friday"

Two boundaries, because firing when you should not is also a failure. A QUESTION about an existing rule is a lookup, not a declaration: "is this a hard rule?" and "what is my rule on dashes?" do not start the routing. A one-off adjustment to a single piece of work ("make this one warmer") is not a rule either. When it is unclear whether the user means a lasting rule or a one-off, ask once: "Is that a standing rule, or just for this piece?" The entry is still written only after the user has seen it and said yes (Step 5).

On activation, extract the thing to be remembered. Name it in one line before routing.

---

## Step 2: Classify the content

Classify the thing to be remembered against these four types. Pick one. (A skill the user has switched off counts as a rule or preference; route it with the row in Step 3.)

| Type | Description | Examples |
|---|---|---|
| **Voice / writing** | How to write - tone, word choice, banned phrases, sentence patterns, register | a voice or style correction, "shorter sentences in openings", a phrase to stop using |
| **Identity / situation** | Who the user is, how they describe themselves, their current context | a change to how the user positions themselves, "current focus is [type of work]" |
| **Rule or preference** | A working preference, operating rule, or approach - not specific to one project | "always ask one question not five", "deliver the finished work not suggestions" |
| **Fact / decision / context** | Something that happened, was decided, or emerged in a specific project or workstation | a project status update, a new contact preference, a locked decision on one project |

If the type is unclear after one read, ask: "Is this a writing rule, a way you describe yourself, a working preference or a project-specific fact?" Wait for the answer before proceeding.

---

## Step 3: Route to the correct layer

Once the type is clear, apply this routing table. Pick the deepest layer it belongs in.

### File layer routing

| Scope | Where to write |
|---|---|
| Voice / writing rule of any kind - global | `about-me/writing-rules.md` |
| Identity / situation change - global | `about-me/about-me.md` |
| Rule or preference - applies across every workstation and project | `about-me/memory.md` |
| Rule or preference - applies across all projects in one workstation | `workstations/<name>/memory.md` |
| Fact / decision / context - applies only to one project | `projects/<name>/memory.md` |
| Rule or preference - applies only to one project | `projects/<name>/memory.md` |
| A skill the user has switched off ("remember I switched off X") | `about-me/disabled-skills.md`, one row in its table |

**No-duplication principle:** never write the same entry to more than one layer. Pick the deepest layer it belongs in. If the fact is project-specific, it does not go in the workstation file. If the rule is workstation-wide, it does not go in `about-me/memory.md`. If in doubt, go deeper.

### Continuous learning rule (apply in parallel with the routing table above)

When the user corrects or confirms an approach during a session without saying "remember this", treat it as an inferred signal, not an instruction to write. Do not write it silently. Surface it in the moment ("Want me to save that? It would go to `[file]`.") and write only on a yes, or hand it to the session-audit skill to propose at the end of the session. The explicit triggers in Step 1 start the routing, and the entry is still written only after the user says yes to the exact words in Step 5; an inferred signal first needs a yes that it should be saved at all. Route an approved inferred signal the same way:

- Writing correction or preference -> `about-me/writing-rules.md`
- Change to how they describe themselves or their situation -> `about-me/about-me.md`
- Project update, decision, or new context -> the relevant project's `memory.md`
- Workstation-level rule -> that workstation's `memory.md`
- New global preference -> `about-me/memory.md`

Then proceed through Steps 5 and 6 (confirm routing, then write). Never skip the confirmation on an inferred signal.

---

## Step 4: Leave Cowork's own memory alone

Cowork has its own built-in memory, which lives outside the workspace folder. This skill never writes to it. The workspace files above are the single home for everything the user asks to keep, so nothing is ever saved twice and nothing is written outside their folder. If Cowork's own memory picks something up by itself, that is Cowork's feature, not this kit's, and the file layer stays the source of truth.

---

## Step 5: Confirm routing before writing

Before writing anything, state the routing decision in one line:

> "Writing to `[file path]`. Reason: [scope in one phrase]."

**Show the exact entry underneath it, as it will appear in the file**, not a summary of it. The user approves the words that land on disk, never a description of them.

Then ask: "Save this?" and wait. Write only on a clear yes to that entry. If the user edits the words, show the new version and ask again. If anything about the routing feels ambiguous, ask one short question first. Nothing is written until the user has said yes to both the file and the exact entry.

---

## Step 6: Write

Before writing to a `memory.md`, count its lines against its ceiling (the `## Memory ceilings` section of root `CLAUDE.md`, or 50 / 150 / 200 by default). If the file is full (the kit's one test for "full": the file's line count plus the entry waiting to be saved would reach or pass its ceiling), do not write it yet: say so and show the exact approved entry in a copy block, then suggest "tidy my memory for [file]" and offer to save the entry straight after in this chat; the copy block means it survives even if the chat ends. When several entries or a new heading are going to the same file in one go, count them together: if the total would reach or pass the ceiling, save only those that fit and send the rest down the same "tidy my memory for [file]" path. Then write to the determined file(s). Follow the existing format and heading structure in the target file. Do not invent new sections - append under the most relevant existing heading, or add a new one only if none fits.

**If the destination is a project that does not exist yet** (no `projects/[name]/` folder), do not create a bare `memory.md`: offer to start the project first with new-project-setup ("start a new project for [name]"), then save the entry into the memory file it creates. **If the destination file does not exist yet** in an existing folder, say so in one line, ask the user whether to create it, and on a yes create it with a simple heading (the file's name) and then append the entry. A new workspace has none of these files until the setup skill runs, so this is the normal case on day one, not an error. Never fail silently and never write the entry somewhere else instead.

For `about-me/writing-rules.md`: append under the most relevant rule category.
For `about-me/about-me.md`: append a short dated entry under the closest existing heading (or a 'Recent updates' heading). Do NOT overwrite or rewrite any existing sentence or paragraph. If the new fact supersedes an old one, append the new dated entry and leave the old text in place. Never delete or replace the user's existing content.
For `about-me/memory.md`: append a dated entry.
For workstation or project `memory.md`: append a dated entry under the most relevant heading.
For `about-me/disabled-skills.md`: add one row to its Disabled table (skill name, what it does, why it is switched off, how to switch it back on). Ask the user for any column you cannot fill; never guess. If the file does not exist yet, create it, on the same yes, with a `# disabled-skills.md` heading, a `## Disabled` heading and the header row `| Skill | What it does | Why disabled | How to re-enable |`, then add the row.

---

## Step 6.5: Keep state files lean (memory tiering)

Sort memory by how often it is read, not by topic. A project's memory can carry up to three tiers; route the entry to the right one:

- **STATE** (`memory.md`): current state, next actions, live decisions, open risks. Read every session, so keep it lean under its soft line ceiling.
- **LOG** (an append-only ledger such as `contact-log.md`, when the project keeps one): dated, chronological entries (calls, messages, events). No line ceiling; loaded only when history is needed. When a project has a log file, append new chronological entries there, not to the state file.
- **ARCHIVE** (`memory-archive.md`): cold, superseded context. Loaded only when explicitly needed.

When a state file is full (Step 6 checks before writing), do not compact or prune it here; this skill only ever appends. Hand off to the consolidate-memory skill (the user says "tidy my memory for [file]"), which archives the originals and backs up the file before compressing. Never raise the ceiling. A young or small project keeps a single `memory.md`; do not split tiers pre-emptively.

---

## Step 7: Confirm in chat

After writing, confirm with a single line:

> "Saved to `[file path]`. Entry: '[one-line summary of what was saved]'."

No further commentary needed.

---

## What this skill does not do

- Does not write before routing is resolved.
- Does not duplicate across layers.
- Does not write to Cowork's own built-in memory, or anywhere outside the workspace folder.
- Does not rewrite, reorganise, or overwrite the target file. It only appends new entries; existing content is never changed or removed.
- Does not activate on casual references to memory (e.g. "I remember when...") - only on clear intent to persist something.
