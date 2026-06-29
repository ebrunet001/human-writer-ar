# Humanization Techniques

> Doctrine for the `human-writer-ar` skill. The ten moves that turn AI-shaped prose into human-shaped prose. Apply in WRITE mode while drafting; apply in CLEAN mode as a targeted checklist. The doctrine is identical to the other family members; the worked examples are rewritten in Arabic (MSA), with one EN example kept for cross-reference. All Arabic text is in logical character order — RTL is a display concern only.

The techniques are ordered by impact-per-edit. If you can only apply one, apply #1. If you can apply two, add #5. If you have time for everything, the full ten will cut analyzer score by 30–50 points on a typical AI draft.

Each technique below answers the same four questions:

1. **Definition.** What is it?
2. **Why it works.** Which statistical / stylistic signal does it disrupt?
3. **Worked examples.** Before and after, in Arabic (plus EN where useful).
4. **How to apply.** Proactive in WRITE mode, targeted in CLEAN mode.

---

## 1. Vary sentence length deliberately

**Definition.** Alternate short (≤ 6 words) and long (≥ 25 words) sentences. Aim for a standard deviation of sentence lengths ≥ 8.

**Why it works.** AI prose averages 18–22 words per sentence with a low standard deviation (typically 3–5). The analyzer measures this via `sentence_length_stdev` (splitting on ؟ ؛ . for Arabic) and flags anything under ~8. Variance is the cheapest, highest-signal humanization edit available.

### Rhythm patterns to build in

| Pattern | Shape | Use case |
|---|---|---|
| **Short-short-long** | 5w + 4w + 28w | Set up a claim, then expand |
| **Long-short** | 30w + 4w | Build then punch (very human) |
| **Fragment-long** | 2w + 25w | Topic + dump |
| **Short-medium-fragment** | 6w + 15w + 3w | Cadence variation |
| **Run-on with semicolon** | 35w with ؛ | One thought, two beats |

Use the Arabic semicolon ؛ to extend without resetting rhythm. Use periods to reset hard. Use the Arabic comma ، to slow without breaking. Explicit fragments ("هذا كل شيء.", "وانتهى.", "نقطة.") are the strongest rhythm breakers.

### Worked example (AR)

Bad (uniform; lengths 14, 13, 15, 12; stdev ~1.2):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> تُصدّر أداتنا للتحليل ملف CSV لكل صنف في الكتالوج. تتحدث كل ليلة وفقا لبيانات المنافسين. يمكن للمستخدمين التصفية حسب الفئة أو المنطقة أو المورد. تعرض اللوحة تطور الأسعار خلال ٩٠ يوما.
<!-- human-writer:ignore-end -->

Good (lengths 3, 12, 28, 5, 18; stdev ~10):

> تصدير CSV. صف واحد لكل صنف، عمود واحد لكل مؤشر سعر. تعمل الأداة كل ليلة وتجمع الأسعار من ثلاثة مصادر بيانات داخلية وتدفقين شريكين. تصفّي في إكسل وانتهى. تسعون يوما من السجل، تغطي تقريبا كل فجوة مخزون رأيناها في ٢٠٢٥.

### Worked example (AR) (second pattern)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> يسترجع السكربت البيانات من المصدر. ينظّفها وفق القواعد المضبوطة. يخزّنها في قاعدة PostgreSQL. يتيحها عبر واجهة REST.
<!-- human-writer:ignore-end -->

Good:

> يبتلع السكربت السطر، ينظّفه بقواعدنا المنزلية (التي لا تصمد أبدا أمام إعادة تصميم المصدر) ويدفعه إلى Postgres. ثلاثة أعمدة. انتهى. واجهة REST تخرج الباقي — وتركنا المخرجات بصيغة JSON لأنها ما تريده الواجهة، لا أكثر.

### How to apply

**WRITE mode (proactive).** As you draft, force yourself to drop a one- or two-word sentence after every long one. If you've written three sentences of similar length, the next one *must* be a fragment or a 30+ word run-on.

