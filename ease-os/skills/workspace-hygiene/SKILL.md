---
name: workspace-hygiene
description: "Runs a systems-architect hygiene audit of the user's full Cowork workspace. Use when the user says 'audit my workspace', 'hygiene check', 'clean up my files', 'workspace audit', 'system architect review', 'find redundancies' or wants a structural review of files, memory, routing, dashboards or state files and scheduled tasks. Also reads what scheduled-task instructions say and reports any that have gone stale, without changing them. Not for first-time or repeat setup, which belongs to the ease-setup skill: that one adds what is missing, this one audits what is already there. Produces a severity-ranked report at outputs/system-audits/hygiene-audit-YYYY-MM-DD.md, asks about each high and medium finding and proposes low-risk fixes for approval. Also restores a file from its backup or from `_archive/` on 'restore [file] from its backup' or 'move [file] back from the archive'. Different from session-audit (which is end-of-session)."
---

# Workspace hygiene audit

## Files this skill reads

- `outputs/system-audits/hygiene-audit-*.md (most recent only)`
- `about-me/custom-workflows.md`
- `Root CLAUDE.md` (read it if it is not already in context)
- `about-me/about-me.md, memory.md` (read them if not already in context)
- `about-me/disabled-skills.md` (if it exists)


End-to-end systems audit of the user's Cowork workspace. Goal: fewer tokens, less drift, more predictable routing.

Findings are workspace-shaped, not Claude-shaped: file structure, memory layers, routing maps, any dashboards or state files, scheduled tasks, skill drift.

---

## When to run

- The user asks for a hygiene check, workspace audit, system review, or "clean up my files"
- A specific symptom: sessions feel slow, the same rule is asked for twice, a folder feels unmanageable
- After any major workspace reorganisation
- Monthly cadence is healthy. Quarterly is the minimum.

Do not run mid-task.

---

## Token-efficiency rules (apply throughout)

These rules cut the audit's token cost roughly in half versus a naive run. Honour them in every step.

- **Never do a full Read on a routing file unless content review is the explicit goal.** Use `wc -l`, `head -30`, `du -sh`, `ls -lh` to inspect metadata. The audit measures structure, not content.
- **Batch reads.** When multiple files need reading, emit multiple Read calls in a single tool turn (parallel) instead of one per turn.
- **Consolidate bash exploration.** Target 2 to 3 well-crafted bash commands for the entire structural inventory, not 6 to 10 narrow ones.
- **Skip steps that don't apply.** If an optional scan returns no actionable signal within about 30 seconds, abandon it. The cost of running it must justify the find rate.
- **A zero must say what it looked at.** Every scan reports how many files or tasks it inspected. A scan that inspected nothing has failed; report it as failed, never as clean.

---

## Procedure

## Safety: propose-only, never assume (read this first)

This is the most important rule in this skill. It runs on a workspace that may not be the one it was written for, and that the user has invested in.

- **Never delete, move, or rewrite a file without the user's explicit, item-by-item approval.** There is no unattended change mode. Every change is shown first. Every delete or move is its own approval item and is never folded into a list of other fixes; the one grouped item is "remove all N `.DS_Store` files", with every path listed. Other fixes are approved one at a time or as a yes on a list the user has read line by line (Step 10).
- **Never assume a path is safe to clean up.** Any specific paths named below (for example `outputs/old-reports/`) are only examples. On this user's system they may not exist, or may hold work that matters. Confirm a path exists and is genuinely disposable, then PROPOSE it; never act on it blind.
- **This skill only ever proposes.** It reports findings and suggests improvements. The user decides and approves. When in doubt, leave it alone and flag it.

### Step 0.5: Is there a workspace to audit yet?

Check whether the workspace has any of the structure this skill audits: an `about-me/` folder, a `projects/` folder, a `workstations/` folder, or a root `CLAUDE.md`. **If none of them exists**, this is a workspace the setup skill has not run on yet. Say so in one line, point the user at "optimise my workspace", and stop. Do not run the scans, and do not produce a severity-ranked report about an empty folder: a new user reading "High severity" findings about a workspace they have not built yet learns nothing and is needlessly alarmed.

Where some structure exists but a given step's target does not, that step skips and says so in the report. No step errors on a missing path.

### Step 0: Read the prior audit FIRST

Before any inventory, find the most recent dated audit report anywhere in the workspace, archived copies included:

