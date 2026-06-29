# Stylistic Tells — Arabic (MSA)

> Doctrine for the `human-writer-ar` skill, Arabic (Modern Standard Arabic) side. Same axes as the master's `tells-stylistic-fr.md` / `tells-stylistic-en.md`, specialized to MSA. Doctrine is written in English; the Arabic words, phrases, and examples are the *surface* the analyzer matches against. All examples are in logical character order — RTL is a display concern only.

Arabic ChatGPT/Gemini output runs *louder* than its English counterpart for three reasons:

1. The model defaults to a high, formal MSA register learned from journalism (Al Jazeera, Al-Sharq Al-Awsat), academic prose, and corporate copy. That register is naturally inflated — "تجدر الإشارة إلى", "مما لا شك فيه", "في عصرنا الحالي" — and it reads as press-release boilerplate the moment it touches anything concrete.
2. English formulas leak through as calques: `dive into` → "انغمس في", `innovative solutions` → "حلول مبتكرة", `seamless` → "سلس", `leverage` → "تسخير / الاستفادة من", `let's explore` → "دعونا نستكشف". A native ear catches these immediately; the model never does.
3. **Connector overuse.** Arabic glues clauses with the proclitic و (waw) and ف (fa). AI sprays "و" and "علاوة على ذلك" / "بالإضافة إلى ذلك" at the head of nearly every sentence, producing a flat, list-like rhythm that no careful Arabic writer uses.

Arabic has one structural feature no Latin sibling shares: its **own punctuation set** — the Arabic comma ، (U+060C), Arabic semicolon ؛ (U+061B), and Arabic question mark ؟ (U+061F). Native Arabic uses these; AI plain-text output frequently emits the **Latin , ; ?** instead (English habit). That substitution is both a typographic tell and a wired detector (`detect_latin_punct_in_arabic`).

---

## Suspect vocabulary (AR)

The list is calibrated against typical ChatGPT/Gemini Arabic output across marketing, editorial, and corporate copy.

**Clitic note.** Vocabulary entries are listed as **bare stems**. Arabic attaches single-letter proclitics (و = and, ف = then, ب = by/with, ك = like, ل = for) and the definite article ال directly to the stem with no space. The analyzer's `detect_suspect_vocab` is **clitic-aware**: the bare stem "كلمة" also matches "الكلمة", "وكلمة", "والكلمة", "بالكلمة". So you list "مبتكر" once and it catches "المبتكر", "ومبتكر", etc. Do NOT pre-prefix entries with ال or و.

**Hard rule.** Never use 2+ tier-1 items in the same paragraph. Cap any single tier-1 item at 1 per 500 words. Treat tier-1 items the way you would treat "delve" or "tapestry" in English: even one is suspicious; two is a giveaway.

**Soft rule.** Tier-2 items are legitimate in their domain. Track frequency: 3+ tier-2 items in a 300-word paragraph = the paragraph reads as AI-generated. Cap any single tier-2 item at 2 per 500 words.

**Pure-frequency rule.** Tier-3 items are individually invisible but flag the prose when 5+ co-occur in a single paragraph. The analyzer does NOT list tier-3 (false-positive risk on common Arabic words is too high for a per-stem matcher); the human reviewer does.

### Tier 1 — High signal (always avoid)

The Arabic equivalents of English `delve`, `tapestry`, `realm`. Each line: the phrase, why it's a tell, a human alternative.

#### Meta-comment openers / hedges

