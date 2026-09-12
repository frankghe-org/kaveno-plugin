---
name: onboard
description: Build or repair the Kaveno market brief — the positioning, vocabulary, pain points, proof, competitors, tone rules and fit criteria that every piece of Kaveno's output is generated from. Use when setting up a new product, segment or market in Kaveno, when opening a second market from an existing one, when the readiness gate reports an objective blocked for missing input, or when the operator says the drafts feel generic, off-voice, or like they were written by someone outside the industry.
---

# Kaveno onboarding — building a brief worth generating from

**Say which project you are in, before anything else.** Kaveno holds several products, and every read comes back with the scope it was read in — `scope.product`, `scope.segment`, `scope.market`, `scope.language`, and the `available_products`, `available_segments` and `available_markets` lists it could have been narrowed to. Open with one of those reads — `sources` for the map, `brief` or `brief_readiness` for the brief — and state in one line what it says. Do not carry the project over from the last conversation, and do not infer it from what the operator is talking about. **Check the contract before you act on anything.** That same block carries `contract.version`. These procedures were written against contract **3**. If the number that comes back is **lower** — or the block is absent, which means a server older than contracts altogether — say so plainly and stop: name what you expected, what you found, and that the server needs updating before this skill can be trusted. Do not work around it; a procedure describing a door the server has not built fails at the point of use, with a refusal about something else entirely. A **higher** number is normal and needs no comment.

**When the server says the choice is ambiguous, ask — never pick.** A call that reaches more than one product without saying which comes back refused, with the memberships named: *"you work on 2 products and this call did not say which."* That refusal is the question — put the names in front of the operator and let them answer. On a write it can also arrive per entry, as `segment_ambiguous`, `market_ambiguous` or `market_not_recorded`, naming the candidates the same way; it means the same thing. Choosing for them, or retrying with the first one, is how work lands on the wrong map.

**One project per session.** If the operator moves to another product, say so in a line and restate the scope. Nothing on the server remembers a session: the narrowing travels on every call and each call resolves on its own, so a silent switch is invisible in the transcript and expensive in the data.

Kaveno's output is bounded by this brief. A thin brief produces drafts that read as though written by someone who has never been on a building site, and no amount of prompting fixes that. Your job is to end this conversation with a brief that `brief_readiness` calls ready — or to end it honestly incomplete, with the operator knowing exactly which objectives are blocked and why.

**Resolve the scope before you ask the first question.** Every row you are about to write lands against a product, and most of them against a segment and a market too. The server refuses a write whose product does not resolve, and refuses a segmented entry when the product is pitched to more than one kind of professional and nothing said which. Discovering that at the write, after a sixty-minute interview, is the expensive way to find out. Open `brief_readiness`, read the scope it returns, and settle anything ambiguous before the interview starts.

## How to run this

**Interview, don't survey.** Ask one thing at a time. Forced choices and specific prompts get better answers than open questions: *"Would a contractor say ליקויי בנייה or אי-התאמות when arguing with a developer?"* beats *"what vocabulary do they use?"*

**Draft first, then correct.** Read whatever the operator already has — product docs, the website, past posts, market assessments — and produce a first version of each section. Corrections are far cheaper to give than blank pages to fill, and the correction is the signal worth capturing.

**Budget 60–75 minutes for a first brief, in one sitting if possible.** Say so at the start. If the operator has less time, do segments and vocabulary first; those block the most.

**Record as you go.** Call `brief_record` after each section rather than at the end. Each call is its own save point, so an interrupted session leaves a brief you can read back rather than nothing — and a save point that recorded nothing is never created, so a wholly refused call leaves no gap in the history.

## 1. What you are producing

Rows, recorded through `brief_record`, resolved per product and — where the kind is scoped that way — per segment, per market and per language. There is no file. Each call to `brief_record` takes a list of `entries`, each naming its `kind`:

