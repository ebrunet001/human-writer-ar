# Adapter: Technical Docs (Internal)

> Doctrine for the `human-writer` skill, applied to internal technical writing: READMEs of internal projects, `PROJECT_HISTORY.md` journals, design docs, RFCs, ADRs, runbooks, and code comments treated as prose. Loaded in addition to language-specific stylistic tells.

This adapter does NOT cover public-facing marketplace READMEs. Those are marketing artifacts and live under `adapter-marketing.md` (or a dedicated structure/content tool, which is more specific). Use the present file for documents read mostly by engineers, including your future self.

## Definition

| Attribute | Value |
|---|---|
| Word count | Variable, 300 to 3000 words typical |
| Purpose | Convey precise technical information to other engineers |
| Audience | Current colleagues, future maintainers, your future self |
| Channels | Internal READMEs, PROJECT_HISTORY journals, design docs, RFCs, ADRs, runbooks |
| Tone | Precise, direct, terse where possible; no persuasion, no SEO |
| Distinct from | `marketing` (no selling), `editorial-seo` (no keyword optimization), `short-comms` (longer, more structured) |

## The "AI sheen" risk specific to technical writing

The most insidious failure mode for technical content: AI **adds sheen on top of factual content**. Florid summaries, motivational paragraphs, executive-summary boilerplate wrapped around code that is otherwise fine. This is the "ChatGPT'd README" smell.

The text is technically correct. The code blocks work. But every section is bracketed by a paragraph that restates the section title in flowery prose, and the document closes with a "Conclusion" that adds nothing.

### Detection signals

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- An "Overview" section whose first sentence restates the document title
- A "Conclusion" or "Summary" section in a doc under 500 words
- Each section opens with a paragraph announcing what the section is about to say
- "في هذا المستند، سوف نستكشف…"
- "بنهاية هذا الـ README، ستتمكن من…"
- Verbose explanations of obvious code (`# Loop over items in the list` next to `for item in items:`)
- Headers like "أبرز الميزات"، "الفوائد"، "لماذا يهم هذا" inside an internal doc
<!-- human-writer:ignore-end -->

When you see these, the fix is rarely to rewrite the prose. Usually you **delete the offending paragraph entirely** and let the code or structure speak.

## Tell priority for technical content

Not all tells weigh the same here. Ranked by impact:

<!-- human-writer:ignore-start (priority table quotes the tells it ranks) -->
| Rank | Tell family | Impact | Action |
|---|---|---|---|
| 1 | AI constructions ("تجدر الإشارة إلى"، "من الجدير بالذكر") | HIGH | Kill on sight |
| 2 | Conclusion templates ("في الختام"، "في نهاية المطاف، هذا التصميم…") | HIGH | Delete the section, don't rewrite it |
| 3 | Hedging openers ("من المهم أن نلاحظ") | MEDIUM | Technical docs do use some hedging; calibrate, see below |
| 4 | Suspect vocabulary ("قوي"، "تسخير"، "قابل للتوسع") | MEDIUM | Many are legitimate in technical context; see calibration |
| 5 | Em-dash overuse | LOW | "—" is foreign to Arabic; target 0 — rarely appears in disciplined technical prose anyway |
| 6 | Tricolons (3-element parallel lists) | LOW | Technical writing legitimately lists 3 attributes (CPU، RAM، قرص) |
<!-- human-writer:ignore-end -->

The dominant problem is items 1 and 2: sheen that AI adds around otherwise-decent factual content. Vocabulary and structure are secondary.

## Calibration: when "AI vocabulary" is OK in technical writing

Tier-1 suspect words have legitimate technical meanings. The rule is contextual.

<!-- human-writer:ignore-start (citation table: suspect words quoted as examples, not used as prose) -->
### Legitimate technical usage

| Word | Legitimate technical sense |
|---|---|
| "قوي" (robust) | Fault tolerance, error handling: "قوي تجاه أعطال الشبكة" |
| "قابل للتوسع" (scalable) | Capacity: "يتوسّع إلى ١٠ آلاف طلب/ثانية تحت الحمل" |
| "تسخير / الاستفادة من" (leverage) | Reusing existing infrastructure: "الاستفادة من طبقة التخزين المؤقت الحالية" |
| "شامل" (comprehensive) | Test or doc coverage: "اختبارات تكامل شاملة لمسار المصادقة" |
| "سلس" (seamless) | Almost never legitimate; flag aggressively |
| "تبسيط" (streamline) | Almost never legitimate as gloss; flag |