- **"تجدر الإشارة إلى"** / **"من الجدير بالذكر"** / **"جدير بالذكر"** — Why: meta-textual hedge; reads as report-writing. The single most frequent MSA AI topic-sentence opener of 2025–2026 (the Arabic "it is worth noting"). Alternative: drop the prefix, state the claim. "تجدر الإشارة إلى أن المبيعات ارتفعت" → "ارتفعت المبيعات ١٢٪."
- **"من المهم أن نلاحظ"** / **"من المهم الإشارة"** / **"من المهم أن نذكر"** — Calque of "it's important to note". Alternative: state the fact inline.
- **"من الضروري أن نشير"** / **"لا بد من الإشارة"** / **"ينبغي الإشارة"** — Same family, pedantic. Alternative: drop the meta-frame.
- **"كما هو معروف"** / **"كما نعلم جميعا"** / **"كما يعلم الجميع"** / **"من المعلوم"** — Appeal to phantom consensus no native writer uses unless ironically. Alternative: drop.

#### Certainty / emphasis phrases

- **"مما لا شك فيه"** / **"لا شك أن"** / **"بلا شك"** / **"دون أدنى شك"** — Why: heavy "without a doubt" assertion AI sprinkles on any claim. Alternative: drop, or back the claim with a number.
- **"من نافلة القول"** — Pompous "it goes without saying". Alternative: then don't say it.
- **"بكل تأكيد"** / **"بالتأكيد"** / **"بطبيعة الحال"** (as openers) — AI's "of course" reflex. Alternative: drop.

#### Spatial-temporal abstractions (date-free scene-setters)

- **"في عالم اليوم"** / **"في عالمنا اليوم"** — Why: meta-frame opener with no date, event, or actor. The canonical MSA AI opener. Alternative: name the year, the event. "في عالم اليوم سريع التغير" → "منذ ٢٠١٨…"
- **"في عصرنا الحالي"** / **"في عصرنا الحديث"** / **"في العصر الرقمي"** / **"في عصر الذكاء الاصطناعي"** / **"في عصر المعلومات"** — Same family. Alternative: a date, a fact.
- **"في الوقت الراهن"** / **"في الوقت الحاضر"** / **"في يومنا هذا"** (as scene-setters) — Vague time-stamps. Alternative: a concrete date.
- **"في عالم يتسم"** / **"في ظل التطور"** / **"في ظل التقدم"** — Lyrical opener. Alternative: name the actual situation.
- **"في خضم"** / **"في قلب"** (metaphorical, not literal) — Calque of "in the midst of / at the heart of". Alternative: "وسط", "داخل", or drop.

#### Context / framing connectors

- **"في هذا السياق"** / **"في هذا الصدد"** / **"في هذا الإطار"** / **"على هذا الصعيد"** — Why: meta-textual signposts AI uses to glue paragraphs. Alternative: drop; the next sentence carries the logic.
- **"في ضوء ذلك"** / **"في ضوء هذا"** — Calque of "in light of this". Alternative: "بناء على ما سبق" (sparingly), or restructure.
- **"بناء على ذلك"** / **"من هذا المنطلق"** — Schoolbook transitions. Alternative: "لذلك", restructure.
- **"من ناحية أخرى"** / **"على صعيد آخر"** (overused) — Alternative: "لكن", "أما".

#### Marketing inflators

- **"مبتكر"** / **"ثوري"** / **"رائد"** / **"متطور"** / **"متقدم"** — Why: empty self-praise adjectives AI sprays on products. Alternative: "جديد" + show why; cite a benchmark.
- **"حديث"** / **"عصري"** (as marketing inflators) — Alternative: a date.
- **"استثنائي"** / **"فريد"** / **"لا مثيل له"** / **"لا غنى عنه"** — AI's intensifier toolkit. Alternative: name what's actually different.
- **"حجر الزاوية"** / **"ركيزة أساسية"** / **"العمود الفقري"** — Structural metaphors AI scatters as if every concept needs one. Alternative: name the function ("X يعتمد على Y").
- **"نقلة نوعية"** — "a qualitative leap"; marketing cliché. Alternative: state the measurable change.

#### Hyped solution language