**CLEAN mode (targeted edit).** Run `analyze.py` and look at `sentence_length_stdev`. If under 8, identify the three longest paragraphs and rewrite them by:
1. Splitting one mid-length sentence into a fragment + a sentence.
2. Joining two short sentences into one long, comma-spliced or semicolon-linked sentence.
3. Adding one explicit fragment ("انتهى.", "هذا كل شيء.", "ونقطة.").

---

## 2. Inject one opinion or specific anecdote per ~300 words

**Definition.** Every 300 words or so, insert one of: a first-person take, a named entity, a specific number, or a small concrete story.

**Why it works.** AI prose is information-dense but opinion-empty and entity-light. LLMs default to generic claims because generic claims minimize the chance of being wrong. Specific entities ("ريوخا ٢٠١٨", "٤٧ طلبا/ثانية", "فريق المشتريات") are statistically rare in AI output; they're high-signal markers of authored prose because they couldn't be generated without first-hand context.

### What counts as an "opinion"

A take that someone could disagree with. Not "X جيد لـ Y"; that's a fact-claim. But "كنتُ سأتجاوز X لأي شيء أقل من ١٠٠ صنف لأن العائد لا يبرّره" is an opinion: opinionated, specific, defensible.

| Not an opinion | Opinion |
|---|---|
| "الأداء مهم" | "الأداء هو الشيء الوحيد المهم تحت ١٠٠٠ طلب/ثانية؛ ما عداه مضيعة للوقت." |
| "اختر الأداة المناسبة" | "لا تستخدم Playwright لصفحات ثابتة. Cheerio أسرع وصيانته أقل." |
| "التسعير مهم" | "أغلب استراتيجيات التسعير تفشل لأن من يضع السعر لا يتحدث أبدا مع من يقدّم العرض." |

### What counts as "specific"

A named entity (person, place, product, version), a date, a number with units, or a concrete event. Not "سوق كبير"; قل أمازون. Not "مؤخرا"; قل "مارس ٢٠٢٤". Not "المستخدمون"; قل "فريق مشتريات من ١٢ شخصا".

### How to inject without breaking flow

Three placements work:

1. **Mid-paragraph aside.** "الخدمة (قِسناها عند ٤٧ طلبا/ثانية على نسخة بذاكرة ٢ غيغابايت) تدير الحزم…"
2. **End-of-paragraph kicker.** "…وهكذا. بصراحة، هنا يتوقف معظم الناس — ومن هنا نبدأ نحن."
3. **New sentence between two claims.** "واجهات API تحتاج حدود معدّل. بالمناسبة، الواجهة الرسمية تقطعك عند ٦٠ في الدقيقة على الخطة المجانية. فتصمّم حول ذلك."

### Bad vs good (AR)

Bad (بديهية غامضة):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> يتطلب التخزين المؤقت على نطاق واسع أدوات مختارة بعناية. اختيار النهج المناسب قد يُحدث فرقا كبيرا في الأداء.
<!-- human-writer:ignore-end -->

Good (موقف محدد):

> التخزين المؤقت على نطاق واسع يتلخص في قرار واحد: أتخزّن الاستجابة كاملة أم نتائج الاستعلامات المكلفة فقط؟ تخزين الاستجابة الكاملة بسيط (~٥ أسطر في nginx) لكن نسبة الإصابة تتوقف عند ٤٠٪ بمجرد تغيّر معاملات الاستعلام. تخزين النتائج في Redis كود أكثر لكنه يحفظ ٨٥٪ من الإصابات على حركتنا. نختار تخزين النتائج للمسارات كثيفة القراءة، وتخزين الاستجابة الكاملة للصفحات الثابتة.

### How to apply

**WRITE mode.** Before writing, list 3 specific facts/anecdotes/opinions you can drop into the piece. Place one every ~300 words.

**CLEAN mode.** Search the draft for paragraphs that contain zero proper nouns, zero numbers, and zero first-person markers. Each such paragraph needs one injection. If the draft has *none* across the whole text, that's an emergency — the piece reads as generic and no amount of sentence-length variance will save it.

---

