---
name: kaveno-retro
description: Run the weekly retrospective — read what was proposed, selected, rejected and edited, and what came back from outreach, and turn it into a short list of proposed repairs to the market brief. Use when the operator asks for the weekly review or retro, wants to know how the week went, says the drafts keep needing the same correction, asks why nobody is replying, or when a month has passed without the brief changing.
---

# Kaveno retro — turning a week of editing into a better brief

Everything Kaveno writes is bounded by the market brief. When drafts are consistently
wrong in the same way, the brief is missing something — and the evidence for *what* is
sitting in the difference between what was generated and what the operator actually
posted.

**This skill produces proposed repairs to the brief, not a report.** A report is something
the operator reads and forgets. A repair is a specific edit they approve or reject in one
line. If you end a session having told them how the week went without proposing a single
change, you have failed even if everything you said was true.

**Objective: all three.** Ten minutes, weekly.

## What this skill refuses

- **A dashboard.** No charts, no trend lines, no vanity counts. Every number here has to
  earn its place by pointing at a repair.
- **Inventing a metric the product does not collect.** See §The honest limit.
- **Proposing more than three or four repairs.** A list of eleven improvements is a list
  nobody acts on. Rank, propose the top few, and say what you held back.
- **Editing the brief yourself.** You propose; the operator approves; then it is edited.

## The four readings, in order

### 1. The editorial signal — `body_generated` against `body_final`

**This is the highest-signal data in the system**, and it is the reason the schema keeps
both. Every time the operator changed a draft before posting it, they were correcting
something the brief failed to tell you.

Read the week's edits and look for the **repeated** correction, not the one-off:

> *You rewrote the opening line in four of five posts, and in each case you replaced a
> general statement with a specific consequence. The brief has no tone rule about openings.
> Proposed: add one.*

> *You changed `אי-התאמות` to `ליקויי בנייה` three times. The brief's vocabulary section
> lists the first as primary. Proposed: swap them.*

A single edit is taste. The same edit three times is a missing rule, and naming it is the
entire job.

### 2. The selection signal — what was rejected, and what was never chosen

Rejections are half the signal and they are free. Look for a *category* that is losing,
not an individual loss:

> *All six product-objective options were rejected this week, all of them problem-led. The
> brief's pain points are thin — three lines, all abstract — so the problem-led mode has
> nothing concrete to build on. Proposed: two paragraphs of specific complaints, in the
> words a contractor would use.*

When one objective's items are consistently passed over, the brief is failing that
objective specifically. That is more useful than an overall acceptance rate.

### 3. The outreach signal — what actually came back

**This is real, and it is the part the MVP genuinely measures.** Contact outcomes are
written by the operator's own hand, so the O3 reply rate is a number rather than an
inference. Read it by channel, and by campaign where one is attached.

Report it plainly, and look for the pattern the editorial view cannot see:

> *Eleven contacts, one reply. Nine of the eleven opened with the same question. The
> opener is doing the work of a template rather than of a qualifying question. Proposed:
> the brief needs two or three genuinely different opening angles, tied to what the person
> actually posted.*

**Count acts, not people, and say so if it matters.** The ledger records recorded acts —
two calls to the same person on one day are two rows — so the denominator is acts. Do not
present it as "people contacted".

### 4. The coverage signal — what the map failed to supply

Thin weeks are usually a source problem, not a scoring problem:

> *Fourteen items scored, three above threshold, and two came from the same site. Four
> sources produced nothing at all this week and two of those have produced nothing for a
> month. Proposed: retire the two, and one gap-directed search for renovator-side
> community sources.*

This hands off to `kaveno-sources`. Do not do the discovery here — name the gap.

## Producing the repairs

End with **three or four proposed edits**, each in this shape:

> **What I saw** — the evidence, with counts. *"Opening line rewritten in 4 of 5 posts."*
> **What I think it means** — the missing rule. *"No tone rule for openings."*
> **The proposed edit** — concrete enough to approve or reject in one word.
> *"Add to Tone: open with the consequence to the reader, not with the news."*

Then ask for a yes or no on each. That is the session.

**Rank by how often the correction recurred**, not by how large the edit would be. The
rule that fires every day is worth more than the one that fires once a month, even if the
second is more interesting to write.

## The honest limit — say it once, do not hide it

**This measures editorial agreement, not published performance.** Nothing here knows
whether a post was read, liked, shared or ignored, because the MVP collects no platform
metrics at all — there is no API for a post made by hand into a trade forum, and
definitively none for a phone call.

So the retro can tell you the drafts are getting closer to what the operator would have
written themselves. It **cannot** tell you they are getting better at reaching anyone. Those
are different claims and conflating them would be the most flattering mistake available
here.

**Outreach is the exception and it is not a small one.** Reply rate is real data. Say which
of your findings rest on it and which rest on editorial agreement, because the operator
should weight them differently.

If asked for reach, followers or engagement: say plainly that platform metrics are not
collected and that measurement arrives with the learning release. Do not substitute a
proxy and do not show a zero.

## When there is nothing to repair

Some weeks the brief is fine. Say so in one line and stop — *"three posts, two edits, both
one-off; nothing recurring. Reply rate 2 of 6, in line with the last three weeks. No
repairs proposed."*

A retro that invents a problem to justify its existence trains the operator to stop
running it, and this skill only works if it runs every week.