- **"حلول مبتكرة"** / **"حلول متطورة"** / **"حل شامل"** / **"حلا متكاملا"** — Why: the canonical "innovative/comprehensive solutions" cliché. Alternative: name the actual thing — "اللوحة", "التصدير CSV", "المهمة المجدولة".
- **"تجربة سلسة"** / **"سلس"** / **"سلسة"** / **"بسلاسة"** — Calque of "seamless experience". The strongest MSA rendering of "seamless" and a heavy tell. Alternative: "مباشر", "بلا تعقيد", "بلا خطوات إضافية".
- **"دون عناء"** / **"بكل سهولة"** — "effortlessly". Alternative: state the number of steps.

#### CTA / opener verbs

- **"دعونا نستكشف"** / **"دعونا نتعرف"** / **"دعنا نستكشف"** — Why: AI's "let's explore" opener. Banned as an introduction. Alternative: state the subject directly.
- **"هيا بنا"** — Conference-bro "let's go". Alternative: drop.
- **"انغمس"** / **"اكتشف"** / **"استكشف"** (as imperatives opening a piece) — Calque of "dive into / discover / explore". Alternative: name the subject.
- **"أطلق العنان"** / **"أطلق إمكانات"** / **"حرر إمكانات"** — Calque of "unleash / unlock the potential". Alternative: "استخدم", "استفد من" (sparingly).

#### Heavy verbs / verbal phrases

- **"تسخير"** (when it means "leverage", "تسخير الإمكانات") — Calque of "leverage / harness". Alternative: "استخدام", "الاستفادة من".
- **"تتيح"** / **"تمكن"** / **"يمكّن"** / **"تمكين"** — AI's "enables / empowers" reflex. Alternative: "تسمح بـ", or name the mechanism.
- **"تعزيز"** / **"يعزز"** (metaphorical "boost") — Alternative: "تحسين", "زيادة" + number.
- **"إحداث ثورة"** / **"يحدث ثورة"** — Calque of "revolutionize". Alternative: "يغيّر", state what changes.
- **"ترسيخ"** / **"تجسيد"** / **"بلورة"** — Heavy verbs where "تثبيت", "يمثّل", "يلخّص" would do. Alternative: the simple verbs.

### Tier 2 — Medium signal (contextual)

Legitimate in domain (e.g. "تحسين" in an SRE post is fine). Flag when 3+ co-occur in 300 words.

#### Connectors (overuse)

- **"علاوة على ذلك"** / **"إضافة إلى ذلك"** / **"بالإضافة إلى ذلك"** / **"فضلا عن ذلك"** / **"وعلاوة على"** — Schoolbook "furthermore / moreover". The strongest connector tells. Alternative: "و", "كما", restructure.
- **"كذلك"** / **"أيضا"** (as paragraph openers) — Alternative: drop or fold into the sentence.
- **"وبالتالي"** / **"ومن ثم"** / **"نتيجة لذلك"** (heavy) — Alternative: "لذلك", "فـ".
- **"في الواقع"** / **"وفي الواقع"** / **"في حقيقة الأمر"** (as filler) — AI's "in fact" reflex. Alternative: drop.
- **"في نهاية المطاف"** / **"في خاتمة المطاف"** — Calque of "ultimately / at the end of the day". Alternative: drop.

#### Conclusion templates

- **"في الختام"** / **"وفي الختام"** / **"في النهاية"** / **"ختاما"** — Conclusion templates. **"في الختام" is the strongest MSA closing tell.** Alternative: end on a fact.
- **"خلاصة القول"** / **"وخلاصة القول"** / **"باختصار"** / **"وفي الأخير"** / **"إجمالا"** — Closers that announce a summary. Either summarize, or don't — don't announce.

#### Importance / intensity adjectives

- **"حاسم"** / **"جوهري"** / **"أساسي"** / **"ضروري"** / **"حيوي"** / **"محوري"** / **"بالغ الأهمية"** — All inflators. AI sprays them. Alternative: drop, or quantify.
- **"ملحوظ"** / **"ملموس"** / **"هائل"** / **"كبير"** — Empty modifiers. Alternative: the number.

