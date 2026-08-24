---
name: kaveno-reachout
description: Run the weekly Kaveno contact review and prepare a private first message to one specific professional — reading the evidence, judging fit, shaping the qualifying question, and driving the assisted send up to the point of the operator's click. Use when the operator asks who is worth contacting this week, wants to reach out to or contact a prospect, wants to send a DM or private message to someone they saw posting, or is making first contact with an individual professional.
---

# Kaveno reachout — one message, to one person, that they might answer

This is O3: **one-to-one, product-centred, individually qualified.** Everything here is sized for one person at a time — one architect who posted something specific, one contractor whose listing says they do handovers. Roughly twenty a week, in Hebrew, by a founder who has no team. Kaveno is not a bulk tool; it removes the mechanical work from personalised contact and nothing else.

**What this skill refuses.** A person who has not passed both gates. A draft that pitches. A run where the operator would be typing the message themselves. Anything that routes around a refusal from the server.

## The gates — the part that is not yours to decide

Both gates apply to O3, and **only** to O3. The server has already evaluated them before a candidate reaches you; the `contacts` tool re-checks suppression at read time. Your job is to read the result and act on it, never to reason your way past it.

| | Question | What must be true | Israel, concretely |
|---|---|---|---|
| **Gate 1** | May we hold and use these details? | A recorded `source_provenance` and `lawful_basis` on the `person` row, and the details taken from a **published business identity** — public register, association directory, the practice's own site, a Google Business listing | `lawful_basis = published_business_contact` on essentially every row |
| **Gate 2** | May we use this channel, in this market? | The channel is open in the recipient's market, and suppression and rate limits are clear before anything is drafted | Phone: screened against the national do-not-call registry. Email: consent-only, so **not offered**. WhatsApp: prior opt-in only |

**Contact details never come from a group's member list.** Same digits, different status: they were shared for participating in that community, and using them for a business approach fails purpose limitation. A group post is a *discovery signal*, nothing more — `flag` it with `kind='discovery'` and the Sunday sweep resolves the person to a published identity or does not.

**When a person fails Gate 1, the correct move is to reply to them publicly, in the thread where you saw them.** Say that out loud rather than reporting a dead end. It is a real action, it is free, and it serves O1 at the same time. `contacts` returns the count in `skipped` precisely so this conversation can happen.

**Never work around a refusal.** Not by suggesting another channel, not by finding the number elsewhere, not by asking the operator to supply it. If the server declined, the answer to the operator is what was declined and why.

## 1. Running the weekly review

Call `contacts` (limit 5 by default; up to 10). For each candidate, put in front of the operator, in this order:

1. **Who** — profession, city, practice size, register number if there is one.
2. **Why them** — the one line of specific evidence. This is the whole value of the list.
3. **Provenance** — the link, named. The operator clicks it before dialling; at five a week that click is the verifier the MVP does not otherwise have.
4. **Channel** — which one, why it is open, and whether the registry screen came back clear.
5. **The opener** — finished, in Hebrew, ready to read.

**What a good "why them" line is.** Dated, sourced, and about a problem the product addresses: *"posted in the architects' forum on 12 Aug asking how others document defects at handover."* What it is not: *"works in construction", "seems relevant", "6-person practice in Tel Aviv."* Size and location are qualifiers, not evidence. **If you cannot write the why-them line from what is on the row, the candidate is not ready — say so rather than padding it.**

**When to skip.** Too small to have the problem. Wrong side of the transaction. Evidence older than a couple of months, unless it was a considered post rather than a passing comment. No published contact — reply in the thread instead. Any doubt about whether the identity and the business are the same person: `identity_evidence_url` is what settles that, and if it is thin, skip.

The operator's verdicts go back through `flag`: `not_interested` for a no (it never resurfaces), `suppress` for someone who asks not to be contacted (it fans out to every identifier that person is known by, and survives deletion of the row).

## 2. Shaping the qualifying question

**A qualifying question, not a pitch.** You are asking whether this person is the right person and whether the subject is worth two minutes. You are not asking for a meeting, a demo or a call — a first message that asks for a meeting is wrong and should be rewritten, including when the operator wrote it that way.

Four parts, in order, and all four arrive finished:

- **Greeting** with the person's name.
- **The specific line** — *"I saw your post about X"*, with the actual X and the actual date.
- **The provenance sentence** — where the interest came from and where the details came from. **This is not optional**; it doubles as the Article 14 notice, and it must survive any editing the operator does. If they cut it, put it back and say why.
- **The question** — one sentence, answerable with yes or no.

Under 60 words. No product name unless the answer is yes. No claim about legal outcomes, no named developer, no liability scare — the brief's hard bans apply here exactly as they do to published content.

