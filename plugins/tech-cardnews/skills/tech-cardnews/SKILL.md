---
name: tech-cardnews
description: Korean Instagram card-news creator in a charcoal-dark + lobster-red tech/subculture tone, producing both PNG (1080x1080, 13 slides) and a single editable HTML file from any source content (article, talk transcript, notes). Use this whenever the user asks to "make card news / 카드뉴스 / cardnews" with a tech, dark-mode, or subculture aesthetic — even if they don't say "tech". Trigger on phrases like "OpenClaw 카드뉴스", "이 글로 카드뉴스 만들어줘", "13장 카드뉴스", "다크모드 카드뉴스", "랍스터 레드 톤", "차콜 + 빨강 카드뉴스", or any time the user wants a Korean carousel that must be both image (PNG) AND code (HTML). Distinct from the basic 600x600 `cardnews` skill — this one is 1080x1080, dark-themed, design-systemized, and produces editable HTML alongside flat PNGs.
---

# tech-cardnews

A reusable workflow for producing Korean card news in a **charcoal dark + lobster red** tech/subculture tone. Outputs 13 slides at 1080x1080 as both flat PNGs and a fully editable single-file HTML.

## When this skill applies

Trigger when:
- The user shares source material (article, talk transcript, blog post, notes) and asks for a card-news series in Korean
- The user wants a **dark / tech / subculture** look (charcoal black + accent red)
- The user wants both PNG (for Instagram) and HTML (for editing in Figma/browser)
- The user references the OpenClaw template or this skill by name

Do NOT use when:
- The user explicitly wants the lighter 600x600 `cardnews` style (gradient/illustration/pattern/minimal)
- The output target is a single image, deck, or document (use `pptx` / `cardnews` / `docx` instead)

## High-level workflow

```
1. Gather input          ← topic / source content / footer handle
2. Write Korean script   ← references/script-guide.md (13-slide structure)
3. Build slide spec      ← spec.json with layout type per slide
4. Render PNGs           ← scripts/render_cards.py (1080x1080)
5. Build HTML            ← scripts/build_html.py (single file)
6. Deliver               ← computer:// links + present_files
```

The whole thing is one file format (`spec.json`) feeding two renderers. Same content, two output formats. If the user asks for design tweaks afterwards, edit `spec.json` once and re-run both scripts.

## Step 1 — Gather input

Always ask for these BEFORE writing the script:

1. **Source content** — Topic, article URL, full text, transcript, etc. Required.
2. **Footer handle** — `@username` shown on every slide. Default `@username` if not given.
3. **Slide count** — Default 13 (long content). Override if user specifies.
4. **Series labels** — The top-right and side-rail labels. Defaults: `"OPENCLAW · SERIES"`, `"AI · AGENT · ERA"`, `"OPEN · CLAW"`. Almost always swap these to match the topic (e.g., `"FOUNDER · NOTES"`, `"BUILD · LOG"`).

Use the **AskUserQuestion** tool to gather missing pieces before proceeding. Don't burn tokens generating a script with the wrong handle/labels.

## Step 2 — Write the Korean script

Read **`references/script-guide.md`** in full and follow it. The guide encodes:

- The Hook → Core → CTA structure (slide 1, slides 2–N–1, slide N)
- Density rules (≥3 sentences for paragraph slides, 1–2 per bullet for lists)
- Number-with-context rule ("매출 6,000억 원 — 창업 9개월 만에 달성한 수치로...")
- Slide count by source length (≤500자 → 5–6장, 500–1500자 → 7–9장, ≥1500자 → 10–13장)
- The 7 layout patterns — pick the right one per slide

Output the script as Markdown with one section per slide, each section listing:
- `tag` (eyebrow hashtag/label)
- `layout` name (one of: cover, list, stat, steps, compare, cases, grid, cta — see `references/layouts.md`)
- `headline` (with which words to accent-color)
- body content matching the layout's slots

Save to `outputs/<projectname>_script.md` so the user can review before rendering.

## Step 3 — Build the slide spec

Translate the script into `spec.json`. The renderer is dumb — it just consumes the spec — so all design decisions live in the spec.

Read **`references/layouts.md`** for the exact slot structure of each layout type. An example complete spec is in **`assets/spec_example.json`** (the OpenClaw 13-slide deck we built originally — read it as a reference for shape and tone).

Save to `outputs/<projectname>_spec.json`.

### Spec shape (top-level)