#### Abstract / buzzy nouns

- **"منظومة"** / **"منظومة متكاملة"** — Calque of "ecosystem / integrated system". Alternative: "نظام", "شبكة".
- **"تآزر"** — "synergy". Buzzword. Alternative: name the relationship.
- **"نموذج"** / **"رؤية"** (vague) — Alternative: the concrete model/plan.
- **"رحلة"** / **"مسيرة"** (metaphorical "journey") — UX/marketing cliché. Alternative: name the steps.
- **"آفاق"** / **"آفاقا"** / **"إمكانات"** / **"إمكانيات"** (vague "horizons / potential") — Alternative: a number, a concrete capability.
- **"قيمة مضافة"** / **"كفاءة"** / **"فعالية"** / **"مرونة"** / **"استدامة"** / **"تميز"** / **"ريادة"** — Corporate Esperanto. Alternative: the concrete benefit.

#### Soft verbs

- **"تحسين"** / **"تطوير"** / **"تبسيط"** / **"تسهيل"** — Domain-fine, AI filler otherwise. Alternative: name the concrete change.
- **"ضمان"** / **"يضمن"** (overused) — Alternative: "يؤمّن", or state the condition.
- **"تحقيق"** / **"مواكبة"** / **"إدارة"** / **"توظيف"** / **"استغلال"** / **"الاستفادة"** (catch-all) — Alternative: name the action.

### Tier 3 — Low signal (frequency-only)

Individually fine. Flag a paragraph if 5+ co-occur. (The analyzer does not list these; the reviewer does.)

نهج، مقاربة، استراتيجية، تحد، فرصة، منظور، طموح، أداء، إنتاجية، ربحية، نمو، مرافقة، فائدة، ميزة، قوة، مورد، ديناميكية، رافعة، محور، بعد، جانب، عنصر، مكون، مجال، إطار، حوكمة، مواءمة، اتساق، شفافية، انسيابية، بساطة، نجاعة، متانة، قابلية التوسع، نمطية، تشغيل بيني، استدامة، أثر، تتبع، أتمتة، رقمنة.

**Why low signal**: each appears naturally in honest Arabic B2B prose. **Why they still matter**: a paragraph with 5+ of them is corporate AI Esperanto.

### Replacements (consolidated)

| Tell | Human alternative |
|---|---|
| تجدر الإشارة إلى / من الجدير بالذكر | drop and state the claim |
| من المهم أن نلاحظ | drop; state the fact inline |
| مما لا شك فيه / لا شك أن | drop, or back with a number |
| في عالم اليوم / في عصرنا الحالي | اليوم / منذ [تاريخ] / هنا |
| في العصر الرقمي | اليوم / منذ [تاريخ محدد] |
| انغمس في / دعونا نستكشف | انظر / لنبدأ بـ / إليك |
| أطلق العنان لإمكانات | استخدم / استفد من (sparingly) |
| حلول مبتكرة / حل شامل | name the actual thing |
| تجربة سلسة / سلس | مباشر / بلا تعقيد / بلا خطوات إضافية |
| تسخير (leverage) | استخدام / الاستفادة من |
| تعزيز / يعزز | تحسين / زيادة + رقم |
| إحداث ثورة | يغيّر، state what changes |
| حجر الزاوية / العمود الفقري | name the function literally |
| منظومة (ecosystem) | نظام / شبكة |
| رحلة / مسيرة (metaphorical) | name the steps |
| علاوة على ذلك / بالإضافة إلى ذلك | و / كما |
| وبالتالي / نتيجة لذلك | لذلك / فـ |
| في الختام / خلاصة القول | end on a fact, drop the announcer |

---

## AI constructions (AR)

Patterns the analyzer matches via regex. Severity drives scoring.

### High severity

#### "ليس مجرد X، بل Y" / "لا يقتصر الأمر على X"

Calque of "It's not just X, it's Y". Never use. Replace with a concrete claim.

