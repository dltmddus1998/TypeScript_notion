<aside>
💡 자바스크립트를 이용하는 곳에서 타입스크립트를 이용할 수 있지만 꼭 컴파일러를 통해 자바스크립트로 변환해야 사용할 수 있다.

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/254c8267-c75c-4972-bb09-13bd014c7492/Untitled.png)

<aside>
💡 이렇게 번거롭게 변환할 필요 없이 바로 실행할 수 있게 도와주는 툴이 있다.

</aside>

```
> npm install -g ts-node
> ts-node main.ts // 바로 실행 가능

> tsc main.ts -w // watch 모드로 ts파일을 변경할 때 지속적으로 ts를 실행하는 역할을 함
```

## What is TypeScript?

---

✍️ TypeScript는 2012년 10월에 MS사에서 만든 언어로 오픈 소스 프로젝트로 존재한다. 
⭐️ TypeScript가 완전히 새 언어가 아닌 JavaScript를 기본으로 한 *“Superset of JavaScript”*로서 JavaScript를 한 단계 감싸기 때문에, **JavaScript가 동작하는 어떤 곳에서나 (Browser, OS) 대체해서 사용이 가능하다**.

> **TypeScipt는 Client side와 Server side 모두 JavaScript가 존재하는 어떤 곳에서도 사용이 가능하다 
→ TypeScript를 그대로 쓸 수 있는 것이 아니라 TypeScript로 transcompiling해서 JavaScript로 변환해서 쓰는 것이다. 
+) compiler: TS 툴 자체에서 제공되는 컴파일러나 BABEL 사용**
> 

![TypeScript & JavaScript 집합도](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a7cb3457-9a97-4a97-b183-07b92a0ea6e3/Untitled.png)

TypeScript & JavaScript 집합도

### JavaScript 특징

1. **Constructor Functions** 
    
    class를 이용해서 Object를 만드는 것처럼, 간편하게 Object를 만들 수 있다.
    
2. **Dynamically Typed**
    
    **동적 타입** → *실시간으로 타입이 결정되기 때문에, 런타임 환경 시 에러 발생*하는 게 문제점이다.
    
3. **Runtime Errors**
4. ES6에서 “*class-like*”이 도입됐지만, prototype-based & class만으로 할 수 있는 게 많지 않다.

### TypeScript 특징

1. **Prototype Based**
    
    JavaScript도 엄밀히 말하면 객체 지향 프로그래밍 언어이므로 prototype-based로 구현 가능하다.
    
2. **Statically Typed**
    
    정적 타입 → JavaScript에서의 동적 타입으로 인한 문제점 (런타임 환경 시 에러 발생)을 보완하여, 컴파일 시 타입 에러를 잡을 수 있다.
    
3. **Compile Errors**

<aside>
💡 `**TS**` 
✔︎ 다른 클래스를 기본으로 한 “*객체 지향 프로그래밍 언어*”처럼 `class` `interface` `generic`도 활용할 수 있다.
✔︎ 더 막강한 객체 지향 프로그래밍 언어가 가능해졌다. (`**TS` = `JS` + `Future` + `그 이상의 것`**)

</aside>

<aside>
💡 **TS vs.JS**
✔︎ `JS`는 프로그램 동작시 타입이 결정되는 조금 위험한 프로그래밍 언어
✔︎ 이에 반해, `TS`는 **타입이** **정적**으로 결정되는, 즉 **우리가 코딩할 때 타입이 결정돼서 코딩할 때 즉각적으로 타입 에러를 받아볼 수 있다**. 
→ “TypeScript를 배워야 하는 이유”

</aside>

### 💡 **프로그래밍 언어를 나누는 기준 중, “타입이 언제 결정되냐”에 따라 나누는 기준이 있다.**

---

**Dynamically Typed (동적 결정)** - `python`, `ruby`, `Lua`, `php`, `JavaScript`

**Statically Typed (정적 결정)** - `TypeScript`, `scala`, `go`, `Java`, `Kotlin`, `Swift`, `C`, `C++`

### 컴파일러 (Compiler)

---

**Compiler**

> ⭐️ **코드를 프로그램 형태로 동작시키기 위해선, 그 프로그램이 동작하는 환경에서 이해할 수 있는 언어나 Binary 형태로 변환해주는 작업을 거쳐야 한다.**
> 

✍️ **컴파일 시간에 타입 결정 및 확인 가능 → 정젹 타입**

```tsx
let age: number = 10;
age = 'hello'; // 컴파일 or 코딩 시 경고 메시지
```

1. 변수 선언시 타입을 명시해서 작성해야 한다.
2. 한번 결정된 타입은 절대 바꿀 수 없다.
**→ 타입이 좀 더 엄격히 관리됨.**

✍️ **프로그램이 동작할 때 타입 결정 및 확인 가능 → 동적 타입**

```jsx
let age = 10;
age = 'hello'
```

1. 변수 선언 및 할당
2. 재할당 가능
    
    → 컴파일 시간 문제 x 
    

### 💡 그렇다면 사용하기 쉬운 JavaScript를 사용하면 되지 않나?

---

> **결론부터 말하자면, 당연히 No다!!**
> 

물론, JavaScript는 easy하고 flexible하며 fast하다. 때문에 Front-end의 경우 JavaScript로서 접근이 비교적 쉬운 것이다. 
그러나 앞에서 말했던 것처럼, JavaScript는 장점도 있지만 단점이 너무 뚜렷하다.

1. JS는 타입이 없어서 **가독성이 매우 떨어진다**.
    1. 해당 변수는 어떤 데이터를 담는지
    2. 해당 함수는 어떤 것을 인자로 받아서 어떤 연산을 하는지

→ 개발시 Issue를 빠르게 잡아낼 수 없다.

👿 사용자가 나의 애플리케이션을 사용하면서 발견하는 에러의 수가 증가한다.

👀 TypeScript를 사용하면 “*실시간으로 에러에 대한 검사 가능*”하다.

→ **따라서, TypeScript를 통해 보다 더 안정적이고, 확장이 쉬운 소프트웨어를 제작할 수 있다.**

### ✚) 객체 지향 프로그래밍 
(OOP: Object-Oriented Programming) → modern programming paradigm

---

1. **modularity: 객체 위주로 모듈성 있는 코드 작성**
2. **extensible: 객체 단위 확장성**
3. **reusability: 재사용성**
4. **maintainability: 기존 코드의 문제 해결이나 새로운 기능 추가시, 유지보수 용이**