> **Worked example — architect, Ramat Gan, discovered in the architects' forum**
>
> Why them: *posted in the architects' forum on 12 Aug asking how others document אי-התאמות at מסירה so the record holds up against the developer.*
> Channel: phone, published on the practice's own site, screened against the registry — clear. (Email would be closed here: consent-only in Israel.)
>
> **Opener (he):**
> *שלום דנה, ראיתי את הפוסט שלך בפורום האדריכלים מה-12.8 על תיעוד אי-התאמות במסירה. הגעתי אלייך דרך הפרטים המפורסמים באתר המשרד. את מי שמטפלת בליקויים אחרי מסירה מול היזם — ושווה לך שתי דקות על זה?*
>
> Note `אי-התאמות` rather than `ליקויי בנייה` — she is arguing against a developer at handover, and the wrong one of the two marks the sender as an outsider. That distinction comes from the brief's vocabulary section; if it is not there, drafting in Hebrew is blocked and you should say so instead of guessing.

## 3. The assisted send

**The body arrives finished.** Greeting, the specific line, the provenance sentence and the question are all rendered server-side, bidi marks stripped. **If the operator is typing "שלום דנה" into a composer, the pipeline failed upstream** — stop and fix that rather than helping them type.

**Where the browser is not needed at all.** Email is a `mailto:` link with subject and body pre-filled — no automation, no platform ToS surface, opens their real client on their real phone. Phone is a `tel:` link. **Only an in-platform DM needs the browser.**

For a DM, use the browser assist contract as it is defined — this skill consumes it, it does not redefine it. In order:

1. **Resolve** the person's stored `identity_url` to a platform compose deep-link, so **no searching and no disambiguation is ever needed.** You should never be looking at a search results page deciding which Dana this is.
2. **Navigate** to that link.
3. **Wait** for the composer to be present and focused.
4. **Insert via clipboard paste — never synthetic keystrokes.** Hebrew is RTL and the body carries bidi context; character-by-character typing mangles it, and the operator will not always see that it did.
5. **Read the composer back and compare it, character for character, to what was sent. On any mismatch, abort and hand over** — the deep link plus the finished text. Do not attempt a correction in the field.
6. **Screenshot**, then **stop.**

The operator reads it and presses send.

**Batch mode, for a twenty-person run.** Prepare them in sequence so the operator moves through the whole run clicking send in one context, rather than switching between reviewing and preparing. Preparation is still one person at a time; only the operator's attention is batched.

**The final click.** Default and expectation: the operator presses send. That click is where verification actually happens and where failure is loudest — a person notices a wrong message before sending it. **An unverified auto-send fails silently, which is the worst available outcome.** Auto-send after a verified insert is mechanically possible, and it is worth saying so plainly rather than pretending otherwise; but it crosses the line the requirements' operating model draws — *"the assistant does everything except the send"* — so switching it on is **a change to a requirement, not a setting to toggle.** If the operator asks for it, that is a decision to record, not a flag to flip.

## 4. Failure and degradation

**The assist is an accelerator, never a dependency.** The extension carries an undocumented server-side domain blocklist that can change without notice, and platform UIs change without warning. Both are outside anyone's control here.

| What happened | What you do |
|---|---|
| Extension unavailable, domain blocked, page not what was expected | Hand over the deep link and the finished text. Same data, slower path, nothing blocked |
| Composer not found, or found and not focusable | Hand over. Do not hunt for a different selector |
| Read-back mismatch | Abort that person, hand over, and say what differed |
| A gate refusal | Report the refusal and the reason. For Gate 1, offer the public reply in the thread |

**Never retry blindly.** One clean attempt, then degrade and say what happened. A silent second attempt against a changed UI is how a half-written message ends up in a stranger's inbox.

## 5. What never happens here

- **No bulk.** No list send, no templated run, no personalisation by mail-merge. Twenty a week means twenty decisions.
- **No unverified provenance.** No detail from a member list, a scraped export or the operator's memory of where they saw it.
- **No pitch on first contact.** No meeting request, no demo link, no attachment.
- **No route around a gate**, and no second opinion sought on a refusal.
- **No send by Kaveno**, and no component in this path that could send.
- **No second account.** One authentic identity per operator; a second voice is astroturfing and loses both accounts at once.

## 6. Hand-off

Close the session with: how many were reviewed, how many were prepared, how many were skipped and why, and anything that failed to a hand-over. Log the outcomes through `flag`. Replies logged the following week are the only directly measurable signal any objective produces in the MVP, so they are worth the thirty seconds.

If the openers needed heavy editing, that is a brief problem, not a drafting problem — carry it to `kaveno-retro`, which is where the vocabulary, pains and fit criteria get repaired. If fit criteria are thin, say so and hand back to `kaveno-onboard`; O3 is the objective they block.

**The division of labour, restated because it is the point: you make the message worth answering; the server makes the gates real.** The tools refuse blocked contacts whether or not this skill was loaded.
