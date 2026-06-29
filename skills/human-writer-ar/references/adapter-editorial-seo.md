# Adapter: Editorial-SEO

> Doctrine for the `human-writer` skill, applied to editorial-SEO: long-form articles written to rank on Google AND read as human-authored. Loaded in addition to language-specific stylistic tells.

Editorial-SEO is the content type where the two pressures pull hardest in opposite directions. Google ranking rewards structure (H2/H3, keyword in title and intro, ~1500+ words, schema). AI detectors flag that same structure when it's executed by template. This adapter is the negotiation: keep the SEO scaffolding, kill the AI signals inside it.

The cost of getting it wrong has gone up. Google's helpful content updates from March 2024 onward demote pages that read as automated, and they have become better at it with each refresh. A page that ranked clean six months ago can quietly lose 60% of its traffic after a core update. The mitigation is not "less SEO"; it's SEO scaffolding wrapped around prose a reader would actually finish.

## What counts as editorial-SEO

- **Word count:** typically 1500–4000 words. Long enough to rank for competitive queries, short enough that a reader makes it to the end.
- **Purpose:** rank for one primary keyword plus 5–15 related queries AND deliver something the reader could not have gotten from the SERP snippet alone.
- **Channels:** standalone blog articles, knowledge-base pages, comparison guides ("X vs Y"), "ultimate guide to X" content, evergreen pillar pages.
- **Tone:** informative and authoritative. Structured for skimming (subheads, lists, bolding) AND for deep reading (paragraphs with actual voice).
- **Out of scope:** product-page copy (handled by `adapter-marketing.md`), API docs (handled by `adapter-technical.md`), social posts (handled by `adapter-short-comms.md`).

## The fundamental tension

SEO writing wants:

- Target keyword in the title, the H1, and the first 100 words
- H2/H3 hierarchy (helps featured snippets and table-of-contents widgets)
- Schema markup (Article, FAQPage, HowTo)
- Internal links to authority pages on the same site
- Word count above ~1500 (correlates with first-page rankings on competitive queries)

AI-generated SEO content has all of that, and adds:

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- Templated transitions ("Now that we've covered X, let's move on to Y")
- Tricolon-heavy lists ("fast, reliable, and scalable")
- "Ultimate Guide" / "Complete Guide" / "Everything You Need to Know" framing
- Section headers that exactly match featured-snippet question targets ("What is X?", "Why does X matter?", "How to do X")
- Tier-1 AI vocabulary distributed evenly through the body
<!-- human-writer:ignore-end -->

The skill's job: keep the SEO column intact, demolish the AI column.

## Tells that matter MOST here

Priority ranking, applied on top of the language adapter (`tells-stylistic-ar.md`):

<!-- human-writer:ignore-start (ranking list quotes the tells it ranks) -->
1. **AI constructions (HIGHEST).** Google's helpful-content scoring and reader trust both punish "In today's fast-paced X landscape" openers harder than any other pattern. Zero tolerance in the first 200 words.
2. **Header pyramid (HIGH).** SEO templates default to H2 followed by exactly 3 H3s. The analyzer flags 2+ H2s with uniform 3×H3 children. Force asymmetry: 0, 1, 2, 4, anything but uniform 3.
3. **Tricolons (HIGH).** "X, Y, and Z" is SEO-friendly (reads well in featured snippets) AND a primary AI tell. Cap at 1 per 200 words. Break the rest with pairs, quads, or asyndeton.
4. **Suspect vocabulary (HIGH).** In a 2000-word article, every tier-1 word ("delve", "leverage", "transformative", "seamless") is a detector hit. Cap at 1 tier-1 word per 400 words.
5. **Em-dash (MEDIUM).** "—" is foreign to Arabic; target 0 in AR articles. (EN/FR caps ~4/2 per 1000 words.)
6. **Sentence variance (MEDIUM).** Long content amplifies uniformity. A 3000-word article with stdev < 6 reads as templated even when individual sentences are fine. Aim for stdev ≥ 8 across the whole piece.
<!-- human-writer:ignore-end -->

## SEO patterns that ARE also AI tells

These are common SEO templates that double as AI tells. If keyword research demands you use one (it sometimes does), compensate harder in the body: more voice, more first-person opinions, more specific data.

