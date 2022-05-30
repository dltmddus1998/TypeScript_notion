## ❕[출처] Dream Coding Ellie's MOTION PROJECT ❕

## ⚙️ Project Setup

---

```json
// tsconfig.json
"target": "es6", /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
"module": "ES2015",                                /* Specify what module code is generated. */
"rootDir": "./src",
"outDir": "./dist",                                   /* Specify an output folder for all emitted files. */
"removeComments": true,                           /* Disable emitting comments. */
"noEmitOnError": true,
"strict": true,                                      /* Enable all strict type-checking options. */
"noUnusedLocals": true,                           /* Enable error reporting when a local variables aren't read. */
"noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read */  
"noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
"noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
"noUncheckedIndexedAccess": true,                 /* Include 'undefined' in index signature results */
```

## 🧭 Project Plan

---

‼️ $**App(Component) = header + footer + document**$

<aside>
💡 **정적인 부분 (header + footer)을 제외한 동적으로 데이터를 주고 받는 document은 Page Component라는 클래스를 만들어서 동적으로 추가된 데이터를 또 다른 컨테이너로 묶어서 페이지에 추가할 수 있도록 해주자.**

</aside>

### 📉 전반적인 App 흐름

✔︎ App이라는 컴포넌트는 PageComponent를 가지고 있다.

✔︎ 사용자가 App에 있는 Button을 click하면 Dialog를 만들어서 사용자에게 보여준다.

✔︎ 사용자가 Data를 Input하게 되면 입력받은 데이터를 이용해서 어떤 Button을 click했냐에 따라서 Image/Note/Video/Todo Component를 만든다.

✔︎ 이렇게 만들어진 Component를 PageComponent에 추가해준다.

## 파일 구조

---

```

```

## 🏗 Make Layout & First Page Component

---

### 👀 Part 1. UI

```css
/* style.css */
:root {
  --bg-main-color: #00000080;
  --bg-accent-color: #2d2d2d;
  --accent-color: #f64435;
  --text-accent-color: #ffe498;
  --text-edit-bg-color: #575757;
  --border-color: #3f3f3f;
  --shadow-color: #202020;
  --document-bg-color: #68686850;
  --component-bg-gradient: radial-gradient(circle, #646464e6 0%, #363636e6 100%);
  --smokywhite: #dddbd8;
  --black: #000000;
  --translucent-black: #00000099;
}

li {
  list-style: none;
  padding-left: 0;
}

p {
  color: var(--smokywhite);
}

label {
  color: var(--text-accent-color);
}

* {
  outline: 0;
  box-sizing: border-box;
}

body {
  background: url('../assets/background.png') center/cover no-repeat;
  font-family: Roboto, Oxygen, sans-serif;
  display: flex;
  justify-content: center;
  height: 100vh;
  overflow: hidden;
  font-size: 1.25rem;
  margin: 0;
}

.app {
  width: 100%;
  max-width: 1000px;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.banner {
  background: var(--bg-main-color);
  border-bottom: 1px solid var(--bg-accent-color);
  text-align: center;
}

.banner__title {
  margin: 20px;
  color: var(--accent-color);
}

.control-panel {
  display: flex;
  align-items: center;
  justify-content: center;
}

.create-button {
  margin: 0.1em 0.2em;
  text-align: center;
  user-select: none;
  padding: 1em 3em;
  text-transform: uppercase;
  font-weight: 600;
  font-size: 0.8rem;
  color: white;
  background-color: var(--accent-color);
  border-color: var(--accent-color);
  border-radius: 2px;
}

.document {
  height: 100%;
  overflow-y: auto;
  background-color: var(--document-bg-color);
  /* Firefox */
  scrollbar-width: 6px;
  scrollbar-color: var(--bg-accent-color);
  scrollbar-face-color: var(--accent-color);
}

.document::-webkit-scrollbar {
  /* Safari and Chrome */
  background-color: var(--bg-accent-color);
  width: 6px;
}

.document::-webkit-scrollbar-thumb {
  /* Safari and Chrome */
  background-color: var(--accent-color);
}

.footer {
  background: var(--bg-main-color);
  border-top: 1px solid var(--border-color);
  text-align: center;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cf9420b8-8768-4bbe-a81b-00a2e7f2f4ab/Untitled.png)

### 👀 Part 2. Component

```tsx
// page.ts
export class PageComponent {
    // HTML의 카드들의 목록을 담고 있을 것 
    private element: HTMLUListElement;
    constructor() {
        // ul 태그 생성
        this.element = document.createElement('ul');
        // class명 -> page
        this.element.setAttribute('class', 'page');
        this.element.textContent = 'This is PageComponent';
    }
    
