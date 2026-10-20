---
name: graduate
description: "The hand-off from EASE Day One to the EASE OS plugin, and the last of the four Day One skills. Use when the user says 'am I ready for EASE OS', 'is my setup ready for EASE OS', 'take me to EASE OS' or has finished setup, logged a first win and passed the health check. It reads the progress file. If the graduation flag is set, it congratulates the user, explains in plain language what EASE OS adds on top of the safe setup they now have, points them to where to get it, walks the same install flow they already know and maps what carries over: their folder, files, connected tools and safety habits all stay, and EASE OS enhances rather than resets. If the flag is not set, it gently explains what is still missing and points to the right next step by its phrase. Read-only and honest: it changes nothing, sets no flag, never overstates EASE OS and recommends rather than requires."
---

# EASE Day One - Graduate

The hand-off, and the last of the four Day One skills. It checks whether the user is ready to move up to the EASE OS plugin, and if they are, it explains what EASE OS adds, points them to where to get it, and walks the same install they already know. It changes nothing.

Boundary: this skill hands over; it does not install EASE OS or run its setup. EASE OS has its own setup skill that takes over once it is installed. Graduate reads the progress file and reports; it never writes to it. The health-check skill sets the graduation flag, not this one.

---

## Step 0 - Operating rules (non-negotiable)

1. **Read-only.** This skill changes nothing: no files, no settings, no flags. It reads the progress file and explains the next move. The graduation flag is set by the health-check skill, never here.
2. **Honest, never oversold.** Describe EASE OS plainly and accurately. It is a quiet background layer that keeps the workspace organised; it does not do the user's work for them. Recommend it; never say it is required.
3. **Earned, not automatic.** Only hand over when the user is genuinely ready: setup complete, a first win logged, and the health check passed. If they are not, say what is left, kindly, and point to the right step.
4. **Guide, do not fake.** Installing EASE OS happens in the user's own Cowork screens. Explain the steps clearly and let the user do them. Never claim to have installed anything.

---

## Step 1 - Read the progress file and check readiness

- **First, make sure the right folder is selected.** A new chat may not have a working folder selected yet. If no folder is selected in this chat, ask the user to select the folder Day One set up (the one holding `ease-day-one-progress.md`, or `cowork-day-one-progress.md` if they started before the kit was renamed) and wait until they have. If a folder is selected but has no progress file, ask whether this is the folder Day One set up before going further: if they set one up elsewhere, ask them to select that one instead. Only if they say they never set one up is setup missing.
- **A progress file from the earlier name.** This kit was called Cowork Day One before it was renamed, and its progress file was `cowork-day-one-progress.md`. If the working folder has that file and no `ease-day-one-progress.md`, it is the progress file: read it in place, never rename it and never look for a second one. Its graduation line may say "Ready for Cowork OS" where a new file says "Ready for EASE OS"; treat the two as the same line.
- Read `ease-day-one-progress.md` in the working folder.
- If there is no progress file, Day One has not been started. Say so in one line and point to **"help me set up Cowork safely"**. There is nothing to graduate from yet.
- Read the "Graduation" section. The user is ready only when the graduation flag reads yes, which the health-check skill sets after a full pass with a first win logged, and no setup row reads "not yet" (a row reset since the flag was set wins over the flag).
- If the flag is not yet set, or any setup row reads "not yet", go to Step 2. Otherwise go to Step 3.

---

## Step 2 - Not ready yet: say what is left, kindly

Read the progress file and name the one thing that is missing, in a plain line or two. Keep the tone encouraging: they are close, this is the last box to tick, not a failure.

- If setup is unfinished (any setup row still "not yet"; "declined" counts as done): point to **"help me set up Cowork safely"** to finish it.
- If setup is done but no first win is logged: point to **"do my first Cowork task"**.
- If setup and a first win are both done but the graduation flag is not set: point to **"check my Cowork setup"** to run the health check, which sets the flag on a full pass.

Do not describe the EASE OS install yet, and do not set the flag. Stop here.

---

## Step 3 - Ready: explain EASE OS and hand over

Congratulate the user in one line: they have a safe setup, one real result and a passing health check. Then cover four things, plainly.

