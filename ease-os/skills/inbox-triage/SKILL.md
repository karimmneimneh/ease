---
name: inbox-triage
description: "Walks the user's inbox file item by item, classifies each item and routes it to the right home with their sign-off. Use when the user says \"add this to my inbox\" (to capture one thought) or \"triage my inbox\", \"process the inbox\", \"clear my inbox\", \"what's in my inbox\", \"let's go through the inbox\" or \"what did I capture this week\". Reads `inbox/inbox.md`, classifies items by type, suggests destinations, asks the user to confirm each move (or a whole group they have seen, if they ask to speed up), then writes the item to its destination, copies it word for word into triaged-archive.md and only then removes it from inbox.md."
---

# inbox-triage

**Working folder check (before any write).** EASE OS lives in the user's working folder. If a file this skill would write to is missing, first check that this chat has a working folder selected and that it is the one EASE OS was set up in (it holds `about-me/` and a root `CLAUDE.md`). If no folder is selected, or the selected one has neither, ask the user to select the folder they set this kit up in (the one holding `about-me/` and `CLAUDE.md`) before offering to create anything, so nothing is written somewhere that will not be there next chat. Only if they confirm this is the right folder does the skill's own missing-file rule apply.

## Files this skill reads

- `inbox/inbox.md`
- `inbox/triaged-archive.md`
- `about-me/writing-rules.md`


Walks the inbox item by item (or a group the user has seen, if they ask to speed up). Classifies. Suggests a destination. Asks for confirmation. Moves on confirmation. Logs everything.

The inbox is a holding pen for thoughts captured between sessions. Each line is appended in the format `- [YYYY-MM-DD HH:MM] [type?] thought goes here`, where the `[type?]` tag is optional. This skill is the only sanctioned way items leave the inbox.

**Capturing a thought.** When the user says "add this to my inbox: [thought]" (or "park this for later", "inbox this"), show the exact line in that format and append it to the end of `inbox/inbox.md` on their yes. Nothing else in the file changes. If the file does not exist yet, say that "optimise my workspace" creates it, or offer to create it with the heading `# Inbox` on the same yes.

## When to trigger

- "Triage my inbox"
- "Process the inbox"
- "Clear my inbox"
- "What's in my inbox"
- "Let's go through the inbox"
- "What did I capture this week"

Do not trigger if:
- The user is in the middle of another task and mentions the inbox in passing
- The inbox is empty (read it first; if no items, say so and stop). **If `inbox/inbox.md` does not exist at all**, which is normal before the setup skill has run, say so plainly, mention that "optimise my workspace" creates it, and stop. Do not create it here and do not surface a raw file-not-found error

## Read first

1. `inbox/inbox.md` (the items to triage)
2. `inbox/triaged-archive.md` (the log of past moves, and the format to append to)
3. `about-me/writing-rules.md`, if it exists (voice for any prose written into destination files)

## Classification logic

For each item, classify by type. The line tag (e.g., `[idea]`, `[pattern?]`), if the user uses tags, is the first signal. If no tag, infer from the content.

| Signal in the item | Classification |
|---|---|
| Tagged `[idea]` or an idea for something to make, write, offer or try later | Idea |
| Tagged `[pattern?]` or describing a recurring pattern, a discipline, a way of operating | Pattern hint |
| Tagged `[contact]` or names a person and an action | Contact follow-up |
| Tagged `[project]` or about a named project, a specific application, ongoing work | Project update |
| Tagged `[decision]` or framed as a question the user needs to answer | Decision |
| Reflection, observation, half-formed thought without a clear next move | Note |
| Cannot be classified, or feels like noise on second look | Discard candidate (flag, ask the user) |

If an item is ambiguous (could be an idea OR a pattern hint), surface both options. The user picks.

## Triage flow

For each item, in order:

1. **Show the user the item** verbatim.
2. **State the classification** and suggest the destination.
3. **If the destination is a structured file** (e.g., a backlog table row, a leads-file row), draft what would be written, including format. Show the user the draft.
4. **Ask for confirmation** in one of three forms. Each one says where the item goes AND that it leaves the inbox, because the removal is a change to their file and they are consenting to it too:
   - "Move it to [destination] and take it out of the inbox?" (yes / no / modify)
   - "Two homes possible: ideas list or pattern note? Either way it leaves the inbox." (multiple choice)
   - "Suggest discard. Keep, modify or release? Released items are kept in full in the archive, not deleted." (three options)
5. **On confirmation:**
   - Append/insert the item at the destination, formatted correctly for that file
   - **Write the item verbatim into `inbox/triaged-archive.md`, then remove it from `inbox/inbox.md`.** (If `triaged-archive.md` does not exist yet, create it first, on the same yes, with the heading `# Triaged archive`, the columns `| Captured | Triaged | Item (full text) | Type | Destination | Notes |` and a `## Discard log` section with `| Captured | Triaged | Item (full text) | Reason |`. If the file exists but has no Discard log section, add it the same way. In the item text, write any `|` character as `\|` so the row stays intact and the words stay exact.) In that order, never the reverse: the archive copy is what makes the removal safe, so it exists before the original goes. The archive row carries capture date, triage date, type, destination, and **the item's own words in full** - a one-line summary is a label on the row, never a replacement for the text. This is the kit's backup promise applied to the one skill that removes content rather than replaces it.
