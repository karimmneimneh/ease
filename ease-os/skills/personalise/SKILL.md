---
name: personalise
description: "Makes the EASE OS workspace the user's own through a short interview, one question at a time and every question skippable, then saves their answers into the files Claude reads at the start of every session: who they are, their goals and areas of work, how Claude should reply and handle decisions, how they like things written and which checks to keep. Proposes skill ideas from the tasks they repeat and ends with a ready-to-paste block for Cowork's Global instructions. Use when the user says 'make it mine', 'personalise my workspace', 'personalize my workspace', 'set up how you work with me', 'update my profile', 'change how you reply to me' or when ease-setup offers it. Re-runnable: a later run changes only what the user picks. Writes only the user's own words, shows every change before saving, backs up anything it replaces and never sends, posts or deletes anything."
---

# Personalise ("make it mine")

The kit ships a structure. This skill fills it with the user, so every later session knows who they are, what they are working towards and how they want Claude to work with them. It writes only what the user says or chooses, and nothing is saved until they have seen it.

---

## Step 0 - Operating rules (non-negotiable)

1. **Their words, never invented ones.** Write only what the user tells you or picks. Never add a fact, a goal, a preference or a style they did not give. If an answer is vague, ask one short follow-up or leave the line as it is.
2. **One question at a time.** Use AskUserQuestion where it is available: two to four likely answers and the user can always type their own. Without it, ask in plain text and wait. Never stack several questions in one message.
3. **Skipping is always fine.** Every question can be skipped, and the user can stop after any round. A skipped answer leaves the line as it is.
4. **Merge, never rebuild.** Start from each file's current text. Change only the lines this run's answers fill: a placeholder line (one containing `_(`) or a section the user chose to change. Keep every other heading and line exactly as it is, including anything the user wrote that the template does not have.
5. **Show, then save.** Before writing, show what changes, then the full file, and ask "Save this?". Write only on a yes. If the user edits the words, show the new version and ask again.
6. **Back up before replacing.** Filling a line replaces text, so first copy the file to `[file].backup-[today's date]` (if a backup of that name already exists, add the time, `[file].backup-[date]-[HHMM]`; never overwrite an existing backup) and never remove the backup. On a first run the file being replaced is the blank template written minutes earlier, so say in one line that the backup is a copy of the blank template and can be ignored.
7. **Nothing outside the folder.** This skill writes only inside the working folder. Cowork's Global instructions and Project instructions are screens only the user can edit: it drafts the text and never claims it is done.

---

## Step 0.5 - Working folder and setup check

EASE OS lives in the user's working folder. If no folder is selected in this chat, ask the user to select the folder they set EASE OS up in and wait.

Then check that setup has run in this version:

- **The files.** The root `CLAUDE.md`, `about-me/about-me.md`, `about-me/how-claude-works-with-me.md`, `about-me/writing-rules.md` and `about-me/verification-rules.md` exist. If any is missing (a new folder, or a workspace set up with an earlier version of the kit), say in one line that setup needs to add it first and offer "optimise my workspace". Stop here until it has run, or run only the rounds whose files exist if the user prefers; this skill fills the files that setup creates and never creates them itself.
- **The session-start lines.** The root `CLAUDE.md` session-start section names `about-me/how-claude-works-with-me.md` and `about-me/verification-rules.md`. If it does not (for example the user declined those lines during setup), say in one line that answers saved to those two files will not be read at the start of each session until the lines are added, offer "optimise my workspace" to add them, and carry on if the user wants to.

---

## Step 1 - See what is already there (read only)

Read `about-me/about-me.md`, `about-me/how-claude-works-with-me.md`, `about-me/writing-rules.md`, `about-me/verification-rules.md`, `about-me/custom-workflows.md` and `about-me/global-instructions-copy.md` where each exists. For each file, note which sections hold real content and which still hold placeholder lines (lines containing `_(`).