    // 외부에서 pageComponent를 만들어서 필요한 곳에 이 페이지를 추가할 수 있는 것을 만들자.
    attachTo(parent: HTMLElement, position: InsertPosition = 'afterbegin') {
        // 부모의 자식요소 어딘가에 추가할 수 있는 API (삽입 포지션 지정)
        parent.insertAdjacentElement(position, this.element);
    }
}
```

**PageComponent**

→ element라는 내부 state (DOM 요소 중 하나)

→ 생성자 안에서 우리가 원하는 DOM 요소 생성 (createElement)

→ attachTo라는 함수를 호출해야 전달받은 parent 요소에 우리가 만든 DOM 요소를 붙여줄 수 있다.

+) parent → HTMLElement를 상속하는 그 어떤 자식 요소들도 전달할 수 있다.

```tsx
// app.ts
import { PageComponent } from './components/page.js';

class App {
    private readonly page: PageComponent;
    constructor(appRoot: HTMLElement) {
        this.page = new PageComponent();
        this.page.attachTo(appRoot);
    }
}

// 동적으로 만드는게 아니라 개발시 정확히 정해진 경우 -> 무조건 null 아니고 HTMLElement 타입이라고 Type Assertion로 표시
new App(document.querySelector('.document')! as HTMLElement)
```

1. Application 실행
2. app.ts 실행
    
    → App이라는 클래스의 인스턴스를 만든다.
    
    → 생성자 안에는 document라는 클래스에 있는 요소를 받아와서 전달한다.
    
    `new App(document.querySelector(’.document’)! as HTMLElement);`
    

‼️ **document라는 클래스를 가진 요소는 header와 footer 사이에 우리가 동적을 추가할 컨테이너 요소이다.**

## 🎞 Make Image Component

---

```tsx
// image.ts
export class ImageComponent {
    private element: HTMLElement;
    constructor(title: string, url: string) {
				// 1. 
        const template = document.createElement('template');
        // 2.
        template.innerHTML = `
        <section class="image">
            <div class="image__holder">
                <img src="" alt="" class="image__thumbnail">
                <p class="image__title"></p>
            </div>
        </section>`;
				
        this.element = template.content.firstElementChild! as HTMLElement;

        const imageElement = this.element.querySelector('.image__thumbnail')! as HTMLImageElement;
				 // ⭐️
        imageElement.src = url;
        imageElement.alt = title;

        const titleElement = this.element.querySelector('.image__title')! as HTMLParagraphElement;
        titleElement.textContent = title;
    }
    // 3.
    attachTo(parent: HTMLElement, position: InsertPosition = 'afterbegin') {
        parent.insertAdjacentElement(position, this.element);
    }
}
```

**ImageComponent**

1. `template` 태그를 통해 어떠한 요소들을 내부적으로 만들 수 있게 한다.
2. `innerHTML`을 통해 string 타입으로 html코드를 작성한다.
→ 사용자에게 전달받은걸 그대로 `innerHTML`을 통해 작성하는건 위험할 수 있으므로 ⭐️와 같이 작성한다.
    1. `.image__thumbnail` 클래스에 있는 요소를 가져와서 `src`와 `alt` 옵션에 각각 사용자에게 전달받은 `title`, `url`을 할당해준다. 
3. 그리고 위에 ImageComponent와 마찬가지로 attachTo를 통해 parent 요소에 우리가 만든 DOM요소를 붙여준다.

```tsx
// app.ts
import { ImageComponent } from './components/page/item/image.js';
import { PageComponent } from './components/page/page.js';

class App {
    private readonly page: PageComponent;
    constructor(appRoot: HTMLElement) {
        this.page = new PageComponent();
        this.page.attachTo(appRoot);

        const image = new ImageComponent('Image Title', 'https://picsum.photos/600/300');
        image.attachTo(appRoot, 'beforeend');
    }
}

new App(document.querySelector('.document')! as HTMLElement)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f84e27db-8f1f-40c0-a328-cce2c6c04c8f/Untitled.png)