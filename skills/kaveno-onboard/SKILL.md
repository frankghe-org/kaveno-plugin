---
name: kaveno-onboard
description: Build or repair the Kaveno market brief — the positioning, vocabulary, pain points, proof, competitors, tone rules and fit criteria that every piece of Kaveno's output is generated from. Use when setting up a new product or segment in Kaveno, when the brief scores below threshold, when Kaveno reports that an objective is blocked for missing input, or when the operator says the drafts feel generic, off-voice, or like they were written by someone outside the industry.
---

# Kaveno onboarding — building a brief worth generating from

Kaveno's output is bounded by this brief. A thin brief produces drafts that read as though written by someone who has never been on a building site, and no amount of prompting fixes that. Your job is to end this conversation with a brief that passes the gate in §3 — or to end it honestly incomplete, with the operator knowing exactly which objectives are blocked and why.

## How to run this

**Interview, don't survey.** Ask one thing at a time. Forced choices and specific prompts get better answers than open questions: *"Would a contractor say ליקויי בנייה or אי-התאמות when arguing with a developer?"* beats *"what vocabulary do they use?"*

**Draft first, then correct.** Read whatever the operator already has — product docs, the website, past posts, market assessments — and produce a first version of each section. Corrections are far cheaper to give than blank pages to fill, and the correction is the signal worth capturing.

**Budget 60–75 minutes for a first brief, in one sitting if possible.** Say so at the start. If the operator has less time, do segments and vocabulary first; those block the most.

**Write as you go.** Update the brief file after each section rather than at the end, so an interrupted session is not a lost one.

## 1. What you are producing

One markdown file per product. Sections:

- **Product** — what it does, in one paragraph, per segment. These are different products to different people.
- **Segments** — each active segment, described so it cannot be confused with the adjacent one.
- **Vocabulary** — per segment, per language: the term, what it means, when to use it, and when not to.
- **Pains** — per segment, in the practitioner's own words.
- **Proof** — what the product demonstrably does, with quantified outcomes where they exist and an explicit "none yet" where they don't.
- **Competitors** — who, what they claim, where that claim was read.
- **Tone rules and hard bans** — what this operator will never say.
- **Fit criteria** — what makes a professional worth contacting, and what rules one out.
- **Objective targets** — the mix ratio and what success looks like for O1, O2 and O3.

## 2. The interview, section by section

### Segments
Start here; everything is scoped by it. For each segment ask what they are hired to do, who pays them, what they are blamed for, and how they differ from the adjacent segment. **If the operator's answer for two segments is interchangeable, they are not yet two segments** — say so and either merge them or dig until they separate.

### Vocabulary — the section that most often fails
Aim for fifteen terms per segment per language, and be sceptical of the first ten. The test is not whether a term is correct but whether a practitioner would *use* it. Ask directly: *"Which of these would appear in a WhatsApp message between two contractors, and which only in a brochure?"*

Capture near-synonyms and the distinction between them — in Hebrew construction, `ליקויי בנייה` and `אי-התאמות` are not interchangeable and using the wrong one marks the writer as an outsider. Those distinctions are the highest-value entries in the whole brief.

### Pains
Five minimum per segment, phrased as the practitioner would phrase it, not as the product's marketing would. For each, ask **where the operator saw it** — a specific customer, a forum thread, a support ticket. A pain nobody can source is a hypothesis; record it as one.

This section is what makes O2 problem-led content possible, which is the most effective and most sustainable content the product can publish. Do not let it be thin.

### Proof
What can be demonstrated, not what is claimed. Quantified customer outcomes if they exist; **"none yet" is a legitimate and useful answer** — it tells the content engine not to fabricate a proof point, which is precisely the failure it would otherwise produce.

### Competitors
Three minimum, with what each claims and where that claim was read, so comparative content can be checked rather than asserted. Record when it was read — competitive claims age.

### Tone rules and hard bans
Ask for the things that would make the operator wince. Typical: no promises about legal outcomes, never name a developer or a client, no scare-mongering about liability, no claims about what a court would decide. Write them as checkable rules, not as sentiment.

### Fit criteria
What makes a professional worth contacting — size, specialisation, evidence they have the problem — and what rules one out. Include the disqualifiers explicitly; they do more work than the qualifiers.

### Objective targets
The mix ratio across objectives (a sensible default is roughly 70% O1 / 30% O2, with O3 governed separately), and what success looks like for each. Be honest that O1 is not directly measurable in the MVP — the ratio is a discipline, not a measurement.

## 3. The gate — check before you finish

Score each dimension **present/absent**, then **specific/generic**. Report the result plainly, naming which objective each gap blocks.

| Dimension | Passes when | Blocks if missing |
|---|---|---|
| Segments | Each is distinguishable from the adjacent one | Everything |
| Vocabulary | ≥15 terms per segment per language, with usage notes and at least two near-synonym distinctions | O1, O2 |
| Pains | ≥5 per segment, in the practitioner's words, each with a source or marked as a hypothesis | O2 problem-led |
| Proof | Present, or an explicit "none yet" | O2 direct, O2 comparative |
| Competitors | ≥3, with claims and where they were read | O2 comparative |
| Tone rules | Written as checkable rules | Everything |
| Fit criteria | Qualifiers **and** disqualifiers | O3 |
| Objective targets | Mix ratio recorded | The mix report |

**Never quietly produce output from a brief that fails.** Say which dimension is thin, which objective it blocks, and roughly how long it would take to fix. A ten-minute gap the operator knows about is worth more than a month of generic drafts they don't understand the cause of.

## 4. Finishing

Write the brief file, then say in one short paragraph: what passed, what is thin, what is blocked, and what to do next. Hand off to `kaveno-sources` if the brief passes — the source map is the next thing that gates output quality.

Remember the division of labour: **you make the brief good; the server makes the gate real.** Kaveno's tools will refuse the blocked objectives whether or not this skill was used.