## 3. Use asymmetric bullets

**Definition.** In any bulleted list, deliberately vary the length, structure, opening word, and grammatical shape of each item.

**Why it works.** AI bullets are parallel by default: same verb (`بناء / بناء / بناء`), same length (8–12 words each), same shape (verb + object). The analyzer flags this when > 80% of bullets share the same first or last word. Asymmetry breaks the fingerprint.

### Three asymmetry axes

| Axis | Bad (symmetric) | Good (asymmetric) |
|---|---|---|
| **Length** | All 6–8 words | Mix of 2-word fragments and 25-word callouts |
| **Opener** | All start with a verb | Mix verbs, nouns, questions, fragments |
| **Shape** | All `verb + object` | Mix commands, observations, questions, mini-paragraphs |

### Worked example (AR)

Bad (متماثل في الطول والفعل):

```
- تحليل ملفات الإدخال
- استخراج بيانات الأسعار
- حفظ النتائج في مجموعة البيانات
- تصدير إلى CSV
```

Good (غير متماثل):

```
- تحليل ملفات الإدخال (JSON مباشر للبسيط، محلّل مخصص للقديم — انظر `parsers.ts`)
- الأسعار: نلتقط السعر المعلن والعرض و«التوفير» منفصلة لأن المصدر يقرّب كما يحلو له
- مجموعة البيانات ← CSV: `make export`، ونقطة
- وبعد؟ هنا يتوقف معظم الناس. نحن نوصله بـ Metabase لعرض الاتجاه.
```

What changed: bullet 1 has a parenthetical with a code path; bullet 2 opens with a noun and uses a colon callout; bullet 3 is one short clause with an arrow; bullet 4 is a rhetorical question + two sentences.

### Sub-bullets for asymmetry

Adding a single sub-bullet under one item (and only one) breaks the visual rhythm hard:

```
- تحليل ملفات الإدخال
  - محلّل مخصص فقط للصيغة القديمة — يضيف ~٢٠٠ مللي ثانية/ملف
- استخراج الأسعار
- تصدير CSV
```

The lone sub-bullet kills the AI shape. It signals "a human chose to elaborate exactly here."

### When symmetric bullets ARE appropriate

Parallel lists where the reader compares like-with-like deserve symmetric formatting: step-by-step procedures, API reference tables, comparison matrices, pricing tiers. There, parallelism is *informational*. The rule: **prose-embedded lists should be asymmetric; reference/comparison structures can stay symmetric**.

### How to apply

**WRITE mode.** When you reach for a bullet list, draft it normally, then deliberately rewrite at least 2 of the 4 bullets to use a different shape.

**CLEAN mode.** Check `bullet_parallelism_ratio` from `analyze.py`. If ≥ 0.80, rewrite half the bullets to NOT start with the dominant verb.

---

## 4. Break tricolons: vary list sizes

**Definition.** Resist the "rule of three" reflex. Use lists of 2, 4, or 5 items. Cap the explicit three-item waw-joined tricolon ("X، Y، وZ") at 1 per 200 words.

**Why it works.** AI prose is saturated with tricolons. Typical specimens:

<!-- human-writer:ignore-start (citation: tricolon tells quoted, not used) -->
"سريع، موثوق، وقابل للتوسع"، "بناء، اختبار، ونشر"، "صغير، متوسط، وكبير".
<!-- human-writer:ignore-end -->

The cadence is comforting and tidy (it echoes the classical سجع rhythm), which is exactly why LLMs default to it. The analyzer flags density above 1 per 200 words.

### Specific alternatives

| Reflex | Alternative | Example |
|---|---|---|
| Tricolon (3 items, و) | **List of 2** | "سريع ورخيص." |
| Tricolon (3 items, و) | **List of 4** | "سريع، رخيص، مخزّن مؤقتا، ومُدقّق." |
| Tricolon | **List of 5 with asyndeton** | "سريع، رخيص، مخزّن مؤقتا، مُدقّق، يُنشر بأمر واحد." |
| Tricolon | **List of 3 with asyndeton (no و)** | "سريع، موثوق، قابل للتوسع." |
| Tricolon | **Pair + parenthetical** | "سريع وموثوق (ورخيص أيضا، لكن هذه هدية)." |

