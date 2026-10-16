---
name: review-checklist
description: Reviews a product brief against a fixed six-point checklist before it goes any further. Use when the user says "review this brief", "run the review checklist", "check this brief", or points at a brief file (e.g. in 05-super-speed or a briefs folder) and asks for the standard check.
---

# Review checklist for briefs

Run the same six checks, in the same order, on any brief the user points at. Do not rewrite the brief. Report what is there, what is missing, and what to fix.

## Input

- The brief is a file path the user gave, or the file they named. If they gave none, ask which brief. Read the whole file before judging.
- If the brief has a short and a long version, check each separately and note any disagreement between them.

## The six checks

For each check, give a verdict of **Pass**, **Partial** or **Fail**, then one or two lines of evidence. Quote or cite the section or line that supports the verdict. A missing item is a Fail, not a guess.

1. **Owner.** The brief names who owns it, by person and role. "The team" or an empty owner field is a Fail.
2. **Success measure.** It says how we will know it worked: a named metric, a baseline, a target or direction, and when it will be read. A metric with no baseline or no read date is Partial.
3. **Scope matches.** Compare the scope stated at the start (summary, goal, in-scope list) with the scope at the end (asks, deliverables, next steps, roadmap). List every item that appears in one and not the other, including scope added later in the document.
4. **Problem before fix.** The problem is explained before any solution is proposed. Flag the first place a solution appears and check that the problem section comes before it and does not just restate the solution.
5. **Evidence labels.** Claims are labelled as **established** (checked against a source), **hypothesis**, or **proposed, not built**. Flag unlabelled claims that look like fact, hypotheses written as fact, and proposed changes that read as shipped. List the worst few, not every sentence.
6. **The decision.** The brief states the decision it needs, who makes it, and by when. Missing any of the three is Partial; missing all is a Fail.

## Output format

1. One line: the brief's name and an overall call: **Ready to send**, **Fix first** (Partial or Fail on any check), or **Not ready** (two or more Fails).
2. A table: check, verdict, evidence.
3. A short "Fix before it goes further" list, ordered by importance, each item a concrete edit ("Add an owner under the title: Priya's successor, Dispatch PM").
4. Only if relevant: facts in the brief that contradict other course sources you already have open. Say they are worth checking, and do not assert which side is right unless you verified it.

Keep the whole review to roughly one page.

## Rules

- Same checks every time. Do not add, drop or reorder them. If you notice something outside the six, put it in one final "Also noticed" line.
- Do not invent an owner, date or metric to make a check pass.
- Do not edit the brief unless the user asks afterwards.
