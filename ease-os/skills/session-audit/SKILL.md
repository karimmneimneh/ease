---
name: session-audit
description: "The session close. At the end of a work session, lists everything the session left unfinished and gives each item an outcome, re-checks what the session said was done, then catches the corrections, stated preferences and locked decisions the user made in THIS conversation and proposes saving each one to the right file so the system learns. Use when the user says 'audit this session', 'session audit', 'what did we learn', 'save what we covered', 'end of session check', 'anything to remember from this session', 'debrief this session', 'let's debrief' (with no call notes or thread pasted) or any equivalent. Also offered proactively at the end of a substantial session. Nothing is written until the user has seen it and said yes: memory entries are only ever appended as dated lines, and finishing an unfinished item changes a file only after the user approves the exact change, with a dated backup first if anything is replaced. Never sends or posts anything; never deletes or reorders anything."
---

# Session Audit

**Working folder check (before any write).** EASE OS lives in the user's working folder. If a file this skill would write to is missing, first check that this chat has a working folder selected and that it is the one EASE OS was set up in (it holds `about-me/` and a root `CLAUDE.md`). If no folder is selected, or the selected one has neither, ask the user to select the folder they set this kit up in (the one holding `about-me/` and `CLAUDE.md`) before offering to create anything, so nothing is written somewhere that will not be there next chat. Only if they confirm this is the right folder does the skill's own missing-file rule apply.

The session close. At the close of a session, this skill reads back over the conversation, lists anything left unfinished, re-checks what the session claimed, finds the things the user taught Claude without filing them anywhere, and proposes saving each one to the right place. The user approves item by item. Nothing is written until then: memory entries are only ever added, never changed, and the only change to an existing file is finishing an unfinished item the user has approved, backed up first. It never sends or posts anything.

It works from the conversation. It reads no files to find signals; to confirm that an unfinished item is done or that a claim still holds, it may look at the file, folder or task concerned, read-only.

---

## When to run

- The user says "audit this session", "session audit", "what did we learn", "save what we covered", "end of session check", "anything to remember from this session", "debrief this session" or "let's debrief" (when no call notes or message thread is pasted; a pasted call or thread goes to correspondence-log).
- Offered proactively after a session with substantial work, in one line: "Want me to run a quick session audit before we close?" Do not push it on a short or casual session.
- When triggered on a short or purely conversational session, run a quick Step 0 check; if it finds nothing open and there is nothing to save, the close is one line (for example "Nothing to save and nothing left open."), not the full steps.

Do not run mid-task. This is an end-of-session pass.

---

## Step 0: List every open loop, first

Before looking for things to save, read the conversation top to bottom and write one line per item that is not finished. Check every source, every time:

- every request the user made this session, including any answered only in part;
- every promise Claude made ("I'll also", "next", "later", "once that is done");
- every task-list item not marked completed;
- every question put to the user that they have not answered;
- every draft, file or change still waiting for the user's yes;
- every background job, reminder or scheduled task this session started that has not reported back;
- every browser tab this session opened and has not closed;
- every file made during the session that is not yet saved in the working folder.

Some sources will not apply to every session. Where a source plainly does not apply (no task list was used, no background job or scheduled task was started, no browser was used), skip it without comment. Where it might apply but this chat cannot check it (the tool is not available or will not answer), list it once as "Not checked: [source]" with the outcome Your step and one plain step for checking it yourself. Never assume an unchecked source is fine.

Give each item **exactly one outcome** and write it beside the item:

| Outcome | What makes it true |
|---|---|
| **Done** | Checked just now, read-only, not taken from an earlier message. If it cannot be checked, it is not Done |
| **Finish now** | Small enough to finish in this close. Anything it would write or change is shown first and done only on the user's yes, with a dated backup before any replacement. Limited to closing small loose ends the session itself opened, such as saving a file it made into the working folder or adding a line it promised. Never used for deleting, moving or reordering anything, for new drafting of the user's own work (emails, slides, documents), for a change to `CLAUDE.md`, for creating, changing or cancelling a scheduled task, or for anything that would send, post or publish: those are Your step or Next session. A line for a memory or `about-me/` file always goes through Steps 2 to 4 like any other save, never through Finish now. Once finished, the item is shown as Done (finished in this close) |
| **Your step** | Only the user can do it. Say what to do in one or two plain steps |
| **Next session** | Carried by a short ready-to-paste prompt, shown in the closing message, that picks the work up in a new chat |
| **Dropped** | Not worth doing. Give the reason in one line |

