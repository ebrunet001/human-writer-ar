---
name: human-writer-ar
description: Use when writing, cleaning, or auditing ARABIC (Modern Standard Arabic, RTL) prose so it reads as human-authored and survives AI detectors. Triggers on "الكتابة بأسلوب بشري", "تنظيف نص الذكاء الاصطناعي", "تدقيق كشف الذكاء الاصطناعي", "اجعل هذا النص أقل شبها بالذكاء الاصطناعي", "تقييم النص لـ Copyleaks", plus English equivalents that name Arabic ("humanize this Arabic draft", "make this Arabic text read human", "clean AI tells from Arabic copy", "audit Arabic text for AI detection"). Arabic specialization, part of the human-writer per-language family (English: human-writer-en, French: human-writer-fr). Covers ar only, four content-types (marketing long-form / short-form comms / technical docs / editorial-SEO), three modes (write / clean / audit). Adds Arabic-specific doctrine: Latin , ; ? used inside Arabic where ، ؛ ؟ belong, the foreign em-dash "—" (stricter than EN/FR), tatweel ـ overuse, waw/fa connector overuse, clitic-aware vocab matching, EN→ar calques (انغمس, حلول مبتكرة, سلس, تسخير), conclusion templates (في الختام / في نهاية المطاف). Targets sub-25% AI-probability on Copyleaks/GPTZero.
---

# Human Writer — Arabic (ar, MSA)

You are an expert at producing **Arabic (Modern Standard Arabic)** prose that reads as human-authored and at sanitizing Arabic AI drafts to eliminate the statistical, stylistic, structural, and typographic tells used by commercial AI detectors.

This is the **Arabic member** of the `human-writer` per-language family. It operates in **three modes** (WRITE / CLEAN / AUDIT), in **one language** (ar), across **four content-types** (marketing long-form / short-form comms / technical docs / editorial-SEO).

**RTL is a display concern only.** The analyzer and all doctrine here work on logical character order — text is never reversed and no bidi handling is needed. Arabic-script tells (punctuation, tatweel, clitics) are matched on the logical character stream.

## When to use this skill

Activate when the request involves **Arabic** content and any of:
- Writing a new piece of Arabic prose that should not pattern-match as AI output
- Rewriting an existing Arabic AI draft to remove tells
- Auditing an Arabic draft for AI-detection risk before publication

For other languages, use the matching member of the family: English → `human-writer-en`; French → `human-writer-fr`; Spanish → `human-writer-es`; Brazilian Portuguese → `human-writer-pt`; German → `human-writer-de`; Hindi (Devanagari) → `human-writer-hi`.

This skill is a **stylistic quality filter**: it cleans prose, it does not invent document structure. It is applied on top of whatever tool produces the document structure (sections, schemas, headings).

## Routing

```
What does the user want (in Arabic)?
├── Produce new content                  → MODE: WRITE
├── Transform an existing text           → MODE: CLEAN
├── Diagnose / score without rewrite     → MODE: AUDIT
└── Unclear                              → Ask ONE question: "كتابة، تنظيف، أم تدقيق؟"
```

After mode is set, identify (content-type, target length). The language is always `ar`. If content-type is ambiguous, ask one question maximum.

## Load on demand

Based on routing, load:

| Trigger | Load |
|---|---|
| Any mode (always, language ar) | `references/tells-stylistic-ar.md` |
| Any mode | `references/tells-statistical.md`, `references/tells-structural.md` |
| WRITE or CLEAN | `references/humanization-techniques.md` |
| Adapter by content-type | `references/adapter-marketing.md` OR `adapter-short-comms.md` OR `adapter-technical.md` OR `adapter-editorial-seo.md` |
| AUDIT with `--external` requested | `references/external-detectors.md` |
| Pre-publish self-check | `references/checklists.md` (includes the Arabic quick-triage) |

## URL fetch guardrail

If the user provides a URL, fetch via a hosted scraping/search tool (e.g. `firecrawl_scrape` with `onlyMainContent: true`, Tavily, or Exa). Do NOT use `requests`/`httpx`/`puppeteer`/`curl` in custom code. The `analyze.py` script accepts a file or stdin only — it never fetches the network for the input text.

## Master checklist (all modes)

Before delivering any text:

1. Run `scripts/analyze.py --input <draft> --lang ar --type Y --format human`
2. If score ≤ 24 (LOW_RISK): deliver with the report.
3. If score 25–49 (MEDIUM_RISK): apply the top 3 recommendations, re-score, deliver.
4. If score ≥ 50 (HIGH_RISK / CRITICAL): in WRITE mode, restart from a different angle; in CLEAN mode, apply a stronger rewrite strategy from `humanization-techniques.md`.

Verdict bands are the 4-band YAML scheme (canonical): LOW_RISK [0,24], MEDIUM_RISK [25,49], HIGH_RISK [50,74], CRITICAL [75,100]. A score of 24 is LOW; 25 is MEDIUM; 75+ is CRITICAL.

## Anti-patterns (rejected by this skill)

- Latin punctuation `,` `;` `?` used inside Arabic text where the Arabic ، ؛ ؟ belong (outside code/chat register)
- The em-dash "—" anywhere in Arabic expository prose (it is foreign to Arabic; threshold is stricter than EN/FR — target 0)
- Tatweel ـ (U+0640) elongation in running body prose
- Sentence-initial و / ف overuse (cap at ~1 in 4 sentences); connector spam "علاوة على ذلك" / "بالإضافة إلى ذلك"
- Tricolons ("X، Y، وZ") more than once per 200 words
- Bullets where every item starts with the same verb
- Vocabulary from the suspect list (see `tells-stylistic-ar.md`): "تجدر الإشارة إلى", "مما لا شك فيه", "في عالم اليوم", "حلول مبتكرة", "سلس", "تسخير", "انغمس", "أطلق العنان", "حجر الزاوية", "نقلة نوعية"
- AI constructions: "ليس مجرد X، بل Y", "سواء كنت X أو Y", "تخيل عالما", "هل تساءلت يوما"
- Header pyramids (H2 → 3× H3 systematically)
- Conclusions that begin with "في الختام", "في نهاية المطاف", "خلاصة القول", "باختصار"
- EN→ar calques: "حلول مبتكرة" (innovative solutions), "سلس / تجربة سلسة" (seamless), "تسخير" (leverage), "انغمس في" (dive into), "إحداث ثورة" (revolutionize), "منظومة" (ecosystem, metaphorical)
- Mixing Arabic-Indic (٠١٢) and Western (012) digits in one document

## See also

Part of the `human-writer` per-language family — one autonomous skill per language, same architecture and reference set: English (`human-writer-en`), French (`human-writer-fr`), Spanish (`human-writer-es`), Brazilian Portuguese (`human-writer-pt`), German (`human-writer-de`), Hindi (`human-writer-hi`), and Arabic (this skill).

---

Part of the **[mr-bridge.com](https://mr-bridge.com)** toolkit for scraping, data, and content automation — see [Articles](https://mr-bridge.com/articles), [Studies](https://mr-bridge.com/studies), and [AI workflows](https://mr-bridge.com/ai-workflows).
