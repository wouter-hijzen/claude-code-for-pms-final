# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.

### 2.

Has anything in this code changed recently? Walk me through what's different, and why it would matter to a responder, specifically for Farlight as a use case. Can we directly point at the cause that led to lower pings going out to Farlight, and what would have to happen for Farlight to recover?

### 3.

Can we dig into the proximity change impact - does this have any impact before and after for Farlight specifically and if so, what is that?

### 4.

Yes, build the timeline of those 31 Uptown callouts

### 5.

Update the below hypothesis based on my prompts in module 4:

Based on what I found, the reason some responders are getting no pings at all is ___, because ___.

### 6.

Is there anything in the code that does not align with what we have discussed across the last four modules?

### 7.

There was a question on the 4.2 release page that was not answered to date: "Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?"

Infer who asked the question. Use all information in this and prior modules to answer the question for this engineering manager.

### 8.

Answer the question below based on our findings this module:

According to my findings in the code, for someone who's gone quiet, they would need to ___.

### 9.

Your summary contains too much fluff. Remove things:

* Why were out-of-area responders first on 15 of 59 pre-4.2 Uptown callouts?
*
Why do you ask "What happened on Farlight’s phone on 13, 14 and 19 Aug?"

What are you referring to when asking "Did Helen’s “traced into the routing code, small fix” claim happen? The wiki records neither"

### 10.

Yes rewrite, but also 

1. remove any reference or questions to data after Sep 7. For this exercise pretend its right after that period and thats the reason there is no data. 
2. Remove "What does “region” in the code mean, and was Farlight on the available list after 12 Aug?": we have already proved that the proximity is not the culprit and that the decline was gradual (implying she was on the list)
3. Specify with what you mean on "Have the commitment conversation with Helen that Priya never had (see the parking lot)." and "Write the product-level description of ranking, which doesn’t exist today."
4. Remove "Ask Ravi Menon for a current weekly report, since we have no data after early September."

### 11.

Can we already draft proposed product answers to these issues?