- **`segment`** — a kind of professional, described so it cannot be confused with the adjacent one. Company-owned, so a segment already recorded for another product is reused by name rather than duplicated.
- **`positioning`** — what the product does, in one paragraph, for one segment. These are different products to different people, which is why this is per segment and not per product.
- **`vocabulary`** — per segment, per language: the `term`, what it `meaning`s, `use_when`, `avoid_when`, and the near-synonym to `contrast_with_term`.
- **`pain`** — per segment, in the practitioner's own words, with a `source_note` or `is_hypothesis` set.
- **`proof`** — the `narrative` of what the product demonstrably does, with quantified outcomes where they exist, or `none_yet` set explicitly where they don't.
- **`competitor`** — `name`, `claim`, `claim_source_url`, `claim_read_at`.
- **`rule`** — tone rules and hard bans: `rule_kind` is `tone` or `hard_ban`, and `rule_text` is what this operator will never say.
- **`fit_criterion`** — `polarity` is `qualifier` or `disqualifier`, and `criterion` is what makes a professional worth contacting, or rules one out.
- **`market_scope`** and **`market`** — the countries and wider scopes this company records against, and which of them this product operates in, in which `languages`.
- **`name_variants`** and **`mix_target`** — how the product's name is written, and the objective mix as `o1_pct`.

Two things about how entries land, both of which change what you say to the operator:

**Field sets are closed.** An entry carrying a field the brief has nowhere to put is refused for having nowhere to put it, rather than silently dropped. If a refusal comes back as `unknown_field`, the answer is not to retry — it is that the thing the operator told you has no home yet, and that is worth saying out loud.

**A correction to brief *content* supersedes; it never overwrites.** That is the five content kinds — `vocabulary`, `pain`, `competitor`, `rule` and `fit_criterion`. To fix one, record a new entry carrying `supersedes: <the old id>`; the corrected wording stays readable as part of the brief as it stood, and `brief(at_revision: N)` reads those kinds back at any earlier save point. Nothing there is ever edited or deleted.

**`supersedes` is refused on the other seven kinds, and six of them overwrite.** A segment's description, a `positioning`, a `market`'s languages, the `proof` narrative, `name_variants` and the `mix_target` are updated in place — offering `supersedes` there comes back `unknown_field`, because the field is not in that kind's set. The seventh, `market_scope`, is **insert-only**: re-recording a code that exists comes back `already_recorded` rather than replacing it, so a scope's name cannot be corrected through this door at all. Two consequences worth telling the operator: correcting one of these **loses the previous wording**, and reading at an earlier save point will not show it as it stood, because these are read from current state rather than from the revision history.

## 2. The interview, section by section

### Segments
Start here; almost everything is scoped by it. For each segment ask what they are hired to do, who pays them, what they are blamed for, and how they differ from the adjacent segment. **If the operator's answer for two segments is interchangeable, they are not yet two segments** — say so and either merge them or dig until they separate. Record the segment, then its `positioning` for this product.

### Vocabulary — the section that most often fails
Aim for fifteen terms per segment per language, and be sceptical of the first ten. The test is not whether a term is correct but whether a practitioner would *use* it. Ask directly: *"Which of these would appear in a WhatsApp message between two contractors, and which only in a brochure?"*

Capture near-synonyms and the distinction between them — in Hebrew construction, `ליקויי בנייה` and `אי-התאמות` are not interchangeable and using the wrong one marks the writer as an outsider. Those distinctions are the highest-value entries in the whole brief, and `contrast_with_term` is where they go.

### Pains
Five minimum per segment, phrased as the practitioner would phrase it, not as the product's marketing would. For each, ask **where the operator saw it** — a specific customer, a forum thread, a support ticket. That goes in `source_note`. A pain nobody can source is a hypothesis; record it as one, with `is_hypothesis` set. It still counts toward the five — an operator-entered guess is something he has stood behind (`Q-BRIEF-1`, 6 September 2026). What does not count is material the system derived and he has not confirmed.

This section is what makes problem-led content possible, which is the most effective and most sustainable content the product can publish. Do not let it be thin.

### Proof
What can be demonstrated, not what is claimed. Quantified customer outcomes if they exist; **"none yet" is a legitimate and useful answer** — it tells the content engine not to fabricate a proof point, which is precisely the failure it would otherwise produce. Record it as `none_yet` rather than leaving proof unanswered.

