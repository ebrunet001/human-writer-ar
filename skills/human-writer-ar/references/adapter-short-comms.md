# Adapter: Short-Form Communications

> Doctrine for the `human-writer` skill, applied to short-form comms: emails, LinkedIn posts and DMs, Slack/Teams messages to clients, X/Twitter replies, GitHub issue comments, customer replies. Loaded in addition to language-specific stylistic tells (`tells-stylistic-ar.md`).

Short texts amplify density. A single em-dash in a 50-word email is louder than four em-dashes in a 1500-word blog post. The thresholds tighten accordingly, the surface area for "redemption" shrinks to nothing, and the reader is paying full attention. This is the hardest content-type in which to sound human, and the cheapest one for AI detectors to flag.

## 1. Scope: what counts as `short-comms`

| Channel | Typical length | Notes |
|---|---|---|
| Business email (Brevo, IMAP, Gmail) | 40-300 words | Both cold and warm |
| LinkedIn post | 80-300 words | Hook in line 1 (preview) |
| LinkedIn DM | 30-120 words | Often the cold ask |
| Slack/Teams to client | 20-200 words | Tone informal, content business |
| X/Twitter reply | < 280 chars | Tiny surface, one tell kills it |
| GitHub issue comment | 50-300 words | Technical, may include code |
| Internal update / status | 100-300 words | Some structure tolerated |
| Newsletter intro | 100-300 words | Body switches to `marketing` |

Distinct from:
- `marketing`: no persuasion structure, no funnel, no hero claim
- `technical`: less precision focus, more relational/conversational tone
- `editorial-seo`: no SEO intent, no keyword targeting

Word count typically 30-500 words. Below 30 words, this adapter still applies but most checks become moot (the only thing left is "does the signoff smell of AI").

## 2. Why short-comms is the trickiest content-type for AI detection

Five reasons the bar is higher here than anywhere else:

1. **Density amplification.** One em-dash in a 100-word email is 10/1000 words, already well over the AR threshold (and "—" is foreign to Arabic to begin with). One AI-flavored idiom in 80 words is one in eighty. There's nowhere to hide.
2. **Signoffs are templates.** AI email tooling has trained millions of readers to spot "أتطلع إلى ردكم" at a glance. The closing words of an email carry disproportionate weight in the reader's "is this a person?" judgment.
3. **No surface to redeem.** A 1500-word blog post can afford one weak section if the rest is strong. A 60-word reply cannot.
4. **Detector attention.** Copyleaks, GPTZero, Originality, and outreach-platform-side detectors are increasingly tuned for sales/cold-email formats. The training set is dense.
5. **Reader attention.** Recipients of a personal email or DM read every word. A blog post reader skims. Tells stand out more under close reading.

## 3. Priority ranking: which tells matter MOST here

Different from long-form. Order matters.

1. **Em-dash discipline (TIGHTER cap).** Hard cap: 0 em-dashes in Arabic short-comms ("—" is foreign). For EN/FR, 1-2 max in the whole message.
2. **Latin-punctuation-in-Arabic (HIGH).** Even in a short message, a Latin `,` `;` `?` next to Arabic letters is a mechanical tell. Use ، ؛ ؟ (chat/SMS register relaxes this — see §10).
3. **Idiosyncrasy (HIGHEST POSITIVE SIGNAL).** One unusual word, fragment, or structure puts the reader firmly in "this is a person" territory. See section 6.
4. **AI-flavored signoffs (HIGH).** Section 4 below: zero tolerance for the listed phrases.
5. **AI-flavored openers (HIGH).** Section 5: same logic, mirror image at the top.
<!-- human-writer:ignore-start (these items quote forbidden vocab and constructions as anti-patterns) -->
6. **Tier-1 vocabulary (HIGH).** 1 tier-1 word in 200 words is a tell. Still zero "تسخير"، "سلس"، "حلول مبتكرة"، "أطلق العنان".
7. **Conclusion templates (HIGH).** "في الختام،"، "في نهاية المطاف،": never in a short message.
<!-- human-writer:ignore-end -->
8. **Sentence variance (LOWER).** Short messages naturally have low variance (few sentences total). Relax the `sentence_stdev_min` check.
9. **Structural tells (LOWER).** Short messages rarely have headers or bullet lists. The `structural: 0.5` weight reflects this.

## 4. AI-flavored signoffs: zero tolerance

The single most expensive few words in any email. Detectors and readers both lock onto these.

<!-- human-writer:ignore-start (forbidden signoff phrases, quoted as anti-patterns not used as prose) -->
### AR — never use