Asyndeton — dropping the final و — is the cheapest variation. It keeps the three-beat rhythm but loses the AI-signature "، و" connector.

### Worked example (AR)

Bad (ثلاثة تريكولونات في جملتين):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> الأداة سريعة، موثوقة، ودقيقة. تدير الحزم، المتغيرات، والمناطق. تعمل يوميا، أسبوعيا، وعند الطلب.
<!-- human-writer:ignore-end -->

Good (ميزانية تريكولون واحدة مصروفة؛ أحجام قوائم متنوعة):

> تدير الأداة الحزم والمتغيرات في ١٢ منطقة. تشغيل يومي، أو عند الطلب لمراجعة موضعية. سريعة، موثوقة، دقيقة: بهذا الترتيب نُحسّنها.

### How to apply

**WRITE mode.** Keep a mental "tricolon budget" of 1 per 200 words. If you've already used one, force the next list into 2 or 4 items, or use asyndeton.

**CLEAN mode.** Search the draft for "، و" followed by a single word ending the sentence. Count instances. If > 1 per 200 words, rewrite the lowest-impact occurrences first.

---

## 5. Cut all hedging openers

**Definition.** Delete the AI-templated qualifier phrases that front-load sentences. State the claim directly.

**Why it works.** Hedging openers are the most distinctive AI signature in long-form prose. They're space-filler with zero information value. Removing them is the highest words-saved-per-edit move available.

### Full forbidden list (AR)

<!-- human-writer:ignore-start (citation table: tells quoted, not used) -->
| Forbidden opener | Why it's a tell |
|---|---|
| "تجدر الإشارة إلى" | The #1 MSA topic-sentence tell |
| "من الجدير بالذكر" | Same family |
| "من المهم أن نلاحظ" | Calque of "it's important to note" |
| "من الضروري أن نشير" | Templated, pedantic |
| "لا بد من الإشارة" | Formal AI register |
| "مما لا شك فيه" | Empty certainty assertion |
| "في الختام" | Conclusion template |
| "في نهاية المطاف" | Calque of "ultimately" |
| "علاوة على ذلك" (as opener) | Connector tell |
<!-- human-writer:ignore-end -->

### Worked example (AR)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> تجدر الإشارة إلى أن الخدمة تحتاج ١ غيغابايت من الذاكرة على الأقل. ومن المهم أن نلاحظ أن الأداء يتراجع مع الحزم. ولا بد من الإشارة إلى أن المخطط قد يتغير.
<!-- human-writer:ignore-end -->

Good (الخيار ١، مباشر):

> تحتاج الخدمة ١ غيغابايت كحد أدنى. الأداء يتراجع مع الحزم (أصلحناه في الإصدار الثاني). المخطط قد يتغير.

Good (الخيار ٢، المؤهّل في السطر):

> تحتاج الخدمة ١ غيغابايت كحد أدنى، غير قابل للتفاوض على الخطة المجانية. على أصناف الحزم يتباطأ ٤٠٪؛ مشكلة معروفة للإصدار الثاني. سيتغير المخطط في الربعين القادمين.

Same content. Half the words. Sounds like someone actually wrote it.

### How to apply

**WRITE mode.** Never start a sentence with a hedging opener. If you catch yourself typing "تجدر الإشارة"، delete and rewrite.

**CLEAN mode.** Grep the draft for every entry in the forbidden list. Delete each occurrence and rewrite the remaining sentence. This is the single highest-ROI CLEAN-mode operation.

---

## 6. Use idiosyncratic markers

**Definition.** Deliberately build 1–2 recurring tics per piece (a favored connector, a pet phrase, a quirky structural pattern) that the analyzer cannot fingerprint but that human readers attribute to authorial personality.

**Why it works.** Human writers have tics. AI prose is *too clean*. It avoids signature moves because LLMs are trained to produce average-of-corpus output. A deliberate tic registers as personality.