6. **Move to the next item.**

If the user says "skip this one for now", leave it in `inbox/inbox.md`.

If they say "do all of them in one go", speed up the asking, never skip it. Group the remaining items by type and show all of a type's proposed destinations together in a single numbered list, each with the same drafted text Step 3 requires for a structured destination, so the group list carries exactly what a one-at-a-time pass would have shown. Then ask once for that whole group: "Move all of these as shown, or tell me which to change?" That is still a yes on every item, because the user has seen every item and its destination before anything moves. Never write or remove an item the user has not seen. An item they pull out of the group goes back to one-at-a-time handling.

## Routing reference

Destinations are generic and depend on the user's own workspace layout. Map each type to the deepest file it belongs in.

| Type | Destination file | How to format |
|---|---|---|
| Idea | The ideas or backlog file the workspace already keeps (e.g. `projects/[name]/backlog.md`, Ideas section). If there is none, ask the user where ideas belong and create that file with a simple heading on their yes. | One row in the ideas table. Add a category if the user states one. Add a one-line note if the item has texture worth keeping. |
| Pattern hint | If first hint: log in `inbox/triaged-archive.md` Notes column with a `pattern-watch:` prefix. If multiple hints converge: propose a short note for the pattern in `library/patterns.md`, and create the file or folder only on the user's yes. It must never depend on a template file that may not exist. | The user decides which case applies. A single hint is not a pattern yet; several hints converging on the same thing is. |
| Contact follow-up | The contacts or CRM file the contact already lives in, wherever the workspace keeps one (a contacts file or a specific project's `memory.md`). If the workspace keeps no such file, ask the user where contacts belong and create that file with a simple heading on their yes, rather than assuming a path. It must never depend on a file that may not exist. | New row using the file's existing column structure. Pick the deepest source file the contact already lives in. |
| Project update | The relevant project's `memory.md` (Project log section; if it has none, add the heading on the same yes). If the project does not exist yet, offer "start a new project for [name]" first and leave the item in the inbox; never create a bare project `memory.md` | One row in the rolling log table, or a dated bullet if there is no table. |
| Decision | The workspace's own weekly review or decisions file, wherever the user already keeps one, under a "Decisions for next week" heading. If the workspace keeps no such file, ask the user where decisions belong and create that file with a simple heading on their yes, rather than assuming a path. It must never depend on a file that may not exist. | New numbered row. |
| Note (general) | `about-me/memory.md`, under a "Notes" heading (add the heading on the same yes if the file has none; the full-file rule below the table applies); if it clearly belongs to one project, that project's `memory.md` instead | Append as a dated bullet. |
| Note (workstation-relevant) | The closest workstation `memory.md`, under a "Notes for the next session" heading (add the heading on the same yes if the file has none) | Append as a bullet or short paragraph. |
| Discard | `inbox/triaged-archive.md` Discard log section only | One row carrying the item's own words in full, plus the reason. A discarded thought is archived verbatim like any other, never reduced to a reason. |

**Full-file rule (every destination that is a `memory.md`).** Before proposing a move into any `memory.md`, apply the kit's one test for "full": the file's line count plus the entry waiting to be saved would reach or pass its ceiling. If it is full, do not move the item: it stays in `inbox/inbox.md`, untouched and not archived. Say so, and suggest "tidy my memory for [file]"; the item is triaged on a later run once there is room. When several entries or a new heading are going to the same file in one go, count them together: if the total would reach or pass the ceiling, save only those that fit and send the rest down the same "tidy my memory for [file]" path.

## Hard rules

- **Never move an item without confirmation.** Suggest, wait, move on yes. This holds in batch mode too: a group confirmation is allowed, an unseen item is not.
- **Never edit existing destination content.** Only append/insert.
- **Never invent context.** If an item is too cryptic to classify, ask the user what they meant. Do not guess.
- **Never lose an item.** Every item is written verbatim into `inbox/triaged-archive.md` before it is removed from `inbox/inbox.md`, discards included. The thought itself survives, not a summary of it: if the archive row would let the user recover only what the thought was about and not what it said, the rule has been broken.
- **Apply the voice rules in `about-me/writing-rules.md`**, if it exists, to any prose written into destination files (the user's own tone, spelling and formatting; do not impose any default).
- **If the inbox is empty**, say "Inbox is empty, nothing to triage" and stop. Do not search for things to do.
- **If an item is older than 14 days**, flag it as stale. Stale items are often discard candidates.

## Output to the user at end of run

After the last item is triaged, summarise:

```
Inbox triaged. N items processed.
- X moved to [destination 1]
- Y moved to [destination 2]
- Z discarded
- W skipped (still in inbox)
```

Then stop. Do not auto-trigger another action.

## Quality gate

Before any item gets written to a destination:
- Voice rules sweep on the prose
- Destination format matches the file's existing columns
- The capture date and triage date are both recorded in `inbox/triaged-archive.md`
- The item's own words are in `inbox/triaged-archive.md` in full BEFORE anything is removed from `inbox/inbox.md`. Check the archive row actually carries the text, not a description of it
- The item is then actually removed from `inbox/inbox.md` after the move (the most common error is moving a copy and forgetting the source)
