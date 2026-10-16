# Brief: keeping responders in the ping order after they miss

For: Helen Achebe (Director of Product) · From: Dispatch PM · Status: draft v2 · Data as of 7 Sep

## Problem
Since 4.2 (12 Aug), some responders stop getting pinged and cannot get back in. Four (Farlight, The Undertow, Vesper, Meteor Mite) went from 75.5% of pings taken to 23.2%, and their pings fell from 306 to 56 (windows of unequal length; per week it is about -69%). Their misses went from 2.9% to 58.9% of pings. Declines stayed flat (22% to 18%), so these are not frequent decliners. Farlight, sole responder in Uptown, was pinged on 59 of 59 Uptown callouts before 4.2 and 6 of 31 after.

## Who this is for
- **Responders who go quiet:** need to be offered work again after a bad stretch, and to know why the phone went silent.
- **Handlers:** cannot see misses or ranking today, so a quiet responder looks like a broken phone. They need to see "not offered lately" before it becomes a ticket.
- **Helen:** needs a clear choice on the trade-offs below.

The group is defined by behaviour: a responder whose pings drop sharply after a run of misses. The four are the evidence, not the scope.

## What changes once it ships
| Who | Before | After | Metric |
|---|---|---|---|
| Responder | A miss costs as much as a decline and the penalty never fades, so fewer pings, no way back | A miss costs less, and a quiet responder gets a re-entry ping in their own area | Pings per responder (the four) |
| Handler | Sees nothing until the responder complains | On a responder's page, sees their missed pings for the past two weeks and a "not offered lately" flag when their pings drop well below their usual level | Quiet-phone tickets |
| Everyone | One missed ping can start a spiral | Scores ease back toward neutral over time | Missed share of pings (the four) |

## Evidence (as of 7 Sep; the data ends here)
- **Fact (database):** acceptance flat at 75-78% for six weeks, then 54%, 66%, 67%, 73% by week. Missed pings per week went from 2-6 to 38, 28, 24, 21.
- **Fact (database):** the other 12 responders' misses rose from 2% to 14% of pings, which fits the shorter ping wait (90s to 60s). The four's rose to 59%, far beyond that.
- **Fact (code):** declined and missed carry the same -0.12 penalty, the score never decays, and a responder who is not asked cannot earn it back.
- **Hypothesis:** this loop explains the four. No per-responder scores exist in the database.

## Goal and metrics
**Defining "quiet"** (per responder, per week; proposed, to agree with Ravi Menon and Wen Li):
- **At risk:** at least 30% of their pings missed, with at least 5 pings that week. In the data this caught all four in the release week and no one else. The other 12 sit at about 14%.
- **Quiet:** pings below 50% of their own earlier average, while callouts in their area have not dropped. In the data this caught all four by the week of 17 Aug (25-36% of normal) and none of the other 12.
- Use each responder's average from before the drop, so the baseline does not shrink as they stay quiet. Based on one release and 16 responders, so these are starting points.

- Acceptance back toward the 75-78% baseline (73% in the week of 31 Aug).
- The four's missed share back to about the others' level (around 14%, versus 59% now). Total missed pings will stay above the old 2-6 a week because of the shorter ping wait, so that is not the target.
- The four back to their pre-4.2 pings per week.
- Guardrail: pings per callout stays near 1.3. Time-to-accept cannot be checked, as no answer times are logged.

## Recommendation
1. **Now:** add missed pings and pings per responder to Ravi Menon's weekly report. One-off score reset for the four, if Wen Li confirms it is safe.
2. **Next release:** make a missed ping cost less than a turned-down one; send a quiet responder a re-entry ping for callouts in their own area; show handlers and responders missed pings and "not offered lately".
3. **Later:** let scores ease back toward neutral over time; and either restore the 90s ping wait or add an "I'm looking" tap.

## What we are NOT doing
- **We are not reverting 4.2.** The 60s wait explains the "gone before I could answer" tickets and the other 12 responders' dip (77% to 68%), A revert would not fix the four's missing way back, because scores never recover on their own. The exception: if scores reset on deploy, a revert would reset them by accident, and the next miss would start the loop again. *Revisit if* scores show the wait is the main cause. (Depends on Wen Li.)
- **We are not only retuning weights.** You asked for a build, and the numbers say weights alone would not explain the drop. *Revisit if* Wen Li's real scores contradict this. (Depends on Wen Li.)
- **We are not waiting for September.** The fall was a step at the release, not a slide, and there is no prior-year data to test seasonality. *Revisit if* post-7 Sep data shows full recovery.
- **We are not touching identity.** Only ping outcomes and area are used. *Revisit if* any option needs more; then Security Policy 4.1 applies.
- **We are not folding in filter-persistence tickets or Availability Confidence.** Separate work.

## Decisions I need from Helen
1. **Should a quiet responder get first call on callouts in their own area, even when someone else could answer faster?** Yes protects them from silent drop-off; no keeps callouts as fast as possible.
2. **Should a missed ping cost a responder less ranking than turning one down? If so, by how much?** Today they cost the same.
3. **Do we restore the 90s ping wait, add an "I'm looking" tap that extends it, or keep 60s?**
4. **Availability Confidence (a confidence score shown beside a responder's stated availability) was Committed for 4.2 and did not ship. Do we re-date it, recommit or drop it?**

## Risks and open questions
- **Security Policy 4.1 is not in the wiki;** only About Rook refers to it. Everything here uses ping outcomes and area, but Security should confirm before design.
- Wen Li to confirm: was the 4.2 change deliberate, and do scores reset on deploy?
- The score loop is untested until we have real per-responder scores.
- **Main uncertainty: why did the misses start?** In the first two days after release the four were pinged at the normal pace, but 9 of 14 pings were missed, before any score could have dropped. The score loop explains why they stay quiet, not why they began missing, and the cause may not be ranking. It could also be delivery, availability or the shorter wait. Cheaper misses and re-entry pings treat the consequence; if the start has another cause, they will not remove it.
- Re-entry pings risk slower callouts if a quiet responder is slow or out of range.

## Next steps
- Wen Li: confirm score persistence, deliberateness, and a safe reset.
- Marcus Oyelaran: size the Next release items.
- Ravi Menon: add the report fields, and check the weeks since 7 Sep.
- Sofia Marino: design the handler and responder views after the brief is agreed. No prototype here.
