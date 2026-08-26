---
name: sources
description: Find, judge and record the sources and groups Kaveno reads — trade media, associations, regulators, forums, communities and statistics, per segment and market. Use when setting up the source map for a new product, segment or market, when extending or filling gaps in an existing one, when adding a group, forum or feed, when a group's self-promotion rules need capturing, when the daily view is thin or repetitive, or when a source has stopped yielding.
---

# Kaveno sources — building the map Kaveno reads from

Everything Kaveno proposes comes from this list. A thin map produces a daily view of the same three news sites saying the same thing, which the operator stops opening within a fortnight. Your job is to end each run with the map measurably less thin in a place you can name.

This is the one part of the system that **cannot be written as a pipeline**, because the control flow does not exist in advance: the second query depends on the results of the first, a lead runs from an association page to a forum nobody would have guessed at, four candidates are discarded and the fifth pursued. That is why it runs here, interactively, with the operator (`02_functional_architecture.md` §6.4).

The operator is a technical founder running Fixeet (construction defect management) alone, targeting architects and renovation contractors in Israel, in Hebrew. France and the UK come later, and the map must be built so that adding them is rows, not code.

## 1. State first, always

**Read what already exists before searching for anything.** The first action of every run is `sources()` — no arguments, one call, no exceptions. It takes no arguments **because it cannot**: the company and the operator are derived from the bearer token, never passed, so there is nothing to scope and no way to ask for someone else's map.

**If `sources` is not among your tools, say why in one sentence and stop.** Do not
search, do not propose, and do not reason aloud about what the map might hold — a run
that cannot deduplicate produces exactly the re-proposed sources this skill exists to
prevent.

The cause is almost never the plugin, and guessing at it wastes the operator's day. On
Cowork and the desktop app the server needs **two** things that the plugin cannot do for
itself: the host allowlisted for network access, and the server added as a connector
under **Customize → Connectors** *and* switched on for this chat from the `+` menu.
Either one missing looks identical to a plugin that never installed. Point at
`SETUP.md`, name those two, and let the operator check them — do not speculate about
expired credentials or a server being down before those are ruled out, because both are
rarer and both send him somewhere else.

It returns the map whole, in two lists:

**`sources[]`** — `id`, `name`, `url`, `source_type`, `segment`, `market`, `objectives_served`, `purpose`, `access_path`, `access_method`, `relevance_score`, `qualification_rationale`, `permission_check` (`passed` and `checked_at`), and **`last_yield_at`**.

**`groups[]`** — `id`, `name`, `provider`, `url`, `segment`, `market`, `access`, `qualification_rationale`, the three `rules` read verbatim (`self_promotion`, `admission`, `private_message`) with `rules_url` and `read_at`, and three flags computed from them: **`rules_read`**, **`offered_as_publication_target`**, **`offered_as_contact_origin`**.

Four things in that response do more work than the rest, and a run that ignores them has not really read the map:

- **`last_yield_at` is null, not zero, when a source has never yielded.** That is a different fact from a low score and it is the one §6 acts on. Do not collapse the two.
- **`objectives_served` comes back as words** — `authority`, `product`, `outreach` — and never as `O1`/`O2`/`O3`. Use the same words when you submit; the codes are refused as arguments as firmly as they are withheld from answers.
- **The three `rules` flags are the server's verdict, not a suggestion.** A group with `rules_read: false` is recorded and *not offered* — not as a publication target, not as an origin for contacts. Reading its rules is the highest-value five minutes available in a run, and the map tells you exactly which groups are waiting on it.
- **`permission_check` is what a source's fetchability rests on.** It reads back as `passed` / `checked_at`; it is submitted as `robots_ok` / `robots_checked_at` (§5). The two names are the same fact seen from either side of the door.

**Never start from a blank slate and never re-propose something already recorded.** Re-proposing a source the operator judged six weeks ago is the fastest way to teach him that this skill does not remember, and he will be right. Deduplicate against what `sources()` returned **before** proposing anything, and say what you found: *"the map holds nineteen sources and six groups; four groups have unread rules; two sources have never yielded."* On a first run the state is empty and this costs one call; on every run after that it **is** the work.

**Say what you found, then say which gap you are addressing.** The two sentences are the whole contract between this section and the next.

## 2. Then work by coverage gap, not by search volume

Compute what is thin before you search, and **say which gap each search is addressing.** Five questions:

