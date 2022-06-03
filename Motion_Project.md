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
:root {
    --bg-main-color: #000000;
    --bg-accent-color: #2d2d2d;
    --accent-color: #EB5454;
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
    border-radius: 4px;
}

.document {
    height: 100%;
    overflow-y: auto;
    background-color: var(--document-bg-color);
    /* Firefox (scrollbar styling) */
    scrollbar-width: 6px;
    scrollbar-color: var(--bg-accent-color);
    scrollbar-face-color: var(--accent-color);
}

.document::-webkit-scrollbar {
    /* safari and chrome */
    background-color: var(--bg-accent-color);
    width: 6px;
}

.document::-webkit-scrollbar-thumb {
    /* safari and chrome */
    background-color: var(--accent-color);
}

.footer {
    background: var(--bg-main-color);
    border-top: 1px solid var(--border-color);
    text-align: center;
    color: white;
}
```

<img src="https://user-images.githubusercontent.com/73332608/171009184-4a20d3e3-2897-4864-93cc-2f54d8f1e5bd.png" width="350" height="400">

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

<img src="https://user-images.githubusercontent.com/73332608/171009359-2331afe9-5855-45c1-982f-c506e3a02a7e.png" width="350" height="400">

<br>

## 🏭 Component Refactoring

---

```tsx
// component.ts
export interface Component {
    attachTo(parent: HTMLElement, position?: InsertPosition): void;
}

// Encapsulate the HTML element creation 
export class BaseComponent<T extends HTMLElement> implements Component {
    // 한 번 만들어진 요소는 변경 불가 (요소 안의 상태들은 변경 가능)
    protected readonly element: T;
    constructor(htmlString: string) {
				// 1.
        const template = document.createElement('template');
        template.innerHTML = htmlString;
        this.element = template.content.firstElementChild! as T;
    }

    attachTo(parent: HTMLElement, position: InsertPosition = 'afterbegin') {
        parent.insertAdjacentElement(position, this.element);
    }
}
```

✔︎ **더 다양한 서브 클래스를 사용할 수 있도록 제네릭을 만들어 보자.**

**BaseComponent**

→ *“Encapsulate the HTML element creation”* **(HTML 요소 생성을 캡슐화한다.)**

✔︎ 즉, `BaseComponent`에 어떤 것을 만들고 싶은지 string 타입의 HTML을 전달하면 알아서 `element`를 만들게 된다. 그리고 `attachTo`라는 `API`를 통해 `parent`에 만든 요소를 붙일 수 있다. 이렇게 `API`가 있다면 `BaseComponent`로 의사소통하는 것보다는 `Interface`를 활용하는 것이 낫다.
**→ attachTo를 선언한 Component 인터페이스 생성 및 BaseComponent클래스에 상속.**

1. `image.ts`에 전달할 부분을 `constructor`내부에 작성해준다. htmlString은 `image.ts`에서 `super`을 통해 전달받은 것을 `template.innerHTML`에 할당해준다.

```tsx
// image.ts
import { BaseComponent } from './../../component.js';
export class ImageComponent extends BaseComponent<HTMLElement> {
    constructor(title: string, url: string) {
        super(`<section class="image">
                <div class="image__holder"><img src="" alt="" class="image__thumbnail"></div>
                <p class="image__title"></p>
            </section>`);

        const imageElement = this.element.querySelector('.image__thumbnail')! as HTMLImageElement;
        imageElement.src = url;
        imageElement.alt = title;

        const titleElement = this.element.querySelector('.image__title')! as HTMLParagraphElement;
        titleElement.textContent = title;
    }
}
```

✔︎ BaseComponent를 import해 ImageComponent클래스에 상속했다.

✔︎ super을 통해 htmlString을 전달해준다.

```tsx
// page.ts
import { BaseComponent } from './../component.js';

export class PageComponent extends BaseComponent<HTMLUListElement>{
    constructor() {
        super('<ul class="page">This is PageComponent!</ul>')
    }
}
```

✔︎ super을 통해 htmlString을 다음과 같이 보낸다. 원래의 방식과는 다르게 string타입으로 표현했다.

**Component**

- App - 어플리케이션 전체를 가지고 있는 제일 큰 컨테이너 클래스
- PageComponent - 사용자가 추가하는 문서를 담을 수 있는 페이지 컨테이너 컴포넌트 클래스
- ImageComponent - 사용자가 추가할 수 있는 문서중 하나의 타입, 이미지 노트

**‼️ 리팩토링 하기전**

서로 다른 역할을 가진 로직들을 각각 다른 클래스로 묶어줌 

**→ 문제점: PageComponent & ImageComponent 코드 중복 (element, attachTo)** 

**→ 해결책**

**중복되는 속성과 행동들을 공통적인 클래스로 정의해둠. (`BaseComponent`)**

**>> 상속을 통해 중복된 코드의 반복 없이 모든 속성과 행동 표현 가능.**

**→ 코드의 재사용성 up**

## Note, Todo Component

---

```tsx
// note.ts
import { BaseComponent } from './../../component.js';
export class NoteComponent extends BaseComponent<HTMLElement> {
    constructor(title: string, body: string) {
        super(`<section class="note">
                <h2 class="note__title"></h2>
                <p class="note__body"></p>
            </section>`);
        const titleElement = this.element.querySelector('.note__title')! as HTMLElement;
        titleElement.textContent = title;

        const bodyElement = this.element.querySelector('.note__body')! as HTMLParagraphElement;
        bodyElement.textContent = body;
    }
}
```

```tsx
// todo.ts
import { BaseComponent } from './../../component.js';
export class TodoComponent extends BaseComponent<HTMLElement> {
    constructor(title: string, todo: string) {
        super(`<section class="todo">
                <h2 class="todo__title"></h2>
                <input type="checkbox" class="todo-checkbox">
            </section>`);
        const titleElement = this.element.querySelector('.todo__title')! as HTMLElement;
        titleElement.textContent = title;

        const todoElement = this.element.querySelector('.todo-checkbox')! as HTMLInputElement;
        todoElement.insertAdjacentText('afterend', todo);
    }
}
```

<br>


```tsx
// app.ts
import { TodoComponent } from './components/page/item/todo.js';
import { NoteComponent } from './components/page/item/note.js';
import { ImageComponent } from './components/page/item/image.js';
import { PageComponent } from './components/page/page.js';