**Be straight with the operator about what that costs, though.** A declared `none_yet` does **not** satisfy the gate: proof comes back `blocked` either way, and the product objective stays blocked with it. What the declaration buys is a *different reason* — the gate says *"you recorded that there are no customer results yet, so nothing that leans on proof can be generated. This is an answer, not a gap"*, rather than *"nothing has been recorded either way"*. The two need different things from the operator, which is why they are distinguished; neither is a pass.

### Competitors
Three minimum, with what each claims and where that claim was read, so comparative content can be checked rather than asserted. Record `claim_read_at` — competitive claims age.

### Tone rules and hard bans
Ask for the things that would make the operator wince. Typical: no promises about legal outcomes, never name a developer or a client, no scare-mongering about liability, no claims about what a court would decide. Write them as checkable rules, not as sentiment.

A rule the operator holds *everywhere* rather than in this market is worth catching here rather than recording once per country. Record it, then move it up with `brief_promote` — see §4.

### Fit criteria
What makes a professional worth contacting — size, specialisation, evidence they have the problem — and what rules one out. Include the disqualifiers explicitly; they do more work than the qualifiers, and the gate requires both polarities before outreach is unblocked.

### Objective targets
The mix ratio across objectives — record it as `mix_target.o1_pct`; a sensible default is roughly 70% authority-building content, with the rest product-led and outreach governed separately. Be honest that authority-building is not directly measurable in the MVP: the ratio is a discipline, not a measurement. Say *authority*, *product* and *outreach* to the operator, never `O1`/`O2`/`O3` — the codes are internal, and the server reports the words.

There is nowhere to record *what success looks like* for each objective. That stays in the conversation; do not promise the operator it has been captured.

## 3. The gate — `brief_readiness` decides, you report

Call `brief_readiness` and report what it says. **Do not score the brief yourself.** It counts rows across the resolved set — what applies here, not only what is attached here — for every market the product operates in, including one with nothing recorded, per segment and per language.

What comes back:

- **`verdict`** — `ready` or `blocked`, and never a third value. **A pain counts toward the threshold when the operator has stood behind it** (decided 6 September 2026, `Q-BRIEF-1`): by sourcing the entry, by entering a guess as a guess, or by confirming one the system derived. What the system derived and he has not confirmed does not count. The test is **provenance, not certainty** — do not re-count anything yourself, and do not treat an entry as weaker because it carries no source.
  The pain `detail` still **splits the count**, and the split is worth relaying: *"five of five, and two of them are guesses"* tells the operator something the bare five does not. Word the second number as **marked as a guess** — the split is on that flag alone. It is **not** "the ones he typed himself": a guess the system proposed and he confirmed lands in it, which is precisely the case the rule admits. Nor is it "the unsourced ones" — an unsourced pain must be admitted as a guess, but a guess may also carry a source. `skills/publish/SKILL.md` §26 quotes this sentence to the operator verbatim, so getting it wrong tells him he wrote entries he only signed off.
- **`checks[]`** — one per dimension, each carrying `dimension`, `verdict`, a `detail` sentence written to be read aloud, the `market`, `segment` and `language` it was counted in, `counted`, `threshold`, `excluded_unconfirmed`, and `blocks`.
- **`blocked_objectives[]`** — each objective that is blocked, and the reasons, in the operator's words: **authority**, **product**, **outreach**.

Report the `detail` sentences as they come. `"0 of 15 he terms for architects count"` is more useful than any paraphrase, and `excluded_unconfirmed` matters: a segment whose entries are all unconfirmed derivations is reported as unready *with a reason that says so*, so that you never send the operator to write content that already exists and only wants reading.

For orientation while you interview — **these are the server's numbers, not yours to apply**:

| Dimension | The server counts | Blocks |
|---|---|---|
| Segments | The product is pitched to at least one kind of professional | Everything |
| Segment descriptions | Each reported segment has one | Everything |
| Markets | The product records at least one market it operates in | Everything |
| Vocabulary | ≥15 terms per segment, per market, per language | authority, product |
| Pains | ≥5 per segment, per market | product |
| Proof | **A recorded narrative.** A declared "none yet" is a *distinct blocked state*, not a pass | product |
| Competitors | ≥3 per market | product |
| Tone rules | At least one per market | Everything |
| Fit criteria | Qualifiers **and** disqualifiers | outreach |
| Mix target | Recorded | Nothing — reported anyway |