| Question | What a gap looks like |
|---|---|
| Which `(segment × market)` pairs are thin? | Architects × IL has eleven sources, renovators × IL has two |
| Which objectives are unserved? | A map full of news sites feeds O1 and does nothing for **O2 problem-led** — the most effective and most sustainable content the product has |
| Which **source types** are missing? | Trade media · association or trade body · regulator or standards body · forum · community (Facebook, Discord, Telegram, WhatsApp) · statistics. A map with no association and no regulator is a map of other people's opinions |
| Which sources have stopped yielding? | `last_yield_at` is old, or **null** — never yielded at all, which is a different fact from a low score. Possibly dead — §6 |
| Which groups have never had their rules read? | `rules_read: false` — the group is recorded but **not offered**, as a publication target or as an origin for contacts. Absence of a rule means *not read*, never *no rule* |

`segment` is singular on a `source` row, so a site serving both architects and renovators is **two rows**, not one with two labels. Count coverage that way too, or the map looks fuller than it is.

## 3. The search strategies, ranked by what actually worked

This ranking is empirical — roughly fifty search and fetch operations across Hebrew, French and English, 58 communities found. Treat it as hard-won, not as a starting hypothesis.

- **Best: `site:facebook.com/groups` plus native-language trade terms.** Worked in all three languages; returned 8–10 real group URLs per query, essentially every time. This is the industrialisable one. Start here.
- **Excellent yield per page: association and trade-body sites.** One fetch can return a dozen groups with the association's own rules attached — but only where the pattern exists. It exists in Israel; it does not in France, where the trade bodies route everything to LinkedIn company pages.
- **Solid: forum directories.** High yield in one fetch, with follower metrics. They also produce false positives that only reading catches: one directory's "Top 10 Construction Forums" ranked a thriving 31,000-member forum at #3 — **for IT contractors**.
- **Failed: `site:linkedin.com/groups`.** Returned company pages and nothing else. LinkedIn Groups are effectively invisible to open-web search. Do not plan discovery around them; if the operator wants them, that is a Path B reading job, not a discovery one.
- **Failed: generic WhatsApp link-aggregator sites.** Near-total SEO chaff — one claiming "870+ architecture groups" had about twenty, in a deprecated URL format, last touched a year earlier.
- **Failed: `"chat.whatsapp.com"` plus French trade terms.** Zero relevant results across multiple phrasings. Hebrew is the only language where WhatsApp-link search returned anything at all.

Hebrew is the easiest market and French the hardest — the opposite of the intuition, and worth saying out loud before the French expansion, not after.

## 4. Qualifying a candidate — this is the product

Finding candidates is close to commoditised. Deciding whether a group is forty Israeli renovation contractors arguing about defect liability or homeowners fishing for quotes requires reading and understanding, and **that is where the operator's attention should go.** Assume noise: one English-language query returned groups for Kolkata, the Philippines and Bangladesh before anything in the target market, and two page-one UK trade forums had five and seven members respectively. **Search ranking is anti-correlated with liveness.**

Three checks, in order, on every candidate:

1. **Robots and terms, before it is proposed** — not after it is adopted. A source that fails is recorded unfetchable and never quietly retried.
2. **Liveness** — is anyone there, and recently. What you can know varies enormously and the skill must be honest about which:

| Platform | Name | Members | Activity | Rankable? |
|---|---|---|---|---|
| Forums | Yes | Yes | Yes — exact dates and post counts | **Excellent** |
| Discord | Yes | **Exact**, via the official unauthenticated invite endpoint (`?with_counts=true`) | Partial — presence count | **Good** |
| Facebook | Yes | **No** — `robots.txt` blocks `/groups/` | Weak proxies only; permalink-ID magnitude is the one real volume signal | **Poor** |
| Telegram | Name only | No | No | **None** |
| WhatsApp | No | No | No | **None** |

   **Record what is knowable and mark the rest unknown. Never guess a member count into a field.**

3. **Relevance** — is this population the segment, in the market, in the language, and can it feed an objective the map is short of. Score it and write the reasoning down; the reasoning is what the later unattended version will be built from.

### Worked example — one Hebrew query, three outcomes

> `site:facebook.com/groups` + `שיפוצניקים` + `ליקויי בנייה`