- تجنّب: "ليست مجرد أداة تسعير، بل أصل استراتيجي."
- فضّل: "تُظهر الأداة هوامش الربح لكل صنف — الأرقام نفسها التي يراها المشترون."

#### "انغمس في…" / "دعونا نستكشف…"

Never use as an introduction. State the subject directly.

- تجنّب: "انغمس في عالم التسعير الديناميكي الرائع."
- فضّل: "التسعير الديناميكي — لنبدأ بعتبات الهامش."

#### "في عالم اليوم…" / "في عصرنا الحالي…" / "في العصر الرقمي…"

Never open with a temporal abstraction. Use a date, an event, or jump straight in.

- تجنّب: "في عالم اليوم، حيث البيانات هي النفط الجديد…"
- فضّل: "منذ صدور لائحة حماية البيانات، صار تصدير قاعدة العملاء أغلى."

#### "تجدر الإشارة إلى…" / "من الجدير بالذكر…"

The strongest single-phrase MSA topic-sentence tell of 2025–2026. Pattern-matched because it fronts a claim.

- تجنّب: "تجدر الإشارة إلى أن الترحيل حسّن الأداء."
- فضّل: "خفّض الترحيل زمن الاستجابة ٤٠٪."

#### "مما لا شك فيه…" / "لا شك أن…"

Cf. suspect vocab. Pattern-matched separately because it acts as a topic-sentence opener.

#### "أطلق العنان…" / "حلول مبتكرة"

CTA/marketing reflexes. Banned. Replace with the concrete thing the product does.

### Medium severity

#### "سواء كنت X أو Y"

Calque of "Whether you're X or Y". Avoid unless the branching is real. By default, pick one reader and write for them.

- محظور افتراضيا: "سواء كنت شركة ناشئة أو مؤسسة كبيرة…"
- مقبول إن كان حقيقيا: "سواء كانت فوترتك باليورو أو بالدولار، التصدير متطابق."

#### "تخيل عالما…" / "تخيل أنك…"

Conference-bro openers. Banned. Replace with the real situation.

#### "هل تساءلت يوما…" / "قد تتساءل…"

AI's "you might be wondering" reflex. Bad. Just answer the implied question.

#### "دعنا نتعمق…"

Meta-sentence that performs thinking instead of doing it. Skip to the content.

#### "في هذا السياق…" / "على هذا الصعيد…"

Paragraph-glue signposts AI overuses. Cap at 0 as openers.

### Low / conclusion severity

#### Conclusion openers: "في الختام،" / "في نهاية المطاف،" / "خلاصة القول،" / "وفي الختام،"

Conclusion templates. **"في الختام" and "في نهاية المطاف" are the strongest MSA closing tells** (the latter a direct calque of "ultimately / at the end of the day"). Cap all at 0 as paragraph openers.

#### "علاوة على ذلك،" (as a paragraph opener)

The strongest connector tell. Each acceptable once; AI uses it at the head of every other paragraph. Cap at 1 per 800 words.

---

## Em-dash discipline (the foreign "—")

**Ban the em-dash "—" (U+2014) entirely. Target 0; the analyzer threshold is stricter than the master's (0.5 per 1000 words), because "—" is FOREIGN to Arabic typography.** Arabic has no native em-dash. A wide "—" used Anglo-style (spaced, mid-sentence, for emphasis or parenthesis) reads as machine output on sight. Convert every "—" to:

- the Arabic comma ، for a short aside,
- a colon : for a setup,
- parentheses ( ) for a true aside,
- a period . for emphatic separation.

The hyphen "-" (in compound transliterations) and the en-dash "–" (in number ranges, "ص ١٢–١٤") are fine. Only the wide "—" misused as an emphasis/parenthetical dash is the tell. When cleaning or writing AR, sweep for "—" explicitly and remove every occurrence.

### Replacement table