### The "replace with 'جيد'" heuristic

If you can replace the suspect word with "جيد" (good) and the sentence keeps its meaning, it is a tell. If the replacement loses precision, it is a legitimate technical use.

**Tell:**
- "منصّتنا القوية، القابلة للتوسع، والبديهية" → "منصّتنا الجيدة، الجيدة، والجيدة" → meaning preserved → all three are sheen.

**Legitimate:**
- "هذه الواجهة قوية تجاه انقسامات الشبكة" → "هذه الواجهة جيدة تجاه انقسامات الشبكة" → meaning lost → "قوي" is precise here.

### The marketing-shaped phrase test

The same word can be legitimate or sheen depending on the surrounding phrase shape. A clue: comma-separated lists of three positive adjectives are almost always sheen, regardless of which words are used.

- "قوية، قابلة للتوسع، وبديهية" → sheen (three abstract qualities, no measurement)
- "قوية تجاه الانقسامات، تتوسّع إلى ١٠ آلاف طلب/ثانية، وموثّقة على /api/v2" → fine (each clause is concrete)
<!-- human-writer:ignore-end -->

## Code blocks and non-prose are exempt

The `analyze.py` analyzer measures prose only. Code blocks, JSON examples, YAML configs, command-line examples, tables of constants, and ASCII diagrams are not analyzed and should not be. A 2000-word doc with 1500 words of code samples is scored on the ~500 words of prose. (Latin punctuation inside fenced code is therefore never counted by the Arabic Latin-punctuation detector.)

Practical implication: do not pad code blocks with explanatory paragraphs. The code is the evidence; the prose connects it, not duplicates it.

## Anti-patterns specific to technical docs

Patterns to delete on sight in internal technical writing:

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- "سيرشدك هذا الـ README خلال…"
- "بنهاية هذا المستند، ستفهم…"
- "هيا بنا!" / "لنبدأ!"
- "دون مزيد من المقدمات،"
- "تجدر الإشارة إلى أن"
- "من المهم أن نفهم أن"
- "كما نعلم جميعا،"
- "في هذا القسم، سنناقش…"
- Repeating the function name in a docstring and describing what it does in three paragraphs
- "سهل كـ ١، ٢، ٣!" tutorials
- "خاتمة" sections in docs under 500 words
- "ترقّبوا…" in changelog entries
- "برمجة سعيدة!" sign-offs
- Headers like "الفوائد الأساسية" or "لماذا يهم هذا" inside a README
<!-- human-writer:ignore-end -->

## Good patterns

Replace the anti-patterns with these shapes:

- **Lead with the WHY, not the WHAT.** State the problem this doc solves in the first sentence. "يوثّق هذا الـ README كيف رحّلنا خدمة الفوترة من قائمة انتظار متزامنة إلى أخرى غير متزامنة."
- **One sentence per concept.** Don't restate. If you wrote "الذاكرة المؤقتة تستخدم Redis"، do not follow with "اخترنا Redis طبقةً للتخزين المؤقت."
- **Code → prose → code.** Prose connects code, it doesn't summarize it. The reader can read the code. The prose says what the code does not show: the *why*, the constraint, the tradeoff.
- **Acceptable hedging when genuine.** "لست متأكدا لماذا يعمل هذا على لينكس ويفشل على ماك" is honest and useful. AI hedging ("تجدر الإشارة إلى أن هذا قد يحتمل…") is the opposite: vague-sounding confidence.
- **Acknowledge what you don't know.** "مُختبر على لينكس ٦٫٨ فقط" beats "دعم شامل عبر المنصات" every time.
- **Past tense for journals.** PROJECT_HISTORY entries are events that happened: "جرّبنا المشغّل ٠٫٤٢، اصطدمنا بانتهاء مهلة `Connection`، تراجعنا إلى ٠٫٤١، نجح."

## PROJECT_HISTORY journals (workflow-specific)

A `PROJECT_HISTORY.md` file is a useful technical journal format for any project. The voice should be:

