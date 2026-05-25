# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

순수 Vanilla JS로 구현된 5개 미니 게임 모음 사이트. 외부 라이브러리·빌드 도구·서버 없이 HTML 파일을 브라우저에서 직접 열어 실행한다.

- **배포 URL:** https://dop731.github.io/minigamegroup/
- **저장소:** https://github.com/DOP731/minigamegroup (Public)
- **브랜치:** `master` → GitHub Pages 자동 배포

## 실행 및 배포

```bash
# 로컬 실행: 브라우저에서 직접 열기
start index.html          # Windows
open index.html           # macOS

# 변경사항 배포 (push 시 GitHub Pages 자동 재빌드, 약 30~60초 소요)
git add -A
git commit -m "feat: 변경 내용"
git push

# Pages 빌드 상태 확인
gh api repos/DOP731/minigamegroup/pages --jq '{status,html_url}'
```

> **커밋 규칙:** 파일을 수정하면 매 작업 후 반드시 `git add -A && git commit && git push`를 실행한다 (자동 훅 미설정, 세션 내 수동 실행).

## 아키텍처

### 파일 구성
| 파일 | 역할 |
|------|------|
| `index.html` | 게임 허브 홈페이지 — 5개 게임 카드 그리드, 스크롤 애니메이션, 미니 프리뷰 |
| `minesweeper.html` | 지뢰찾기 — DOM 기반 셀 그리드 |
| `snake.html` | 스네이크 — Canvas API |
| `tetris.html` | 테트리스 — Canvas API + 사이드 패널 |
| `2048.html` | 2048 — CSS Grid 타일 렌더링 |
| `memory.html` | 기억력 카드 — CSS 3D 카드 플립 |

각 게임은 **단일 HTML 파일에 `<style>` + `<script>` 인라인**으로 완결된다. 공유 CSS 파일이나 모듈이 없다.

### 공통 디자인 시스템 (모든 파일에 동일하게 적용)
```css
--bg: #1a1a2e       /* 페이지 배경 */
--panel: #16213e    /* 카드·패널 배경 */
--text: #e0e0e0
--text-dim: #8899aa
```
각 게임마다 `--highlight` 색상만 다르다 (지뢰찾기 `#e94560`, 스네이크 `#4fa3e0`, 2048 `#fbbf24`, 테트리스 `#a78bfa`, 기억력 `#56d98a`).

### 상단 내비게이션 패턴
`snake.html`, `tetris.html`, `memory.html`, `2048.html`은 동일한 `.topbar` + `.home-btn` 구조로 `index.html`로 돌아가는 링크를 제공한다. `minesweeper.html`만 인라인 스타일의 고정 `<a>` 태그를 사용한다 (추후 통일 고려).

### 게임별 핵심 구조

**minesweeper.html**
- `LEVELS` 객체로 3개 난이도(easy/medium/hard) 정의
- `board[][]` 2D 배열: `{ mine, revealed, flagged, question, adj }` 셀 객체
- `placeMines(safeR, safeC)` — 첫 클릭 3×3 영역 보호 후 지뢰 배치
- `floodReveal()` — 재귀 홍수 채우기
- 모바일: `touchstart` 0.45초 이상 → 깃발 설치, 깃발 모드 토글 버튼

**snake.html**
- `COLS=20, ROWS=20, CELL=20` (px) Canvas
- `snake[]` 좌표 배열 (head = index 0), `tick()` — `setInterval` 기반 루프
- 속도: `getSpeed() = max(80, 220 - (level-1)*20)` ms
- 최고 점수: `localStorage('snake_best')`

**tetris.html**
- `COLS=10, ROWS=20, CELL=28` Canvas + 별도 `next-canvas(80×80)`
- `PIECES` 객체: 7종 테트로미노 shape 배열 + color
- `rotate(shape)` — 시계 방향 회전, `valid()` — 충돌 검사
- 하드드롭: `hardDrop()` → `getGhostY()` 고스트 블록 위치로 즉시 이동
- 속도: `max(80, 600 - (level-1)*50)` ms

**2048.html**
- `grid[4][4]` 숫자 배열, `move(direction)` — left/right/up/down 4방향 슬라이드
- `slideLeft(row)` — 1행 슬라이드 + merge 로직의 기본 단위 (다른 방향은 배열 반전/전치 후 재사용)
- 타일 색상: `.t2` ~ `.t2048` CSS 클래스
- 최고 점수: `localStorage('2048_best')`

**memory.html**
- 난이도 3단계: 4×4(16장), 4×5(20장), 6×6(36장)
- `cards[]` — 셔플된 이모지 배열, `matched` Set으로 맞춘 쌍 추적
- CSS `rotateY(180deg)` 3D 플립 + `.shake` 오답 애니메이션
- `flip(i, el)` — 2장 선택 후 700ms 대기 → 불일치 시 되돌리기

### 입력 처리 공통 패턴
- **키보드:** `document.addEventListener('keydown', ...)` — `ArrowLeft/Right/Up/Down` + `WASD`
- **터치/스와이프:** `touchstart`에서 좌표 저장 → `touchend`에서 dx/dy 비교 → 방향 결정
- 모든 `touchstart` 리스너는 `{ passive: true }` 옵션 적용

### index.html 미리보기 렌더링
각 게임 카드의 미니 프리뷰는 JS로 동적 생성된다 (`mine-preview`, `snake-preview`, `tetris-preview`). 하드코딩된 좌표 패턴 배열을 순회해 DOM 셀을 추가하는 방식이므로, 게임 로직과 무관하게 수정 가능하다.