- "في انتظار ردكم الكريم،"
- "أتطلع إلى ردكم،"
- "يسعدني سماع آرائكم،"
- "لا تترددوا في التواصل معنا،"
- "لأي استفسار، لا تترددوا في السؤال،"
- "شكرا لتفهمكم،"
- "شكرا على وقتكم واهتمامكم،"
- "وتفضلوا بقبول فائق الاحترام والتقدير،" (formal — OK in legal/administrative contexts, but AI defaults to it always)
- "مع خالص التحية والتقدير،" (overused — fine 1 email in 5, never every email)
- "مع أطيب التمنيات،"

### EN — never use

- "I look forward to hearing from you."
- "Looking forward to your response."
- "Please don't hesitate to reach out."
- "Best regards," (overused — fine 1 email in 5, never every email)
- "Warm regards,"
- "Kind regards,"
<!-- human-writer:ignore-end -->

## 5. AI-flavored openers: zero tolerance

The first words set the read. Same logic, opposite end.

<!-- human-writer:ignore-start (forbidden opener phrases, quoted as anti-patterns not used as prose) -->
### AR — never use

- "أتمنى أن تصلك رسالتي وأنت بخير،" (full calque, ouch)
- "أتمنى أن تكون بخير،"
- "أكتب إليكم بخصوص…"
- "أتواصل معكم بشأن…"
- "أود أن أغتنم هذه الفرصة لـ…"
- "متابعة لرسالتي السابقة،"
- "كما تمت مناقشته سابقا،"

### EN — never use

- "I hope this email finds you well,"
- "I wanted to reach out to..."
- "Just wanted to follow up on..."
- "Per our last conversation,"
- "As discussed,"
<!-- human-writer:ignore-end -->

## 6. Human-sounding alternatives

<!-- human-writer:ignore-start (quoted signoff/opener literals, cited as examples not used as prose) -->
### AR — signoffs that pass