One tic per ~500 words is invisible to the analyzer (which thresholds on density) but registers to readers. Two tics per 200 words is noise.

### AR-specific tics

| Tic | Cadence | Use case |
|---|---|---|
| "انظر،" as sentence pivot | 1 per ~1000 words | Pivot to a strong claim |
| "والحاصل،" as paragraph closer | 1 per ~800 words | Casual register |
| "يعني،" as connector | 1 per ~400 words | Spoken register |
| "بصراحة،" as opener | 1 per ~600 words | First-person take |
| "وانتهى." / "ونقطة." as fragment | 1 per ~600 words | Hard stop after a claim |
| "وهكذا." as closer | 1 per ~1000 words | Wrap-up beat |

### Worked example (AR)

Without tic (نظيف، بطعم الذكاء الاصطناعي):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> تعمل مهمة المزامنة كل ١٥ دقيقة. تراقب ٥٠ صنفا افتراضيا. تصل النتائج إلى عرض في Postgres يطّلع عليه الفريق من Metabase.
<!-- human-writer:ignore-end -->

With "انظر،" and "يعني":

> تعمل المهمة كل ١٥ دقيقة. انظر، جرّبنا ٥ فقتلتنا حدود المعدّل — ١٥ هي الأرضية. تراقب ٥٠ صنفا افتراضيا. يعني، تنتهي النتائج في عرض Postgres يطّلع عليه الفريق من Metabase على أي حال.

The two tics are invisible to bot detection but read as a person with a voice.

### How to apply

**WRITE mode.** Before drafting, pick one or two tics. Use them at the cadence listed. Resist adding more.

**CLEAN mode.** If the piece is otherwise good but reads as bot-clean, inject *one* tic at *one* natural insertion point. Re-run the analyzer.

---

## 7. Inject digressions and parentheticals

**Definition.** Humans wander. AI stays on-track relentlessly. Insert one short digression per ~500 words. Use parentheses for genuine asides.

**Why it works.** LLMs are trained to follow the prompt without drift. The result is unnaturally focused prose: every paragraph stays inside its topical lane. Human writers tangent constantly. The drift is the signal of authentic thought. The key constraint: the digression must *return* to the main thread.

### Worked example (AR)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
بلا استطراد:

> تسعير النبيذ متقلب. المنتجون يتكيفون بمراقبة إشارات السوق.
<!-- human-writer:ignore-end -->

With a digression:

> تسعير النبيذ متقلب (هبط بورغندي ٢٠٢٠ بنسبة ١١٪ في ثلاثة أسابيع). المنتجون يتكيفون — على الأقل من يراقب إشارات السوق كل أسبوع.

### Worked example (AR) (longer)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
بلا استطراد:

> ترحيل قاعدة البيانات يحتاج تحضيرا جيدا. المخطط والفهارس والقيود — كلها يجب ضبطها وإلا انكسر النشر في الإنتاج.
<!-- human-writer:ignore-end -->

With a digression:

> ترحيل قاعدة البيانات يحتاج تحضيرا جيدا. مخطط، فهارس، قيود — إغفال واحد والنشر ينكسر في الإنتاج. (تعلّمناها بالطريقة الصعبة في ترحيل في مارس ٢٠٢٤: المخطط نظيف، الفهارس نظيفة، لكننا نسينا قيد NOT NULL على عمود مليء بقيم NULL. تراجعنا في ٤٧ دقيقة.) والحاصل، حزام وحمّالة في كل طبقة.

### How to apply

**WRITE mode.** Plan one digression per major section (every ~500 words). Mark insertion points in your outline.

**CLEAN mode.** Read each paragraph and ask: "Did the writer think of anything specific while writing this?" If every paragraph is locked to its topic, inject one parenthetical with a real specific fact.

---

## 8. Choose concrete over abstract

**Definition.** When given the choice between a generic noun and a specific one, always pick the specific. AI defaults to abstractions ("حلول", "شركات", "تدفقات العمل"); humans default to concrete examples ("جدول إكسل من ١٤ ورقة", "فريق مشتريات من ١٢ شخصا", "تحضير صباح الاثنين").