- **Past tense.** "جرّبنا X، فشل بسبب Y، انتقلنا إلى Z."
- **Specific.** Dates, error messages, score deltas, commit SHAs, RAM numbers.
- **Decisions documented.** Each entry should answer "what did we learn?", even if the answer is "nothing, this was wasted time."
- **No marketing language.** Not a single tier-1 vocab word should appear in a PROJECT_HISTORY entry.
- **Optional lessons-learned.** Not required at the end of every entry. If there is no lesson, do not invent one.

## Design docs / RFCs / ADRs

Design documents have their own conventional structure. The AI failure mode here is wrapping that structure in marketing prose.

- **Open with the problem statement.** Not a context-setting introduction. The first paragraph is "حاليا، X. نحتاج Y. يقترح هذا المستند Z."
- **"القرار" is fine, "الخاتمة" is not.** The decision IS the conclusion. A separate Conclusion section is sheen.
- **Pros / Cons / Alternatives Considered.** These are technical structures, not AI tells. Use them.
- **Status header.** "الحالة: مقترح / مقبول / متجاوَز بـ RFC-N" is conventional. Keep it.
- **No "ملخّص تنفيذي"** in a doc read by engineers. If you need a TL;DR, write three bullet points and call it "ملخّص" (max 50 words).

## Code comments and docstrings as prose

When code comments or docstrings get long enough to need humanization (multi-paragraph module headers, README-length docstrings on public APIs), the same doctrine applies, but with two extra rules:

- **Docstring openers should be a single sentence in the imperative or third-person present.** `Return the parsed event count.` Not `This function will return the parsed event count for you.`
- **Do not restate the function signature in prose.** A docstring for `def parse_event(payload: dict) -> int` that begins "This function takes a dictionary called `payload` and returns an integer" is sheen. The reader can see the signature. The docstring's job is the *behavior*, the *failure modes*, and the *invariants*.

Common AI tells in docstrings:

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- "This function is designed to…" → cut "is designed to", start with the verb
- "It takes the following parameters:" → use the docstring format (`Args:` / `:param:`) and stop announcing
- "Returns: An integer representing the count of…" → "Returns: number of parsed events. -1 on schema mismatch."
<!-- human-writer:ignore-end -->

Inline comments rarely trigger AI detection (too short), but the same instinct applies: `# Increment counter` next to `counter += 1` is noise. `# Counter wraps at 2^32, see issue #418` is signal.

## Changelog entries

Changelogs are a technical doc subgenre with very specific failure modes. AI-flavored changelog entries cluster around:

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- "يسعدنا أن نعلن…" — never in an internal changelog
- "يجلب هذا الإصدار…" — drop "يجلب", just list what changed
- "تحسينات متنوعة وإصلاحات للأخطاء" — meaningless filler
- Section headers in marketing language ("أبرز ما في الإصدار"، "الجديد") for an internal-only release
<!-- human-writer:ignore-end -->

Good changelog voice is past-tense, terse, one line per change, grouped by Added / Changed / Fixed / Removed (Keep a Changelog format). No prose intro to a release section. The version number and date are the intro.

## Pre-publish checklist (technical)

Before publishing internal technical content, confirm:

```markdown
- [ ] Ran `analyze.py --type technical --lang ar` — score ≤ 24
- [ ] No "في هذا المستند، سوف…" opener
- [ ] No "خاتمة" section (or it's >500 words and earns it)
- [ ] Every "قوي / قابل للتوسع / تسخير / شامل" passes the "replace with 'جيد'" test
- [ ] Code blocks are not surrounded by paragraphs that re-explain them
- [ ] No "برمجة سعيدة!" / "هيا بنا!" filler
- [ ] One sentence per concept; no restating
- [ ] If it's a PROJECT_HISTORY entry: past tense, dated, no marketing words
- [ ] If it's a design doc: opens with problem statement, not context
```

## Calibration with `analyze.py`

Technical weights from `rules.yaml`:

```yaml
technical:     {statistical: 0.5, stylistic: 0.7, structural: 0.3}
```

All three axes are weighted DOWN relative to marketing. The reasoning:

