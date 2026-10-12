---
name: consolidate-memory
description: "Compresses a memory file that has grown past its soft ceiling, merging and summarising older entries WITHOUT losing any information, so the file stays lean. Use when the user says 'consolidate my memory', 'tidy my memory', 'my memory file is too long', 'compress my memory', 'my memory is over the limit' or 'clean up my memory'. Approval-gated: shows a clear before and after and waits for a yes. Archives, never deletes: original older entries are moved to a memory-archive.md in the same folder so nothing is ever lost. Never raises a ceiling."
---

# Consolidate Memory

## Files this skill reads

- `about-me/memory.md`
- `workstations/[name]/memory.md (only when that workstation is named)`
- `projects/[name]/memory.md (only when that project is named)`


Memory files have soft ceilings so they stay cheap to load every session. When a file breaches its ceiling, this skill compresses the older entries to bring it back under, without losing a single distinct fact or decision. Every original entry it compresses is preserved in an archive in the same folder. It never deletes information, and it never raises a ceiling.

---

## When to run

- The user says "consolidate my memory", "tidy my memory", "my memory file is too long", "compress my memory", "my memory is over the limit", or "clean up my memory".
- A specific file is named, or the user asks for a sweep of all memory files.

If the user names no file, check all memory files (see Step 1). If they name one, scope to that one.

---

## Step 0: Know the ceilings

The soft ceilings live in the `## Memory ceilings` section of the root `CLAUDE.md` (or wherever this workspace keeps them). The defaults the EASE OS kit ships with are:

| File | Soft ceiling |
|---|---|
| `about-me/memory.md` | 50 lines |
| `workstations/*/memory.md` | 150 lines |
| `projects/*/memory.md` | 200 lines |

If the user's root `CLAUDE.md` states different ceilings, theirs win; read them from there. These ceilings are a constraint to respect, never to raise.

---

## Step 1: Measure against the ceiling

Measure line count, do not read content yet. Use `wc -l` on the in-scope memory file(s), counting only those that exist. If none of them does, say the workspace has no memory files yet, point the user at "optimise my workspace", and stop. For a full sweep, one command across `about-me/memory.md`, every `workstations/*/memory.md`, and every `projects/*/memory.md`; list them with `find about-me workstations projects -name memory.md -exec wc -l {} + 2>/dev/null` (quoting is handled and missing folders are ignored), never a bare glob, which fails on a workspace with no workstations yet.

- A file the user names ("tidy my memory for [file]") is always treated as full and tidied: every other skill in the kit names the file when it sends the user here, so a hand-off never ends in "nothing to do". On a general sweep with no file named, a file is full when the file's line count plus the entry waiting to be saved would reach or pass its ceiling (the kit's one test for "full"), or, with nothing waiting, when it is at or over its ceiling. If no file is full, say so in one line ("All memory files have room, nothing to consolidate") and stop. Do not compress a file that does not need it.
- For each full file, note how many lines need to come out to leave room under the ceiling for the next few entries. That number is the target.

Only now read, in full, the files that are actually full.

---

## Step 2: Draft a compressed version (do not write it yet)

For each full file, draft a tighter version that leaves room under the ceiling (if entries are waiting, enough room for all of them: the file's line count plus every waiting entry must end below the ceiling). Rules for compression:

- **Merge duplicates.** Two entries saying the same thing become one.
- **Summarise the oldest context** into fewer lines. Never summarise, reorder or trim a `## Contact log` section: it is an append-only ledger. If it is what makes the file too long, propose moving the whole log, verbatim, into its own `contact-log.md` beside the file instead. The oldest entries usually carry the least live value and compress hardest.
- **Keep every distinct fact and decision.** Compression reduces words, never information. If two entries hold two different facts, both facts survive, even if the two lines become one.
- **Preserve dates** on decisions and events; they are part of the fact.
- **Touch the newest entries last.** Recent context is the most useful and should stay closest to verbatim.
- **Apply the workspace voice rules** from `about-me/writing-rules.md`, if it exists (the user's own tone, spelling and formatting; do not impose any default).

The draft is held in the proposal only. Nothing is written to the live file yet.

---

## Step 3: Show a clear before and after

Show the user exactly what would change, so they can see no information is lost:

- Headline: "[file]: N lines, ceiling is M. Proposing a compression to K lines."
- The specific entries you would merge or summarise, shown as before -> after pairs.
- The list of older entries that would move to the archive (verbatim), so the user can confirm the archive captures everything being removed from the live file.

If the user wants the full side-by-side rather than just the changed entries, show the whole before and after. Default to showing the changed entries plus the archive list, since that is what carries the risk.

Then ask for approval. Per file if a sweep found several; do not bundle unrelated files into one blanket yes.

---

## Step 4: On approval, compress and archive in the same action

Only after the user approves a file:

1. **Archive first.** Append the original older entries (verbatim, exactly as they were in the live file) to `memory-archive.md` in the SAME folder as the source. Create that archive file if it does not exist; append to it if it does. Archives have no ceiling and are never loaded at session start. This guarantees nothing is lost.
2. **Back up the whole live file.** Copy the current live file, exactly as it is, to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) in the same folder. This is a complete, recoverable snapshot taken before any change. Never delete this backup.
3. **Then write the compressed live file** (only now, with the archive and backup above already done, so every original is safe). Write the approved compressed version from Step 2 over the live file's body. The compressed file plus the archive together must contain every distinct fact the original held.
4. **Never delete the archive or the backup**, and never compress the archive.

**Contact log split (only when approved in Step 3).** In this order: (a) create `contact-log.md` beside the file, holding the whole `## Contact log` section verbatim (if a `contact-log.md` already exists there, stop and ask instead); (b) back up the live file as in point 2; (c) replace the section in the live file with one line, `## Contact log` then "Moved to `contact-log.md` on [date]."; (d) confirm in one line. Nothing in the log is summarised, reordered or dropped.

Do these writes in this order so the originals are safe before the live file changes. If anything looks off mid-way, stop and tell the user rather than leaving a half-consolidated file.

---

## Step 5: Confirm and close

Confirm in one line per file:

```
about-me/memory.md: compressed 58 -> 41 lines (ceiling 50). 12 older entries preserved in about-me/memory-archive.md.
```

State plainly that the originals are intact in the archive. Provide `computer://` links to the consolidated file and its archive. If the user came here with entries waiting to be saved, show them and offer to save them now through memory-router's usual show-and-yes. If an entry came from the inbox, do not save it here: tell the user to say "triage my inbox", so the item is archived and taken out of the inbox in the usual order rather than saved twice. Then stop.

---

## What this skill does not do

- Does not delete information. It archives every original entry it compresses to a `memory-archive.md` in the same folder, then compresses the live file. The two together always hold every distinct fact.
- Does not compress anything without first showing a before and after and getting an explicit yes, per file.
- Does not raise a ceiling, ever. If the user came with entries waiting, the target is enough room for all of them. If a file cannot get there without dropping a real fact, it says so and offers two ways on: move the oldest whole entries, verbatim, into `memory-archive.md`, or move entries that belong to one project or workstation into that deeper file. It never loosens the limit.
- Does not touch a file that still has room for its next entries, unless the user names it.
- Does not load or compress archive files; archives are write-only here and never loaded at session start.
- Does not reword the user's facts into something they did not say; compression tightens, it does not editorialise.
