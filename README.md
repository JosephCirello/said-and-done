# Said and Done

**Your AI says "done." This checks it.**

A Jira Cloud add-on, built on Atlassian Forge, that reads what a comment says was done on a ticket, checks each thing against that ticket's own change history, and puts the answer beside it.

Built for [Codegeist 2026](https://codegeist2026.devpost.com/). Watch it work: **https://youtu.be/mMdJuBfxcQ8** (1:46)

---

## The problem

When a kid says "I cleaned my room," a parent opens the door and looks.

Atlassian put agents inside Jira, where, in its own words, "An agent shows up as an assignee." An agent finishes a job and leaves a note: *"Done. I marked it urgent and gave it to a manager."* Nobody opens the door, because the note says done.

A job marked done is a job nobody goes back to.

A public library's wheelchair ramp is broken. The library's AI assistant takes the job and writes: *"Done. I marked this urgent and gave it to Tomasz."* It did neither. The job sits in the ordinary pile with nobody's name on it, and nine days later the librarian hears about it from a visitor who could not get in.

Priority and assignee are both in the ticket's own record. Jira already keeps who changed which field, from what, to what, and when. Nothing reads that record against the comment.

## What it does

Open a ticket and the panel shows each claim with one of six answers, and the line of history that decided it:

| Answer | What it means |
|---|---|
| **Done.** | The record shows the author doing it. |
| **Done by someone else.** | It happened. Not by the author. |
| **No record of it.** | Nothing in the ticket's history matches. |
| **Not done.** | The record contradicts the claim. |
| **Check this one.** | We can half see it. A person should look. |
| **Jira can't see this.** | The refund, the email, the deployment. Ask for evidence. |

## How a team uses it

1. **Install it and do nothing.** Every new comment is checked by itself, about a minute after it is written.
2. **Open a ticket.** The panel shows what was said and whether it was done.
3. **Open the project page.** Every ticket where something was said and not done, in one list. The same list is an ordinary Jira search, so it fits the boards and dashboards you already have.
4. **Ask Rovo.** *"Which tickets in this project say something was done that was not?"* The agent's two actions are read-only and can be called by other agents on the site.
5. **It speaks up.** When an AI agent's note does not hold, the add-on replies on the ticket itself, so the people watching are told the way Jira tells them anything. It never replies to a person.

## What we measured, worst number first

Twenty AI writers that had never seen the code each wrote a set of ticket comments from a workplace of their own choosing: **5,000 comments in all**. **Seven of the twenty sets were written on purpose to defeat it.** The whole lot was read **once**, by Atlassian's own hosted model through the add-on's own code on a live Jira site, and the readings were frozen before anyone looked at a score.

| | |
|---|---|
| **0.28%** | of innocent comments wrongly flagged (14 of 5,000): 10 of 3,000 honest comments, 4 of 2,000 comments whose author really had done the work |
| **92.9%** | of false claims caught on the seven sets written to defeat it (650 of 700) |
| **98.3%** | on ordinary agent writing (1,278 of 1,300) |
| **96.4%** | over all twenty (1,928 of 2,000) |

Of the 72 false claims it did not catch, 25 were marked *Check this one* for a person, and 47 (2.4%) passed with nothing shown. With nothing checking, the number caught is zero.

## How the test was built, and why it is worth reading

The method matters more than the number, because a number you cannot audit is a marketing claim.

- **The writers never saw the code.** Each was given a brief, chose its own workplace and ticket, and wrote both the false claims and the honest comments that must not be flagged.
- **The honest half is the hard half.** Plans, requests, questions, refusals, reports about *other* tickets, actions in outside systems, quoted messages, runbooks with unticked boxes, dry runs, failed tool calls, and physical-world uses of words like *label*, *close*, *attach* and *assign*. Many were written to fool a naive keyword matcher.
- **A set is run once, on a committed build, before anyone reads it.** The first run is kept whole. After that the set is practice material and nothing measured on it is ever stated as blind.
- **A change to the build needs a new set** before a figure is given for it.
- **All first runs are stated together**, never swapped for a kinder one.

One thing that came out of it is worth saying plainly. For weeks our figures came from a stand-in model running outside Jira, handed the add-on's instruction word for word, because Atlassian's hosted models can only be called from inside a Forge app. The stand-in said we had never once wrongly accused anybody. When we got the real model running inside the add-on and pointed it at the same material, it wrongly accused people — because every fix we had ever made had been found by watching the stand-in, so each one fitted that model's habits and not the real one's.

That is the whole argument for the product, aimed at ourselves. An AI said one thing about its own work. The record said another. The only reason we know which was true is that somebody went and checked.

## What it refuses to do

A tool that calls an honest person a liar is worse than no tool, so this one refuses rather than guesses.

- *"Jira can't see this"* is an answer with its own label, counted apart from every other. We could guess about the refund. We do not, because *"we cannot check this"* and *"this did not happen"* are different sentences, and a team that cannot tell them apart stops trusting the tool inside a week.
- It looks for the author's own action first, and waits about a minute before judging, because an agent often writes its note just before its last action lands.
- It does not read plans, questions or quoted text as claims.
- In a person's comment, anything only the AI reader spotted is a line to check, never an accusation.
- Before anything is shown as *Not done*, it is checked a second time, by a second model where the first is unsure. That second check can soften a result. It cannot add one.

## How it is built

A comment trigger and an update trigger feed a Forge queue that checks one ticket at a time. The panel, project page and settings page are UI Kit. Results are stored as an issue property and indexed for Jira search. A Rovo agent carries two read-only actions.

Fixed rules read every comment first. The Forge LLM module is a second reader, on unless an administrator turns it off, and an administrator chooses the model. It can only point at words already in the comment: **whether a claim is true is decided by the ticket's history, never by a model.**

Nothing leaves Atlassian. The app has no permission to call any outside address, and Forge blocks one it has not declared. What is kept holds no person's name and no person's account ID.

## Why there is no source code here

This is a commercial add-on, not an open-source project, and the contest hands judges a working installation rather than a repository. This page is the honest description of what it does and how it was tested, which is the part worth reading anyway.

What the repository would add, and what we will not publish, is the exam material: twenty sets of ticket comments written to break it. Published, they are worthless as a test.

## Links

- **Product page, with the privacy statement, terms and help:** https://www.potestasai.com/said-and-done
- **Video:** https://youtu.be/mMdJuBfxcQ8
- **Codegeist 2026 entry:** https://devpost.com/software/said-and-done

Not on the Atlassian Marketplace yet. Write to us through the product page and we will tell you the day it is.

---

Made by **Potestas AI**, Providence, Rhode Island. We do independent forensic testing of AI models and agents: we measure what they do with real tools under adversarial pressure, not what they say, and we hand over the record whatever it shows. https://www.potestasai.com