- **First run** (the Background, Goals and Your choices sections still hold placeholders): say in one line that this takes about ten minutes, that every question can be skipped and that they can stop after any round and say "make it mine" later to pick up where they left off. Then start Step 2.
- **A later run:** show a short summary of what is on file, section by section, listing any section that is missing or still a placeholder as "not set yet", and ask which parts the user wants to change (multiple choice, more than one allowed, with the "not set yet" parts pre-selected). Run only the rounds and questions for those parts. Never re-ask a question whose answer is on file unless the user picks that part.

---

## Step 2 - The interview, saved round by round

Five short rounds. Tell the user which round they are on ("Round 2 of 5: your goals"). **At the end of each round, show that round's changes and save them on a yes (Step 3)** before starting the next, so nothing is lost if the chat ends.

**Round 1 - You** (goes to `about-me/about-me.md`)
1. What do you do, and what is your background, in a line or two?
2. What are you focused on right now?
3. Who is your work for (clients, a team, readers, yourself)?
4. Which tools do you work in every day (for example email, calendar, documents, spreadsheets)?
5. What are the main areas of your work or life you want Claude to help with (two to five, for example "clients", "job search", "the house move")? These go under Areas of work. For each area that has no project yet, mention that "start a new project for [name]" (the new-project-setup skill) sets one up; this skill never creates folders itself.

**Round 2 - Your goals** (goes to the Goals section of `about-me/about-me.md`)
6. What are the one to three things you most want to move forward in the next three months?
7. For each one: how will you know it worked? Offer to help phrase it as something they could check ("three new clients", "the report shipped"), but keep their wording.

The weekly review (the goals-review skill) measures against these, so they are worth a minute.

**Round 3 - How Claude works with you** (goes to `about-me/how-claude-works-with-me.md`)
8. How long should replies be? (Short: the answer and the next step / Medium: the answer with a little context / Detailed: full reasoning.)
9. When you need to make a decision, how should it reach you? (A multiple-choice question with Claude's recommendation first / One clear recommendation to accept or reject / The options side by side.)
10. For small choices inside a task (wording, layout, which order to do things in), should Claude make the call and tell you, or ask you? (Ask me / Make the call and tell me.) Either way, anything that changes a file, sends, publishes or deletes still needs your yes.
11. Then show the Kit defaults list in that file, one line each, and ask which to keep, change or delete. Lines they keep stay, with any wording they change. Mark any line that is not from the kit template as theirs. A line is removed only if they pick delete, and the file is backed up first.

**Round 4 - How you like things written** (goes to `about-me/writing-rules.md`)
12. Three or four words for the tone you want (for example plain, warm, direct).
13. British or American spelling, and how should dates read (for example 13 October or October 13)? These go under Spelling and formatting.
14. Any words, phrases or styles you never want to see? When they describe a style ("nothing that sounds like a sales pitch"), ask for two example sentences that would break it and save them as examples of the style, in their words, not as exact phrases to ban. If they cannot think of any, save the description alone.
15. Offer, as an option they can decline, the starter list of common AI writing habits (Templates below). Show the list and let them keep all of it, some of it or none. Save only what they keep.
16. Any labels you never want applied to yourself (for example "expert" or "guru")?
17. Optional: a short sample of something they wrote that sounds like them.

**Round 5 - Checks and repeat tasks**
18. Show the Kit defaults in `about-me/verification-rules.md`, one line each, and ask which to keep, change or delete, and whether there is a check of their own to add ("always check prices against the supplier's site"). Same rule as question 11.
19. What do you do every week that follows the same steps each time? For each one they name, propose one row for the Skill ideas table in `about-me/custom-workflows.md`: the idea, how often it happens, the steps in their words, today's date as Date logged and `idea` as Status. These are the tasks most worth turning into skills; "turn this into a skill" (the skill-builder skill) builds one when they are ready.

---

## Step 3 - Show and save (at the end of every round)

1. Show which lines change, then the full file as it will be saved (for the Skill ideas, show only the rows being added).
2. Ask "Save this?" and wait.
3. On a yes, back up the existing file (Step 0, rule 6), then write it, updating its `_Last updated:` line to today's date in the same write. Skill ideas rows are added under the table's header row, and the date line is the only other line that changes in that file; because the date line is replaced, back the file up first like any other.

If `about-me.md` has no Goals or Areas of work section (older setups), show the heading with the new lines and add both on the same yes. If `custom-workflows.md` has no Skill ideas table, show the heading and header row from the ease-setup template (`| Idea | How often | Steps it follows | Date logged | Status |`) and add them with the rows on the same yes.

---

## Step 4 - Make it apply in every chat

Files in the folder are read when that folder is selected. To make the essentials apply in every Cowork chat, they need to sit in Cowork's Global instructions, a screen only the user can edit.

1. Draft the block from their answers in this shape. The first two lines are the kit's fixed safety lines, the same words EASE Day One uses; the rest come only from their answers, and a line whose answer they skipped is left out:

```
Never send, post, publish or delete anything without my explicit OK.
Draft, do not send. Show me first; I review and send it myself.
When my "[folder name]" folder is the working folder, read CLAUDE.md at its root first and follow its session-start list before any work.
Replies: [their length choice]. Decisions: [their decision choice].
[Their spelling choice] spelling. Dates written like [their date example].
```

2. Show the block in a code block so it can be copied, then give the steps:
   1. Open the Claude desktop app.
   2. Go to Settings, then Cowork, then Global instructions.
   3. Add the block. Keep everything else already in the box; if it holds the two safety lines from EASE Day One, remove only those two, because the block repeats them word for word.
   4. Save. You will know it worked when a new chat follows your reply preferences without being told.

3. If the user uses Cowork Projects for parts of their work, offer the third line of the block for each Project's own instructions too (in the Project's panel), so a Project chat also reads the workspace.

