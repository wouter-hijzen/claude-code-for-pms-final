# 05 · Super Speed — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent three sessions finding out what went
wrong: the two piles of feedback that didn't agree, the numbers that
hid four people inside an average, and the code that settled it —
the shorter timeout is what did it.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I am going to have to create a Product Brief based on the ask of the Product Director. Give me advice in what is included in a good product brief and a proposed prompt for you to build one for me. Do not write or build the brief yet. I want you to give me ideas of what could be a good way to ask you to improve a first version of a product brief.

### 2.

What about the part of 'what we are NOT going to do to fix this'?

### 3.

Can you merge your prompt into the original ask?

### 4.

Should we focus on the four quiet responders, or just Farlight? Should we add to the prompt that it is just for a brief, not for a prototype?

### 5.

Update the full prompt.

### 6.

Does the prompt/brief include "who the solution is for, what changes once it ships, what it does not do"?

### 7.

Update the prompt for this.

### 8.

What will it change if we add "the data ends 7 sep"?

### 9.

Questions: 

1. Why would we track this? "Missed pings below 6 per week (21 now)" Is missed pings not expected to stay higher than before since response time went down from 90 to 60 seconds?
2. In your Recommendations you refer to lettered options, but those are not familiar to Helen.
3. The section "and a full acceptance gap is worth about 19 travel minutes, too small to explain the first-week loss." is difficult to understand. Either make it very simple or leave it out?
4. What do you mean with "Own-area priority vs callout speed."
5. Should we phrase the following as an ask? "How much cheaper a miss is than a decline."
6. Remind me what Availability Confidence is.
7. SHould "Least Sure of" be a separate section? Is it not more "Open Questions"? Remove at least #3 from that part.

### 10.

Not to be added to the brief yet:

* Act as Helen. What are the five hardest questions she’d ask on this brief, and does the draft answer each one?
* Evidence audit: list every claim in the brief. Tag each as database-verified, code-verified, wiki or Priya opinion, or hypothesis. Re-check the numbers against rook-database.”
* Reduce the options: “Recommend one primary path out of A–G and say what we’d lose by not doing the others.”
Make the metrics testable: “For each success metric, say how Ravi Menon’s weekly report would measure it, the baseline, and the target. Say where we lack the data.”
* Engineer’s view: “Review as Wen Li. What would she say is wrong, naive or missing about the ranking behaviour?”

### 11.

What do you mean with "Qualify the revert row with the scores-reset-on-deploy caveat."? and where would you put "Say what handlers would see, in one sentence."?

### 12.

What would you propose as threshold for quiet?

### 13.

Apply all four proposed edits

### 14.

Can this brief be fit into one page?

### 15.

Fit both options into a word document

### 16.

Save the brief product-brief-one-page.md. as 05-super-speed/brief.md. Show me the file when it's done.

### 17.

Remove any of the doc files, its not relevant.

### 18.

Keep 1, remove the one page .md, and rename the third to be 'longer version'

### 19.

make it 'longer-version-brief' rather than just 'longer-version'

### 20.

Take the brief you just wrote and build me a working prototype, an actual screen I can click through, not a description of one. Save it as 05-super-speed/prototype.html, a single file I can just open in my browser. Show me where this would actually happen, and make at least one thing on it respond when I click it.

### 21.

What does the "Pings last week" mean when it says for example "12 of ~8"?

### 22.

Yes change it to “12 (usual ~8)”

### 23.

What are key things in our problem statement that are not covered in this prototype?

### 24.

Add Taken % as a column and a taken-rate line. 

Should we have an area-level view that shows pings in an area? Do we know if handlers have visibility to this data if they for other handlers/responders today?

### 25.

In the area view, what does "Local not asked' mean?

### 26.

I want this screen to be for the handler, not for us as product or the company. What would the handler want to see in this view?

### 27.

What does the "Copied" button do I see in the Farlight Handler option

### 28.

Is this relevant?

### 29.

Cut them

### 30.

What is "The work-still-coming line."?

### 31.

Any of the main issues not yet being addressed in this view?
Should the handler see "(10 by responders based in other areas). Farlight was asked about 0 of them." Should that not just be Product/Rook?

### 32.

1. The handler has to go and look. Meteor Mite and Vesper’s handlers filed no tickets, so they may not have noticed. The flag only helps if someone sees it. There is no alert on the console’s start screen or a notification. --> Add this
2. The taken rate. The headline drop (75.5% to 23.2% taken) is on the internal tab only. The handler card shows counts, which is probably right, but it means the handler doesn’t see the scale of the change. --> add percentage

### 33.

Add the same visuals for handles as what is on the internal view

### 34.

Farlight now has a message"Farlight is not being asked about callouts lately. Responders do not always raise this, so Rook is telling you."

"Review button doesn't mean anything"

### 35.

Can we expand "What you can do" with options?

### 36.

Send a [area] callout to [responder] yourself
Is that something handlers should be able to do?

### 37.

Can we instead do "Request next Uptown callout to go to Farlight" ? This allows for audit/rules to be applied (and we can say no as Rook)

### 38.

Remove the "What Happens Next" section - this is no longer relevant since we had the "What you can do part"