- **Statistical (0.5).** Technical writing has naturally lower sentence-length stdev. A series of API descriptions all run 8-12 words. That is correct, not robotic.
- **Stylistic (0.7).** Still the dominant axis, because AI sheen lives here. But many tier-1 vocab words have legitimate technical use, so the per-hit penalty is reduced.
- **Structural (0.3).** Parallel structure in code-doc lists is expected. Lexical repetition of technical terms (the function name appears 20 times in its own documentation) is fine.

Practically: a technical doc with a stylistic score of 25 is concerning; a structural score of 25 usually is not.

## Worked example

A ~200-word technical opener, rewritten.

### Before (AI-flavored, score ~52)

<!-- human-writer:ignore-start (deliberately AI-flavored sample for teaching; must not self-flag) -->
> ## نظرة عامة
>
> في هذا الدليل الشامل، سوف نستكشف القدرات القوية لنظام الترحيل لدينا، المصمَّم لنقل بنية الفوترة بسلاسة من نموذج الدفع حسب الاستخدام إلى نموذج أكثر قابلية للتوسع قائم على الدفع لكل حدث. بنهاية هذا المستند، سيكون لديك فهم شامل للقرارات المعمارية وخطوات الترحيل والحالات الحدّية المتنوعة التي واجهناها أثناء التنفيذ. سواء كنت مطوّرا يسعى للتكامل أو صاحب مصلحة يريد فهم الفوائد، سيرشدك هذا الـ README خلال كل جانب من العملية. هيا بنا!
<!-- human-writer:ignore-end -->

Issues, in order of appearance:

<!-- human-writer:ignore-start (quoted fragments from the bad example above) -->
- "الدليل الشامل" — tier-1 sheen
- "سوف نستكشف" — AI construction (announce-then-deliver)
- "القدرات القوية" — "قوي" fails the "replace with 'جيد'" test
- "بسلاسة" — "سلس" is almost never legitimate
- "أكثر قابلية للتوسع" — sheen, no measurement
- "بنهاية هذا المستند" — announce-then-deliver
- "فهم شامل" — sheen
- "سواء كنت … أو" — tier-1 AI construction
- "سيرشدك هذا الـ README خلال كل جانب" — AI meta-narration
- "هيا بنا!" — sign-off filler
<!-- human-writer:ignore-end -->

Six tier-1 tells and three AI constructions in one paragraph. The doc has not started yet.

### After (clean, score ~12)

> يوثّق هذا الـ README الترحيل من الفوترة حسب الاستخدام إلى الفوترة لكل حدث في خدمة القياس. كان نموذج الاستخدام يفرط في تحصيل رسوم حسابات الطبقة المجانية بنحو ٨٪ لأن قراءات الذاكرة المؤقتة كانت تُحتسب أحداثا قابلة للفوترة؛ نموذج الحدث يحصّل فقط على استدعاءات `commitResult()`.
>
> جرى الترحيل في ٢٠٢٦-٠٣-١٤. ظهرت حالتان حدّيتان (انظر §٣): الكتابات المجمّعة احتُسبت حدثا واحدا، وإعادات المحاولة على الكتابات الفاشلة فُوترت مرتين في النشر الأول.

The second version is shorter, contains specific dates and numbers, names the actual problem, and trusts the reader to know what the two billing models mean. The reader is an engineer on this codebase; treat them accordingly.

## Edge case: when the doc IS for non-engineers

Sometimes an internal doc has a mixed audience, like a runbook read by both on-call engineers and a non-technical product manager. The temptation is to add an "نظرة عامة لغير التقنيين" section in marketing prose. Resist this.

Better: write the doc for engineers, then add a separate one-paragraph "سياق" block (≤ 60 words, no sheen vocab) at the top. The runbook stays a runbook. The PM gets a paragraph they can read without scrolling through alert thresholds.

If the audience is genuinely majority non-technical (board memo, exec briefing), this is not a technical doc. Switch to the `marketing` or `short-comms` adapter.

## See also

- `tells-stylistic-ar.md`: vocabulary tells, with the suspect-word lists
- `humanization-techniques.md`: apply LIGHTLY for technical writing; most humanization moves (varied sentence rhythm, conversational asides) are marketing tools
- `checklists.md`: full pre-publish checklists across content types
- a dedicated structure/content tool: for public marketplace READMEs (use `adapter-marketing.md`, not this one)