class App {
    private readonly page: PageComponent;
    constructor(appRoot: HTMLElement) {
        this.page = new PageComponent();
        this.page.attachTo(appRoot);

        const image = new ImageComponent('Image Title', 'https://picsum.photos/600/300');
        image.attachTo(appRoot, 'beforeend');

        const note = new NoteComponent('Note Title', 'Note Body');
        note.attachTo(appRoot, 'beforeend');

        const todo = new TodoComponent('Todo Title', 'Todo Item');
        todo.attachTo(appRoot, 'beforeend');
    }
}

// 동적으로 만드는게 아니라 개발시 정확히 정해진 경우 -> 무조건 null 아니고 HTMLElement 타입이라고 Type Assertion로 표시
new App(document.querySelector('.document')! as HTMLElement)
```

## Video Component

---

**Video 주소 가져오는 방법**

1. 주소창에 있는 URL
2. 영상에서 마우스 오른쪽 클릭 → copy video url

‼️ **궁극적인 목적 
→ 위에서 받아온 video 주소를 이용하여 embedded url로 만들어 코드에 작성해야 한다.**

✔︎ **“정규표현식(Regex)”**를 이용하여 문자열의 다수의 특정한 패턴에서 해당하는 문자열을 가져올 수 있다.

```tsx
// video.ts
import { BaseComponent } from './../../component.js';
export class VideoComponent extends BaseComponent<HTMLElement> {
    constructor(title: string, url: string) {
        super(`<section class="video">
                <div class="video__player"><iframe class="video__iframe"></iframe></div>
                <h3 class="video__title"></h3>
            </section>`);
        
        const iframe = this.element.querySelector('.video__iframe')! as HTMLIFrameElement;
        // url에서 video_ID만 추출하는 함수를 만들어보자.
        iframe.src = this.convertToEmbeddedURL(url);
        

        const titleElement = this.element.querySelector('.video__title')! as HTMLHeadingElement;
        titleElement.textContent = title;
    } 
    

    private convertToEmbeddedURL(url: string): string {
        const regExp = /^(?:https?:\/\/)?(?:www\.)?(?:(?:youtube.com\/(?:(?:watch\?v=)|(?:embed\/))([a-zA-Z0-9]{11}))|(?:youtu.be\/([a-zA-Z0-9]{11})))/;
        const match = url.match(regExp);

        const videoId = match? match[1] || match[2] : undefined;
        if (videoId) {
            return `https://www.youtube.com/embed/${videoId}`;
        }
        return url;
    }
}

// <iframe width="950" height="534" src="https://www.youtube.com/embed/yA4d5ZydVVQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

✔︎ `super`을 통해 비디오를 넣어줄 `section` 태그를 전달해준다.

✔︎ url에서 video ID만 따로 추출하는 함수 `convertToEmbeddedURL`을 선언한다.
→ 이 때, **정규 표현식**을 활용한다.
→ videoId가 존재할 경우 임베드된 video url을 반환할 수 있도록 한다.

```jsx
// app.ts
import { VideoComponent } from './components/page/item/video.js';
import { TodoComponent } from './components/page/item/todo.js';
import { NoteComponent } from './components/page/item/note.js';
import { ImageComponent } from './components/page/item/image.js';
import { PageComponent } from './components/page/page.js';

class App {
    private readonly page: PageComponent;
    constructor(appRoot: HTMLElement) {
        this.page = new PageComponent();
        this.page.attachTo(appRoot);

        const image = new ImageComponent('Image Title', 'https://picsum.photos/600/300');
        image.attachTo(appRoot, 'beforeend');

        const video = new VideoComponent('Video Title', 'https://www.youtube.com/embed/yA4d5ZydVVQ');
        video.attachTo(appRoot, 'beforeend');

        const note = new NoteComponent('Note Title', 'Note Body');
        note.attachTo(appRoot, 'beforeend');

        const todo = new TodoComponent('Todo Title', 'Todo Item');
        todo.attachTo(appRoot, 'beforeend');
    }
}

// 동적으로 만드는게 아니라 개발시 정확히 정해진 경우 -> 무조건 null 아니고 HTMLElement 타입이라고 Type Assertion로 표시
new App(document.querySelector('.document')! as HTMLElement)
```

### ⚙️ 정규 표현식이란?

[정규 표현식](https://www.notion.so/a55958d343b44ca0b9c190063661e478) 

[RegExr: Learn, Build, & Test RegEx](https://regexr.com/517nr)
