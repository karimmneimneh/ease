# EASE OS

Eleven skills that turn a blank Claude Cowork folder into a self-organising, self-improving system, including a setup skill that wires it up safely (one approved step at a time) and a sparring partner that pressure-tests your decisions. None of them are tied to a person or an industry. They manage the workspace itself, so they work the same whether you work in marketing, law or research or run a bakery.

The name carries the four habits the kit keeps: **E**verything has a home, **A**sk before acting, **S**ave before you replace, **E**nd each session with a debrief ("audit this session").

One expectation to set before anything else: EASE OS is a quiet background layer, not a set of tools that do your work. It will not write your emails or build your slides, and nothing flashy happens on day one. The payoff builds as you use Cowork for real work.

## What is inside

| Skill | What it does | Say this to trigger it |
|---|---|---|
| **ease-setup** | Sets up the files and folders the other skills need, and never replaces anything you already have without showing you first and keeping a dated backup. Run it on day one; safe to run again after big changes. | "optimise my workspace" |
| **workspace-hygiene** | Full structural audit: files, memory, routing, scheduled tasks and drift. A severity-ranked report first, then fixes only where you say yes. | "audit my workspace" |
| **memory-router** | Routes any "remember this" to the correct file, so memory never duplicates or lands in the wrong place. | "remember this" |
| **new-project-setup** | Scaffolds a project: folders, a blank instruction file, a memory file and a row in your routing map. | "start a new project for the house move" |
| **inbox-triage** | Walks a capture inbox item by item (or a group at a time, if you ask) and files each where it belongs, on your sign-off. | "triage my inbox" |
| **correspondence-log** | Files an email, Slack or message thread or your notes from a call onto the right project's record as one tight, decision-relevant entry, on your sign-off. | "log this to the house move" |
| **sparring-partner** | A critical-thinking partner that watches for blind spots as you work (setup adds the line that keeps it on) and pressure-tests any decision, plan or draft for blind spots, backfire and stronger moves. | "spar with me" |
| **session-audit** | At session end, lists anything left unfinished, then catches your corrections, preferences and decisions and proposes saving each. | "audit this session" |
| **consolidate-memory** | Compresses an over-long memory file without losing anything (archives, never deletes). | "tidy my memory" |
| **orientation** | A read-only tour of what the kit can do and how to start. | "what does the EASE OS plugin do" |

Where the examples say the house move, use your own project's name. One more skill, **disabled-skill-check**, runs on its own: once you have told the kit about a skill you switched off ("remember I switched off [skill]"), anything you ask that needs it gets flagged instead of improvised around. To park a thought for later, say "add this to my inbox: [thought]"; "triage my inbox" files it.

## Install and set up

1. Add the plugin. If you found EASE OS in Claude's plugin directory, add it from there. If someone sent you the zip file, in Cowork click Customize (left menu) > Add plugin > Create plugin > Upload plugin and choose the zip; keep it as a .zip, do not unzip or rename it (if the labels look slightly different, look for the upload option on the Plugins page).

   In Claude Code, install it from GitHub with:

   ```
   /plugin install ease-os --marketplace karimmneimneh/ease
   ```
2. Pick one folder on your computer for your work (if you used EASE Day One, earlier called Cowork Day One, keep using that folder; otherwise a new, dedicated one is best) and select it as the working folder whenever you start a chat. EASE OS keeps everything in that folder, so the same folder each time is what lets it remember.
3. Type **"what does the EASE OS plugin do"**. A short read-only tour of what you now have. It changes nothing.
4. Type **"optimise my workspace"**. The setup skill compares the kit against what you already have, keeps everything that is yours and proposes only missing pieces or small improvements for your approval. It never resets your workspace and never starts you over, and it never overwrites or deletes anything without showing you first and getting your yes, so a stray click can never wipe your system.
5. Fill in `about-me/about-me.md` and `about-me/writing-rules.md`. Setup offers a short interview that fills both in a few minutes, and five minutes there changes every output.

The setup skill adds only what is missing, safely, and only with your approval. You never copy anything in by hand.

Then bring it one real piece of work (a plan, a draft, some research) and say **"remember this"** when something matters. In your next chat, it is already known.

## What it never touches

Every file it writes stays inside your Cowork folder; the only other thing it can create is a reminder task you have seen and approved, and that task can only remind you. The kit never sends an email, never posts anything, never changes a scheduled task and never touches files outside your workspace, including Cowork's own built-in memory. Nothing you already have changes without showing you first and getting your yes.

- **Replacing.** Anything the kit ever replaces is copied to a dated backup first (`[file].backup-[date]`, with the time added if a backup from the same day already exists), so you can always restore the original. Backup files are safe to keep; delete them yourself only when you are sure.
- **Removing.** Nothing you wrote is ever deleted. The audit only ever deletes system clutter (`.DS_Store` files and empty folders), and even then only on your yes; anything else it clears is moved to `_archive/`. inbox-triage copies your item into its archive word for word before taking it out of your inbox.
- **Undo.** Say "restore [file] from its backup" or "move [file] back from the archive". The audit skill finds the right copy, shows you what will change, backs up the current version first and acts only on your yes. Every move to `_archive/` is logged in `_archive/archive-log.md`, so it always knows where a file came from.

## Upgrading or removing

If you installed from a zip, upgrade by removing the old version first (Customize > Plugins > Uninstall), then uploading the new zip. To remove EASE OS for good, uninstall it the same way. Either way your folder and your files stay exactly as they are.

**Coming from Cowork OS?** EASE OS is the same kit under a new name. Uninstall Cowork OS first, because the two answer to the same phrases, then add EASE OS. Your folder and files stay exactly as they are, and "optimise my workspace" picks up everything Cowork OS set up.

## The one idea that makes it work

Skills are logic. Your personal content is data. A skill describes a process; the files it reads hold the specifics. Keep them separate and your system stays clean, easy to change and shareable. Build your own skills the same way: the logic goes in the skill, your specifics in the files it reads.

## Licence

MIT. See `LICENSE` in this folder.
