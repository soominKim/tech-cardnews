# tech-cardnews

> **차콜 다크 + 랍스터 레드** 톤의 한국어 인스타그램 카드뉴스 메이커.
> 어떤 글이든 받아서 13장의 PNG(1080×1080)와 **편집 가능한 단일 HTML**을 동시에 만들어주는 Claude 스킬.

<p align="center">
  <img src="docs/screenshots/slide_01.png" width="280" alt="표지"/>
  <img src="docs/screenshots/slide_06.png" width="280" alt="단계 레이아웃"/>
  <img src="docs/screenshots/slide_13.png" width="280" alt="CTA"/>
</p>

이 레포는 **플러그인 + 마켓플레이스** 둘 다입니다. 편한 설치 방법을 고르세요.

## 설치 (추천) — URL만으로, 다운로드 불필요

**Claude Code** 또는 **Cowork** 에서:

```bash
/plugin marketplace add sbaekkr-design/tech-cardnews
/plugin install tech-cardnews@tech-cardnews
```

끝입니다. 마켓플레이스가 이 레포의 `main` 브랜치에서 플러그인을 직접 가져옵니다. 업데이트도 한 줄(`/plugin marketplace update tech-cardnews`).

## 대안 — `.skill` 파일 원클릭 설치

마켓플레이스 시스템을 안 쓴다면:

1. [`tech-cardnews.skill`](./tech-cardnews.skill) 다운로드
2. 파일을 열면 Claude가 "Save skill" 확인창 표시 → 설치

`.skill` 파일은 `plugins/tech-cardnews/skills/tech-cardnews/`와 동일한 스킬의 zip입니다.

## 어떤 일을 해주나요

기사·강연 스크립트·블로그 글·메모 같은 소스를 던지면:

1. **한국어 스크립트**를 카드뉴스 제작 가이드(Hook → 핵심 → CTA, 정보 밀도 규칙, 한글 조사·강조 규칙)에 따라 작성
2. **`spec.json`** 한 파일에 13장의 슬라이드 명세(레이아웃, 헤드라인 스팬, 불릿, 카드, 통계 박스)를 정리
3. **13장의 1080×1080 PNG**를 차콜+랍스터레드 디자인 시스템으로 렌더
4. 같은 내용을 **편집 가능한 단일 HTML**로도 빌드

업로드용은 PNG, 편집·Figma 가져오기·PDF 인쇄는 HTML. 둘 다 같은 `spec.json`에서 나오니까 동기화가 자동.

## 사용법

Claude한테 그냥 한국어로 말하면 자동 발동:

- "이 글로 카드뉴스 만들어줘"
- "13장 카드뉴스" / "다크모드 카드뉴스"
- "랍스터 레드 톤"
- "유튜브 영상 스크립트인데 카드뉴스로 바꿔줘"

## 라이선스

MIT. [LICENSE](./LICENSE) 참고.