| AI overuse | Human AR |
|---|---|
| "سريع — فعّال — بسيط." | "سريع، فعّال، بسيط." or "سريع. فعّال. بسيط." |
| "الأداة — المصممة للمصانع — تعمل يوميا." | "الأداة، المصممة للمصانع، تعمل يوميا." |
| "تعمل — في أغلب الأحيان." | "تعمل (في أغلب الأحيان)." |
| "بسيطة — وتعمل." | "بسيطة. وتعمل." |
| "ثلاثة خيارات — أ وب وج — كلها صالحة." | "ثلاثة خيارات: أ وب وج. كلها صالحة." |
| "تسعير مرن — دفع لكل حدث." | "تسعير مرن: دفع لكل حدث." |

---

## Arabic punctuation discipline (the `، ؛ ؟` detector + more)

### The Arabic marks vs the Latin marks — the signature AR tell

Arabic has its own punctuation that mirrors the Latin marks rotated for RTL flow:

- **Arabic comma ، (U+060C)** — replaces the Latin `,`
- **Arabic semicolon ؛ (U+061B)** — replaces the Latin `;`
- **Arabic question mark ؟ (U+061F)** — replaces the Latin `?`

**Native Arabic uses these throughout; AI plain-text output frequently emits the Latin , ; ? instead** (English habit). A Latin comma, semicolon, or question mark sitting next to Arabic letters is a strong, mechanical tell.

- AI output: "هل تريد تحسين التسعير?"  →  Latin `?` where ؟ belongs
- Native AR: "هل تريد تحسين التسعير؟"
- AI output: "هذا جيد, لكن…"  →  Latin `,` where ، belongs
- Native AR: "هذا جيد، لكن…"

The detector (`detect_latin_punct_in_arabic`) counts every Latin `,` `;` `?` adjacent to an Arabic letter and flags above the threshold; it is low-weight and calibrated NOT to fire on clean native prose. Latin punctuation that sits among Latin letters (acronyms like `REST,` `GraphQL;`, URLs, code) is NOT counted because the detector requires Arabic-letter adjacency. Register note: technical Arabic and code-mixed copy legitimately carry some Latin punctuation; for that register use `--type technical` and tolerate a higher count, or wrap code in fenced blocks (stripped before scoring).

**Cleanup rule.** When cleaning AI Arabic for publication, sweep every Latin `,` `;` `?` adjacent to Arabic and replace with ، ؛ ؟.

### Note on the period

Arabic has **no dedicated full stop**; the Latin period `.` is the standard sentence terminator in modern Arabic, so `count_sentences` / `sentence_lengths` split on `.` as well as ؟ ؛. The period is therefore NOT a tell, and it is not counted by `detect_latin_punct_in_arabic`.

### Tatweel ـــ (U+0640) overuse

The **tatweel** (kashida) is the elongation character ـ used to stretch a letter's connecting stroke for justification or decoration (e.g. "مرحـــبا"). In running digital prose it should be **absent**: modern typesetting handles justification without it. AI sometimes injects tatweels imitating decorative/Quranic-style text, or they survive from scraped sources. A stream of tatweels (ـــ) in body text is a tell. The analyzer leaves this to the human reviewer (`tatweel_max` is documented in `rules.yaml` but currently unread); sweep and delete every U+0640 in plain prose.

### Latin vs Arabic-Indic digits

Both ٠١٢٣٤٥٦٧٨٩ (Arabic-Indic) and 0123456789 (Western) are used across the Arab world — Mashreq tends to Arabic-Indic, Maghreb tends to Western. Inconsistency within one document (mixing both digit sets) is a tell of stitched-together output. Pick one set per target locale and hold it.

### Quotation marks

Arabic editorial style uses the angle quotes «…» (guillemets). AI defaults to straight `"…"` or curly `"…"`. Prefer «…» in editorial AR and be consistent.

---

## Connector overuse (و / ف at sentence head)