```
find . -name 'hygiene-audit-[0-9]*.md' ! -path '*/.git/*' | awk -F/ '{n=$NF; sub(/\.md$/,"",n); print n"\t"$0}' | sort | tail -1 | cut -f2
```

Read it in full. Two minutes here saves an hour of rediscovery. Search the whole tree, because an approved archive move puts old reports under `_archive/outputs/system-audits/`, and a search of the live folder alone wrongly concludes this is a first run. Match the dated form `hygiene-audit-[0-9]*.md`, so a file whose name merely starts the same way is never taken as the baseline. Use only this skill's own reports, never another kind of report in the same folder.

Extract:
- Headline numbers (file count, size, biggest sinks) - these become the "delta" baseline
- Findings ranked H1, H2, H3 - these tell you what to verify first ("is H2 still present, is H3 still present")
- Items in the "Recommended fix sequence" that were never executed - these become candidates for THIS audit
- Structural weaknesses surfaced previously - note any that recur
- The `SESSION-START-COST:` line, if present

If no prior audit exists, this is the first run. Note that fact and proceed.

### Step 1: Scope the audit (use AskUserQuestion)

Two questions before any work starts. If AskUserQuestion is not available, ask the same questions in plain text and wait for the answers:

1. **Depth:** Structural scan + content sampling (recommended, ~85% of value), Deep content read (slower, exhaustive), or Quick structural only (folder tree + sizes, shallow).
2. **Output mode:** Report + interactive approval (recommended), or Report only. Every proposed change is shown before it happens, and every deletion or move is its own approval item. The skill only ever proposes; it acts only on items the user has approved.

Default to recommended on both.

### Step 2: Map structure (one bash call)

A single bash command that captures: file count (excluding `*.backup-*`, `_archive/*`, `.git/*`, `.DS_Store`), total disk size, breakdown by top-level folder, top 20 biggest files, files older than 30 days, empty folders, `.DS_Store` count. Aim for no more than 45 lines of output.

In parallel (separate tool turn), only if a scheduled-tasks tool is available (depending on the Cowork version it may be called `list_scheduled_tasks` or `list_triggers`; use whichever this session has, including completed tasks where the tool offers that), list the tasks. If none is available, skip this call and Step 5.

### Step 3: Sample-read routing-critical files (parallel, metadata-first)