1. **What EASE OS is, honestly.** EASE OS is the plugin Day One leads to. Where Day One got the user safe and working, EASE OS keeps the workspace organised over time: it keeps the folder tidy, remembers the right things in the right place, sets up projects properly, and can pressure-test a decision before they commit to it. It also makes the setup theirs: a short interview ("make it mine") records their goals and how they want Claude to reply and handle decisions, a weekly review checks their effort against those goals, and when they notice themselves repeating a task, it helps them turn it into a skill of their own. Set the same expectation EASE OS sets for itself: it is a quiet background layer, not a set of tools that do the user's work, and nothing flashy happens on day one. The payoff builds the more real work they bring it.

2. **What carries over.** Everything from Day One stays: the working folder, the `rules.md` and `notes.md` files (if they made them), the progress file, the connected tools, and the safety habits (manual approvals, draft-not-send). EASE OS's own setup compares itself against what the user already has, keeps everything that is theirs, and adds only what is missing. It never resets, wipes, or deletes. It adds its own fuller structure (a routing map, an about-you file with their goals, a file for how they want replies and decisions, a writing-style file, a short list of checks before calling something done, a memory file) alongside the Day One files. Its setup reads `rules.md` and `notes.md` and offers to carry their rules and notes into its own files, shown first and only with the user's yes, and it leaves the Day One files in place. Nothing is lost, and the safe base becomes the foundation EASE OS builds on.

3. **Where to get it.** If they added Day One from Claude's plugin directory, look for EASE OS there first. If it is not listed there, or they got Day One as a zip, point the user to the EASE OS download page. The link is on the Day One one-page guide that came with this kit (and on the web page they downloaded Day One from, if they got it from one). If they added Day One from the plugin directory, the homepage link on Day One's directory listing leads to it. Use only these links; do not guess or invent one. If they have none of these, say so plainly and tell them to ask whoever sent them the kit for the link, rather than searching for one themselves.

4. **How to install it, the same flow they already know.** It installs exactly like Day One did. Ask whether they are adding EASE OS from the plugin directory or as a zip, if it is not clear. First, if an earlier version of EASE OS, or Cowork OS (its earlier name), is installed, they remove it (Customize, then Plugins, then Uninstall); their folder and files stay as they are. If they are adding EASE OS from the plugin directory, they add it from there and skip the two zip steps below. If they are installing from the zip, they do both. Either way they finish with the last step:
   - Save the EASE OS zip anywhere on the computer. Keep it as a `.zip`; do not unzip or rename it.
   - In Cowork, open Customize (left menu), then Add plugin, Create plugin and Upload plugin, and choose the zip.
   - Last step, for everyone: open a new chat, select the same working folder Day One uses (so EASE OS finds `rules.md`, `notes.md` and the progress file), then type **"what does the EASE OS plugin do"** for its short tour, then **"optimise my workspace"** to set it up, then **"make it mine"** to fit it to them. EASE OS takes it from there. Its interview drafts a fuller block for Cowork's Global instructions that repeats the two Day One safety lines word for word, so those two lines can be removed when the block is added; everything else in the box stays.

---

## Step 4 - Close on the journey

- Give a one-line summary of where the user now is: Day One is complete, and EASE OS is the next step whenever they want it.
- Close with the journey breadcrumb, in these words: "You have finished Day One: a safe setup, one real task and a clean health check. When you are ready to go further, install EASE OS and type **'what does the EASE OS plugin do'**."
- Then the shared map line: "Day One has four steps: 1 set up safely, 2 your first task, 3 check your setup, 4 move up to EASE OS. You are on step 4."
- Recommend, do not push. If the user is not ready to install EASE OS today, that is fine: Day One keeps working, and they can come back to this any time.

---

## What this skill does not do

- Does not install EASE OS, sign the user in, or set anything up inside it. It explains and hands over; EASE OS's own setup takes over from there.
- Does not write to the progress file or set the graduation flag. The health-check skill sets the flag; this skill only reads it.
- Does not send, post, publish, delete, or schedule anything.
- Does not overstate EASE OS or claim the user must move to it. It recommends, and only once the user is genuinely ready.
- Does not invent a download link. It points the user to the link the kit shipped with.
