# 02 · Super Hearing — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session you built a context file and met the
company.

You still do not know what actually went wrong.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Use the rook-wiki connector. Open the Customer interviews database with search_wiki, then read_database, and read_page for each interview. These are four conversations with the people who run the console for our responders. Tell me what they're unhappy about, group it, tell me how many of the four raised each thing, and quote one line for each so I can hear how they actually said it.

### 2.
Before linking to any context data, redo your review with only the interviews. For anything where you did use other sources list the source. Also review for any inconsistencies in your analysis.

### 3.
Interviews are with handlers. What issues could they have overlooked with the new release based on how they use the software vs the responders?

### 4.
Compare the outcome of the interviews with the findings from Module 1, and make an analysis on what the trouble is with 4.2.

### 5.
Finish this sentence based on your analysis: Based on the interviews, the trouble with 4.2 is ___.

### 6.
Use the rook-database connector. Call describe_table on support_tickets, then run_query. Same treatment as before: group them, tell me how many are in each group, and quote one line from each. Count every row, including ones that look unrelated.

### 7.
Can we separate tickets that clearly show similar patterns before and after August 12th, and are highly unlikely to be related to the new release? Then do a thorough review of the remaining tickets to see if there are more detailed groups to list.

### 8.
How would tickets in group C or F impact pings going out to handlers or responders? Can we exclude these tickets from our analysis, focusing only on groups A and B for our problem analysis?

### 9.
Finish this sentence based on your analysis: Based on the tickets, the trouble with 4.2 is ___.

### 10.
You've now read both folders. Where do they disagree? What's loud in the interviews but rare in the tickets, and what's all over the tickets that nobody brought up in the interviews?

### 11.
Make one visual that compares the interviews with the tickets on the same themes that are relevant to 4.2 issues as defined in the tickets analysis.
Panel 1 (left): for each theme (quiet, gone before answer, others if I missed it), a bar for how many of the 4 interviewees raised it. Label it "of 4".
Panel 2 (right): for the same themes, a bar for how many of the 147 tickets are in it. Label it "of 147".
Keep the themes in the same order in both panels so the mismatch is visible. Highlight quiet vs gone-before-answer, since the ranking flips between the panels.
Under each panel add one short quote: the interview line for the left, the ticket line for the right.
Add a small timeline strip of weekly tickets with a marker on 12 Aug.
Use the counts from this conversation only. Don't add numbers I haven't seen.

### 12.
Only consider tickets in the relevant groups for ping issues in total counts.

### 13.
Distill this information to a more clear visual - it should be easy to understand and focus on responders who collapsed. Pay special attention to the area where there is one over-worked superhero and one who has nothing to do.

### 14.
What would I have gotten wrong if I read the interviews and not the tickets?

### 15.
What was my hypothesis in module 1?
