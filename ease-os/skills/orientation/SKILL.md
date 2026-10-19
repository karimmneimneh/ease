---
name: orientation
description: "A friendly, read-only tour of the EASE OS kit: what it can do for the user's workspace and how to start. Use when the user says 'what does the EASE OS plugin do', 'what does EASE OS do', 'what does this plugin do', 'what can this kit do', 'what can you do with my workspace', 'what workspace skills do I have', 'how do I use this kit', 'help me get started', 'getting started' or 'orientation'. Lists the kit's skills with their trigger phrases, gives a short quick start and states the safety promise. Read-only: it changes nothing, creates nothing and deletes nothing."
---

# Orientation

A short, friendly welcome to the EASE OS kit. It tells the user what the kit does, lists each skill with the phrase that triggers it, gives a short quick start and states the safety promise. It changes nothing.

Keep this specific to the workspace and the kit, so it does not fire on unrelated "what can you do" questions about general topics.

---

## Step 1: Explain what the kit is for

Open with the version in one line, "You are on EASE OS 13.0.0", so any help request starts from the right version. Then one or two plain lines. The kit runs the workspace itself: it keeps things tidy, manages memory across sessions, sets up projects, sorts captured thoughts, fits itself to the user's goals and preferences and keeps learning them over time. It is the operating layer for a Cowork workspace, not a set of one-off content tools. The name carries the four habits it keeps: Everything has a home, Ask before acting, Save before you replace, End each session with a debrief ("audit this session").

Then set expectations in one more line: it does not do the user's work for them (it will not write their emails or build their slides; "sharpen this" only raises the bar on a piece of work they ask it to), and nothing flashy happens on day one; it runs quietly in the background and pays off more the more real work they do in Cowork.

---

## Step 2: List the kit's skills and how to trigger each

Present this as a simple table. Each row: the skill, what it does in one line and the phrase that triggers it.

| Skill | What it does | Say |
|---|---|---|
| `ease-setup` | Sets up or upgrades the workspace by comparing the kit against what the user already has, preserving everything good | "optimise my workspace" |
| `workspace-hygiene` | A propose-only structural audit of files, memory, routing and tasks, proposing tidy-ups for approval; also restores a file from its backup or the archive | "audit my workspace", "restore [file] from its backup" |
| `memory-router` | Files a "remember this" moment to the correct memory layer | "remember this" |
| `new-project-setup` | Scaffolds a new project's folders, blank files and routing | "start a new project for the house move" |
| `inbox-triage` | Captures a thought for later, then goes through the inbox and routes each item to its home, one at a time or grouped, with sign-off either way | "add this to my inbox: [thought]", "triage my inbox" |
| `correspondence-log` | Files an email, message thread or call notes onto the right project's record as one tight entry, with sign-off | "log this to the house move" |
| `sparring-partner` | Pressure-tests any decision, plan or draft for blind spots, backfire and stronger moves | "spar with me" |
| `session-audit` | At session end, lists anything left unfinished, then catches corrections and decisions from the conversation and proposes saving them | "audit this session" |
| `session-checkpoint` | Saves an unfinished session as one small checkpoint file and a resume prompt, so a fresh chat carries on where this one stopped | "pause this", "resume [thread]" |
| `consolidate-memory` | Compresses a memory file that is over its ceiling, archiving the originals so nothing is lost | "tidy my memory" |
| `personalise` | Makes the workspace yours: a short interview that records who you are, your goals, how you want replies and decisions and how you like things written, then drafts a block for Cowork's Global instructions | "make it mine" |
| `output-sharpen` | Sets a scoring bar for one piece of work before drafting, then scores and revises it until it clears the bar | "sharpen this" |
| `skill-builder` | Turns something you keep doing the same way into a skill of your own, ready to upload | "turn this into a skill" |
| `goals-review` | A weekly check of your effort against your goals, ending in a few decisions | "weekly review" |
| `orientation` | This tour | "what does the EASE OS plugin do" |

Under the table, add two short lines: (1) where the examples say the house move, use your own project's name; (2) one more skill, `disabled-skill-check`, runs on its own with nothing to type: if something the user asks for matches a skill they have switched off and told the kit about ("remember I switched off [skill]"), the kit says so instead of improvising around it.

If any of these skills is not present in the available list for this user, simply leave its row out rather than claiming it exists. List only what is actually installed.

---

## Step 3: Give a short quick start

Lay out the fastest path to a working setup (one line first, then three steps):

0. **Use one folder.** EASE OS lives in the folder the user works in, so they should select that same folder as the working folder whenever they start a chat; otherwise what it set up and remembered is not there.
1. **Run "optimise my workspace".** This compares the kit against whatever the user already has, preserves everything that is theirs and proposes only what is missing, for their approval.
2. **Make it yours: say "make it mine".** A short interview, every question skippable, records who the user is, their goals, how they want replies and decisions and how they like things written, then drafts a block for Cowork's Global instructions so the essentials apply in every chat. Setup offers it at the end of its first run.
3. **Do one real piece of work.** A plan, a draft, some research: whatever the user actually needs, saying "remember this" when something matters. The payoff builds from here.

Mention three habits for later: "weekly review" once a week to check the work against their goals, "turn this into a skill" when they notice themselves doing the same thing again and "audit my workspace" as the full health check worth running now and then (it reports first and changes nothing without the user's yes on each fix).

---

## Step 4: State the safety promise

Close with the promise in one or two lines: nothing is deleted, sent or published without the user's explicit OK. Skills that write to the workspace propose first; they never overwrite or delete anything without showing the user and getting a yes, they back up anything they replace and the only things ever deleted are system clutter (`.DS_Store` files and empty folders); anything else is archived, never deleted (the audit uses `_archive/`, and each other skill names its own archive). When in doubt, the kit shows the user and asks.

---

## Step 5: Offer the next step

Do not end the tour on a full stop. Offer one concrete next move and let the user pick or decline: if setup has not run yet, offer to run "optimise my workspace" now; if setup has run and the Background or Goals section of `about-me/about-me.md`, or Your choices in `about-me/how-claude-works-with-me.md`, still holds its placeholder line, offer to run "make it mine"; otherwise suggest bringing one real piece of work and saying "remember this" when something matters. Offer only. Nothing runs unless the user picks it, and any next step runs as its own skill.

---

## What this skill does not do

- Does not create, edit, move or delete anything. It is read-only.
- Does not kick off any action on its own. It offers the next step and hands over only when the user picks it.
- Does not list skills that are not actually installed in this workspace; it describes only what is present.
- Does not fire on general "what can you do" questions unrelated to the workspace or the kit.
