# Brief: keeping responders in the ping order after they miss
For Helen Achebe · Dispatch PM · Draft v3 · Data as of 7 Sep (it ends there)

## Problem
Since 4.2 (12 Aug), four responders (Farlight, The Undertow, Vesper, Meteor Mite) went from 75.5% of pings taken to 23.2%. Their pings fell 69% a week and their misses rose from 3% to 59% of pings; declines stayed flat (22% to 18%). Farlight, sole responder in Uptown, was pinged on 59 of 59 Uptown callouts before 4.2 and 6 of 31 after.

## Who this is for, and what changes
| Who | Needs | Before | After |
|---|---|---|---|
| Responder who goes quiet | Work offered again, and to know why | A miss costs as much as a decline and never fades: fewer pings, no way back | A re-entry ping in their own area; a missed ping costs less |
| Handler | To spot a quiet responder before a ticket | Sees nothing | On a responder's page: missed pings for two weeks and a "not offered lately" flag |
| Helen | A clear trade-off to decide | n/a | Decisions below |

Affected group is defined by behaviour, not name: **at risk** = 30%+ of a week's pings missed (min. 5 pings); **quiet** = pings below 50% of their own earlier average while area demand holds. In the data these caught all four by 17 Aug and none of the other 12. Proposed; to agree with Ravi Menon and Wen Li.

## Evidence
- **Fact (database):** acceptance 75-78% for six weeks, then 54%, 66%, 67%, 73%. The other 12 went 77% to 68% taken, misses 2% to 14%, which fits the shorter ping wait.
- **Fact (code):** missed and declined cost the same -0.12; the score never decays; a responder who is not asked cannot earn it back.
- **Hypothesis:** that loop explains why the four stay quiet. No per-responder scores exist yet.

## Recommendation
1. **Now:** add missed pings and pings per responder to the weekly report; one-off score reset for the four, if Wen Li confirms it is safe.
2. **Next release:** the re-entry ping (primary; a missed re-entry ping costs nothing), cheaper misses, handler visibility.
3. **Later:** scores ease toward neutral; 90s wait or an "I'm looking" tap.

**Goal:** the four's missed share back to about the others' level (14%, not 59%); the four at half their old pings per week within 4 weeks; pings per callout no higher than 1.31. Total misses stay above the old 2-6 a week because of the 60s wait.

## What we are NOT doing
- **Reverting 4.2.** The 60s wait explains the other 12's dip but not the four's lost way back. If scores reset on deploy a revert would reset them by accident. *Revisit if* scores show the wait is the main cause. (Wen Li)
- **Only retuning weights.** You asked for a build, and weights alone would not explain the drop. *Revisit if* real scores disagree. (Wen Li)
- **Waiting for September.** The fall was a step at release; there is no prior-year data to test seasonality. *Revisit if* post-7 Sep data shows full recovery.
- **Touching identity.** Only ping outcomes and area. *Revisit if* an option needs more (Security Policy 4.1).
- **Filter tickets or Availability Confidence.** Separate work.

## Decisions I need from Helen
1. Should a quiet responder get first call in their own area, even when someone else could answer faster?
2. Should a missed ping cost less ranking than turning one down, and by how much?
3. Restore the 90s wait, add an "I'm looking" tap, or keep 60s?
4. Availability Confidence (a confidence score beside stated availability) was Committed for 4.2 and did not ship: re-date, recommit or drop?

## Open questions and risks
- **Main uncertainty: why did the misses start?** The four had 9 of 14 pings missed in the first two days, before any score could drop. The loop explains staying quiet, not starting. If the cause is delivery or availability, cheaper misses and re-entry will not remove it.
- Wen Li: was the 4.2 change deliberate, and do scores reset on deploy?
- Security Policy 4.1 is not in the wiki (only referenced); Security should confirm before design.
- Re-entry pings could slow callouts if the responder is slow or out of range.

**Next:** Wen Li (confirm scores, reset); Marcus Oyelaran (size next-release items); Ravi Menon (report fields, check weeks since 7 Sep); Sofia Marino (design after agreement; no prototype here).
