# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I am the new PM for **Rook Dispatch**, taking over from Priya Raghunathan
(left 21 Aug 2026; she was the only Dispatch PM for 14 months). Sources: rook-wiki
(search_wiki, then read_page / read_database), rook-database for numbers, and
`00-rook/company/notes/handoff-from-priya.docx`. Wiki comments name people by role only.

### Company and products
- Rook Industries sells coordination and provisioning software to independent masked
  responders and their handlers and quartermasters. Subscription, priced per active responder.
  Ships monthly on a release train; point releases are 4.x.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged about a callout, whether
  they take it. Users: handlers (console, desktop) and responders (phone).
- **Rook Supply**: requisitions, maintenance, field failure reports. Users: handlers, quartermasters.
- **Confidentiality:** cover identities are never stored; never try to work out who anyone is,
  and read Security Policy 4.1 before designing anything touching responder records.

### People (roles in wiki comments -> names)
| Role | Name | Notes |
|---|---|---|
| Director of Product | Helen Achebe | My director; owns roadmap and commitments |
| Engineering Manager | Marcus Oyelaran | Runs Dispatch eng; Priya said start here |
| Staff Engineer | Wen Li | Built the ping-ranking logic; no doc exists, talk to her |
| Product Designer | Sofia Marino | Console and phone app; ran the September interviews |
| Support Lead | Nadia Hoffmann | Owns tickets; has the ticket breakdown |
| Data Analyst | Ravi Menon | Weekly report on how often responders take pings (Priya wrongly pointed to the EM for numbers) |

### Vocabulary
- Callout (unit of work) -> **ping** (offered to one responder) -> **taken**, **turned down**, or
  **missed** (ping wait ran out). Turned down and missed are recorded separately; both move the ping on.