```json
{
  "meta": {
    "title": "...",
    "footer_handle": "@username",
    "series_label": "OPENCLAW · SERIES",
    "side_label_left": "AI · AGENT · ERA",
    "side_label_right": "OPEN · CLAW"
  },
  "slides": [ { "n": 1, "layout": "cover", ... }, ... ]
}
```

### Headlines & bullets — mixed-weight syntax

Headlines and bullet lines are **arrays of spans** so you can color/bold individual words. Each span:
```json
{ "t": "텍스트", "bold": true, "accent": true, "muted": false }
```
- `bold: true` → 700/900 weight (default headline weight is 900)
- `accent: true` → lobster-red color (`--accent`)
- `muted: true` → secondary gray text (use for body/conversational tone)

Korean grammar rule (carry over from TOBIT analysis): **never accent-color or bold an empty grammatical particle ("을/를", "이/가", "에", "하다" alone)**. Accent only nouns, key verbs, and emphasized adjectives. The script-guide reinforces this.

## Step 4 — Render PNGs (1080x1080, 13 slides)

```bash
python3 scripts/render_cards.py path/to/spec.json --out outputs/<projectname>_slides/
```

The script:
- Auto-installs NanumGothic via `koreanize-matplotlib` if not present (pip)
- Renders each slide at 1080x1080 PNG with the foundation (frame, corner markers, side labels, top label, footer badge, NEXT arrow)
- Falls back to layout-specific renderers based on `slide.layout`

Visual sanity check: read at least slide 1 (cover), one mid-slide, and the CTA. If text overflows or overlaps, reduce headline font sizes by ~10% in the spec or split the text across more lines (you can edit `headline_lines` directly).

## Step 5 — Build the HTML

```bash
python3 scripts/build_html.py path/to/spec.json --out outputs/<projectname>.html
```

Single self-contained HTML file with:
- Pretendard via jsdelivr CDN
- All design tokens as CSS variables (one place to recolor)
- Each slide as a `<section class="slide layout-XXX">`
- Inline SVG for charts (slide 9 pattern)
- A floating TOC (01–13 jump links)
- Print stylesheet (page-break per slide)

This file is the **editable source of truth** going forward. Hand this to the user as the canonical asset; the PNGs are render-time exports.

## Step 6 — Deliver

Always provide BOTH output formats with computer:// links:

```
[전체 보기 (HTML, 편집 가능)](computer:///path/to/<projectname>.html)
[01 표지](computer:///path/to/<projectname>_slides/slide_01.png)
[02 ...](...)
...
```

Then a brief note explaining:
- Edit the HTML directly for text/color/layout tweaks
- Re-run `render_cards.py` to regenerate PNGs from `spec.json`
- Drag PNGs into Figma if they want native Figma frames

## Design system (summary)

Full details in `references/design-system.md`. Quick reference:

```
--bg          #0F0F12   (charcoal canvas)
--bg-card     #18191E   (elevated card)
--accent      #E63946   (lobster red — single accent)
--accent-deep #B41E2D
--cream       #F5F0E8   (primary text)
--gray-hi     #B4AFAA   (secondary text)
--border      #3C1E23   (frame border / card stroke)

font:    Pretendard (HTML) / NanumGothic (PNG fallback)
canvas:  1080×1080
inset:   36px (frame border)
safe:    130px (content padding)
```

The single-accent rule is the heart of the system — never introduce a second color. Hierarchy comes from **weight (Black/Bold/Regular)** and **size**, not hue. This is what makes everything cohere across 13 slides.

## Common failure modes

1. **Forgetting to ask for `footer_handle`** — every slide shows it; getting it wrong means re-rendering 13 PNGs
2. **Adding extra colors** — resist the urge. Even sub-emphasis stays in the gray scale
3. **Bolding particles** — destroys the typography; only emphasize content words
4. **Cramming too much per slide** — if a slide has >5 bullets or >4 paragraphs, split into two
5. **Skipping the script step** — people sometimes ask "just make me 13 slides" and you go straight to spec; always write the script.md first so the user can review copy before render

## Sub-references

- `references/script-guide.md` — the 카드뉴스 제작 가이드 (script-writing rules)
- `references/design-system.md` — full color/type/spacing tokens
- `references/layouts.md` — the 7 layout variants with exact slot specs
- `assets/spec_example.json` — full OpenClaw spec for shape reference
- `scripts/render_cards.py` — PNG renderer
- `scripts/build_html.py` — HTML generator