**Why it works.** AI prose lives in the abstraction layer because abstractions are safer. Concrete nouns ("Postgres", "المهمة الليلية", "١٢ دقيقة") commit to facts that must be true. Their presence is a strong signal of first-hand writing.

### Abstract → concrete substitution table (AR)

<!-- human-writer:ignore-start (citation table: abstract AI nouns quoted, not used) -->
| Abstract (AI default) | Concrete (human alternative) |
|---|---|
| "الشركات" | "فريق مالية من ١٢ شخصا" / "مناوبة الطوارئ" |
| "حلول" | الأداة المحددة: "اللوحة"، "المهمة المجدولة"، "تصدير CSV" |
| "تدفقات العمل" | الخطوة المحددة: "تحضير الاثنين"، "سكربت الاستيراد" |
| "المستخدمون" | "وكيل الدعم الذي يفرز التذاكر" / "محلل طاولة التداول" |
| "البيانات" | "٤٧ عمودا: المعرّف + المبلغ + الحالة + الطابع الزمني" |
| "الأداء" | "٤٧ طلبا/ثانية على نسخة ٢ غيغابايت" |
| "قابلية التوسع" | "شغّلناها على ٢٫٣ مليون سطر في ٩ ساعات" |
| "رؤى قيّمة" | "الرقم المحدد الذي لم يكن لديك من قبل" |
| "أصحاب المصلحة" | سمّهم: "المدير المالي"، "فريق المشتريات" |
| "المنظومة" | سمّها: "سجل npm"، "إضافات Postgres" |
<!-- human-writer:ignore-end -->

### Worked example (AR)

Bad (مجرّد):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> يساعد حلّنا الشركات على تحسين عملياتها.
<!-- human-writer:ignore-end -->

Good (ملموس):

> استبدلنا جدول إكسل من ١٤ ورقة كان الفريق يستخدمه للتقارير بلوحة واحدة. تحضير صباح الاثنين هبط من ساعتين إلى ١٢ دقيقة.

### Worked example (AR) (longer)

Bad:

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> تتيح المنصة للمؤسسات تسخير البيانات الفورية لاتخاذ قرارات أفضل.
<!-- human-writer:ignore-end -->

Good:

> تدفع المنصة تغيّرات الحالة من ٨ خدمات إلى Slack، قناة بقناة وفريقا بفريق. حين يتجاوز زمن الاستجابة p99 عتبة ٥٠٠ مللي ثانية، تعرفه مناوبة الطوارئ في ٩٠ ثانية. أُغلقت ثلاثة حوادث الربع الماضي بتنبيهات أخرجتها الأداة قبل أول تذكرة عميل.

### How to apply

**WRITE mode.** Each time you reach for an abstract noun ("حل"، "المستخدمون"، "البيانات")، pause: "What's the concrete version?" Write that.

**CLEAN mode.** Grep the draft for the abstract nouns in the table above. Replace each with a concrete equivalent or rewrite the sentence around it.

---

## 9. Vary transitions, drop the formal connectors

**Definition.** AI-generated transitions are predictable and connector-heavy. Humans transition with simple conjunctions, restructure sentences, or skip transitions entirely. In Arabic this matters doubly because of the sentence-initial و/ف overuse problem.

**Why it works.** The connector-class AI tells are the formal-register conjunctions below. They appear when an LLM tries to make logical structure visible at the surface, which humans rarely do. And AI opens clause after clause with "و…" until the prose flattens into a list.

### Forbidden transitions (AR)

<!-- human-writer:ignore-start (citation list: tells quoted, not used) -->
- علاوة على ذلك (as paragraph opener)
- بالإضافة إلى ذلك
- إضافة إلى ذلك
- فضلا عن ذلك
- وبالتالي
- نتيجة لذلك
- من ناحية أخرى (as paragraph opener)
- في هذا السياق (as paragraph opener)
- في هذا الصدد
- على صعيد آخر
<!-- human-writer:ignore-end -->