4. Offer to keep a copy in `about-me/global-instructions-copy.md` (a file sessions do not read at start, so the text does not load twice), with a heading, the date and the block. Then when a reply or spelling choice changes later, the change can be shown against what was pasted. Show the file and save it on a yes, with the backup rule if it exists.

Ask the user to say when they have pasted it. Never say it is done until they confirm.

---

## Step 5 - Close

Give a short report: which files were saved, which were left as they were and whether the Global instructions block was pasted. Then one next step: bring a real piece of work, and say "remember this" when something matters. Mention two phrases for later: "make it mine" to change any of this, and "weekly review" to check progress against the goals they just set.

---

## What this skill does not do

- Does not invent facts, goals, preferences or a style. Every saved line is something the user said, chose or kept.
- Does not rebuild a file: it changes only the lines this run fills and keeps everything else exactly as it is.
- Does not save anything without showing the change first, and backs up any file before replacing its text.
- Does not create the workspace files; ease-setup does ("optimise my workspace").
- Does not edit Cowork's settings, Global instructions or Project instructions. It drafts the text and gives the steps.
- Does not create projects, skills or scheduled tasks. It proposes them and names the skill that does each.
- Does not send, post, publish or delete anything.

---

## Templates

### Starter list of common AI writing habits (offered in Round 4, never imposed)

Show this list and let the user keep any part of it. Save the kept lines under `## Words and phrases to avoid` in `about-me/writing-rules.md`, labelled "Common AI habits I want avoided".

```
- Filler openers and closers: "Great question", "Certainly", "I hope this helps", "In conclusion".
- Inflated words: delve, robust, seamless, leverage (as a verb), unlock, game-changer, cutting-edge, pivotal, crucial.
- Signposting phrases: "It's worth noting", "It's important to note", "Furthermore", "Moreover".
- "Not just X, it's Y" and other set-ups that manufacture drama.
- A closing line that repeats what the paragraph already said.
- Lists of exactly three, again and again.
- Long dashes used in place of commas or full stops.
- "Experts say" or "research shows" with no named source.
```