Arabic links clauses with the proclitic **و** (waw, "and") and **ف** (fa, "then/so"). This is idiomatic — but AI massively overuses sentence-initial و/ف, opening clause after clause with "و…" until the prose reads as one flat run-on list with no subordination.

- AI rhythm: "وتعمل الأداة يوميا. وتراقب الأسعار. وترسل تنبيها. وتصدّر تقريرا."
- Human rhythm: "تعمل الأداة يوميا، تراقب الأسعار وترسل تنبيها عند أي تغيّر. التصدير اختياري."

**Doctrine.** Vary the connectors: use subordination (الذي، حيث، لأن، رغم أن)، full stops، and occasional asyndeton. Cap sentence-initial و at roughly 1 in 4 sentences. The analyzer does not count this directly (و is too common to threshold safely); it is a human-review pass, but it is one of the strongest rhythm tells in AI Arabic.

---

## Pedantic / formal-register turns

Schoolbook MSA. The training corpus is heavy with academic and journalistic writing, so the model defaults to phrases a *مدرّس لغة عربية* would have written in red pen.

Banned by default:

- **"تجدر الإشارة إلى"** — cf. supra.
- **"من نافلة القول"** — pompous "needless to say".
- **"وفي هذا الصدد"** / **"وفي هذا السياق"** — fake academic transitions.
- **"يمكن القول إن"** — hedging "one could say". Drop.
- **"على النحو المذكور آنفا"** / **"كما أسلفنا"** — bureaucratic back-reference. Drop.
- **"الجدير بالملاحظة"** — cf. supra.
- **"وعليه فإن"** — heavy "therefore". Use "لذلك".
- **"في الختام"** — heavy closer.

Rewrite by **stating** instead of **announcing**.

- تجنّب: "وفي هذا الصدد، تجدر الإشارة إلى أهمية تحليل الهوامش."
- فضّل: "الآن، الهوامش."

---

## Conclusion templates (AR)

Never start a closing paragraph with any of:

- "في الختام،"
- "وفي الختام،"
- "في نهاية المطاف،"
- "في خاتمة المطاف،"
- "خلاصة القول،"
- "وخلاصة القول،"
- "باختصار،"
- "ختاما،"
- "إجمالا،"
- "وفي الأخير،"
- "كما رأينا،"
- "كما ذكرنا آنفا،"

End on a concrete action, a number, or a sharp opinion. Examples:

- "الاختيار يعتمد على الحجم. أقل من ١٠٠ صنف: استخدم أ. أكثر: ب."
- "ثلاث خطوات: جرّب، قِس، قرّر. الباقي يأتي وحده."
- "غير مقتنع؟ شغّل تجربة على ١٠٠ سطر. سترى بنفسك."

---

## Voice and POV tells (AR)

### Absence of first person in 800+ words

AI defaults to detached third-person or impersonal passive (المبني للمجهول). A native author of an opinion piece **uses first person at least once per 500 words**. Cleanup rule: insert one first-person sentence (أرى، لاحظتُ، في تجربتي) per 500 words to break the impersonal register.

- تجنّب (طوال النص): "يُلاحظ أن الهوامش تنخفض. ومن المحتمل أن…"
- فضّل: "أرى الهوامش تنخفض عند ثلاثة عملاء من كل عشرة. على الأرجح…"

### Overuse of the pedagogical "نحن"

The "we" of a textbook ("سوف نرى أن…"). AI defaults to this when asked to explain. Replace with direct address (أنت) or first-person singular.

- تجنّب: "سوف نرى كيف نرحّل."
- فضّل: "هكذا تُرحّل." / "ترحّل في ثلاث خطوات."

### Prescriptive future overuse

"ستكتشف"، "ستتمكن من"، "سوف تفهم" — AI's prescriptive future. Replace with present or imperative.

- تجنّب: "ستكتشف ركائزنا الثلاث."
- فضّل: "ثلاث ركائز. لنرها."

