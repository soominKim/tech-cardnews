# tech-cardnews (plugin)

Korean Instagram card-news creator in a **charcoal-dark + lobster-red** tech/subculture tone.

## What it includes

| Component | Name | Purpose |
|---|---|---|
| Skill | tech-cardnews | Turns any source content into a 13-slide Korean card-news series — as PNG (1080×1080) and as a single editable HTML file |

## Install

If you have already added this marketplace, run:

    /plugin install tech-cardnews@tech-cardnews

Otherwise add the marketplace first (see the repo root README).

## Trigger phrases

The skill auto-triggers on:
- 이 글로 카드뉴스 만들어줘
- 13장 카드뉴스 / 다크모드 카드뉴스
- 랍스터 레드 톤 / 차콜 + 빨강 카드뉴스
- OpenClaw 카드뉴스 or similar tech/dark-mode references

## Output

Every run produces:
1. <projectname>_script.md — the Korean script, reviewed before render
2. <projectname>_spec.json — slide spec (single source of truth)
3. <projectname>_slides/slide_01..13.png — flat 1080×1080 images
4. <projectname>.html — editable single-file HTML for tweaking/Figma import