Routing files to inspect: root `CLAUDE.md`, every file its session-start section names (read the section rather than assuming the kit's three, because the user may have changed it), each workstation `CLAUDE.md`, each project `CLAUDE.md`, `custom-workflows.md`.

**Inspect metadata first.** A single bash command returning `wc -lc` (lines and bytes) + `head -3` for every routing file. Compare line counts against the ceilings in root CLAUDE.md's `## Memory ceilings` section (or wherever this workspace keeps them; if none are written down, use the kit defaults: 50 / 150 / 200 lines). Compare last-updated dates against today.

**Report bytes as well as lines.** The line ceiling catches a file gaining entries; it misses entries growing heavier, which is what happens when an entry is added to rather than replaced. Report both for every routing file, and compare bytes per line with the prior audit: a rising figure means a file is gathering history, not topics. For any file over its ceiling, also name its largest single section, because splitting that section is usually the fix.

**Treat a file with no room for its next entry as full.** If what is left under the ceiling is smaller than one typical entry in that file, flag it now rather than next time.

**Name the writer.** When proposing to shrink a file, name what keeps adding to it (a skill, a scheduled task or a habit). A file brought under its ceiling with nothing capping its writer is back over within days; propose the cap as its own item alongside.

**Rank by read cost, not by how far over the ceiling.** A file read at the start of every session costs more than a larger one opened once a month. Rough read cost is size times how often it loads: root `CLAUDE.md` says what loads every session, and a scheduled task's own instructions say what it reads. Sort findings by read cost, and where that order differs from the over-the-ceiling order, say so. A session-start file can be worth trimming while still under its ceiling; a large reference file read only on demand is usually not a finding at all.

**Read in full ONLY when:**
- A line count breaches its ceiling (compress candidate). For a `memory.md` over its ceiling, the fix to propose is "run consolidate-memory on [file]" (the user says "tidy my memory for [file]"), which archives before it compresses; never compress a memory file in place from this skill
- A header date is >30 days old and the file has no newer dated entry (staleness candidate; an old header on a file whose entries are recent is not stale)
- Step 0 flagged a specific routing file as recurring problem
- A bash command surfaced an unexpected anomaly
- A byte count is high for the file's line count, or has risen since the prior audit

When full reads are needed, batch them in a single tool turn.

Look for:
- Rules duplicated across files (same banned word list, same audience definition)
- Pointers gone stale (file references that no longer match the file's name or location; ignore template rows marked `_(example)_`)
- Overrides not marked as overrides
- "Load on demand" promises broken by pre-loading

### Step 3.5: CLAUDE.md drift checks (the six)

Six structural checks on the workspace's CLAUDE.md files and always-loaded MDs. Each is generic logic-vs-data hygiene: keep volatile state, restated rules and stacked changelogs out of the files that load every session. Run them whenever Step 3 surfaces a routing file worth a closer look. All six are propose-only: detect, then show the fix and wait for approval.

**Check A - State in the charter.** A CLAUDE.md should hold routing plus stable rules; volatile state (a live status, a near-term date) belongs in a `memory.md` with a one-line pointer. The tell: a status changed and a CLAUDE.md had to be edited. Detect:

```
grep -rinE "current status|pending|deadline|awaiting|this week|next step:|as of [0-9]" --include=CLAUDE.md projects workstations 2>/dev/null
```

Flag any CLAUDE.md carrying a live status or near-term date that will change within weeks. Fix (on approval): move the volatile lines into that project's `memory.md`, leave a one-line pointer ("Current status in `memory.md`"). A single rarely-changing charter line (for example "DORMANT", "Closed") is fine; rolling week-to-week status is not.

**Check B - Cross-file restatement.** The same rule, list or definition copied into several files instead of owned once and pointed to. This breaks single-source-of-truth: a rule is owned by one file and referenced from others, never re-stated in full. Detect by taking a known owned rule's signature phrase and searching for it across CLAUDE.md files, then counting how many restate it:

```
grep -rl "[signature phrase of a rule that should live in one file]" --include=CLAUDE.md .
```

Distinguish a full restatement (the whole rule or list copied, a breach, fix it) from a one-line reminder or pointer (acceptable; do not over-normalise, a one-liner can cost fewer tokens than the cross-link the agent must otherwise chase). Fix (on approval): replace the full restatement with a pointer to the owning file, keeping at most a one-line summary.

**Check C - Inline changelog bloat.** A `_Last updated` header that has grown into a multi-entry changelog, sitting at the head of a file that loads often, costs tokens on every read. Two shapes count as stacked history: two or more labels (`Previous:`, `Previously:`, `Prior:`, `Earlier:`), or one label followed by a run of dated entries separated by semicolons. Raw length is a secondary hint, not the trigger: a single current entry can legitimately run long, and is never truncated.

Look at the first ten lines of each file, not one fixed line (a frontmatter block or a blank line pushes the header down), and let the folder tree say which files to look at rather than a typed list of folders (a typed list misses every project `memory.md` and anything the user keeps elsewhere):

```
n=0; hits=0
while IFS= read -r f; do
  n=$((n+1))
  L=$(sed -n '1,10p' "$f")
  c=$(printf '%s' "$L" | grep -oE "Previously:|Previous:|Prior:|Earlier[^:]{0,20}:" | wc -l | tr -d ' ')
  if [ "$c" -ge 2 ]; then echo "BLOAT ${c} labels: $f"; hits=$((hits+1)); continue; fi
  LU=$(printf '%s\n' "$L" | grep -m1 -i "last updated")
  [ -n "$LU" ] || continue
  d=$(printf '%s' "$LU" | grep -oE "[0-9]{4}-[0-9]{2}-[0-9]{2}" | wc -l | tr -d ' ')
  [ "$d" -ge 3 ] && { echo "BLOAT ${d} dates: $f"; hits=$((hits+1)); }
done < <(find . -name '*.md' ! -path '*/.git/*' ! -path '*/_*/*' ! -path './library/*' ! -name '*-changelog-archive.md' ! -name 'memory-archive.md' ! -name 'contact-log.md' ! -name '*.backup-*')
echo "files inspected: $n | hits: $hits"
```

The `*/_*/*` clause skips every folder whose name starts with an underscore, `_archive/` included, so history that has already been archived is never handed back. If this workspace writes dates in another format, adapt the date pattern. Two rules the code carries. The last line is not decoration: a zero-hit run must say how many files it opened, or a clean workspace and a broken search look the same. And if it inspected 0 files while the workspace has a `CLAUDE.md` or an `about-me/` folder, the search failed: say so and never report a clean result. Before proposing a trim, open the hit and confirm the stack sits in a real `Last updated` header; the word "Previous:" in ordinary body text is a false positive.

Fix: apply Check D.

**Check D - the changelog-archive rule.** The fix for Check C, and a standing convention for every `_Last updated` line:

- Keep only the newest 1 to 2 entries inline. Never truncate the newest entry's own content: it is current; only the stacked history (labelled entries, or a run of dated entries) is archived.
- Move the older history verbatim to `_archive/[name]-changelog-archive.md` (the workspace's `_archive/` folder, which setup lists in the folder layout; create the folder or the file if absent, on the approval that covers this fix). In the archive file, add each newly moved block at the top, above older history, and never delete anything already there. Naming: for a CLAUDE.md, prefix it with its folder so archive names never collide (for example `root-claude-...`, `[workstation]-CLAUDE-...`); an `about-me/*.md` basename is already unique.
- End the inline line with a pointer: "Older history in `_archive/[name]-changelog-archive.md`."
- Log the move like any other: append `date | [file] changelog history | _archive/[name]-changelog-archive.md | changelog trim` to `_archive/archive-log.md`.

**Check E - Doc-vs-executable lag (the routing prose trails the live behaviour).**
A rule, policy or connector-access change gets rolled out to the things that *run* (skill sources and scheduled-task prompts), but a CLAUDE.md routing table, connector-usage table, or a "pulls from / reads / uses" line still describes the *old* behaviour. The doc now contradicts the live skill it points to. The tell: an executable was updated for a rule change and a CLAUDE.md describing it was not.

Detect: for any CLAUDE.md line that says a named skill "pulls from / reads / uses [X]", open that skill's source and confirm it still does it; a mismatch (source updated, doc not) is the lag.

Fix (on approval): update the routing doc to match the live executable and the established rule. Standing habit this check enforces: when a rule or policy changes, grep the CLAUDE.md routing and connector tables in the same sweep, not just the skill sources and task prompts, because the prose layer is the one that silently lags.

**Check F - A file restating a fact the workspace can measure.** Checks B and E compare a file with another file. This one compares a file with the live workspace: a line that restates something countable (how many projects or skills there are, a file's size, a task's schedule) goes stale the moment the thing changes, and no search for a second copy will find it, because there is none. Detect: in each routing file, look for a number attached to something this run can measure (folders under `projects/`, skills in this session's list, tasks in the task list, a file's size) and measure it. Flag every mismatch. Fix (on approval): prefer replacing the number with a pointer to where the live figure lives over correcting it, because a corrected number starts drifting again the next day. Keep a number only where the reader cannot see the live source, and say why it was kept.

### Step 3.6: Clock-time drift in docs that describe tasks

Extends the CLAUDE.md drift checks to any doc that *describes* a scheduled or recurring task rather than being the task itself: a registry doc, a how-to note, or a template's empty-state copy should name the task it describes, never restate its clock time or day. The time rots the moment the task reschedules, and the describing doc is rarely the one that gets updated when it does.

Detect:
```
n=0
while IFS= read -r f; do
  n=$((n+1))
  grep -nE "[0-9]{1,2}:[0-9]{2}( ?(am|pm|AM|PM))?" "$f" | grep -viE "changelog|Last updated" | while IFS= read -r h; do printf '%s:%s\n' "$f" "$h"; done
done < <(find about-me workstations projects -name '*.md' ! -path '*/_*/*' ! -name '*.backup-*' 2>/dev/null)
echo "files inspected: $n"
```
Adjust the folders to wherever this workspace keeps its task-describing docs. For each hit near a named task, compare against that task's live schedule (the scheduled-tasks tool if available, or `custom-workflows.md` if a fresh list is impractical mid-audit). Flag any mismatch, and flag every hit as a candidate even if it currently happens to be correct - a correct clock-time today is still a future drift risk once the task reschedules.

Fix (on approval): strip the clock time, name the task, and where useful point to the registry file for the current cadence. Exempt (leave as they are): frozen historical changelog entries; dated records in log or tracker rows (for example "sent 2026-05-25 10:30"), which record when something happened rather than when a task runs; and example timestamps inside sample data. Only a live statement of when a task runs gets stripped.

### Step 3.7: Open items left open (report only)

Walk every `memory.md` and `CLAUDE.md` for lines or sections headed "Open items", "Pending", "To do" or "Decisions needed". List each item open longer than 30 days (or the window the user's `CLAUDE.md` sets): file, item, days open. Where an item carries no date, say so rather than guessing its age. Report only: the user decides whether each is done, dropped or still live.

### Step 4: Audit the outputs folder

If the workspace has an `outputs/` folder, list its subfolders read-only and note any obviously old or dated state files as cleanup candidates to PROPOSE (never act on them). If there is no such structure, skip this step. Do not assume any specific subfolder such as daily-audit or morning-brief exists.

### Step 5: Audit scheduled tasks (propose registry reconciliation)

If no scheduled-tasks tool is available in this session, skip this whole step and note in the report: "scheduled-task audit skipped: no scheduled-tasks tool in this workspace." Do not error.

**Never trust an empty or short task list at face value.** If the list comes back empty, or holds fewer tasks than `about-me/custom-workflows.md` records (count real rows only, never a placeholder row such as `_(add a row when you create a scheduled task)_`), treat it first as a read that failed or looked in the wrong place, not as "no tasks": from inside the audit the two look identical. Re-check once; if the counts still will not reconcile, report "task list could not be read reliably", never "clean".

**If the list shows whether a task needs this computer**, flag (never change) any task whose instructions read or write files in this folder but which is not set to run on this computer: a task like that can run with no access to the folder and quietly do nothing.

From the task list:
- Sort by enabled / disabled. Disabled tasks that fired (one-time) are normal; disabled tasks that should be running are broken.
- For each enabled task: when did it last run? If older than its cadence suggests, the task may be stuck.
- **Cross-check against `about-me/custom-workflows.md`.** If that file does not exist, note in the report that the workspace keeps no task registry and skip the diff; do not create one as a side effect. Otherwise build the diff in two columns: tasks live but not registered, tasks registered but not live.

Flag the diff as a finding and PROPOSE reconciling the registry (adding the missing tasks, updating changed schedules, in the existing table format). Show the exact rows you would add and get the user's approval before writing. Never write to their registry unattended. **This step never touches the live scheduled tasks themselves: it reconciles the written registry against them and nothing more.** It does not pause, edit, reschedule or delete a task at any point. Step 11 may CREATE a new task, but only one the user has seen and approved in Step 10, and creating a new task is the only scheduled-task write anywhere in this skill.

### Step 5b: Read what the task instructions say (report only)

Step 5 checks whether a task exists and is recorded. This step checks whether its instructions still make sense, because each defect below leaves a task running and reporting normally while doing the wrong thing. Run it only if the task list includes each task's instructions; if it does not, skip and say so. With more than about ten tasks, read a sample (the oldest first, plus any whose project changed since the last audit) and say in the report which ones were read and why, so a sample is never mistaken for a full sweep. Check 1 is cheap: run it on every task.

1. **Broken paths:** a file or folder the instructions name that no longer exists.
2. **Retired skills:** a skill the instructions call that is no longer in this session's skill list, or is listed in `about-me/disabled-skills.md`.
3. **Expired dates and frozen counts:** "as of [date]", "the three projects", "all five tasks". A count written into prose about something that grows will be wrong later, and it will look like a fault in the thing counted.
4. **Schedule restated in the instructions:** the schedule belongs in the task's own schedule setting. Instructions that also say "every Monday at 9" give two sources of truth, and only one gets updated.
5. **Questions in a run nobody watches:** an instruction to ask the user or wait for approval inside a task that fires on its own. With nobody there, the run stalls or the question gets answered without the user.

Report each finding with the task name, the exact line and suggested new wording. This step never edits a task: the user makes the change, exactly as Step 5 and Step 11 say.

### Step 6: Audit skills (source vs installed, proposing a Pending reinstall update)

One bash command: `ls outputs/skills/` for source folders, but only if that folder exists. Many workspaces do not keep skill sources there; if it is absent, skip the source comparison (the stale-row check at the end of this step still runs). Cross-check any sources you do find against the runtime `<available_skills>` list visible in this session (look for matching names under any plugin namespace, e.g. `[plugin-name]:*`).

For each source skill not visible in the runtime list:
- Confirm the SKILL.md exists and has substantive content (`wc -l` >= 30, has frontmatter)
- Categorise: never installed, removed or not reinstalled, intentionally disabled (check `about-me/disabled-skills.md`)

Flag the drift as a high-severity finding and surface the count. Show the exact rows you would add, then, when the user approves, add them to the `Pending reinstall` table in `custom-workflows.md` with each drift candidate (skill name, source line count, why-it-matters, date logged, status). If the file exists but has no `Pending reinstall` table (older setups), show the heading and header row `| Skill | Source lines | Why it matters | Date logged | Status |` with the rows, and add both on the same yes. If `custom-workflows.md` does not exist, say so and stop there; do not create it as a side effect, exactly as in Step 5. This is a reversible documentation update, but it is still only made on the user's approval, on rows they have seen.

**Then close the loop the other way.** For each row in the `Pending reinstall` table whose Status is still open, check whether that skill now appears in this session's skill list. If it does, the row is stale: it tells a future session the skill is missing when it is not, and invites a needless reinstall. Propose updating that row's Status to `resolved [today's date]: now installed`, showing the row before and after. Never remove the row. This is an edit to an existing file, so it follows the Step 11 backup rule and happens only on the user's yes. A skill still missing keeps its row unchanged. This check runs whenever `custom-workflows.md` has a Pending reinstall table, even when there is no `outputs/skills/` folder.

### Step 7: Identify dead weight (use the ALWAYS SAFE list)

The items below are the ONLY ones the skill may propose to clean up, and even these are never executed without approval. List every item by its full path, then ask about each one on its own: a delete or a move is never approved as part of a batch of other fixes. The single exception is `.DS_Store` files, below. **Impact check first.** Before proposing any move or delete, search the workspace's instruction and registry files (every `CLAUDE.md`, `about-me/custom-workflows.md`, and any scheduled-task prompts the scheduled-tasks tool can list) for the path. If anything points at it, name that dependent in the approval item and recommend keeping the file where it is; moving it would break the pointer silently. Before proposing any path-specific item, confirm that path actually exists in this workspace and is genuinely disposable - the example paths come from one particular setup and may mean nothing, or hold real work, on this one.

**Safe to propose DELETING (system clutter only, never anything the user wrote; always confirm the path exists first):**
- `.DS_Store` files (macOS folder metadata; the system recreates them). These count as ONE item: "remove all N `.DS_Store` files", with every path listed in the report
- Empty folders (no `.gitkeep`, no `.keep`, no hidden file, nothing inside at all), **except structure the kit sets up and expects to exist even when empty**: the top-level `about-me/`, `workstations/`, `projects/`, `inbox/`, `outputs/`, `library/` and `_archive/` folders, any folder inside `projects/` or `workstations/`, and any `outputs/[name]/` whose project exists. Those are never proposed for deletion

**Safe to propose MOVING to `_archive/` (never deleting; the file stays recoverable):**
- Files whose filename contains `(old)`, `(obsolete)`, `-legacy`, `-deprecated`
- Clearly superseded files the user confirms are dead. **Successor or it stays:** name the newer file that replaced it in the approval item; if no successor can be named, it is not superseded and it stays where it is
- Old dated state or brief files in an outputs subfolder, where the user confirms they are archives
- `INSTALL.md` files where a registry like `about-me/custom-workflows.md` is the canonical source

Nothing the user wrote is ever deleted by this skill, only moved to `_archive/`. If they want an archived file gone for good, that is their own action outside the kit.

**Always flag, never act on:**
- Multiple drafts of the same document (e.g. report-v1 to v7) - the user's call which versions to retain
- Image folders with rejected options - needs image-by-image judgement
- System-audit plans in `outputs/system-audits/` other than this skill's own `hygiene-audit-*` reports - the user's call on which were executed
- Anything not on the ALWAYS SAFE list

When executing an approved delete in Step 11 (a `.DS_Store` file or an empty folder, nothing else), if the bash sandbox refuses with "Operation not permitted," call `mcp__cowork__allow_cowork_file_delete` then retry. If that tool is not available, or the user declines the permission prompt, leave the item in place and list it in the report as "approved, not removed: delete not permitted in this workspace". Never work around the block. Only ever reach this point for an item the user approved in Step 10; never grant delete permission or remove anything during this propose step.

### Step 8: Sparring-partner mode (structural weaknesses)

Step away from the file list. Look at the system as a whole. Ask:
- Is the "load on demand" architecture actually enforced?
- Are voice rules unified, or scattered with overlap?
- Does each high-frequency workflow have a written procedure it follows, or only some of them?
- Is skill-trigger telemetry available? If not, how would the user know if a skill is failing to trigger?
- Are scheduled tasks growing without the registry keeping pace?
- Are output sub-folders accumulating without an archival cadence?
- Is there a master health check beyond this skill?
- Does the same theme, correction or blind spot show up in two or more memory files without ever being written down as one rule? Flag at most three, each with the files it appears in and the one file the rule belongs in. None is a fine answer; never force one.

This step is where the audit earns its keep. Spend real attention here.

### Step 9: Write the report

Save to `outputs/system-audits/hygiene-audit-YYYY-MM-DD.md` (if a report of that name already exists from an earlier run today, add the time, `-HHMM`; never overwrite an earlier report). Use this structure:

1. **Headline numbers** (file count, size, biggest sinks, scheduled-task count): present as a delta table with the prior audit's numbers if Step 0 found one.
   Include one **session-start cost** figure: the bytes of root `CLAUDE.md` plus every file its session-start section names (derive the list from that section each run, never from a typed list), divided by four as a rough token count, with the file count beside it. It is an estimate to watch across audits, not a gate: it never fails and never produces a recommendation on its own. Write it as one literal line so the next audit can find it: `SESSION-START-COST: ~<tokens> tokens | files:<n> | bytes:<n>`. If the prior audit carries that line, show the change; if it has none, say "first reading".
2. **What was changed this session (approved fixes only)**: table form, one row per fix, filled in at Step 12 once the fixes have run. Skip this section if output-mode was report-only.
3. **Delta from prior audit**: what's resolved, what regressed, what's new. Skip if first run.
4. **Findings ranked H/M/L**: each with detail, cost, consequence, fix, severity.
5. **Structural weaknesses (sparring-partner)**: patterns not individual files.
6. **Recommended fix sequence (cheapest first)**: so the user can stop mid-list and still have value.
7. **Estimated savings**: token cost per session, disk savings (Step 12 appends the actual).
8. **Token cost of this audit run**: rough estimate of context tokens consumed. Calibrates whether future runs need tightening.

The report is the deliverable. Even if no fixes are approved, it stands alone.

### Step 10: Run AskUserQuestion for risky findings (interactive mode only)

Skip if output-mode is report-only.

Use AskUserQuestion (if it is not available, ask the same questions in plain text, one at a time, and wait for each answer). Batch the high-severity decisions into one call with up to 4 questions. Always provide a Recommended option. Phrase each option as a concrete action with consequences spelled out.

For M-severity findings, **show the full list first**, one line per fix naming the file and exactly what changes, then ask a single "apply these?" question with three options: do all as shown, approve individually, skip. **The list is not optional and never collapses into a count.** "Do all" is a yes on items the user has read, which is what the Safety section above requires; a yes on a hidden batch is not, and this skill never asks for one. Never put a change to a CLAUDE.md or a memory.md in the "do all" batch, and never a delete, a move or a changelog trim (which moves text into `_archive/`): those are always shown in full and approved on their own.

### Step 11: Execute approved fixes

For each approved fix, do the file operation directly. Rules:
- Archive (move to `_archive/`) where the file has any historical value. **Keep its folder path under `_archive/`** (`projects/x/INSTALL.md` goes to `_archive/projects/x/INSTALL.md`), so two files with the same name never meet. If that path is already taken, add the date to the new name (`INSTALL.2026-09-30.md`); never overwrite anything in `_archive/`. Then append one line to `_archive/archive-log.md` (create it with a heading if absent): `date | original path | archived path | reason`. Show that log line as part of the move's approval item, so the user approves the move and its record together. The log is what makes every move reversible
- Delete only items on the ALWAYS SAFE list that the user explicitly approved in Step 10 of this session; the ALWAYS SAFE classification only ever permits proposing, never deleting on its own
- Edit any file in place surgically, never wholesale. **Before replacing text in ANY existing file - a CLAUDE.md, a memory.md, an about-me file, a resource file, a template, anything - copy the original to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) and never remove that backup.** The kit promises a dated backup of anything it replaces, so the rule follows the act, not the filename: a changelog trim and a stale-date fix are replacements exactly as much as a constitution edit is
- Any edit to a CLAUDE.md (the constitution) **or to a memory.md** must be shown in full and approved on its own, never applied as part of a blanket "do all"
- Create scheduled tasks for items that need recurring enforcement (e.g. a monthly clean-up reminder). A task this skill creates may only remind the user (for example "time for your monthly workspace audit: say audit my workspace"); its prompt must never apply, move or delete anything, and never ask questions in a run nobody is watching. The task's approval item also shows the row it adds to the `Scheduled tasks` table in `about-me/custom-workflows.md` (if that file exists), so the next audit finds it registered. **And only ones the user saw and approved in Step 10, and only if a scheduled-tasks tool is available; if it is not, say so and put the proposed task in the report instead.** Never pause, edit, reschedule or delete an existing task: a task the user already has is theirs, and this skill proposes changes to it rather than making them
- Where an approved fix changes a rule, propose the matching one-line note in the relevant CLAUDE.md (date and audit reference) as its own item: show it in full and write it only on its own yes, like any other CLAUDE.md edit

### Step 12: Re-measure, then confirm and close

Re-measure before summarising: a summary written first becomes the thing the numbers are fitted to.

If no fixes were applied (report-only, or everything skipped), say "no fixes applied, nothing to re-measure" in one line and go straight to the close.

Otherwise re-run the Step 2 inventory, unchanged so the two readings compare, and show a three-column table: metric, before (Step 2), after. At minimum: file count, total size and the size of every file a fix touched.

Then compare against the estimated savings in the report (Step 9, section 7): estimated, actual and the gap. Name what explains the gap rather than averaging it away: fixes the user declined, fixes approved but not yet done and any fix that recovered much less than predicted. That last one is evidence about the estimate itself. State two honest limits. Disk size is not session cost: a smaller file that nothing loads saves nothing per session, while a smaller session-start file saves every time, so split the saving that way. And some fixes only show on the next run: list them as "deferred to next run" rather than claiming or discounting them.

Fill in section 2 of the report with the fixes actually applied, and append this reconciliation to section 7, so the next audit's Step 0 reads the outcome and not only the forecast. The report is this skill's own new file, so adding to it in the same run needs no backup.

Summarise what was done in one line per fix. Provide clickable links to the report and any other deliverables. If the audit surfaced a new check that should be added next time, note it in the report under a "Skill improvement suggestions" heading. Do not edit this or any SKILL.md; installed plugin skills are read-only.

### Restore mode (undo)

This mode stands on its own: it skips Steps 0 to 12 and needs no audit first. It runs when the user says "restore [file] from its backup", "move [file] back from the archive", "undo that change" or similar, at any time, not only after an audit.

1. **Find the source.** If there is no backup and no archive-log line for the file (for example after a plain append, which keeps the earlier text in place anyway), say so plainly and offer to show the file so the user can say what to remove; any edit then follows the Step 11 backup rule.
    For a backup: list the `[file].backup-*` copies beside the file, newest first, and ask which one if there is more than one. For an archived file: find its line in `_archive/archive-log.md` to get the original path; if there is no log line, ask the user where it should go.
2. **Show the change.** Say exactly what will happen: which copy comes back, to which path, and what is currently at that path (if anything).
3. **Protect the current version.** If a file already sits at the destination, back it up first under the same backup rule as Step 11 (`[file].backup-[date]`, with the time added if that name exists). Restoring never destroys the version it replaces.
4. **Act only on a yes**, then confirm in one line. For an archived file, append `date | [archived path] | [original path] | restored` to `archive-log.md`. A backup stays where it is. An archived file is moved back to its original path and the log records it. Nothing is deleted.

---

## Outputs

| File | Always produced? | Purpose |
|---|---|---|
| `outputs/system-audits/hygiene-audit-YYYY-MM-DD.md` | Yes | The full report |
| File moves and edits | Only on approved fixes | The actual cleanup |
| Updates to `about-me/custom-workflows.md` | When scheduled-task registry drifted, or skill install drift detected | Registry sync |
| New scheduled tasks | Only when proposed and approved | Recurring enforcement |

---

## What this skill does NOT do

- Does not delete anything the user wrote: only `.DS_Store` files (as one listed item) and empty folders (each on its own yes) can be deleted. Everything else is moved to `_archive/`, each on its own yes, and logged so it can be moved back
- Does not modify installed plugin skill files (they are read-only); it only reads skill source files (Step 6) and never edits any SKILL.md
- Does not run as a scheduled task; this is on-demand
- Does not edit, pause, reschedule or delete a scheduled task or rewrite its instructions; Step 5b only reports what it finds and the user decides

---

## Pairs with

- `session-audit` skill: end-of-session check for uncaptured corrections (lighter, different scope)
- Any recurring clean-up reminder the user has approved as a scheduled task
- `consolidate-memory` skill: the fix when a memory file is over its ceiling; this audit finds the file, that skill compresses it (archives, never deletes)