**One judgement stays yours: specific versus generic.** Fifteen terms that any brochure could have produced pass the count and fail the brief. The server counts rows and cannot read them; nothing else in the system will say so, so you must. The same goes for two segment descriptions that are interchangeable, and for a pain written in the product's voice rather than the practitioner's.

**Never quietly produce output from a brief the gate calls blocked.** Say which dimension is thin, which objective it blocks, and roughly how long it would take to fix. A ten-minute gap the operator knows about is worth more than a month of generic drafts they don't understand the cause of.

## 4. Opening a second market or segment — the export interview

This is a different conversation from the cold one above, and it is shorter. When a product already has a brief in one market and is opening another, the job is not to ask for everything again — it is to walk the operator through confirming what carries over.

The sequence, all through tools:

1. **Record the place.** `brief_record` with a `market_scope` entry for each new scope — a country, and any wider scope it sits inside. An entry that names no `parent` lands under the existing root rather than starting a second one.
2. **Record that the product operates there.** `brief_record` with a `market` entry naming the `market_scope` and the `languages`. For a new *segment* instead, record the `segment` and then its `positioning`.
3. **Export.** `brief_derive`, naming `from_market` and whichever of `to_market`, `to_segment` and `to_language` apply — **and, for every axis you are moving, the `from_` side of it too.** Moving into another language without `from_language` is refused outright (`language_move_needs_a_source`): without it, every language's terms would be copied into one, which is not a re-finding of anything.

   **The same pairing matters just as much for `from_segment`, and here nothing enforces it.** Naming a `to_segment` with no `from_segment` is **accepted**, and it copies **every** segment's vocabulary, pains and fit criteria into the one target segment — the silent merge, arriving by a different door. Pair them yourself; the server will not stop you.

   What comes back are **proposals**: every entry marked derived, recording what it came from, and **not counted by the gate until a human confirms it**. Derived entries are usable in drafting immediately — they are simply not evidence that the brief is ready.
4. **What is not copied.** Anything that already reaches the target through the scope tree. A rule recorded for the world already applies in France and is skipped; a rule recorded for the Middle East is carried, because France is not under it.
5. **Nothing is translated.** A derived vocabulary entry lands under the target language carrying the source language's words. Say this plainly at the start of the walkthrough — the operator is about to see Hebrew terms filed under French, and that is the mechanism working, not a bug. Re-finding the equivalent insider term *is* what confirming means.
6. **Review together.** Read the target back with `brief`. Derived entries carry `origin: "derived"`, `derived_from`, and a null `confirmed_at`, and render marked `[derived, unconfirmed]`. There is no tool that lists only the unconfirmed ones, so filter the lists yourself.
7. **Confirm what the operator can actually judge**, with `brief_confirm`. Recorded once, ever. **A wrongly confirmed term is indistinguishable from one somebody in that trade wrote** — so do not confirm in bulk to clear the list, and do not confirm anything the operator hedged on. An unconfirmed entry is a known gap; a wrongly confirmed one is an invisible defect.
8. **Where the equivalent differs**, do not confirm — record the real term with `brief_record`, carrying `supersedes: <the derived id>`. It lands as authored content, which is what it is.

**Widening rather than exporting.** When the operator says *"that ban isn't an Israeli thing, we hold it everywhere"*, that is `brief_promote`, not a second recording: it moves the entry to a wider scope as supersede-and-create in one save point, so the history shows the promotion happened and the entry keeps whatever standing it had. Moving sideways — one country to another — is refused and pointed back at `brief_derive`. Catching these during the interview is what stops the same ban being recorded once per country.

## 5. Finishing

Say in one short paragraph: what passed, what is thin, what is blocked, and what to do next — sourced from `brief_readiness`, not from your own reading of the conversation. Name the save point, so the operator can ask for the brief as it stood today. Hand off to the `sources` skill if the gate passes — the source map is the next thing that gates output quality.

Remember the division of labour: **you make the brief good; the server makes the gate real.** That promise was false while the brief was a file and nobody could count it. It is true now wherever the gate is reachable — and it is not yet reachable from the daily loop, because the tools that would consult it there are not built. So say what the gate says, and do not imply a refusal further downstream than the server can actually make one.
