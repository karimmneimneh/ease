# EASE Day One

EASE Day One takes a complete beginner on Claude Cowork from just installed to a safe, working setup and one real result. It relies on Claude's own onboarding for getting started in Cowork and connecting your tools, and adds the safety habits that onboarding does not cover: manual approvals, draft-not-send and one tightly scoped working folder. It checks what you already have and never resets anything.

One expectation to set first: Day One is a guided on-ramp, not a set of tools that do your work. It gets you safe and confident, walks one real task with you, then points you to EASE OS, the plugin where the longer-term workspace system lives. Nothing flashy happens on day one; the point is a safe start you can build on.

## The four steps, in order

Everything below goes into a normal Cowork chat, the same box you always type in. Do them in order the first time. The check in step 3 is worth re-running any time.

| Step | Skill | What it does | Say this to run it |
|---|---|---|---|
| 1 | **day-one-setup** | The spine. Checks what you have, then walks you through connecting your tools, one scoped folder, a minimal workspace and safe defaults. Resumable: it picks up where you left off. | "help me set up Cowork safely" |
| 2 | **first-win** | Runs your single most-repeated real task end to end, draft only, nothing sent. You get a genuine result and learn the approval habit. | "do my first Cowork task" |
| 3 | **health-check** | A re-runnable safety check of your setup, and the gate to graduating. Run it any time. | "check my Cowork setup" |
| 4 | **graduate** | Explains EASE OS and hands you over, once your setup passes and a first win is logged. | "am I ready for EASE OS" |

## Install and start

1. Add the plugin. If you found EASE Day One in Claude's plugin directory, add it from there. If someone sent you the zip file, in Cowork click Customize (left menu), then Add plugin, Create plugin and Upload plugin, and choose the zip. Keep it as a .zip; do not unzip or rename it. (If the labels look slightly different, look for the upload option on the Plugins page.)

   In Claude Code, install it from GitHub with:

   ```
   /plugin install ease-day-one --marketplace karimmneimneh/ease
   ```
2. Open a new chat and type **"help me set up Cowork safely"**. Day One checks what you already have and walks the rest, one approved step at a time.
3. You can carry on in the same chat. If you start a new one for a later step, select the same working folder Day One set up before you type the phrase, so it can pick up where you left off.

If a different setup guide starts instead of Day One, type: use the EASE Day One skill to help me set up Cowork safely.

**Coming from Cowork Day One?** EASE Day One is the same kit under a new name. Uninstall Cowork Day One first (Customize > Plugins > Uninstall), because the two answer to the same phrases, then add EASE Day One. Your folder and your progress stay exactly as they are, and it picks up where you left off.

## What it never does

It never sends an email, never posts anything, schedules nothing, never moves or deletes your files and never resets your setup or starts you over. Files it writes stay inside the folder you choose; an email finishes as a draft in your own Gmail that only you can send. Every change is shown to you first and only happens when you say yes, and on the rare occasion something you already have would be replaced, it keeps a dated copy of the original first.

To remove Day One, go to Customize > Plugins and uninstall it. Your folder and your files stay exactly as they are.

## Where it leads

Day One ends at a clean hand-off. Once your setup passes the health check and you have logged a first win, say **"am I ready for EASE OS"**, and the graduate skill explains EASE OS, tells you where to get it and walks the same install you already know. Everything you set up here carries over; EASE OS builds on it and never resets it.

## Licence

MIT. See `LICENSE` in this folder.
