# Human Writer — Arabic (ar, MSA) — a Claude skill

A Claude Code skill that produces **Arabic (Modern Standard Arabic)** prose reading as human-authored, sanitizes Arabic AI drafts to remove detector tells, and scores any Arabic draft for AI-detection risk before publication.

This is the **Arabic member** of the `human-writer` per-language family — one autonomous skill per language (`en`, `fr`, `es`, `pt`, `de`, `ar`, `hi`), all built on the same architecture. They install side by side and do not conflict: each activates on its own language triggers.

**RTL is a display concern only.** The analyzer works on logical character order — it never reverses text and needs no bidi handling. The Arabic-specific tells (punctuation, tatweel, clitics) are matched on the logical character stream.

## Why this skill exists

`mr-bridge.com` ships localized marketing and editorial copy in Arabic. Modern Sonnet / Opus / GPT-class Arabic output is fluent enough to ship, but it carries fingerprints that commercial detectors (Copyleaks, GPTZero, Originality.ai) latch onto, and Arabic carries its own tells the bare statistical detectors never checked:

- **Statistical:** low burstiness, narrow type-token ratio (language-agnostic).
- **Stylistic:** the same inflated MSA register repeated across drafts ("تجدر الإشارة إلى", "مما لا شك فيه", "في عالم اليوم", "حلول مبتكرة", "سلس", "تسخير"), formulaic frames ("ليس مجرد X، بل Y", "سواء كنت X أو Y"), and sentence-initial waw/fa connector overuse.
- **Typographic (Arabic-specific):** the **Latin , ; ?** used inside Arabic where the Arabic ، ؛ ؟ belong, the foreign em-dash "—" (no native Arabic equivalent), and tatweel ـ (U+0640) elongation in body text.

A draft drops, a client runs it through a detector, the score comes back at 70%+, and it's rejected. The fix is a disciplined sweep of the specific Arabic tells detectors weight. This skill encodes that doctrine plus a deterministic analyzer so any Claude Code session can produce, clean, or audit Arabic prose to a target score before delivery.

## What it can do

**Three modes:**
- **Write (الكتابة):** produce new Arabic content already engineered to score LOW_RISK.
- **Clean (التنظيف):** rewrite an existing Arabic AI draft to strip tells, preserving meaning.
- **Audit (التدقيق):** score an Arabic draft, surface the top offending tells, recommend fixes without rewriting it.

**One language, fully specialized:**
- **Arabic (ar, MSA):** ~150-entry suspect vocabulary (Tier 1 + Tier 2), Arabic AI-construction regex bank, Arabic tricolon detector (`X، Y، وZ` comma-waw rhythm), **clitic-aware vocabulary matching** (the bare stem catches its ال/و/ف/ب/ك/ل-prefixed forms), Arabic sentence splitting on ؟ ؛ ., and a dedicated **Latin-punctuation-in-Arabic** typography detector — calibrated NOT to false-fire on clean native prose.

**Four content-types** (each with its own adapter): marketing long-form, short-comms, technical, editorial-SEO.

**Optional external integration:** live scoring via Copyleaks, GPTZero, or Originality.ai (`--external <provider>`). Lazy `httpx` import, so the analyzer works fully offline.

## What's inside

```
human-writer-ar/
├── SKILL.md                          # Orchestration hub: routing + master checklist + anti-patterns (ar)
├── README.md                         # This file
├── INSTALL.md                        # Installation instructions
├── requirements.txt                  # pyyaml (required) + httpx (optional)
├── references/
│   ├── tells-stylistic-ar.md         # ⭐ CORE: AR suspect vocab + AI constructions + Arabic typography + calques
│   ├── tells-statistical.md          # burstiness / TTR (language-agnostic)
│   ├── tells-structural.md           # bullets / headers / tricolons / emoji (+ RTL note)
│   ├── humanization-techniques.md    # the ten humanization moves (Arabic worked examples)
│   ├── adapter-marketing.md          # marketing long-form adapter
│   ├── adapter-short-comms.md        # short-form comms adapter
│   ├── adapter-technical.md          # technical-docs adapter (prose-only contract)
│   ├── adapter-editorial-seo.md      # editorial-SEO adapter
│   ├── external-detectors.md         # Copyleaks / GPTZero / Originality.ai integration notes
│   └── checklists.md                 # pre-publish checklists + Arabic quick-triage
└── scripts/
    ├── rules.yaml                    # ⭐ ar: vocab + constructions + thresholds (incl. latin_punct_in_arabic_max)
    └── analyze.py                    # ⭐ ar-adapted: ؟؛ splitting + tricolon (،…،و) + clitic vocab + detect_latin_punct_in_arabic
```

