# 🎮 Mini Game Hub

브라우저에서 바로 즐기는 5가지 고전 미니 게임 모음 사이트.  
설치 없이, 로그인 없이, **Vanilla JS** 만으로 구현되었습니다.

## 🕹️ 게임 목록

| 게임 | 파일 | 난이도 |
|------|------|--------|
| 💣 지뢰찾기 (Minesweeper) | `minesweeper.html` | ⭐⭐ |
| 🐍 스네이크 (Snake) | `snake.html` | ⭐⭐ |
| 🔢 2048 | `2048.html` | ⭐⭐⭐ |
| 🧱 테트리스 (Tetris) | `tetris.html` | ⭐⭐⭐ |
| 🃏 기억력 카드 (Memory Match) | `memory.html` | ⭐ |

## 🚀 실행 방법

`index.html` 파일을 브라우저에서 열면 됩니다.  
별도 서버나 설치 과정이 필요 없습니다.

## 🛠️ 기술 스택

- **HTML5** — 시맨틱 마크업
- **CSS3** — 변수, Grid, Flexbox, 애니메이션
- **Vanilla JavaScript** — 외부 라이브러리 없음
- **Canvas API** — 스네이크, 테트리스
- **localStorage** — 최고 점수 저장

## 📱 모바일 지원

모든 게임이 모바일 환경을 지원합니다.
- 스네이크: 스와이프 / D-패드 버튼
- 2048: 스와이프
- 테트리스: 스와이프 / 하단 컨트롤 버튼
- 기억력 카드: 터치 클릭
- 지뢰찾기: 길게 터치 = 깃발

## 📁 파일 구조

```
minigamegroup/
├── index.html        # 홈 허브 페이지
├── minesweeper.html  # 지뢰찾기
├── snake.html        # 스네이크
├── 2048.html         # 2048
├── tetris.html       # 테트리스
├── memory.html       # 기억력 카드
├── .gitignore
└── README.md
```
