---
name: goals-review
description: "A weekly check of the user's effort against their own goals: what moved, what stalled and where the effort went, ending in at most three decisions asked as multiple-choice questions. Use when the user says 'weekly review', 'review my week', 'how am I doing on my goals', 'what moved this week', 'goals review' or 'am I working on the right things'. Reads the goals in about-me/about-me.md and the workspace's own records, never impressions. Read-only until the user decides; approved decisions are saved as dated lines and each run is logged in outputs/reviews/. Never sends, posts or deletes anything, and never creates a scheduled task except a weekly reminder the user has seen and approved."
---

# Goals review

Most systems record activity. This skill asks whether the activity is moving the things the user said matter. It reads the goals they set, looks at what the workspace shows for the past week and turns what it finds into a few clear decisions. It is how the system keeps reviewing itself while the user stays in charge.

---

## Step 0 - Operating rules

1. **Evidence, not impressions.** Every claim points to something in the folder: a dated memory line, a file made or changed this week, a logged message. If nothing is recorded, say "no record this week", never "nothing happened": the user may have done the work and not written it down.
2. **Decisions, not advice.** The review ends in decisions the user answers, never a list of loose suggestions.
3. **Read-only until a yes.** Nothing is written until the user has seen the exact text and said yes.

---

## Step 0.5 - Working folder and goals check

If no folder is selected in this chat, ask the user to select the folder they set EASE OS up in and wait.

Read the Goals section of `about-me/about-me.md`. If it is missing or still holds only its placeholder line, say in one line that the review needs goals to measure against and offer "make it mine" (the personalise skill), which asks for them. Stop there.

---

## Step 1 - Gather the week (read only)

Take today's date from the clock, not from the conversation. Read `outputs/reviews/goals-review-log.md` if it exists: the date of the last review and the status it gave each goal. Look back to that date, or seven days if there is no log.

- **Goals:** each goal and how the user said they would know it worked.
- **Projects:** the Active projects table in `about-me/memory.md`; for each active project, the dated entries in its `memory.md` (and `contact-log.md`, if it has one) since the last review. Read only the recent entries, not whole files.
- **Work produced:** files in `outputs/` and `projects/` created or changed since the last review (names and dates only).
- **Captured thoughts:** how many lines are waiting in `inbox/inbox.md`.

Say in one line what was read, so a quiet week is never mistaken for an unread one.

---

## Step 2 - Match effort to goals

For each goal, sort what the records show:

| Goal | Evidence since the last review | Status |
|---|---|---|

Status is one of: **Moving** (a record shows it advanced), **Stalled** (no movement this time, and the last review's log also showed none), **No record** (nothing written down this time; on a first review, anything without movement is No record, never Stalled).

Then add two lines under the table:
- **Where the effort went:** the projects with the most activity, and which goal each serves. Name any busy project that serves no goal.
- **The one thing to watch:** the single biggest gap between effort and goals, in one sentence.

---

## Step 3 - Ask the decisions

Pick at most three, the ones with the biggest effect: a stalled goal, a busy project that serves no goal, an inbox that keeps growing. Ask each as its own multiple-choice question (AskUserQuestion where available), one at a time, with Claude's recommended option first and a one-line reason:

- Keep going as planned
- Change the approach (say how, in one line)
- Pause it until a date
- Drop it

If nothing needs a decision, say so in one line. That is a good week, not an empty review.

---

## Step 4 - Save what was decided

Propose, all shown together before anything is written:

1. **One dated line per decision**, in the file it belongs to: a decision about a goal under Decisions and context in `about-me/memory.md`; a decision about one project in that project's `memory.md`. Before proposing a line for a `memory.md`, apply the kit's one test for "full": if the file's line count plus the lines waiting for it would reach or pass its ceiling (the `## Memory ceilings` section of root `CLAUDE.md`, or 50, 150 and 200 lines by default), do not propose writing there; keep the line in a copy block and suggest "tidy my memory for [file]" first.
2. **One entry in `outputs/reviews/goals-review-log.md`** recording the run: the date, each goal with its status and the decisions made. Create the file with a heading on the same yes if it does not exist. The run log lives here, not in a memory file, so weekly reviews never fill the files read at session start.

Save on a yes, appending new dated lines and never changing an existing one. If a decision changes a goal itself (dropping it, or a new one), offer "make it mine" to update the Goals section rather than editing it here.

---

## Step 5 - Close, and offer a weekly reminder once

Close in two lines: the week in one sentence, and the one next step.

Offer a weekly reminder only if all of these hold: a scheduled-tasks tool is available; the task list (where it can be read) and `about-me/custom-workflows.md` show no weekly review reminder; and the review log records no earlier "reminder created" or "reminder declined". Show:

- the task's name ("Weekly review reminder");
- its schedule (ask which day and time suits them);
- its full instructions, which are only: `Tell the user: time for your weekly review, say "weekly review". Do nothing else: read no files and ask no questions.`;
- if `about-me/custom-workflows.md` exists, the row it would add to its `Scheduled tasks` table. If that file does not exist, say the task will not be recorded in a registry;
- the line it adds to `outputs/reviews/goals-review-log.md`: "reminder created [date]".

Create the task (and the row) only on a yes, and on the same yes add "reminder created [date]" to the review log. If the user says no, ask separately whether to note "reminder declined" in the review log so it is not offered again, and write that line only on that yes. If no scheduled-tasks tool is available, suggest a calendar reminder instead.

---

## What this skill does not do

- Does not judge from memory or impressions; every status points to a record or says "no record".
- Does not give a list of suggestions; it asks at most three decisions.
- Does not change goals, projects or existing lines; it proposes new dated lines and saves them on a yes.
- Does not create any scheduled task except one weekly reminder the user has seen in full and approved, and never changes or deletes an existing task.
- Does not send, post, publish or delete anything.