<!-- human-writer:ignore-start (quoted template phrases, cited not used) -->
- "Ultimate Guide to X" / "الدليل الشامل لـ X"
- "Everything You Need to Know About X" / "كل ما تحتاج معرفته عن X"
- "X: A Complete Guide" / "X: الدليل الكامل"
- "How to X in Y Easy Steps" / "كيف تـ X في Y خطوات سهلة"
- "X vs Y: Which Is Better?" / "X مقابل Y: أيهما أفضل؟"
- "The 7 Best X for Y" / "أفضل ٧ X لـ Y"
- "X 101" / "X للمبتدئين"
<!-- human-writer:ignore-end -->

If the keyword tool says the audience searches "ultimate guide to apify actors", you may use the phrase in the title, but the article's first sentence cannot also be templated. Open with a specific story or a concrete data point. Earn the title with substance.

## SEO patterns that are NOT AI tells

Don't strip these out trying to humanize:

- **H2/H3 headers themselves.** Necessary for SEO. Just vary the H3 count per H2.
- **Internal links.** Fine. No impact on AI-detection scoring. Anchor text matters (see below).
- **Schema markup (JSON-LD).** Fine. Structured data is not in the prose body, not analyzed.
- **Target keyword in first 100 words.** Fine if it reads naturally. Force-insertion is what trips detectors, not the keyword itself.
- **Meta description.** Separate concern. Not scored by `analyze.py`. Tune it freely for CTR.
- **Bolded key phrases inside paragraphs.** Fine in moderation (1–2 per ~300 words).
- **Table of contents at the top.** Fine if the article has 6+ sections.

## Featured snippet bait: calibration

Featured snippets (the box above the SERP results) usually quote a 40–60 word block from your page that directly answers the query. AI templates over-tune for this: every section becomes a snippet-bait paragraph (definition-first, structured, Q&A-shaped).

**Doctrine:** include ONE snippet-optimized paragraph per article, usually the definition or the main answer, placed near the top. Make the rest of the article sound like a person wrote it, with voice and specifics and opinions. Do not stack five definition-first paragraphs in a row. The result reads as a content farm and ranks worse than one strong human paragraph would.

A clean snippet-bait paragraph looks like:

> Apify Actor pricing comes in three models: Pay-per-Result (PPR, deprecated), Pay-per-Event (PPE, current default), and Pay-per-Usage (compute time + storage). PPE charges users for specific outcomes such as a successfully scraped product page. It replaced rental pricing for most new Actors in 2024.

40–60 words, definition-first, keyword-rich. One per article. Move on.

## Internal linking voice

When you insert internal links inside editorial body copy:

- Anchor text should be the natural noun phrase, not the SEO target. "A recent study on Cloudflare bypass rates" reads human; "click here to learn more about Cloudflare bypass" reads AI.
- Don't insert a link in every paragraph. 3–6 internal links per 2000-word article is the sweet spot. More than that reads as SEO-stuffed.
- Don't use "as we discussed earlier" / "as mentioned previously" / "كما ذكرنا سابقا" as the linking phrase. Both are AI tells AND poor anchor text.
- Don't link the exact target keyword every time. Vary anchor wording: partial matches, broader phrases, the occasional verb-phrase anchor.

Bad anchor placement:

<!-- human-writer:ignore-start (deliberately bad anchor sample, cited not used) -->
> Apify Actor pricing is critical for monetization. To learn more about [Apify Actor pricing](/pricing/), click here. As we discussed earlier, [Apify Actor pricing](/pricing/) drives revenue.
<!-- human-writer:ignore-end -->

Good anchor placement:

> Apify Actor pricing is critical for monetization. [Pay-per-Event in particular](/pricing/) has eaten most of the new submissions in the Store over the last 18 months.

One link instead of two; anchor is a noun phrase that previews the destination instead of begging for the click.

## Keyword density

- **Target keyword:** 0.5–1.5% density across the article. For a 2000-word piece, that's 10–30 occurrences. Lower in the FAQ section, higher in the intro and the first H2.
- **LSI / related terms:** distribute naturally. Don't force a synonym sweep.
- **Do not keyword-stuff.** Google detects exact-match density above ~2% as suspicious.
<!-- human-writer:ignore-start (synonym-sweep example, cited not used) -->
- **Do not synonym-sweep.** Writing "العائد على الاستثمار، عائد الاستثمار، فترة الاسترداد، والربحية" in one sentence is an AI pattern (training data overweights this for "thorough" answers). Pick the one the reader uses.
<!-- human-writer:ignore-end -->

A practical test: read the paragraph aloud. If you would say all four synonyms in conversation, keep them. If you would naturally pick one, cut the others. AI drafts fail this test predictably; human drafts pass it without thinking.

## Editorial-SEO anti-patterns (specific)

These show up in AI-generated SEO drafts. Search and remove.