### AR alternatives

- **"و"** sparingly: yes, but cap sentence-initial و at ~1 in 4 sentences.
- **"لكن"**: sharp pivot.
- **"رغم أن"**: informal contrast / subordination.
- **"المشكلة أن"**: register varies but this is human.
- **"حسنا،"** / **"والحاصل،"**: Arabic rhythm markers.
- **"وإلا،"**: branching alternative.
- **Restructure**: often the cleanest transition is no transition — rewrite the next sentence to flow without a connector, or fold two clauses into one with subordination (الذي، حيث).

### Worked example (AR)

Bad (مثقل بالروابط وبواو البداية):

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
> تسترجع الخدمة أسعار أمازون. وعلاوة على ذلك، تراقب مستويات المخزون. وبالإضافة إلى ذلك، تتكامل مع Slack. ومن ناحية أخرى، تدعم التصدير اليومي.
<!-- human-writer:ignore-end -->

Good (متنوّع):

> تسترجع الخدمة أسعار أمازون، وتراقب المخزون. التكامل مع Slack يأتي في الإصدار الثاني. تصدير يومي — أو كل ساعة إن طلبت.

### How to apply

**WRITE mode.** Never use the forbidden list. At a transition point, pick from the human alternatives or restructure. Watch your sentence-initial و count.

**CLEAN mode.** Grep for every forbidden transition. Delete and rewrite. Then count sentence-initial و and vary it down. One of the fastest, highest-impact CLEAN-mode operations.

---

## 10. Build in productive imperfection

**Definition.** Humans pause, repeat for emphasis, change midstream, use casual interjections. AI hyper-corrects. A light imperfection ratio (1–2 instances per 500 words) registers as human without seeming sloppy.

**Why it works.** LLMs are trained out of the small imperfections real writing carries. Self-corrections, repetitions for emphasis, and casual interjections are statistically scarce in AI output and common in human prose. A small dose flips the signal.

### Imperfection categories (AR)

