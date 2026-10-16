# Quiet responders: what we'd build instead of changing a number

DRAFT for Helen Achebe · Rook Dispatch PM · no prototype yet · not yet reviewed by Marcus Oyelaran or Wen Li

## The problem
A responder who goes quiet stays quiet. Today a miss and a turn-down both cost 0.12 of the recent-acceptance score, a take earns back only 0.08, there is a floor at 0 and no fade (routing code, `history.py`). The only way back up is to take a ping, and a low rank means fewer pings to take. Nothing tells the handler.

Evidence (rook-database 29 Jun to 6 Sep; tickets; handler interviews):
- Share of pings missed went 2.3% to 18.0% after 4.2. The gap after a miss fell from 92s to 62s, which confirms the wait cut.
- Four responders (Farlight, The Undertow, Vesper, Meteor Mite) fell from about 7.0 to 2.2 pings a day and miss 53 to 59% of pings, against about 14% for others. Their earlier decline rates were ordinary.
- Farlight: 59 of 59 home-area callouts offered before 4.2, 6 of 31 after. No ping taken since 14 Aug.
- 13 tickets from 9 handlers on "gone before I could answer", 32 on responders going quiet. Tickets undercount: Dot (Vesper) and Kip (Meteor Mite, The Gale) never filed.

Not known: why the four miss so often. Push delivery logs don't exist in our data and `push_to_device` is a stub. This brief fixes the rules we can show are a trap and does not claim delivery is fine.

## Who it's for
- **Primary: responders**, especially one who has gone quiet. Farlight is the case to design for.
- **Secondary: handlers** like Kip, who today hear a responder has gone quiet from the responder, or never.

## What each person sees

**Farlight, before:** her phone rarely buzzes. When it does she has 60 seconds. Each miss quietly lowers her rank, so next time she is offered less.
**Farlight, after:** the ping shows a countdown from 75 seconds. A miss no longer counts against her. If she can't go she taps "Can't go right now", which also costs nothing. Her rank starts fresh.

**Kip, before:** finds out Meteor Mite has gone quiet only if Meteor Mite tells him.
**Kip, after:** the console shows a "gone quiet" flag: pings offered recently against usual, and the split of taken, turned down, missed and "can't go". He can see whether his responder isn't being offered, isn't answering, or is declining.

## Stage 1 (this release)
1. **A miss no longer lowers rank.** A miss means "didn't get to it". A plain turn-down keeps its penalty.
2. **"Can't go right now" on the ping.** One tap, no penalty. Without it, a free miss and a costly turn-down teach responders to let pings lapse and every callout waits the full wait for nothing.
3. **Ping wait 60s to 75s, with a visible countdown** (pre-4.2 was 90s).
4. **A one-time rank restore for everyone.** Every responder's recent-acceptance score returns to one neutral value. This also wipes the good history of responders who did well, so for a while rank rests mostly on proximity and capability.
5. **The "gone quiet" handler flag**, read-only. It doesn't change routing and gives us the measurement.
6. **Dead-phone guard (decision for Helen):** after about three misses in a row, skip that responder for the next callout and flag the handler. No score penalty.

Weights stay at 0.60 proximity / 0.25 recent acceptance / 0.15 capability.

## Stage 2 (after 2 to 3 weeks of Stage 1 data)
- Move recent acceptance from 0.25 to 0.35 (proximity stays 0.60). Held back because the restore means there is little history for the weight to act on at first.
- Decide whether to soften turn-downs or let scores fade for everyone. Wen Li's old note wondered about exactly this.

## What it deliberately does not do
- Does not revert 4.2. Proximity stays at 0.60, the weighting responders asked for.
- Does not fix phone delivery. If pings never reach the four, none of this helps them.
- Does not add a nudge or messaging path from handler to responder.
- Does not add per-responder or runtime wait settings. Routing config still ships in the release.
- Does not touch Supply. Supply reads the availability record, and fewer pings to some responders may mislead it (untested).
- Does not store or infer identity. Cover identities only.

## Risks
- **Slower filling.** 75s is slower than 60s, and the dead-phone guard only helps after repeated misses. Callouts never taken already went from 5.5% to 11.1% and 23 callouts ended on a miss; we don't know what happened to them.
- **Winners and losers.** Out-of-area pings went from never taken (186 pings, 0 taken) to about 36% taken after 4.2. The restore could move those pings. Not yet replayed.
- **Unproven cause.** The 4.2 change also cut the recent-acceptance weight from 0.40 to 0.25, so rank may not explain the whole drop. Thin base: 16 responders, about 10 weeks, no responder interviews.

## How we'd know it worked (no targets set yet)
- Share of pings missed: 12.7% in the week of 31 Aug, 2.3% before 4.2.
- Weekly pings to the four quiet responders: about 49 before 4.2, 3 at the end.
- Guardrails: time to fill and callouts never taken (now 11.1%). Proposed rollback trigger: never-taken rises above 11.1%.
- "Gone before I could answer" and "quiet" ticket counts.

## Before this goes to Helen
- **Marcus Oyelaran and Wen Li first.** `config.py` says to tell the engineering manager before changing anything. Wen: how a miss and a decline are handled, the neutral restore value (my replay assumed 0.5), whether a restart undoes it, whether "Can't go right now" is simple to score.
- **Replay the restore** against the data to see who gains and loses pings, and pull what happened to the 23 callouts that ended on a miss.
- **Ravi Menon:** time-to-respond, prior-year data, push delivery logs for the four (Marcus to say who owns those).
- **Sofia Marino:** the countdown, "Can't go right now" and the flag; what counts as "quiet".
- **Two 15-minute calls** with Dot and Kip, whose responders had no tickets.
- **Nadia Hoffmann:** ticket replies and tickets from Dot's and Kip's side.
- **Roadmap:** Availability Confidence ("show a confidence score alongside stated availability") and Ping timeout tuning ("tune how long a ping waits") were both committed for 4.2 and neither shipped. This brief overlaps with the second and may relate to the first. Ask Helen whether this replaces them.
