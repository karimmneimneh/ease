---
name: output-sharpen
description: "Drives one piece of work to a high, task-specific standard by locking a scoring rubric before drafting, then scoring the draft in a separate critic pass and revising until every criterion clears the bar. Use when the user says 'sharpen this', 'get this to 10/10', 'make this bulletproof', 'grade and revise this', 'build a rubric for [task]' or wants a one-off deliverable (a document, a plan, a message, a prompt) to be as good as it can be. Reads the user's writing rules so they never restate them. Not for pressure-testing a decision (sparring-partner). Never sends, posts or publishes the result; it hands it back."
---

# Output sharpen

Turns a rough task into a finished piece of work that clears a high bar set for that task, without grading its own homework. The bar is set before the answer exists and is scored by a separate critic pass. This fixes the usual failure of asking a model to rate its own work: it sets an easy bar, then declares victory.

---

## Read first

1. `about-me/writing-rules.md`, if it exists: tone, spelling, words to avoid, formatting. Every rubric inherits these so the user never restates them.
2. `about-me/about-me.md`, if it exists, when the output is something other people will read: who the user is and who their work is for. Skip it for internal or non-prose work.
3. Any source the task names: a brief, a reference document or an earlier draft to improve.

If a file is missing, carry on without it and do not invent its contents.

---

## Decide

**Hand off first.** If another installed skill already owns this kind of output with its own quality check, say so in one line and use that skill instead. This skill is for work with no dedicated home.

**Set the stakes.** Anything the user will send, publish or act on is high stakes and gets the critic pass as a separate helper (a sub-agent) where one is available. Anything else runs the critic pass in the same chat, with an explicit switch of role. When unsure, treat it as high stakes.

---

## Produce

Run the loop in this order. Do not reorder steps 1 to 3: locking the bar before the draft exists is what stops the rubric being gamed.

**1. Lock the rubric (before any drafting).**
- Four to six criteria specific to THIS task, never generic boilerplate. Weight them when some matter more than others.
- Each criterion carries a failure mode written as "fails if...". Failure modes separate good from bad better than positive descriptions.
- Anchor the scale: one line on what a 9 to 10 looks like and one on what a 6 looks like, so a score means something.
- Fold the user's writing rules in as pass or fail gates, not as scored criteria. They are not trade-offs.
- When the output is a message or document for a specific person, add four pass or fail gates: it is written for that reader, the answer or ask comes first, nothing the reader needs is missing and the reader knows exactly what to do next.
- For any written output, add one more pass or fail gate, always: every sentence is understood on the first read with no extra effort. The main point comes first, no sentence reads two ways, no word sends the reader back up the page and nothing is left for the reader to work out. The critic pass checks it sentence by sentence.

Show the rubric to the user in a few lines before drafting, so they can change the bar if it is wrong.

**2. Draft** the output against the brief and the rubric.

**3. Score in a separate critic pass.** Switch into an evaluator whose job is to find the weakest criterion, not to confirm the draft. Score each criterion from 1 to 10 with one line of evidence quoted from the draft. No score without a specific reason. For a message to a specific person, also read it cold as that person and list every point of confusion, open question or step they could not take.

**4. Revise** to close the lowest-scoring gaps first. Re-score only what changed.

**5. Stop** when every criterion scores 8 or more and every gate passes. If two passes in a row bring no real improvement before that, stop and follow the "Scores stall below the bar" edge case below; never call the work finished below the bar.

---

## Deliver

- The finished output, ready to use. Anything the user will copy goes in a code block.
- A short scorecard underneath: each criterion, its final score and the one change that moved it most.
- The locked rubric, offered as a block the user can keep for similar tasks.
- The result stays in chat by default. When the output is a long document or something the user will come back to, offer to save it, name the path in `outputs/` and write only on a yes; an existing file is backed up first (`[file].backup-[today's date]`, adding the time if that name is taken).

---

## Edge cases

- **The task is a prompt the user will run somewhere else.** The deliverable is the sharpened prompt: build the rubric, write the prompt, score it in a separate pass, revise. Same loop.
- **The task is too vague for clear criteria.** Ask one scoping question before building the rubric, not five.
- **Scores stall below the bar.** Name the criterion that is stuck and what is missing (usually a fact, a source or a decision only the user has) rather than raising the score.
- **The first score looks suspiciously high.** Treat it as a sign of a rubric that is too easy. Raise the bar or add a failure mode and score again.

---

## What this skill does not do

- Does not replace a skill that already owns a quality check for that kind of work; it hands off to it.
- Does not pressure-test a decision or plan; that is sparring-partner ("spar with me").
- Does not invent facts to clear a criterion. A missing input is named, never made up.
- Does not send, post or publish anything. It hands the finished work back to the user.