> **Kept — `facebook.com/groups/israeli.architects`.** Hebrew-named, on-segment, and the permalink IDs run above 4.2 million against a few hundred thousand in the neighbouring groups: a crude but real volume signal, and the only one Facebook gives. Recorded as `segment = architects`, `market = IL`, `objectives_served = ['authority']`, `access_path = operator_session` (nothing server-side can read it), rules **unread**, therefore **monitor-only** until he reads them.

> **Rejected — a `דרושים`-style renovation group.** Nominally the right trade; the visible post previews are homeowners requesting quotes and contractors bidding for them. That is the wrong population twice over — O1 authority spent on people whose opinion of him carries no weight with his buyers, and O2 problem-led content aimed at people who do not have the defect-liability problem. Rejected on audience, not on size.

> **Neither — `prog.co.il` architecture and interior design forum.** Genuinely relevant, and returns 403 to an automated fetch. Not rejected and not adopted: recorded with `access_method = 'manual'`, `robots_ok` unset, out of the fetch query, with a note saying why. A source that fails the check is **recorded as unfetchable, never quietly retried.**

## 5. What gets recorded

Per candidate, and none of it is optional:

- **name, URL, `source_type`, `segment`, `market`** — one row per segment;
- **`objectives_served`** — a non-empty array, **in words**: `authority`, `product`, `outreach`. Submitting `O1` is refused (`value_not_in_enum`), which is the same rule that keeps the codes out of the answer. If you cannot name an objective it feeds, it is not a source, it is a bookmark;
- **`access_path`** — `server` for anything fetchable logged-out; `operator_session` for anything visible only to a logged-in member. Submit it once and only once: `requires_login` is **derived** from it server-side, so there is no second field to keep in agreement and no way to record the two disagreeing. The scheduler never touches an `operator_session` row;
- **whether a feed, API or structured markup exists** — that decides tier 1 versus tier 2 and nothing else in the map is cheaper to check;
- **`relevance_score` with the reasoning**, in the operator's terms;
- **`robots_checked_at` and `robots_ok`.** A source is born `access_method = 'manual'` and is promoted only after the check passes. The database enforces the ordering; do not try to record it the other way round.

**For groups, the rules-reading step — and it is three rules, not one.** `self_promo_policy` (what the group allows in the way of self-promotion), `admission_policy` (will this group admit a vendor to the profession rather than a member of it) and `dm_policy` (does it prohibit messaging members privately) are the group's own rules, **read once by a human and recorded verbatim**, with `rules_url` and `rules_read_at` as evidence. A rule that is **absent means not read** — never *no rule* — and until all three plus the reading date are recorded the group comes back `rules_read: false`, which means **monitor-only: it is offered neither as a publication target nor as an origin for contacts, by any skill.** Say this to the operator as what it is — five minutes per group, once, that unblocks every future reply into that group. Guessing a group's rules from its name is how the account is lost.

**The candidate schema is closed, and that is the enforcement.** A candidate carries `kind` (`source` or `group`), `name`, `url`, `segment`, `market` and `qualification_rationale`, plus — for a source — `source_type`, `objectives_served`, `purpose`, `access_path`, `access_method`, `relevance_score`, `robots_ok`, `robots_checked_at`, `admission`; or — for a group — `provider`, `access`, `self_promo_policy`, `admission_policy`, `dm_policy`, `rules_url`, `rules_read_at`. **Any other key is refused** with `unknown_field`, whatever it holds. That is not a validation nicety: it is why a submission enumerating members is refused for having nowhere to land, rather than by anyone trying to recognise one.

**Never record members, and never record admin identities.** Group name, URL, description, member count and privacy setting are not personal data; admin names and profile URLs are. There is no member table and no person-to-group edge, deliberately — a schema shaped like a roster is an invitation to fill one. Surface a live link to the public page instead of storing a person.

### How it lands — three paths, and choosing between them is the job

Three tools sit close enough together to be confused, and **only one of them records a source**. Reach for the nearest rather than the right one and you will either lose the judgement you just did, record a judgement nobody made, or file a source as though it were an item.

| What you are holding | Path | Why this one |
|---|---|---|
| **A candidate you have judged** — score, reasoning, objectives, segment, market, permission check | **`sources_add`** | The only door into the map. It is what turns a judgement into a row |
| **A bare URL nobody has judged yet** | **`flag(kind='source_candidate', url=…)`** | Objective-neutral parking. *"The URL is stored, never fetched in-session"* — the sweeper fetches it later, elsewhere, in a process holding no tools |
| **An item read inside the operator's own logged-in session** | **`capture`** | That is an `info` item from a group, not a source. Derived items only, capped per sweep, never unattended |