<!-- human-writer:ignore-start (quoted AI tells, cited not used) -->
- "In today's [industry] world," / "في عالم [الصناعة] اليوم،"
- "في عصر [الصناعة] الرقمي،" / "في ظل التطور المتسارع لـ،"
- "Whether you're a beginner or an expert," / "سواء كنت مبتدئا أو خبيرا،"
- "Let's dive in!" / "هيا ننغمس!" right after the intro
- "In this article, we'll explore..." / "في هذا المقال، سنستكشف..."
- "By the end of this guide, you'll know..." / "بنهاية هذا الدليل، ستعرف..."
- "As mentioned earlier" / "كما ذكرنا سابقا"
- "Now that we've covered X, let's move on to Y." / "الآن بعد أن غطّينا X، لننتقل إلى Y."
- A "Frequently Asked Questions" / "الأسئلة الشائعة" section with templated Q+A pairs that restate body content
- A "Conclusion" / "خاتمة" section that summarizes the intro
- Every H2 ending with a question mark
- Every H3 starting with "How to" / "كيفية"
<!-- human-writer:ignore-end -->

## Editorial-SEO good patterns (specific)

<!-- human-writer:ignore-start (good vs bad opener contrast quotes a bad opener) -->
- **Open with a specific story, number, or data point**, not with context-setting. ("في مارس الماضي خسرنا ٣٨٪ من الزيارات العضوية على صفحة /pricing/ واحدة بعد تحديث جوهري من Google." beats "في عالم السيو التنافسي اليوم، صار الترتيب أصعب من أي وقت مضى.")
<!-- human-writer:ignore-end -->
- **First 100 words contain the target keyword AND a specific take.** Both. The keyword for the bot; the take for the reader.
- **H2s are a mix of statements and questions.** Never all questions. Never all noun phrases.
- **H3s vary in form**: some fragments, some full sentences, some imperatives.
- **Body paragraphs vary in length by 2:1 minimum.** One paragraph of 2 sentences, the next of 6, the next of 1.
- **At least one data point with a source** (real or anonymized) per 800 words.
- **At least one opinion someone could disagree with** per article. Sharp, not hedged.
- **End on a specific action OR a question.** Never on a summary.

## Pre-publish checklist (editorial-SEO)

Before publishing, confirm:

- [ ] Ran `analyze.py --type editorial-seo --lang ar`, score ≤ 24
- [ ] Target keyword appears in the title, the first 100 words, and at least one H2
- [ ] H2 hierarchy is NOT a pyramid (not 3 H3s per H2 uniformly across 2+ H2s)
- [ ] First 100 words contain a specific data point, named entity, or opinion
- [ ] Title does NOT use "الدليل الشامل" / "كل ما تحتاج معرفته" UNLESS keyword research demands it (and if it does, the intro compensates)
- [ ] At least 1 internal link with natural anchor text, no more than 6 per 2000 words
- [ ] No "في عالم X اليوم،" opener
- [ ] No "خاتمة" section that restates the intro
- [ ] At least one sharp opinion someone could disagree with
- [ ] Sentence-length stdev ≥ 8 (analyzer green)
- [ ] Tricolon count ≤ 1 per 200 words
- [ ] Tier-1 vocabulary ≤ 1 per 400 words

If any item fails, fix before publishing. If the analyzer score lands 26–49 after fixes, apply the top 3 recommendations and re-score. Above 50, rewrite the opener and the conclusion first; those two sections account for most of the residual signal in long content.

## Calibration with `analyze.py`

The editorial-SEO weights in `rules.yaml`:

```yaml
editorial-seo: {statistical: 0.9, stylistic: 1.0, structural: 1.0}
```

Stylistic and structural rules apply at full weight; those are where SEO drafts most often fail. Statistical is slightly relaxed (0.9) because long, header-driven content naturally trends toward lower sentence-length variance than short prose. The 0.9 multiplier prevents false positives on otherwise-clean articles that have a few uniform-length paragraphs in their how-to sections.

It does NOT mean uniform sentence length is fine; stdev ≥ 8 remains the target. It means a 7.5 stdev on a 2500-word piece scores less harshly than a 7.5 stdev on a 600-word marketing post.

## Worked example

A 300-word excerpt from a hypothetical editorial-SEO article on "Apify Actor Pricing Models", first the AI-flavored draft, then the rewrite, with score deltas. (Kept in English here to show the SEO-vs-AI negotiation on a familiar topic; the same moves apply line-for-line in Arabic.)

### Before: AI-flavored draft (`analyze.py` score ≈ 62)

