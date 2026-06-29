# Pre-Publish Checklists (human-writer-ar)

> Ready-to-paste self-review checklists, one per content-type, for Arabic (MSA) prose. Run `analyze.py --lang ar` first, then walk through the relevant checklist before delivery. Each checklist is short, actionable, and covers the tells most likely to trigger AI detectors for that specific format. Adapted from the master `human-writer` skill; the Arabic quick-triage at the end is the satellite-specific addition. All checks operate on logical character order — RTL is a display concern only.

---

## Marketing Long-Form

Before publishing blogs, READMEs, landing pages, or long-form newsletters in Arabic, confirm:

- [ ] Analyzer score ≤ 24 with `--lang ar --type marketing`
- [ ] First 100 words contain zero Tier-1 suspect vocabulary
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] First paragraph does not open with "في عالم اليوم"، "في العصر الرقمي"، "تجدر الإشارة إلى"، or "سواء كنت X أو Y"
- [ ] Closing paragraph does not start with "في الختام"، "في نهاية المطاف"، "خلاصة القول"، or "وفي الأخير"
<!-- human-writer:ignore-end -->
- [ ] Zero em-dashes "—" (run `analyze.py`; threshold is strict, 0.5/1000w — "—" is foreign to Arabic)
- [ ] Every `,` `;` `?` adjacent to Arabic letters is the Arabic ، ؛ ؟ (not Latin)
- [ ] Zero tatweels ـ (U+0640) in body prose
- [ ] At least one specific number, named entity, or first-person opinion per ~300 words
- [ ] Bulleted lists vary opening verbs (≤ 80% share the same first word)
- [ ] No H2 has exactly 3 H3 children (if 2+ H2s exist, vary H3 counts)
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "أبرز النقاط" / "ما ستتعلمه" / "أفضل X" standalone block
<!-- human-writer:ignore-end -->
- [ ] No emoji as section header
- [ ] Sentence-initial و capped at ~1 in 4 sentences

If any item fails: fix before delivery. If analyzer score is 25–49 after fixes, apply the top 3 recommendations and re-score. If 50+, reconsider your angle.

---

## Short-Form Communications

Before sending emails, LinkedIn posts, or customer replies in Arabic, confirm:

- [ ] Analyzer score ≤ 24 with `--lang ar --type short-comms`
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "أتمنى أن تصلك رسالتي وأنت بخير" opening
- [ ] No "في انتظار ردكم الكريم" / "وتفضلوا بقبول فائق الاحترام" boilerplate closing
- [ ] No "أتواصل معكم بخصوص"، "لا تترددوا في مراسلتي"، "لأي استفسار لا تترددوا"
<!-- human-writer:ignore-end -->
- [ ] Em-dashes: 0; never any "—" in messages under 200 words
- [ ] Arabic ، ؛ ؟ used, not Latin , ; ? (chat register may relax this — use `--type short-comms`)
- [ ] At least one first-person verb or sentence fragment in replies over 30 words
- [ ] Signoff is a first name, short phrase, or nothing — not corporate boilerplate every email

If any item fails: revise before sending. (Note: in casual chat/SMS register, some Latin punctuation creeps in legitimately; the analyzer's `latin_punct_in_arabic_max` is looser under `--type short-comms`.)

---

## Technical Documentation

Before publishing internal READMEs, RFCs, or technical journals in Arabic, confirm:

- [ ] Analyzer score ≤ 24 with `--lang ar --type technical`
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] First sentence states the WHY (problem this doc solves), not "في هذا المستند سوف نستكشف..."
- [ ] No "هيا بنا!" / "لنبدأ دون مزيد من المقدمات" filler
- [ ] No "خاتمة" section (unless >500 words and earns a summary)
- [ ] Every instance of "متطور"، "قابل للتوسع"، "تسخير"، "أنيق"، "قوي" passes the "replace with 'جيد'" test
<!-- human-writer:ignore-end -->
- [ ] Code blocks are not surrounded by paragraphs that re-explain them line-by-line
- [ ] Latin punctuation inside CODE is fine (wrap in fenced blocks — stripped before scoring); Arabic prose around it uses ، ؛ ؟
- [ ] One sentence per concept: no restating or hedging the same idea twice
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] Zero hedging openers ("تجدر الإشارة إلى"، "من المهم أن نلاحظ"، "لا بد من الإشارة"، "ملاحظة:")
<!-- human-writer:ignore-end -->

If any item fails: revise. Technical docs should be direct and economical.

---

## Editorial-SEO Articles

Before publishing ranking-optimized articles in Arabic, confirm:

- [ ] Analyzer score ≤ 24 with `--lang ar --type editorial-seo`
- [ ] Target keyword appears in: title, first 100 words, and ≥1 H2
- [ ] First 100 words contain a specific data point or opinion (not just the keyword)
- [ ] H2 hierarchy is varied, not pyramid (not 3 H3s per H2 uniformly)
- [ ] At least 1 internal link with natural anchor text (not "اضغط هنا")
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "في عالم اليوم لـ X،" / "في العصر الرقمي،" opener
- [ ] No "خاتمة" that just restates the intro / tl;dr
<!-- human-writer:ignore-end -->
- [ ] At least one opinion that someone could reasonably disagree with
<!-- human-writer:ignore-start (checklist quotes the tells to look for) -->
- [ ] No "الدليل الشامل"، "كل ما تحتاج معرفته"، "الدليل الكامل" UNLESS your keyword data demands it
<!-- human-writer:ignore-end -->
- [ ] Sentence-length standard deviation ≥ 8 (mix short and long)
- [ ] Title in normal Arabic case; digits consistent (one of ٠١٢ or 012)

If any item fails: fix before publishing.

---

## Universal Baseline

Apply to **all** content types before delivery:

- [ ] Final `analyze.py --lang ar` run; score ≤ 24
- [ ] No Tier-1 suspect vocabulary in the first 100 words (check `rules.yaml` for the list)
- [ ] Every `,` `;` `?` adjacent to Arabic is the Arabic ، ؛ ؟ (except deliberate chat/SMS or code)
- [ ] Zero em-dashes "—", zero tatweels ـ
- [ ] At least one specific element: number, name, date, fact, quote, not generic claims
- [ ] At least one stylistic asymmetry: a short sentence after long ones, a varied list length, unexpected structure
- [ ] Doesn't end with a summary or call-to-action that merely restates the opening

---

## Arabic quick-triage (satellite-specific)

A 60-second eyeball pass for any Arabic draft, in priority order — the fastest path from "looks AI" to "reads human". Mirrors the order in `tells-stylistic-ar.md` § Quick triage:

1. **Latin punctuation.** Scan every `,` `;` `?` next to Arabic letters. Latin instead of ، ؛ ؟? Replace it. (Any occurrence flags.)
2. **Em-dashes / tatweel.** Count "—" in the prose → replace with ، : ( ) or a period. Sweep for ـ (U+0640) → delete.
3. **Opener.** First sentence starts with "في عالم اليوم / في العصر الرقمي / تجدر الإشارة إلى / انغمس في"? Rewrite.
4. **Closer.** Last paragraph starts with "في الختام / في نهاية المطاف / خلاصة القول / باختصار"? Rewrite.
5. **Tier-1 vocab.** "تسخير (leverage)، سلس (seamless)، حلول مبتكرة، مبتكر، متطور، حجر الزاوية، نقلة نوعية" — cap 1 per paragraph.
6. **Constructions.** "ليس مجرد…، بل…"، "سواء كنت… أو…"، "مما لا شك فيه"، "هل تساءلت يوما".
7. **Connector overuse.** Count sentence-initial و/ف and "علاوة على ذلك / بالإضافة إلى ذلك"; vary them, cap و at ~1 in 4 sentences.
8. **Tricolons.** Count "X، Y، وZ"; cap 1 per 200 words.
9. **POV.** At least one first-person mark if it's opinion.
10. **Digits.** One digit set (Arabic-Indic ٠١٢ OR Western 012), consistent throughout.

A passing piece: 0 tier-1, ≤2 tier-2, 0 constructions, ≤1 tricolon/200w, every `,;?` is ، ؛ ؟, 0 em-dashes, 0 tatweels, one first-person mark if opinion, و capped at 1-in-4. That lands LOW_RISK (≤ 24) and survives Copyleaks/GPTZero at sub-25 % in most domains.

---

## How to Use These Checklists

1. Write or clean your draft.
2. Run `python3 scripts/analyze.py --input <draft> --lang ar --type <type> --format human` from the skill root.
3. If score > 24, apply the analyzer's top recommendations until ≤ 24.
4. Walk through the relevant checklist above (marketing, short-form, technical, or editorial-SEO) plus the Arabic quick-triage.
5. Check off each item; revise any failures.
6. Re-run the analyzer if you made major changes.
7. Deliver only when both gates pass (analyzer ≤ 24 AND checklist complete).

---

## See Also

- `tells-stylistic-ar.md`: vocabulary, constructions, calques, punctuation tells (Arabic)
- `adapter-marketing.md`: doctrine specific to long-form marketing
- `adapter-short-comms.md`: doctrine specific to short communications
- `humanization-techniques.md`: positive techniques for adding human voice (Arabic examples)
- `scripts/analyze.py`: deterministic analyzer (run before checklist)
- `external-detectors.md`: optional integration with Copyleaks, GPTZero, etc.