### Register consistency

A piece mixing high MSA with sudden colloquial (عامية) bursts, or mixing Mashreq and Maghreb conventions, reads as stitched-together output. Pick one register per target locale and hold it.

---

## Tricolon rationing (AR)

Same rule as EN/FR: **cap at 1 tricolon per 200 words.** Arabic has a strong rhetorical pull toward triadic structures (the classical سجع rhythm), so a high count reads as borrowed cadence rather than authored prose. The analyzer's `detect_tricolons` matches the "X، Y، وZ" pattern: single-token items separated by the Arabic comma ، with the waw و attached (no space) to the final item — the tight list rhythm AI loves. It deliberately matches only the tight single-token cadence so it does not over-fire on loose multi-word lists.

Vary list sizes (2, 4, 5 items). Use asyndeton ("سريع، موثوق، نظيف" without the final و). Don't close every list with "، وZ".

- تجنّب: "الأداة سريعة وموثوقة ودقيقة. تدير الحزم والمتغيرات والمناطق. تعمل يوميا وأسبوعيا وعند الطلب."
- فضّل: "تدير الأداة الحزم والمتغيرات في ١٢ منطقة. تشغيل يومي، أو عند الطلب لمراجعة موضعية."

### Sub-rule: tricolons in titles and bullets

AI title format: "سريع، موثوق، وفعّال". AI bullet last item: "وقابل للتوسع". Both are tricolon tells. Rewrite to two items, or drop the final و.

---

## Quick triage (for the human reviewer)

When auditing a 500-word AR piece, scan in this order:

1. **Latin punctuation.** For every `,` `;` `?` adjacent to Arabic letters, is it the Arabic ، ؛ ؟? If not, replace (it flags).
2. **Em-dashes.** Count "—" in the prose. Any occurrence in expository text → replace with ، : ( ) or a period.
3. **Tatweel.** Sweep for ـ (U+0640) in body text → delete.
4. **Opening sentence** — if it starts with a temporal abstraction ("في عالم اليوم"، "في العصر الرقمي") or a meta-frame ("تجدر الإشارة إلى")، rewrite.
5. **Closing sentence** — if it starts with any conclusion template ("في الختام"، "في نهاية المطاف")، rewrite.
6. **Tier-1 vocab** — scan; cap at 1 per paragraph.
7. **AI constructions** — pattern-match by eye ("ليس مجرد…، بل…"، "انغمس في…"، "سواء كنت…").
8. **Connector overuse** — count sentence-initial و/ف and "علاوة على ذلك"; vary them.
9. **Tricolons** — count "X، Y، وZ"; cap at 1 per 200 words.
10. **POV** — at least one first-person mark if it's opinion.
11. **Digits** — one digit set (Arabic-Indic OR Western), consistent.

A passing AR piece, by this skill's bar:

- 0 Latin `,` `;` `?` inside Arabic (all ، ؛ ؟).
- 0 em-dashes "—", 0 tatweels.
- 0 tier-1 vocab items, ≤ 2 tier-2.
- 0 AI constructions.
- ≤ 1 tricolon per 200 words.
- ≤ 1-in-4 sentence-initial و.
- At least one first-person mark if opinion.

That piece will score LOW_RISK (≤ 24) on the deterministic analyzer and survive Copyleaks / GPTZero at sub-25 % AI probability in most domains.

---

## See also

- `tells-statistical.md` — burstiness, TTR, comma density doctrine (language-agnostic)
- `tells-structural.md` — bullets, headers, tricolons, emoji (+ short RTL/script note)
- `humanization-techniques.md` — how to write with intentional asymmetry, with Arabic worked examples
- `scripts/analyze.py` — `detect_suspect_vocab` (clitic-aware), `detect_ai_constructions`, `detect_tricolons`, `detect_latin_punct_in_arabic`
- `scripts/rules.yaml` — AR thresholds, suspect vocabulary, content-type weights