- Just a first name on its own line
- "إلى اللقاء،"
- "شكرا،"
- "يومك سعيد،"
- "تحياتي،" (better than the full formal stack)
- Nothing at all (a 2-sentence reply doesn't need a signoff)

### AR — openers that pass

- Open with the actual point: "سؤال سريع عن تسعير الربع الثالث."
- Open with a fact: "رأيت منشورك عن تغيّر تسعير PPE في Apify. نقطة فاجأتني:"
- Open with a question: "كيف يسير التشغيل؟"
- Skip the opener entirely: drop straight into the content. "نسخة العقد التي أرسلتها فيها مشكلة في البند ٤-٢…"

### EN — signoffs/openers that pass

- Just a first name; "Talk soon,"; "Cheers,"; "Thanks,"; "Best,"; nothing at all
- Open on the subject: "Quick question about the Q3 pricing."
- Open on a fact, a question, or skip the opener entirely
<!-- human-writer:ignore-end -->

## 7. Idiosyncrasy: the #1 humanization technique here

In long-form, you can vary rhythm and use specific data. In short-form, your single best tool is **one** idiosyncratic phrase or structure that no AI template would generate. One is enough. Two starts to look like you're trying.

Concrete moves:

<!-- human-writer:ignore-start (quoted sample phrases demonstrating the moves, cited not used as prose) -->
- Open with a specific observation only you would make ("منشورك يوم الجمعة عن تجميع اتصالات Postgres — تلك الحاشية عن PgBouncer في وضع المعاملات هي بالضبط ما أصطدم به.")
- Use a sentence fragment for emphasis ("يستحق العناء.")
- Drop a side joke (one line, never two)
- Mention a specific shared context: a person, a date, a previous conversation, a place
- Use a casual register marker ("انظر،"، "بصراحة،"، "يعني،") — AI defaults to formal MSA in business email
- End mid-thought, with no closing summary
- Reference a number that's clearly from your own measurement, not a benchmark ("٤٫٢ مللي ثانية p50 والذاكرة المؤقتة دافئة، مقابل ٠٫٨ ثانية باردة")
- Drop one word of jargon or in-group vocabulary
- Use one English/Arabic borrowing if you naturally code-switch (if you do)
<!-- human-writer:ignore-end -->

See also `humanization-techniques.md`, technique #6 (idiosyncratic markers).

## 8. Length calibration by use case

| Use case | Word count | Notes |
|---|---|---|
| Cold intro email | 80-150 words | One specific opener, one clear ask |
| Warm follow-up | 60-120 words | Reference the previous, ask one thing |
| Internal status update | 100-300 words | Light structure OK, no headers |
| Reply to client | 40-150 words | Direct, no preamble, no signoff filler |
| Newsletter intro | 100-200 words | Body switches to `marketing` adapter |
| LinkedIn post | 80-300 words | Section 9 |
| LinkedIn DM (cold) | 30-80 words | Section 9 |
| X/Twitter reply | < 280 chars | One thought, no emoji |
| Slack message to client | 20-150 words | Most informal; relaxed register |

Below 30 words, only the signoff/opener checks apply.

## 9. LinkedIn post specifics

- **First line is the hook.** Short enough to fit in the feed preview before the "...see more" cut.
- **No emoji in the first line.** Heavy AI tell: AI templating defaults to opener emojis.
- **Max 5 emojis total** across the whole post. Zero is fine. Six is a tell.
- **No bullet lists for content.** LinkedIn algorithm doesn't favor them, and they look templated. If you need parallel items, write them as short standalone lines separated by blank lines.
- **No "هل توافق؟ علّق بالأسفل" CTA.** Heaviest AI tell on the platform. Same for "ما رأيك؟" closers.
- **No "أفكار؟" closer.** Also templated.
- **Hashtags max 3, at the very bottom.** Avoid #ابتكار #قيادة #ذكاء_اصطناعي stacks.

LinkedIn DMs follow the cold-email rules. Skip the "أتمنى أن تكون بخير" entirely.

## 10. Tells that are tolerated more here

### Latin punctuation (relaxed in chat/SMS register)

In a casual Slack/SMS message, some Latin `,` `?` creep in legitimately (people type fast on phone keyboards). Use `--type short-comms`, where `latin_punct_in_arabic_max` is more forgiving — but in a real email to a client, still prefer ، ؛ ؟.

### Tier-1 vocabulary (slightly relaxed in narrow technical context)

<!-- human-writer:ignore-start (calibration paragraph quotes the suspect word as its worked example) -->
In a 50-word Slack message to a developer ("النشر قوي بما يكفي لاختبارات التحقق")، "قوي" reads as a technical adjective, not AI gloss. The analyzer's `stylistic: 1.2` weight still flags it, but a single instance in a strictly technical context is defensible.
<!-- human-writer:ignore-end -->

<!-- human-writer:ignore-start (forbidden tier-1 vocab list, quoted as anti-patterns not used as prose) -->
Still zero: "تسخير"، "سلس"، "حلول مبتكرة"، "أطلق العنان"، "حجر الزاوية"، "نقلة نوعية"، "منظومة"، "تآزر". These never pass regardless of context.
<!-- human-writer:ignore-end -->

### Headers (irrelevant)

Short comms rarely have headers. Skip structural checks. If a message has H2s, it's not short-comms: switch adapter.

### Tricolons (relaxed if asyndeton)

"سريع، رخيص، تم." in a Slack message reads as human punch. "سريع، رخيص، وتم." in the same message reads as AI. Same content, different punctuation. The "و" is the tell. Asyndetic tricolons (no "و") pass; syndetic tricolons trip.

### Sentence variance (relaxed)

A 5-sentence email naturally has low burstiness; there's no statistical room to vary. The `statistical: 0.6` weight reflects this. Don't force long-short alternation in a 50-word reply.

## 11. Calibration with `analyze.py`

Short-comms weights from `scripts/rules.yaml`:

```yaml
short-comms:   {statistical: 0.6, stylistic: 1.2, structural: 0.5}
```

Stylistic enforced HIGHER (1.2×: most important axis here). Statistical and structural relaxed (0.6× and 0.5×). Run:

```bash
python3 scripts/analyze.py --type short-comms --lang ar draft.txt
```

Target: score ≤ 24 (LOW_RISK band from `rules.yaml verdict_bands`). For very short messages (< 80 words), interpret the score loosely; a single em-dash can spike density above the threshold without the message actually reading as AI. Cross-check with the checklist in section 12.

## 12. Pre-publish checklist: short-comms

```markdown
- [ ] Ran `analyze.py --type short-comms --lang ar` — score ≤ 24
- [ ] No "أتمنى أن تصلك رسالتي وأنت بخير" opener
- [ ] No "في انتظار ردكم الكريم" / "وتفضلوا بقبول فائق الاحترام" closing
- [ ] No "مع خالص التحية والتقدير،" every email (rotate it)
- [ ] Em-dashes: 0 (AR — "—" is foreign)
- [ ] Arabic ، ؛ ؟ used, not Latin , ; ? (chat register may relax this)
- [ ] At least one idiosyncratic phrase, fragment, or specific reference
- [ ] No tier-1 AI vocab (see `tells-stylistic-ar.md`)
- [ ] No "في الختام،" / "في نهاية المطاف،"
- [ ] Signoff is a first name or short phrase, not corporate boilerplate
- [ ] For LinkedIn posts: short hook line, no opener emoji, no "أفكار؟" closer
```

If any item fails, fix before sending. The bar for "sounds human" is higher here than in any other content type.

## 13. Worked examples

### Example 1: AR client reply (~80 words)

<!-- human-writer:ignore-start (deliberately AI-flavored AR sample + tells citation; must not self-flag) -->
**AI-flavored draft (analyzer score: ~55, HIGH_RISK)**

> مرحبا مروان،
>
> أتمنى أن تصلك رسالتي وأنت بخير. أشكرك على ملاحظاتك بخصوص التكامل مع الـ API. تجدر الإشارة إلى أن حلّنا يقدّم نهجا قويا وثوريا يتيح تسخير كامل البيانات المتاحة.
>
> لا تتردد في مراسلتي لأي استفسار إضافي.
>
> مع خالص التحية والتقدير،
> ليلى

Tells: calque opener "أتمنى أن تصلك رسالتي وأنت بخير"، "تجدر الإشارة إلى"، "قوي"، "ثوري"، "تسخير"، "لا تتردد في مراسلتي"، "مع خالص التحية والتقدير". Six high-severity tells in 50 words of body.
<!-- human-writer:ignore-end -->

**Humanized version (analyzer score: ~14, LOW_RISK)**

<!-- human-writer:ignore-start (quoted clean-example AR email body; cited as a sample not used as prose) -->
> مرحبا مروان،
>
> شكرا على الملاحظات حول التكامل. من الأصناف الـ٤٧ التي أرسلتها، ٤٣ تمر؛ والأربعة التي تتعطل لها العرض نفسه (انتهاء المهلة على نقطة التفاصيل). أنظر فيها صباح الغد وأعيد إليك التصحيح خلال اليوم.
>
> إلى اللقاء،
> ليلى
<!-- human-writer:ignore-end -->

Changes: no opener cliché, straight into thanks + content. Specific numbers (٤٧، ٤٣، ٤). Technical detail (timeout, details endpoint). Concrete commitment with a time ("صباح الغد"، "خلال اليوم"). Signoff is "إلى اللقاء،" warm but not templated. Zero em-dashes. Zero tier-1 vocab. Arabic ، ؛ ؟ throughout.

### Example 2: LinkedIn DM ask (~50 words)

<!-- human-writer:ignore-start (deliberately AI-flavored sample + tells citation; must not self-flag) -->
**AI-flavored draft (analyzer score: ~47, MEDIUM_RISK)**

> مرحبا أليكس،
>
> أتمنى أن تكون بخير. أتواصل معك لأنني لاحظت عملك المميز في مجال السحب. يسعدني التواصل واستكشاف أوجه التآزر المحتملة بين حلولنا المبتكرة.
>
> في انتظار ردك.
>
> تحياتي،
> ليلى

Tells: "أتمنى أن تكون بخير"، "أتواصل معك"، "عمل مميز"، "تآزر"، "حلول مبتكرة"، "في انتظار ردك". Generic and templated end to end.
<!-- human-writer:ignore-end -->

**Humanized version (analyzer score: ~8, LOW_RISK)**

<!-- human-writer:ignore-start (quoted clean-example DM body; cited as a sample not used as prose) -->
> مرحبا أليكس،
>
> ما كتبته الأسبوع الماضي عن نفاد تجمّع الاتصالات — هذا بالضبط نمط الفشل الذي اصطدمت به في مارس على عميل API عالي الإنتاجية. هل توصّلت إلى ما إن كانت تكلفة مصافحة TLS أم تدوير تجمّع الجلسات؟
>
> بلا أجندة، فضول فقط.
>
> ليلى
<!-- human-writer:ignore-end -->

Changes: opens on specific shared context (the write-up, a date). Specific technical question (TLS vs session churn). Explicit "بلا أجندة" disarms the cold-DM frame. Fragment ("بلا أجندة، فضول فقط."). No signoff filler. Zero em-dashes (used ، for the aside). The "ask" is implicit, a real question, not a templated meeting request.

## See also

- `tells-stylistic-ar.md`: full vocabulary and construction lists to avoid
- `humanization-techniques.md`: technique #6 (idiosyncratic markers) is the critical one here
- `checklists.md`: master checklist; section 12 above is the short-comms specialization
- `adapter-marketing.md`: for newsletter bodies and persuasion-structured content
- `adapter-technical.md`: when the message is a GitHub comment with heavy code/diagnostics