Do not close while any item has no outcome, including items that Step 0b, Step 1 and Step 2b add to this list. If the session left nothing open, say so in one line. Step 0 only lists and asks; it writes nothing. Finish now items are done after Step 5, one at a time: show the exact change, wait for the user's yes and first back up any file it would replace as `[file].backup-[date]` (add the time if a backup from the same day already exists; never overwrite a backup). If the user says no, the item becomes Your step, Next session or Dropped.

---

## Step 0b: Re-check what the session claimed

Some of what the session said may no longer be true. For anything it reported as done, sent, fixed, saved or finished, and anything it recorded as pending, blocked or not existing, ask what would have to be true for that to still hold. Check that on the real thing (the file, the folder or the task), read-only, rather than re-reading the message that said it. Before handing over any next step or recommendation, check that nothing later in the session overtook it (a later decision or a change of plan); if something did, rewrite or withdraw it. A claim that turns out false becomes an item on the Step 0 list. Where a claim cannot be checked, say "not checked" rather than treating it as true or false.

---

## Step 1: Scan the conversation for signals

If the sparring-partner skill is available, run its condensed session-end Sweep first (Gear 3). It is advisory and writes nothing. Route each finding it raises: a correction, preference or decision joins this scan as a signal; anything that needs doing is added to the Step 0 open-loops list and gets one of its five outcomes; anything else is Dropped with a one-line reason. A Sweep finding is never handed over as a loose list.

Read back over this session only. Look for the three kinds of signal worth saving in the table below, and ignore everything else.

| Signal | How it looks | Save it? |
|---|---|---|
| **Correction** | The user rewrote, rejected, or redirected an output: "no, not like that", "change X to Y", "that's not how I say it" | Yes, save the underlying rule |
| **Stated preference** | The user used "always", "never", "I prefer", "from now on", "I like it when" | Yes, save it literally |
| **Locked decision** | The user resolved an ambiguity going forward: "let's go with X", "use this framing from now on" | Yes, save it |
| **One-off request** | The user adjusted a single output without generalising: "make this one warmer", "shorter opening this time" | No, ignore it |

The line between a rule and a one-off matters. A one-off is a tweak to one piece of work. A signal is a preference the user would want applied again next time. When you genuinely cannot tell, keep the item but flag it as uncertain in Step 3 and let the user decide; do not save it silently and do not drop it.

If the scan finds nothing worth saving, say so in one line, work through any Finish now items (end of Step 5), then go to Step 6 to show the open-loops list. Do not invent signals to justify the run.

---

## Step 2: Route each signal to the closest-scoped file

For each caught signal, decide where it belongs, using the same routing the memory-router skill uses. Pick the deepest layer it fits.

| Nature of the signal | Destination |
|---|---|
| A voice or writing rule (tone, word choice, banned phrase, sentence pattern) | `about-me/writing-rules.md` |
| A change to how the user describes themselves or their situation | `about-me/about-me.md` |
| A global rule or working preference that applies everywhere | `about-me/memory.md` |
| A rule or preference for one workstation only | `workstations/[name]/memory.md` |
| A fact, decision, or context for one project only | `projects/[name]/memory.md` |

No-duplication rule: each signal goes to exactly one file, the deepest one it belongs in. If a destination file does not exist in this workspace, note that and propose creating it as part of the approval, rather than assuming it is there. If the destination is a project that does not exist yet (no `projects/[name]/` folder), never create a bare `memory.md` for it: offer to start the project with new-project-setup ("start a new project for [name]") and give the item the outcome Next session, or file it in `about-me/memory.md` if the user prefers.

Before proposing an item, check its destination has room. The soft ceilings are in the `## Memory ceilings` section of the root `CLAUDE.md`. Count the file's lines: if the file is full (the kit's one test for "full": the file's line count plus the entry waiting to be saved would reach or pass its ceiling), do not propose adding it; say so, suggest running consolidate-memory first (the user says "tidy my memory for [file]"), and give the item the outcome Next session, with the exact entry written word for word into its next-session prompt. When several entries or a new heading are going to the same file in one go, count them together: if the total would reach or pass the ceiling, save only those that fit and send the rest down the same "tidy my memory for [file]" path. If the workspace records no ceilings, use the kit defaults: `about-me/memory.md` 50 lines, `workstations/*/memory.md` 150, `projects/*/memory.md` 200. For any other file with no ceiling, say "no ceiling on record" and go ahead. Never raise a ceiling.

---

## Step 2b: Check the skills against the work

Where a skill ran this session and the work departed from what its instructions say, name each departure in one line. Installed skills cannot be edited from here, so give each departure the outcome Your step: say what the instructions say, what the work did instead, and that the user can pass it to whoever made the skill. Separately, if the session did the same thing twice, or the user asked for the same thing three times, say in one line that it may be worth its own skill and offer it as Next session. If no skill ran and nothing repeated, output nothing.

