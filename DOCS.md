# 포트폴리오 프로젝트 핵심 개념 정리

## 1️⃣ 시맨틱 HTML – 왜 사용하는가?

| 이유 | 설명 |
|------|------|
| **접근성** | `<nav>`, `<section>` 등은 스크린리더가 페이지 구조를 바로 파악하게 함. |
| **SEO** | 검색 엔진이 시맨틱 태그를 통해 페이지 의미를 이해하고 색인 품질이 향상됨. |
| **가독성·유지보수** | 개발자가 코드를 읽을 때 레이아웃과 역할을 즉시 파악 가능 → 협업 효율이 증가함. |
| **표준화** | HTML5 표준에 맞는 마크업은 브라우저 호환성을 보장. |

**우리 프로젝트 구조 설계 기준**
- `<header>` + `<nav>` : 페이지 네비게이션을 명시
- `<section id="...">` : Hero, About, Skills, Projects, Contact 등 주요 영역 구분
- `<footer>` : 푸터
- 의미 없는 레이아웃용 요소는 최소한으로 `<div>` 사용

## 2️⃣ CSS – Flexbox와 Grid 차이 및 선택 기준

| 특성 | Flexbox | Grid |
|------|---------|------|
| **축** | 1‑차원(행 또는 열) 레이아웃에 최적 | 2‑차원(행 + 열) 레이아웃에 최적 |
| **주용도** | 내비게이션, 버튼 그룹, 가변 리스트 등 | 전체 페이지 레이아웃, 복잡한 격자형 UI |
| **정렬** | `justify-content`, `align-items` 로 한 축 정렬 | `grid-template-rows/columns`, `place-items` 로 양 축 정렬 |
| **아이템 순서** | `order` 로 재배열 가능 | `grid-area` 로 영역 지정 가능 |
| **반응형** | 미디어 쿼리에서 `flex-direction` 전환이 쉬움 | 미디어 쿼리에서 `grid-template` 재정의로 레이아웃 변화 | 

**선택 기준**
- **Flexbox** → 아이템이 한 줄(또는 한 열) 안에서 흐르는 경우에 사용합니다. 예) 네비게이션 링크, 버튼 그룹.
- **Grid** → 행·열 모두에 제어가 필요한 레이아웃에 사용합니다. 예) About 섹션의 이미지·텍스트 2‑컬럼 레이아웃, Projects 카드 격자.

## 3️⃣ DOM 조작 흐름 – `querySelector`와 `addEventListener`
```js
// 1️⃣ 요소 선택
const menuBtn = document.querySelector('#hamburger');   // CSS 선택자를 사용해 첫 번째 요소 선택
const nav      = document.querySelector('#navLinks');

// 2️⃣ 이벤트 리스너 등록
menuBtn.addEventListener('click', () => {
  nav.classList.toggle('open');   // 클래스 토글 → CSS 전환
});
```
- `querySelector` : CSS 선택자를 그대로 사용해 일치하는 첫 번째 요소를 반환합니다. 직관적이고 간결합니다.
- `addEventListener` : 특정 이벤트(`click`, `scroll` 등)와 콜백 함수를 연결합니다. 콜백 안에서 **상태(state)** 를 바꾸고 **DOM** 을 업데이트합니다.

## 4️⃣ 최신 JavaScript 문법
| 개념 | 필요성 | 사용 예시 |
|------|--------|-----------|
| **화살표 함수** | `this` 바인딩이 자동이고 코드가 짧아짐 | `const add = (a, b) => a + b;` |
| **구조분해 할당** | 객체·배열에서 필요한 값만 골라 추출해 가독성을 높임 | `const { name, email } = user;` |
| **배열 메서드 (`map`, `filter`)** | 순수 함수형 변환을 가능하게 하고 부수 효과 없이 새로운 배열을 반환 | `projects.map(p => …);`  `projects.filter(p => p.stars > 10);` |

## 5️⃣ 비동기 API 호출 – `fetch` + `async/await`
```js
async function fetchProjects() {
  try {
    toggleLoading(true);                // UI: 로딩 표시
    const resp = await fetch('https://api.github.com/users/skandnl/repos');
    if (!resp.ok) throw new Error('Network error');
    const data = await resp.json();
    renderProjects(data);               // UI: 성공 → 프로젝트 카드 삽입
    toggleLoading(false);
  } catch (err) {
    showError(err.message);            // UI: 오류 메시지 표시
    toggleLoading(false);
  }
}
```
- **로드 상태** : `toggleLoading(true/false)` 로 로더 스피너를 보이거나 숨깁니다.
- **성공** : 데이터를 받아 `renderProjects` 로 DOM에 동적으로 카드들을 삽입합니다.
- **실패** : `showError` 로 사용자 친화적인 에러 메시지를 표시합니다.

## 6️⃣ “하나의 기능” 흐름 – 이벤트 → 상태 변경 → DOM 업데이트
**예시: 다크 모드 토글**
1. **이벤트** – 사용자가 토글 버튼을 클릭 (`click` 이벤트).  
2. **상태 변화** – `localStorage`에 `theme=dark` 혹은 `light` 저장, 변수 `isDark` 를 토글.  
3. **DOM 업데이트** – `document.documentElement.dataset.theme = 'dark'` 로 CSS 변수 전환 → UI 전체가 즉시 다크 모드 적용.

> 이 흐름은 React에서 `setState → render` 로 매핑됩니다. 여기서는 순수 JavaScript 로 직접 구현했지만 원리는 동일합니다.

## 📦 프로젝트 기능 요구사항 만족 현황
| 요구사항 | 구현 여부 | 구현 위치 |
|----------|----------|-----------|
| 시맨틱 HTML | ✅ | `index.html` |
| 반응형 레이아웃 (Flex/Grid) | ✅ | `css/style.css` |
| 다크 모드 토글 + 상태 지속 | ✅ | `js/main.js` |
| 햄버거 메뉴 | ✅ | `js/main.js` + CSS |
| 스크롤 애니메이션 | ✅ | `js/main.js` (IntersectionObserver) |
| GitHub API 연동 (프로젝트 로드) | ✅ | `js/main.js` (`fetchProjects`) |
| 로딩/에러/빈 상태 UI | ✅ | `projects` 섹션 (`loader`, `error`, `empty` div) |
| 폼 검증 | ✅ | `js/main.js` (`validateForm`) |
| 프로필 이미지 (이더리움 펭귄) | ✅ | `assets/profile.png` + HTML src |
| Seed‑Design CSS 적용 | ✅ | CDN `base.css` |
| 문서화 (Markdown) | ✅ | 현재 `DOCS.md` 파일 |

---

### 📂 파일 트리 (핵심)
```
portfolio-website/
├─ index.html
├─ css/
│  └─ style.css
├─ js/
│  └─ main.js
├─ assets/
│  └─ profile.png   ← 귀여운 이더리움 펭귄
└─ DOCS.md          ← 이번에 만든 개념 정리 문서 (전부 한글)
```

---

*위 문서는 `git add DOCS.md && git commit -m "Add Korean concept documentation" && git push` 로 레포에 반영되었습니다.*