**The distinction that actually gets confused is the first two.** `flag` is not a lightweight `sources_add`; it is the **absence** of a judgement, recorded honestly. Flagging a candidate you have already qualified throws away the reasoning, the score and the permission check, and leaves the operator to do the work again from a bare URL. Submitting an unqualified URL through `sources_add` is the opposite error and the server refuses it — with no `qualification_rationale`, no `objectives_served` and no recorded permission check, it is refused three times over. **If you did the work, `sources_add` it. If you did not, `flag` it and say so.**

**Which of the three doors is open today, stated plainly.** The server registers **three** tools — `sources`, `sources_add` and `flag`. `capture` is specified (`02_functional_architecture.md` §6A) and is **not yet built**, so a page the operator puts in front of you still has nowhere to go but the conversation. **Do not invent a call.** A skill that names a tool the server does not expose is worse than one that says the gap out loud — the operator finds out at the point of failure instead of at the point of planning.

**`flag` implements one of its four kinds.** `kind='source_candidate'` works. `discovery`, `not_interested` and `suppress` are specified and refused as *not built yet* — they belong to the O3 outreach loop, whose person resolution and suppression paths do not exist. The refusal says so and names what is available; it is not a rejection of what you asked for.

**Parking is for the pile you did not judge, and only that.** Reviewing a sweep produces three outcomes: approved → `sources_add`; rejected → nothing, correctly; undecided → `flag`. Keep those apart, because the two mistakes are opposite and both are silent:

- **Never park a candidate you HAVE judged.** Parking throws the judgement away — the objectives, the score, the permission check and the reasoning all have nowhere to go on a parked row, by design. If you have judged it, it goes through `sources_add`.
- **Never route an unjudged URL through `sources_add` to get it recorded.** It is refused three times over (no reasoning, no objectives, no permission check), and that refusal is correct.

A parked address takes a `url` and an optional `note` (≤500 characters) and nothing else. Giving it a `subject_id` is refused: naming who it is about is itself a judgement.

**Read the parked pile before proposing anything.** `sources` returns it under `parked`, alongside `sources` and `groups`, precisely so a sweep does not re-find what the operator already declined to decide last time. Dedup your proposals against **all three** — a candidate already parked should be surfaced as *"you kept this on 3 August, still undecided?"*, not proposed as new. A parked address is **not** in the map: it has no segment, no market, no objectives and no rationale, and nothing in the pipeline will ever read it.

**`sources_add` answers per candidate, and the answer is worth reading.** A submission of twenty comes back as twenty results, each `created`, `enriched`, `duplicate` or `refused`, plus counts. One candidate's refusal never discards another's acceptance.

- **`created`** — a genuinely new address.
- **`enriched`** — already held, and your resubmission filled a blank. This is the normal, wanted outcome of re-verifying a stale row (§6), not a near-miss.
- **`duplicate`** — already held, nothing new offered. Your dedup against `sources()` should have caught this; a `duplicate` count above zero is a signal about **this run**, not about the map.
- **`refused`** — with a `reason` and a `detail` that say what it was refused *for*. Read it and fix the candidate; do not resubmit the same shape hoping for a different verdict.

**A resubmission may complete a record; it may never rewrite one.** Five rules, and the fifth is the one that makes §6 possible:

1. Missing data is **filled in** rather than discarded — that is the whole point of resubmitting.
2. A value already recorded is **not replaced**.
3. A recorded judgement, or text quoted verbatim, is **never overwritten** once it is there.
4. A classification only **widens**, never narrows — `objectives_served` grows, `purpose` widens.
5. **A re-measurable finding may move — but only with evidence dated later than what it replaces.** The permission check is the case that matters: a fresh `robots_ok` with a newer `robots_checked_at` is accepted, and one dated no later than the recorded check is refused.

And underneath all five: what a source *is* — its name, address, type, segment, market — **cannot be moved at all**, by any resubmission, ever.

**So re-verification and retirement both land here, through `sources_add`, and nowhere else.** Re-running a permission check is rule 5. Retiring a failing source by setting `access_method` back to `manual` (§6) is permitted outright — demotion always is; it is *promotion* off `manual` that demands a passing check dated later than the last. What this tool cannot do is correct a mistake: that is a conversation with the operator, not a resubmission.