## Installation

See `INSTALL.md`. TL;DR for macOS/Linux:

```bash
mkdir -p ~/.claude/skills
unzip human-writer-ar.zip -d ~/.claude/skills/
pip install --user -r ~/.claude/skills/human-writer-ar/requirements.txt
```

## How to invoke

Once installed, the skill auto-activates on Arabic prose requests. Example prompts:

- "اكتب مقالا من ٦٠٠ كلمة عن التسعير الديناميكي، بأسلوب بشري لا يشبه الذكاء الاصطناعي"
- "نظّف مسودة الذكاء الاصطناعي هذه لخفض درجة Copyleaks تحت ٢٥"
- "دقّق هذا البريد بالعربية لكشف علامات الذكاء الاصطناعي قبل إرساله"
- "اجعل هذا النص أقل شبها بـ ChatGPT"
- "Make this Arabic landing-page copy read human, not AI"

If auto-activation misses, force it: "Use the `human-writer-ar` skill to..."

## The analyzer

`scripts/analyze.py` is a deterministic scorer that runs offline. It loads `scripts/rules.yaml` (Arabic vocab lists, regex patterns, thresholds) and emits a 0-100 score plus a list of flagged tells.

```bash
# Audit mode (default, JSON output for tooling)
python3 scripts/analyze.py --input draft.md --lang ar --type marketing

# Human-readable report
python3 scripts/analyze.py --input draft.md --lang ar --type editorial-seo --format human

# Pipe via stdin
cat draft.md | python3 scripts/analyze.py --lang ar --type technical --format human
```

Required flags: `--lang ar`, `--type` (`marketing` / `short-comms` / `technical` / `editorial-seo`). `--input` is optional; stdin otherwise.

**Score bands** (4-band YAML, canonical):
- `0-24` **LOW_RISK:** ship it.
- `25-49` **MEDIUM_RISK:** apply the top 3 recommendations, re-score.
- `50-74` **HIGH_RISK:** in WRITE mode restart; in CLEAN mode apply a stronger rewrite.
- `75-100` **CRITICAL:** major rewrite.

**Detectors implemented:** em-dash density (stricter for Arabic — "—" is foreign), sentence-length stdev (burstiness, split on ؟ ؛ .), TTR (lexical diversity), clitic-aware Arabic suspect-vocabulary, Arabic AI-construction regex bank, Arabic tricolon (`X، Y، وZ`), bullet parallelism, header pyramid, and the Arabic-only **Latin-punctuation-in-Arabic** detector (low-weight, register-calibrated).

**Prose-only scoring.** The analyzer strips fenced code blocks, markdown data tables, and opt-in ignore regions before scoring. Wrap any intentionally AI-flavored citation so it doesn't count against you:

```
<!-- human-writer:ignore-start (مثال سيئ مقتبس) -->
في عالم اليوم، تجدر الإشارة إلى حلول مبتكرة وسلسة.
<!-- human-writer:ignore-end -->
```

## What's NOT inside

- **English content:** use `human-writer-en`. Other locales: `human-writer-fr` / `-es` / `-pt` / `-de` / `-hi`.
- **Document structure** (which sections, which schemas, which headings): use a dedicated structure/content tool. This skill is the stylistic filter applied on top of whatever produces the structure.
- **Technical SEO audit of a web project:** use a dedicated SEO tool. This skill handles content style only.

This skill is a stylistic filter invoked on top of structure-producing tools, never as a replacement.

## Part of the mr-bridge.com toolkit

This skill is part of the [mr-bridge.com](https://mr-bridge.com) toolkit for scraping, data, and content automation. Related resources:

- [mr-bridge.com](https://mr-bridge.com) — home
- [Scrapers](https://mr-bridge.com/scrapers) — the Apify Actor portfolio
- [MCP servers](https://mr-bridge.com/mcp-servers) — Model Context Protocol servers
- [AI workflows](https://mr-bridge.com/ai-workflows) — agents and automation
- [Studies](https://mr-bridge.com/studies) — data studies and one-pagers
- [Articles](https://mr-bridge.com/articles) — write-ups and guides
- [Solutions](https://mr-bridge.com/solutions) — end-to-end solutions

## License

Personal use. Customize freely. No warranty. The external-detector endpoints in `analyze.py` carry `# (verify)` markers (language-agnostic POSTs) and were not re-verified for Arabic specifically.