---

## Step 3: Show the user the proposal

Present a short numbered list. One row per caught item. For each, show three things: the item in plain English, its proposed destination file, and the exact one-line entry you would append.

```
Caught this session:

1. [Correction] "Keep openings to one sentence."
   -> about-me/writing-rules.md
   Entry: "- YYYY-MM-DD: Openings are one sentence. No two-sentence openers."

2. [Preference] "Always give me the file link in your first message."
   -> about-me/memory.md
   Entry: "- YYYY-MM-DD: Link every written file in the first summary message."

3. [Uncertain] "Make the Acme note warmer." -- might be a one-off, might be a standing tone for Acme.
   -> projects/acme/memory.md (only if you confirm it is a standing rule)
```

Keep the entries short, dated, and in the format the destination file already uses. Do not draft anything you would not be happy to append verbatim.

---

## Step 4: Ask which to save

Ask the user which items to save. Use AskUserQuestion where it fits, or a simple numbered pick ("save all", "save 1 and 2", "skip 3", or per-item yes/no). For any item you flagged uncertain, make the user choose explicitly; do not default it to saved.

"Save all" is allowed because Step 3 has already shown the exact entry for every item, so it is a yes on things the user has read, which is what item-by-item approval is protecting. A yes on a batch they have NOT seen is a different thing and this skill never asks for one: if any item's exact text was not shown, it does not go in the "save all".

Save only what the user approves. If they approve none, that is a valid outcome: skip the appends, but still work through any Finish now items (end of Step 5) before Step 6.

---

## Step 5: Append the approved items

For each approved item, append it to its destination file as a dated entry. Strict rules:

- **Append only.** Add the new line under the most relevant existing heading. If no heading fits, add a short new one rather than forcing it into the wrong place.
- **Never overwrite, reorder, or rewrite** any existing line in the file. Even if the new entry supersedes an old one, leave the old text in place and add the new dated entry beneath it.
- **Match the file's existing format** (table row, bullet, dated line) so the file stays consistent.
- **Apply the workspace voice rules** from `about-me/writing-rules.md`, if it exists (the user's own tone, spelling and formatting; do not impose any default) to anything you write.
- If a destination file was missing and the user approved creating it, create it with a simple heading and then append the entry.

Then work through any Finish now items from Step 0, one at a time, as Step 0 describes.

---

## Step 6: Confirm and close

If anything was saved, confirm it in one line per saved item: what was saved and where. If nothing was saved, say so in one line. Then show the open-loops list from Step 0, including any items Steps 0b, 1 and 2b added, each with its outcome, under a one-line count. If any item is Next session, show its ready-to-paste prompt straight after the list.

```
Saved:
- about-me/writing-rules.md <- "Openings are one sentence."
- about-me/memory.md <- "Link every written file in the first summary message."
Skipped 3 (you judged it a one-off).

Open loops: 4 (2 done, 1 your step, 1 next session)
- Draft reply to the supplier: Your step (send it from your email when ready)
- Second half of the report: Next session

Next-session prompt (paste into a new chat):
"Continue the quarterly report in projects/report. The first half is in outputs/report-draft.md; write sections 4 to 6 in the same format."
```

Provide a clickable link to any file you wrote to. Then stop. Do not chain into another action.

---

## Edge cases

- **The user already said "remember this".** memory-router has probably filed it. Check, and do not file the same item twice.
- **A finding contradicts a locked decision.** Do not resolve it. Show both sides in one line and let the user choose.
- **A finding belongs to the root `CLAUDE.md`.** This skill never appends to that file. Name the file and the exact line you would add, and leave it as Your step.

---

## What this skill does not do

- Does not write anything before the user approves it, item by item. It is read-only until then, and that includes finishing an open loop.
- Does not close with an unfinished item left unmentioned: every open loop gets one outcome.
- Does not delete, reorder or reorganise existing content. Memory files are append-only: new dated entries only. The one change it makes to an existing file is finishing an open loop the user approved after seeing the exact change, and that file is backed up first.
- Does not send, post or publish anything. An item that needs sending is always Your step.
- Does not write to `CLAUDE.md`. A finding that belongs there is shown as the exact line, for the user to decide.
- Does not save one-off tweaks, or save an uncertain item silently; uncertain items are surfaced for the user to decide and never dropped.
- Does not read files to hunt for signals; it works from the conversation, and only looks at a file, folder or task, read-only, to confirm an open loop is done or a claim still holds.
- Does not raise any memory ceiling or force a write into a full file. It measures each destination before proposing and suggests consolidate-memory when one is full.
- Does not invent signals to justify a run; if nothing qualifies, it says so and stops.