| Category | Example | Cadence |
|---|---|---|
| **Colloquial interjection** | "بصراحة،"، "انظر،"، "يعني،" | 1 per ~400 words (overlaps with #6); informal register only |
| **Repetition for emphasis** | "جيد. جيد جدا." | 1 per ~500 words |
| **Self-correction** | "تحرّك السعر ١١٪ — أو بالأحرى ١٢٪ إن حسبت العمولات." | 1 per ~700 words |
| **Mid-sentence pivot** | "جرّبنا أرخص خيار، لكن — المهم، تتخيّل النتيجة." | 1 per ~800 words |
| **Trailing "وما إلى ذلك"** | "…أو ما يناسب مكدّسك، وما إلى ذلك." | 1 per ~1000 words (informal only) |

### Worked example (AR)

<!-- human-writer:ignore-start (before-example, deliberately AI) -->
بلا عيب (ذكاء اصطناعي مفرط التصحيح):

> تحرّك السعر ١١٪ في ثلاثة أسابيع. هذا مهم. يجدر التحقق من السبب الكامن.
<!-- human-writer:ignore-end -->

With imperfection (تصحيح ذاتي + اعتراض):

> تحرّك السعر ١١٪ في ثلاثة أسابيع — أو بالأحرى ١٢٪ إن حسبت العمولات. هذا كثير. بصراحة، يستحق أن ننظر فيه.

### Calibration warning

Imperfection is dosed. Too much and you cross from "human writer" to "sloppy draft". The cadences above are upper bounds. In a technical doc, lean low. In casual marketing copy, the upper end works. Note: colloquial (عامية) interjections are register-dependent — fine in informal marketing, wrong in formal MSA editorial.

### How to apply

**WRITE mode.** Use natural interjections where the register allows. Add one self-correction or casual interjection per ~500 words.

**CLEAN mode.** Identify one paragraph that reads as too-polished and inject a single self-correction.

---

## Bonus: how to combine techniques

The techniques compound. Applying #1 alone drops `sentence_length_stdev` flags. Applying #1 + #5 + #9 drops the three most common AI signatures simultaneously. Below is a worked example showing the cumulative effect on an Arabic paragraph.

### Starting paragraph (vanilla AI output, ~90 words)

<!-- human-writer:ignore-start (citation: deliberately AI) -->
> في عالم اليوم للتجارة الإلكترونية، تُعد مراقبة أسعار المنافسين أمرا جوهريا. تقدّم أداتنا لذكاء الأسعار حلا سلسا، متطورا، وبديهيا. لا يقتصر الأمر على تتبّع الأسعار، بل تمكين فريقك برؤى قابلة للتنفيذ. سواء كنت شركة ناشئة أو مؤسسة كبيرة، تساعدك منصّتنا على تجاوز تعقيد التسعير الديناميكي. علاوة على ذلك، تتكامل بسلاسة مع تدفقات عملك. وفي الختام، ستتمكن من اتخاذ قرارات مبنية على البيانات والتفوق على منافسيك.
<!-- human-writer:ignore-end -->

**Analyzer baseline:** suspect vocab high (في عالم اليوم، جوهري، سلسا، متطورا، تمكين، قابلة للتنفيذ، بسلاسة)، construction "لا يقتصر الأمر على"، "سواء كنت … أو"، conclusion "في الختام"، connector "علاوة على ذلك". Estimated AI-probability: HIGH/CRITICAL.

### Final humanized version (same content, ~90 words)

> الأسعار تتغير كل ساعة على أمازون في ذروة موسم الأعياد. انظر — أداتنا تعمل كل ١٥ دقيقة على أغلى ٥٠ صنفا لديك وتؤشّر أي حركة تتجاوز ٣٪. وانتهى. اللوحة عرض في Postgres (بلا واجهة أنيقة) لأن الفريق الذي يحتاجها يعيش في Metabase على أي حال. بنيناها لتاجر متوسط الحجم في ٢٠٢٤، بعد أن خُفّض سعره ستة أسابيع متتالية قبل أن ينتبه. تريد المنصّة نفسها؟ كل شيء نحو ٢٠٠ سطر بايثون على مهمة مجدولة.

What changed: temporal abstraction → concrete season + marketplace; tricolon and suspect vocab gone; "لا يقتصر الأمر على…" and "سواء كنت…" gone; "في الختام" gone; "علاوة على ذلك" gone; added an "انظر —" tic, a "وانتهى." fragment, a real 2024 anecdote, concrete numbers (١٥ دقيقة، ٥٠ صنفا، ٣٪)، and a closing pointer.

### Order of operations summary

| Order | Technique | Reason |
|---|---|---|
| 1 | #5 Cut hedging openers | Highest words-saved, fastest fix |
| 2 | #9 Vary transitions (and و overuse) | Cuts the connector class in one pass |
| 3 | #1 Vary sentence length | Statistical signal most analyzers test |
| 4 | #4 Break tricolons | Density signal — easy to target |
| 5 | #3 Asymmetric bullets | Only if the piece has lists |
| 6 | #8 Concrete over abstract | Compounds with #2 |
| 7 | #2 Inject opinion/anecdote | Highest authorial-signal move |
| 8 | #7 Digressions | Adds drift |
| 9 | #6 Idiosyncratic markers | Final personality layer |
| 10 | #10 Imperfection | Final humanity layer |

The first five fix the statistical signature. The last five inject the authorial signal. Throughout, sweep the Arabic typography: replace Latin , ; ? with ، ؛ ؟, delete every "—" and tatweel ـ.

---

## See also

- `tells-stylistic-ar.md` — Arabic vocabulary and construction lists that techniques #5, #8, #9 reference directly.
- `tells-statistical.md` — the metrics (sentence-length stdev, bullet parallelism, tricolon density, em-dash density) these techniques target.
- `tells-structural.md` — bullet, header, conclusion anti-patterns that techniques #3 and #4 disrupt.
- `adapter-marketing.md` / `adapter-short-comms.md` / `adapter-technical.md` / `adapter-editorial-seo.md` — content-type-specific calibration.
