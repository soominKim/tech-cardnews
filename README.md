# tech-cardnews

> Korean Instagram card-news creator in a **charcoal-dark + lobster-red** tech/subculture tone.
> A Claude skill that turns any source content into 13 slides — as PNG (1080×1080) **and** as a single editable HTML file.

<p align="center">
  <img src="docs/screenshots/slide_01.png" width="280" alt="Cover"/>
  <img src="docs/screenshots/slide_06.png" width="280" alt="Steps layout"/>
  <img src="docs/screenshots/slide_13.png" width="280" alt="CTA"/>
</p>

This repo is **both** a plugin and a plugin marketplace — pick whichever install path works for your setup.

## Install (recommended) — URL-based, no download needed

In **Claude Code** or **Cowork**:

```bash
/plugin marketplace add sbaekkr-design/tech-cardnews
/plugin install tech-cardnews@tech-cardnews
```

That's it. The marketplace pulls the plugin straight from this repo's `main` branch. Updates are one command away (`/plugin marketplace update tech-cardnews`).

## Alternative — One-file `.skill` install

If you prefer the standalone format:

1. Download [`tech-cardnews.skill`](./tech-cardnews.skill)
2. Open it — Claude shows a "Save skill" prompt → install

The `.skill` file is a zip of the same skill that lives in `plugins/tech-cardnews/skills/tech-cardnews/`.

## What it does

Drop in an article, transcript, or set of notes. The skill:

1. Writes a **Korean script** following a documented carousel guide (Hook → Core → CTA, density rules, particle/accent rules)
2. Builds a **`spec.json`** — a single source of truth describing each slide (layout type, headlined spans, bullet lines, cards, stats)
3. Renders **13 PNGs at 1080×1080** in a charcoal + lobster-red design system
4. Builds a **single self-contained HTML** with the same content — fully editable text, colors, layouts

PNG for posting, HTML for editing/Figma import/PDF print. Both stay in sync because they share the same spec.

## Why this style

A tested visual system that stops the scroll in a sea of light-mode Instagram posts:

- **Single accent color** (lobster red `#E63946`) — hierarchy comes from weight, not hue
- **Charcoal dark canvas** (`#0F0F12`) with subtle glow + frame + corner markers
- **6 layout variants** (cover / list / stat / steps / compare / cases / grid) — pick the right one per slide for rhythm
- **Korean typography rules built in** — particles never bold/accent, mixed-weight headlines are the norm

## Usage

Just ask Claude in Korean or English. The skill auto-triggers on phrases like:

- "이 글로 카드뉴스 만들어줘"
- "13장 카드뉴스" / "다크모드 카드뉴스"
- "랍스터 레드 톤" / "차콜 + 빨강 카드뉴스"
- "make card news from this article, dark tech style"

Claude will:
1. Ask for source content + your `@handle`
2. Write the script as Markdown for you to review
3. Generate `spec.json` from the approved script
4. Run `render_cards.py` → 13 PNGs
5. Run `build_html.py` → 1 HTML
6. Hand you both with computer:// links

## Editing the result

The HTML is the **canonical editable source**. To tweak:

- **Text**: open in any editor, edit the strings
- **Color theme**: change the CSS variables in the `:root` block at the top — all 13 slides update
- **Per-slide layout**: each slide is a `<section class="slide layout-XXX">`
- **Re-export PNGs**: change `spec.json`, re-run `python3 scripts/render_cards.py spec.json`

For Figma users: drag the HTML into [html.to.design](https://www.figma.com/community/plugin/1159123024924461424/html-to-design) and you get editable Figma layers.

## Repository layout

```
.
├── .claude-plugin/
│   └── marketplace.json            ← marketplace declaration
├── plugins/
│   └── tech-cardnews/
│       ├── .claude-plugin/
│       │   └── plugin.json         ← plugin manifest
│       ├── skills/
│       │   └── tech-cardnews/
│       │       ├── SKILL.md        ← entry-point for Claude
│       │       ├── references/     ← script guide, design system, layouts
│       │       ├── scripts/        ← render_cards.py, build_html.py
│       │       └── assets/         ← spec_example.json
│       └── README.md
├── docs/screenshots/               ← README cover images
└── tech-cardnews.skill             ← alternative one-file install
```

## Requirements

- **Runtime**: Python 3.10+ (renderer auto-installs Pillow and `koreanize-matplotlib` if missing)
- **For HTML viewing**: any modern browser (Pretendard loaded from jsdelivr CDN)

## License

MIT. See [LICENSE](./LICENSE).

## Acknowledgements

- Design system inspiration: TOBIT Korean card-news series (single-accent + weight hierarchy)
- Korean fonts: [Pretendard](https://github.com/orioncactus/pretendard) (HTML) / NanumGothic via `koreanize-matplotlib` (PNG)
- Skill format: [Anthropic Claude Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)

---

한국어 안내가 필요하시면 [README.ko.md](./README.ko.md)를 참고하세요.