<!-- human-writer:ignore-start (deliberately AI-flavored sample for teaching; must not self-flag) -->
> ## Understanding Apify Actor Pricing Models
>
> In today's fast-paced web scraping landscape, choosing the right pricing model for your Apify Actor is crucial. Whether you're a beginner or an experienced developer, understanding the nuances of Apify's pricing options can be transformative for your business. In this article, we'll explore the three main pricing models and help you make an informed decision.
>
> Apify offers a seamless, flexible, and robust pricing system that empowers developers to monetize their Actors effectively. The three pricing models are designed to cater to various use cases, from simple scrapers to complex data extraction tools. By the end of this guide, you'll have a comprehensive understanding of which model best suits your needs.
>
> ### Pay-per-Result (PPR)
>
> Pay-per-Result is a legacy pricing model that charges users for each result returned. It's simple, predictable, and easy to implement.
>
> ### Pay-per-Event (PPE)
>
> Pay-per-Event is the modern approach, allowing developers to charge for specific events such as successful scrapes, dataset items, or API calls.
>
> ### Pay-per-Usage
>
> Pay-per-Usage charges based on compute time and storage consumption. It's the most granular model and is ideal for resource-intensive Actors.
>
> Now that we've covered the three pricing models, let's dive into how to choose the right one for your Actor.
<!-- human-writer:ignore-end -->

<!-- human-writer:ignore-start (issue list quotes fragments from the bad draft above) -->
Issues: "In today's fast-paced", tricolons ("seamless, flexible, and robust" / "simple, predictable, and easy"), "transformative", "empowers", H2 → 3×H3 pyramid, "by the end of this guide", "Now that we've covered X, let's dive into Y", every H3 starts the same way ("Pay-per-X is"), zero specific data, zero opinion.
<!-- human-writer:ignore-end -->

### After: clean rewrite (`analyze.py` score ≈ 18)

<!-- human-writer:ignore-start (quoted sample output; intentional-voice em-dashes are part of the demonstration) -->
> ## Apify Actor pricing: three models, one that actually works
>
> Apify Actor pricing comes in three flavors. Two of them, you can mostly ignore. The third — Pay-per-Event — is what every monetized Actor I've shipped in the last 18 months uses, and it's what Apify itself defaults new templates to.
>
> Here's the short version. Pay-per-Result still exists for legacy Actors. Pay-per-Usage charges by compute time and storage; useful for very heavy workloads, mostly painful to communicate to buyers. Pay-per-Event charges for whatever you decide counts as a billable outcome — a successfully scraped page, a returned dataset row, an API call that resolved.
>
> ### When Pay-per-Result still makes sense
>
> Almost never on a new Actor. The model rounds badly on partial runs and Apify quietly deprecated it for new submissions. If you inherited a PPR Actor making real money, leave it alone until users complain. If you're starting fresh, skip it.
>
> ### Pay-per-Event in practice
>
> One `Actor.charge()` call per billable event. The trick is the event taxonomy — too granular and your buyers can't model their cost; too coarse and you lose money on edge cases. I default to three events: a per-run base charge, a per-result charge, and one optional per-API-call surcharge for AI-extraction Actors.
>
> ### Pay-per-Usage: the awkward middle
>
> Charges by compute seconds and KV-store bytes. Honest, predictable for you, hard to explain to a buyer in a Store listing. I've shipped exactly one Pay-per-Usage Actor and switched it to PPE within a quarter.
<!-- human-writer:ignore-end -->

<!-- human-writer:ignore-start (score-delta analysis quotes fragments from both drafts above) -->
Score delta: 62 → 18. Same SEO scaffolding (one H2, three H3s, but the H3s no longer pyramid because the H2 also has body text before the first H3, and the H3 counts vary in body length 2:1). Same keyword density (Apify Actor pricing in title, intro, and one H2). Same internal-link-ready surface. What changed: opener carries a specific take, tricolons gone, "transformative/seamless/robust/empowers" gone, "in today's/whether you're/by the end of this guide" gone, every H3 has a different shape, body paragraphs vary in length, one sharp opinion per section.
<!-- human-writer:ignore-end -->

## See also

- `tells-stylistic-ar.md`: language-specific vocabulary and construction tells
- `tells-structural.md`: header pyramid, tricolon discipline, conclusion templates
- `humanization-techniques.md`: the core moves applied in WRITE and CLEAN modes
- `adapter-marketing.md`: closely related; shares most stylistic doctrine but marketing is shorter and conversion-driven
- your CMS / SEO workflow: for technical SEO audit (robots.txt, schema, Core Web Vitals); this skill handles content style only