- **Ping wait**: how long a ping stays on a phone (Priya's doc says "timeout"); same for everyone.
- **Acceptance rate** (taken / pings; headline metric), **time-to-accept** (median seconds).
- **Coverage gap**: no available responder had the needed capability tags. Not the same as low acceptance.
- **Routing priority**: ranks responders by travel-time proximity, availability, capability match,
  recent acceptance history. Declining or missing a ping lowers recent acceptance, so lowers later rank.
- Capability tags, Responder Availability Record (written by Dispatch, read by Supply), mutual aid
  (unsupported, Q4 exploration). Supply: requisition, field failure report, service interval.

### Where things stand
- **4.0** (7 Apr): new nav, profile redesign, routing-override audit log. **4.1** (16 Jun): travel-time
  proximity, bulk callout, push reliability. **4.2** (12 Aug): proximity weighted up vs. acceptance
  history, ping wait 90s -> 60s, filters persist, three fixes. Clean rollout.
- **The 4.2 problem:** since ~12 Aug callout tickets run ~3x normal. About two thirds "phone never
  goes off", one third "gone before I could answer" (explained by the 60s wait). Wen Li has not answered
  Marcus's 14 Aug question: does the change apply to responders who keep turning pings down, or was
  that unintended? Config doesn't distinguish. A handler emailed Nadia directly (rare).
- **Two changes shipped together** (proximity weighting, 60s ping wait), so the cause is confounded.
  Priya's view is "seasonal, August is soft, don't revert 4.2". Marcus, Nadia and Priya all deferred
  the question to September.
- **Data check (rook-database, 29 Jun-6 Sep; no prior-year data, so seasonality can't be tested):**
  - Acceptance rate was flat at 75-78% for six weeks, then dropped as a step at the 12 Aug release,
    not a slide: 54% (week of 10 Aug), 66%, 67%, 73% (week of 31 Aug). Recovering, not back to baseline.
  - Missed pings went from 2-6/week to 38, 28, 24, 21. Turned-down share is roughly unchanged.
    Pings per callout rose from ~1.23 to ~1.30 (1.61 in release week). Weekly callouts fell from ~140
    to 110-127, so demand is a bit lower, but that doesn't explain the acceptance step.
  - Tickets: 5-8/week before, then 20, 27, 32, 25. Matches Nadia's "~3x". Of 107 tickets since 12 Aug
    (classified by subject line), 30 are "phone never goes off / quiet" and 15 "gone before I could
    answer": a 67/33 split, matching Nadia. The other 62 are unrelated (filters, gear, dark mode).
    Twenty of the 30 "quiet" tickets are about just Farlight and The Undertow (10 each); the rest
    are Corporal Ashgrove and Halfmoon (5 each), whose pings fell only from ~11 to ~7-8 a week.
  - **Four responders account for the "phone never goes off" pattern:** Farlight, Meteor Mite,
    The Undertow, Vesper. They had ~75 pings each pre-release (~77% taken). Post-release they got 56
    pings combined (14 each), 13 taken, 33 missed. Farlight has had no ping since 28 Aug. The other 12
    responders went from 77% to 68% taken, missed 2% -> 14%, consistent with the shorter ping wait.
  - **Geography doesn't explain it** (only named areas in the data, no distances). Farlight (Uptown),
    The Undertow (Harborside) and Vesper (Old Town) are the sole responder in their area and were pinged
    for nearly every callout there pre-release (90%+ taken). Post-release they got ~6-10 own-area pings
    each; those callouts went to responders from other areas. Meteor Mite shares Eastgate with The Gale,
    who got 54 of the pings to Meteor Mite's 9. Proximity-up should have favored them, so the likelier
    suspect is a ranking penalty from misses/declines (hypothesis; Wen Li to confirm).
  - Meteor Mite and Vesper (handlers Kip and Aunt Dot) show the same collapse but have filed no tickets.
  - Not established: why those four. Pre-release their turned-down share (~22%) matched everyone
    else's, so I haven't confirmed Marcus's "frequent decliners" idea. Ranking inputs aren't in the
    database; Wen Li can confirm. Ping wait length isn't in the data either.
- **Roadmap (Q3, last reviewed 30 Jun, stale):** Change to who gets pinged, Ping timeout tuning: 4.2,
  shipped. **Availability Confidence: marked Committed for 4.2 but absent from the 4.2 notes**, so it
  was squeezed out (Priya's "things I didn't get to"). Requisition approval chains: 4.3 Committed.
  Handler phone app, Shared cover between responders: Q4 Exploring. Priya never had the commitment
  conversation with Helen; I need to.
- **Briefs:** Bulk Callout (Jan) and Routing Override Audit Log (Mar) read as unbuilt but shipped in
  4.1 and 4.0, so they are stale. Handler Phone App (8 Sep, Sofia exploring, unstaffed, no baseline).
  Requisition Approval Chains (Supply, 14 Jul) has one real ask (second approval over $2,000) buried
  under four scope additions.
- **Routing code** (`00-rook/code/dispatch-routing`, owner Wen Li): weights are global, so 4.2 applies
  to everyone, answering the config half of Marcus's 14 Aug question; whether it was a decision is
  still unanswered. 4.2 weights proximity/acceptance/capability 0.60/0.25/0.15 (was 0.45/0.40/0.15),
  offer timeout 60s (was 90). Accepted +0.08, declined **or missed** -0.12 (same penalty), floor 0,
  start 0.5. The score never decays (Wen's 2019 TODO asks if it should), so "recent acceptance" isn't
  recent. It sits in an in-memory dict, so it may reset on deploy (ask Wen). Ranking only orders the
  list, but callouts average ~1.3 pings, so low rank means rarely asked, and a responder who isn't
  asked can't earn the score back. That is the leading explanation for the four quiet responders
  (hypothesis; no per-responder scores are in the database).
- **Contradictions and gaps:** "ping wait" (glossary), "timeout" (Priya, roadmap) and
  `OFFER_TIMEOUT_SECONDS` (code) are one setting. Helen's undated `05-super-speed/director-request.txt`
  says engineering traced the quiet-responder problem into the routing code and a fix is "small enough
  to ship this afternoon"; the wiki records neither. The DB has no answer times, so time-to-accept and
  the 60s wait can't be checked, and no data after early Sep. Roadmap owners are all Priya and stale.
  Tickets 3063/3128 show requisitions already stuck 9+ days, while the requisition brief adds a step.
  `06-sidekicks/scheduled-run-output.txt` cites a `briefs/` folder that isn't here and misses that
  two briefs shipped. Halloran is a handler in the interviews but a quartermaster in the Supply briefs.
  I have not read Customer interviews or the Training Class PDFs.
- **Open to-dos:** write the product-level description of ranking (code and README exist, no product
  doc); do not let console filter-persistence tickets eat the first month (though some are real bugs).

### Session notes (Module 1)
- Sources that mattered most: the routing code and the rook-database `pings` table. The wiki alone
  can't explain 4.2. Next-session first step: ask Wen Li whether scores persist across deploys and
  whether the 4.2 weights were a deliberate decision.
- Biggest open question: why Farlight, Meteor Mite, The Undertow and Vesper fell out of the ping order.
  Helen wants a one-pager on what to build for them (and a clickable version), not a number change.
- Marcus asked to regroup on 4.2 once the new PM had looked; nobody has recorded that it happened.
- Wiki and ticket claims have held up when I checked them (ticket volume, the 67/33 theme split);
  Priya's "seasonal" and "mostly noise" calls have not.

### How to help me
Separate wiki fact from opinion (Priya's handoff is opinion). Check claims against rook-database
before repeating them (tables: callouts, pings, responders, handlers, support_tickets). Name people and roles together. Flag stale wiki pages.

- Module 2 (interviews and tickets): the 4 interviews (Aunt Dot/Vesper, Ambrose/Captain Vantage, Halloran/Sgt. Bulwark, Kip/Meteor Mite and The Gale) are louder on "gone before answer" (3 of 4) than "quiet" (2 of 4). The 147 tickets flip it: 30 quiet vs 15 gone, all filed after 12 Aug and all open, none before. Quiet is under-reported in interviews because nobody complains about a ping that never arrives.
- Ping-issue tickets only (45): quiet is Farlight 10, The Undertow 10, Corporal Ashgrove 5, Halfmoon 5; gone-before-answer spans 12 responders. Per week: 7, 11, 15, 10, then 2 (data ends 7 Sep). The grouping is my own reading of subject and body, not a database column. Filter tickets (16) and the 19 post-only tickets (account, profile, Monday slowness, maintenance) are set aside as not ping-related. Saved-filter bugs (silent reset, shared computers) are real and need their own fix list.
- The four collapsed fell to 18-29% taken (Farlight 73 to 18, The Undertow 77 to 21, Vesper 81 to 29, Meteor Mite 69 to 21); the other 12 combined went 77 to 68. Eastgate is the sharpest pair: Meteor Mite about 11 to 4 pings a week, The Gale about 13 to 19. Kip (handler for both) says "Gale's exhausted". Every area except Eastgate has a single responder, so "sole responder" does not separate the four.
- Handlers can't see the phone or the ranking, so none connected lost pings to a change; the handlers for Farlight and The Undertow, the worst hit, were not interviewed. Interviews alone would overweight dark mode and text size and understate quiet.
- Still to rule out with Wen Li: Halfmoon's availability time zone ticket (3050), the phone-number tickets (3059, 3132), and Ironvale's older phone (3034) behind two "gone" tickets. Ambrose's ticket 3043 (13 Aug) and his interview ("two weeks ago") disagree on the date. Module 1 hypothesis (symmetric miss penalty, no decay, no recovery) is consistent with all of this but untested; per-responder scores are the missing evidence.

- Module 3 (rewind), compare weekly rates, not raw counts: the before window (29 Jun-11 Aug) is 6.3 weeks and the after window (12 Aug-6 Sep) is 3.7. The "Eastgate ~110 to 52" figure was taken pings over unequal windows; per week Eastgate went 17.8 to 13.7 taken (callouts 18.8 to 15.3). Citywide callouts fell ~15% a week. The four collapsed pooled: 75.5% taken to 23.2%, missed 2.9% to 58.9% of pings, declined flat (22% to 18%), pings per week -69%. Missed per week rose ~6x (1.4 to 8.9), so it is not a smaller-denominator artifact.
- Sequence: misses came first. In the first two days after release the four were pinged at the normal pace (14 pings) but 9 were missed; pings then fell (-26%, -75%, -88%, -96% by week). In-area miss rate went 1% to 53% (others 1% to 6%); out-of-area pings were never taken before release (0 of 186), and the four's share of them rose 16% to 39%, so mix explains some misses but not the collapse. Ping wait (gap after a miss) is 92s to 62s for everyone, the four included, and time of day does not explain it. The cause of the early misses is still unexplained; ticket wording ("gone before she could answer") says the ping arrived, so I withdrew my own delivery-failure idea.
- Score replay (assumes everyone starts 0.5 on 29 Jun, no reset at deploy, every logged ping updates it): break-even acceptance is 60% (+0.08 per taken, -0.12 per declined or missed); the four hit 0 on 27 Aug (The Undertow), 28 Aug (Farlight), 1 Sep (Vesper), 5 Sep (Meteor Mite), the other 12 dip to 0.75 and recover to ~0.90. If misses cost nothing the four end at 0.76-0.84. A full 1.0 acceptance gap is worth ~19 travel minutes under the 4.2 weights (~40 under 4.1), so the score gap alone looks too small to explain the first-week loss of first pings (four at 30% of the others' rate in the week of 17 Aug). No passive recovery exists; leftover pings are out-of-area ones nobody takes. Still needed: real per-responder scores and whether they reset on deploy (Wen Li), travel times, who was available per callout, response-time logs.
- Individual case: Farlight (handler Linda Pruitt, area Uptown, sole responder). 11 quiet/missed tickets (3060 to 3139), every number matches the ping log; ticket 3110 describes the 28 Aug missed ping. No ticket 12-16 Aug (handlers cannot see misses). Uptown demand stayed 6-11 callouts a week; others (Sgt. Falkirk, Cindermark, Sgt. Bulwark) took them from 17 Aug. The Undertow (handler Desmond Okafor) is the equal backup case. Next: interview Linda Pruitt and Desmond Okafor.

- Module 4 (routing code): the proximity weight change (0.45 to 0.60) is mostly ruled out as the cause. Farlight, sole Uptown responder, was pinged on 59 of 59 Uptown callouts before 4.2 (first on 44) and on 6 of 31 after; it missed on 13, 14 and 19 Aug, took 12 and 14 Aug, and has had no Uptown ping since 22 Aug (a slide over ~10 days, not a step). The lower acceptance weight (0.40 to 0.25) makes a low score hurt less, so it doesn't explain it either. Treat 7 Sep as "now": do not ask for later data.
- Code facts: a failed push and an ignored ping both log as "missed"; scores live only in memory and are logged nowhere; no answer times are kept; "region" in the code is wider than the database "area" (Kingsbridge and Mill District responders were candidates for Uptown). Marcus's 14 Aug question is answered: 4.2 applied to everyone, nothing in the code targets decliners, and the four were not frequent decliners (22% before, flat after). Still open for Wen Li: was it deliberate, and do scores reset on deploy.
- Helen's `05-super-speed/director-request.txt` is my assignment, not a claim to check: a one-pager and clickable version of what to build for quiet responders, not a number change. Nothing has shipped (changelog ends at 4.2, no decay in the code). I have not yet read Security Policy 4.1, which I must before designing anything touching responder records.
- Draft product answers (not decided): A a miss costs less than a decline; B a re-entry ping for quiet responders in their own area; C scores ease back toward neutral; D handlers and responders can see misses and "not offered lately"; E restore 90s or add an "I'm looking" tap; F missed pings and pings per responder in Ravi Menon's weekly report; G a stopgap score reset for the four. Order: F and G now, A, B, D next release, C and E later. Decisions for Helen: own-area priority vs callout speed, miss vs decline cost, 90s vs the tap.
- Availability Confidence conversation with Helen means telling her it was Committed for 4.2 but didn't ship, and agreeing to re-date, recommit or drop it. The ranking needs a plain-language description for non-engineers (inputs, what a miss does, ping wait, what handlers and responders can see). Farlight ticket count is 10 quiet in the Module 2 breakdown but 11 in the Module 3 note; check before quoting.