**Where the candidate came from does not matter, and that is the design.** A source typed by the operator, one surfaced by a search in this session, and one read off a logged-in page by the Claude browser extension all enter through the same `sources_add` and all come back through the same `sources()`. **The extension never talks to the server; Cowork does** — the extension holds no Kaveno credential and no token of any kind, it only reads pages the server cannot reach, and this session is what writes what it found. So *"the skill and the server work together to retrieve what was collected in Cowork or the extension"* needs no integration work and no second path: it is the single-door property, already true. The map does not record which surface found a row, and it should not — that is provenance about the operator's tooling, not about the source.

**This says where a candidate may have come from; it does not widen how *you* go and get one.** Discovery stays logged-out (§7 guardrail 1). Reading inside the operator's own session is a different act on a different path with its own skill, and `source_discovery.md` SD-33 — normative, and it wins over any requirement that disagrees — forbids building discovery on logged-in browser automation, the operator's session or a consumer browser extension. A group the operator has already joined needs no discovering, which is why that carve-out costs nothing.

**A skill is not an enforcement point.** Everything above is routing, and routing is procedure. Only what the server refuses can be pinned by a test, so nothing here invents behaviour: the closed candidate schema, the four outcomes, the completion rules and every refusal named in this section are the server's, and they hold whether or not this skill was loaded.

## 6. Re-verification, and dead sources

Sources rot quietly, which is worse than rotting loudly. Each run, take the stalest few:

- **No yield since a date** — check whether the site changed shape (extraction problem, fixable) or went quiet (source problem, retire it). These look identical in the digest and have opposite remedies.
- **Fetch failing** — set it back to `access_method = 'manual'`. That is how a source is retired; **nothing deletes a `source` row**, and its history stays attached.
- **A group that has not been read in weeks** — that is not a dead source, it is an unrun sweep. Hand to the `scan` skill rather than retiring anything.
- **Re-verify liveness on the platforms that permit it**, forums and Discord especially, since those are the ones where a number can be checked rather than assumed.

Say what you retired and why, in one line each. A retired source the operator disagrees with is a thirty-second correction; a silently retired one is a gap he finds in three months.

## 7. Guardrails

1. **Logged-out, always.** Discovery never authenticates. It is the best-defended activity in this whole space *precisely because* it does not, and a single logged-in discovery pass throws that position away. Reading sources the operator has already joined is a different act, on a different path, with its own skill.
2. **Robots and terms before proposal**, per §4. Not after.
3. **Metadata only** — name, URL, description, activity, rules. Never members, never admin identities.
4. **Bounded.** Agree a maximum number of searches and fetches with the operator **before starting**, and stop there. A loop following links has no natural stopping point, and this is the one task in the system where cost genuinely runs away.

## 8. The two modes — say which one you are in

**First pass** — the state is empty. A deep, broad sweep: run the strategies in §3 in ranked order across the active segments and markets, collect widely, then spend the bulk of the time judging. **Budget roughly 30 minutes of the operator's attention** and say so at the start. Prioritise associations and forums, because they qualify themselves fastest, and leave WhatsApp to whatever the association pages hand you.

**Incremental** — the normal case, and the one to optimise for. Read state, compute gaps, run a small number of targeted searches against the two thinnest ones, propose **three or four candidates**, re-verify anything stale. This also runs without a session at all: three source suggestions ride along in the daily view, and the operator accepts or dismisses them in a few seconds. If that is the shape of the run, keep the reasoning to one line each — he is reading it between other things.

## 9. Hand-off

- **the `onboard` skill** — if the brief cannot distinguish the segments, you cannot judge relevance and neither can he. Stop and say so; a source map built against a vague segment is worse than none, because it looks like coverage.
- **the `scan` skill** — every `access_path = 'operator_session'` source you record is work for that skill, and a group nobody sweeps is a row, not a source.
- **the `react` skill** — when a reply is blocked on a group whose rules were never read (`rules_read: false`), this is the skill that unblocks it, and reading one group's three rules is five minutes.
- **the `publish` skill** — a source with `purpose` of `publish` or `both` is a place to post, not only to read. Say which of the new rows are which.

The division of labour is unchanged: **you judge the source; the server makes the constraint real.** A source stays unfetched until `robots_ok` is true whether or not this skill was loaded, and a group with unread rules blocks a drafted reply whether or not anyone remembers why.
