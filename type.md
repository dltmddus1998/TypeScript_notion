# 2. 기본 타입 마스터 하기

## 기본 타입 정리 - JavaScript

---

```jsx
// JavaScript
// let - 선언 및    할당 이후 재할당 가능
// es6에 도입
let name = 'hello';
name = 'hi'

// const
// let과 달리 재할당 불가능
const age = 5;
age = 1;
```

### Primitivie (원시 타입)

### Object (객체 타입)

---

---

number, string, boolean, bigint, symbol, null, undefined

function, array ...

```tsx
// number
const num:number = 1;

// String
const str:string = 'hello';

// boolean
const boal:boolean = false;

// undefined - 값이 결정되지 않은 상태
let name: undefined; //💩 - 이렇게 쓰지 않음
let age: number | undefined; //👌
age = undefined;
age = 1;

// 무언가가 있고 없을 때 undefined를 많이 씀
function find(): number | undefined {
    return undefined;
}

// null - 값이 비어져 있는 명확한 상태
let person: null; //💩
let person2: string | null; //👌
```

```tsx
// unknown - 무슨 타입인지 모르는 상태 -> 가능하면 쓰지 말기!! 💩
// 타입스크립트에서 자바스크립트 라이브러리를 이용할 때 자바스크립트에서 리턴하는 타입을 모를 때 쓰긴 하지만 웬만하면 쓰지말기
let notSure: unknown = 0;
notSure = 'he';
notSure = true;

// any - 어떤 것이든 담을 수 있음 💩
let anything: any = 0;
anything = 'hello';
```

```tsx
// void - 함수에서 아무것도 리턴하지 않으면 저절로 void타입이 된다. (함수에서 타입 지정하는 것은 관례적이니 써주는게 좋다)
// 변수에서 쓰는 일은 굉장히 드물다.
function print(): void {
    console.log('hello');
    return;
}

// never
// 애플리케이션에서 예상치 못한 핸들링 못하는 에러를 발견했을 때 사용 - 리턴할 계획 없음을 명시하는 never
function throwError(message: string): never {
    // message -> server (log)
    throw new Error(message);
}
let neverEnding: never; // 💩

// object - primitive type 제외한 모든 object type 할당 가능 
let obj: object; // 💩 - object 중 어떤 타입인지 자세히 명시해줘야함!!
function acceptSomeObject(obj: object) {}
acceptSomeObject({name: 'ellie'});
acceptSomeObject({animal: 'dog'});
```

## 타입스크립트에서의 함수

---

> **자바스크립트에서 함수가 긴 경우에는 어떤 값을 전달해야할지 어떤 값을 리턴한다는 건지 모를 때가 많다. 
또한 내가 원하던 동작이 아닌 엉뚱한 동작을 할 때도 있다.
이를 개선하기 위해 TypeScript로 다음과 같이 표현할 수 있다.**
> 

```tsx
// JavaScript 💩
function jsAdd(num1, num2) {
    return num1 + num2;
}

// TypeScript 👌
function isAdd(num1: number, num2: number): number {
    return num1 + num2;
}
```

```tsx
// JavaScript 💩
function jsFetchNum(id) {
    // code ...
    // code ...
    // code ...
    return new Promise((resolve, reject) => {
        resolve(100);
    });
}

// TypeScript 👌
function fetchNum(id: string): Promise<number> {
    // code ...
    // code ...
    // code ...
    return new Promise((resolve, reject) => {
        resolve(100);
    });
}
```

## 함수 타입 이용 - spread, default, optional

---

> **타입스크립트에서는 함수를 호출할 때 파라미터 값 설정하는 부분을 좀 더 디테일하게 표현할 수 있다.**
> 

```tsx
// Javascript 🔥 => TypeScript
// Optional parameter
// 파라미터에 필수적으로 값이 들어가는 게 아니라 선택할 수 있다. (원래는 필수적이어서 값이 없거나 타입이 다르면 error) -> '?' 사용
function printName(firstName: string, lastName?: string) { // lastName은 optional
    console.log(firstName);
    console.log(lastName);
}
printName('Steve', 'Jobs');
printName('Anna', undefined);

// Default parameter - 기본값 설정
function printMessage(message: string = 'default message') {
    console.log(message);
}
printMessage();

// Rest parameter - 개수에는 상관없이 동일한 타입의 데이터를 함수 인자로 전달할 때 사용
function addNumbers(...numbers: number[]): number {
    return numbers.reduce((a, b) => {
        return a + b;
    })
}

console.log(addNumbers(1, 2)); // 3
console.log(addNumbers(1, 2, 3, 4)); // 10
console.log(addNumbers(1, 2, 3, 4, 5)); // 15
```

## 배열과 튜플, 언제 튜플을 사용해야 할까?

---

### 배열

> **Object의 불변성은 굉장히 중요하기 때문에 readonly는 빈번하게 사용된다.
readonly를 사용할 경우 뒤에 string 타입을 Array<.type.>이 아닌 string[] 형태로 써야하므로 일관성 있는 코드를 위해서 string[]로 써주는 것이 좋다.**
> 

```tsx
// Array
    const fruits: string[] = ['🍅', '🍌'];
    const scores: Array<number> = [1, 3, 3];
    function printArray(fruits: readonly string[]) { // readonly로 명시된 배열은 변경 불가능하다
```

### 튜플

> **배열이긴 하나 서로 다른 타입을 함께 가질 수 있는 배열이다.
원래는 배열을 선언하면 이 배열에는 지정해놓은 타입만 만들 수 있는데, 튜플은 서로 다른 타입을 가질 수 있는 것이다.**
> 

<aside>
💡 **튜플 사용은 권장하지 않는다.**

</aside>

> **튜플 대신에 interface, type alias, class로 대체해서 사용하는 것을 권장한다.
즉, 더 명시적으로 사용할 수 있는 타입이 많다.**
> 

```tsx
// Tuple 
let student: [string, number];
student = ['name', 123];
student[0]; // name
student[1]; // 123

// 구조분해할당을 이용하면 좀 더 명확하게 확인할 순 있지만,
// 데이터를 사용하는 곳에서 임의로 변수명을 써야한다는 단점이 존재한다.
const [name, age] = student;
```

<aside>
💡 **대체해서 사용할 수 있는 게 있음에도 불구하고 무작정 튜플을 사용하는 것은 나쁜 예제이다.
무언가 동적으로 리턴하는데 클래스나 인터페이스로 묶기가 애매하거나 동적으로 관련있는 다른 타입의 데이터를 묶어서 사용자가 이름을 정의해서 쓸 경우에는 튜플이 유용할 수 있지만, 그 외의 경우에는 클래스나 인터페이스로 사용할 수 있지 않은지 잘 생각해봐야 한다.
(튜플을 잘 사용한 예제로는 React Hooks에서의 useState를 예로 들 수 있다.)**

</aside>

## TS의 🌼 Type Alias

---

### Type Alias

> **새로운 타입을 내가 정의할 수 있다.**
> 

```tsx
// Type Aliases - 다양한 타입 직접 정의 가능
type Text = string; // Text라는 새로운 타입은 '문자열'을 의미함
const name: Text = 'seungyeon'; // const name: string = 'seungyeon'과 동일
const address: Text = 'Korea';
type Num = number;

type Student = { // 특정 객체 정의 가능
    name: string,
    age: number;
};
const student: Student = {
    name: 'seungyeon',
    age: 23,
}
```

```tsx
// String Literal Types - 타입을 문자열로도 지정할 수 있음
// 특정 문자열로 지정한 타입에는 해당 문자열만 할당할 수 있음
type Name = 'name';
let seungyeonName: Name ;
seungyeonName = 'name';
type JSON = 'json';
const json: JSON = 'json';
```

<aside>
💡 **String Literal Types는 왜 쓰는 거지? 라고 생각할 수 있는데 이를 아래에서 바로 다루겠다.**

</aside>

<aside>
💡 **원시 타입뿐만 아니라 객체 타입도 새로 정의할 수 있다.**

</aside>

## 진정한 타입스크립트의 시작! Union 타입

---

### Union 타입

> `**OR` 로 이해할 수 있다.
TS에서 코딩할 때 굉장히 많이 쓰는 타입이다.**
> 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e15779f-e68f-4447-b3d6-c417019a8f51/Untitled.png)

<aside>
💡 **왼쪽 사진과 같이 함수 move를 호출하는 순간 함수가 받을 수 있는 네 가지 타입을 자동으로 보여준다.**

</aside>

```tsx
// Union Types: OR
type Direction = 'left' | 'right' | 'up' | 'down';
function move(direction: Direction) { 
  // direction이라는 인자를 받아오는데 이 때 이 인자의 타입을 위에서 정의한 Direction으로 정해준다.
    console.log(direction);
}
move('right');

type TileSize = 8 | 16 | 32;
    const tile: TileSize = 16// 위에 정의된 3가지 숫자중 하나만 할당 가능하다.
```

```tsx
// 실전 예제
// function : login -> success || fail 
// 로그인 성공시 네트워크에서 받아온 response 리턴, 
//실패시 해당 이유를 보여주는 함수를 만들 것이다.

type SuccessState = {
    response: {
        body: string;
    };
};

type FailState = {
    reason: string;
};

type LoginState = SuccessState | FailState;

function login(id: string, password: string): LoginState {
    return {
        response: {
            body: 'logged in!',
        },
    }
}
```

```jsx
// printLoginState(state)
// success -> 🎉 body
// fail -> 😭 reason

// 💩 바람직하지 않은 코드
function printLoginState(state: LoginState) {
    if ('response' in state) {
        // state 객체 안에 response가 있다면
        // state.response.body에 접근이 가능해진다
        console.log(`🎉 ${state.response.body}`);
    } else {
        console.log(`😭 ${state.reason}`);
    }
}
```

<aside>
💡 **위 코드는 바람직한 코드는 아니다.
이를 개선한 Discriminated Union에 대해 바로 아래에서 설명하겠다.**

</aside>

<aside>
💡 위 **코드와 같이 Union Type은 발생할 수 있는 다양한 케이스 중에 하나만 정하고 싶을 때 자주 사용한다.**

</aside>

## 필수 타입! Discriminated Union

---

### Discriminated Union

> **Union Type의 차별화되는 이름이 동일한 타입을 둠으로써 간편하게 구분할 수 있는 것을 말한다.**
> 

```tsx
type SuccessState = {
        result: 'success';
        response: {
            body: string;
        };
    };
    type FailState = {
        result: 'fail';
        reason: string;
    };
    type LoginState = SuccessState | FailState;

    function login(id: string, password: string): LoginState {
        return {
            result: 'success',
            response: {
                body: 'logged in!',
            },
        }
    }

    // printLoginState(state)
    // success -> 🎉 body
    function printLoginState(state: LoginState) {
        if (state.result === 'success') {
            // state 객체 안에 response가 있다면
            // state.response.body에 접근이 가능해진다
            console.log(`🎉 ${state.response.body}`);
        } else {
            console.log(`😭 ${state.reason}`);
        }
    }
```

<aside>
💡 **위 코드를 보완하자.
먼저 정의해둔 successState, FailState 타입에서 result라는 똑같은 키를 추가해주고 각각 다른 값을 할당해준다.
그러면 각각 동일한 키를 가지고 있지만 어떤 state인지에 따라 다른 타입이 지정되어 있는 것이다.

결국 LoginState에서 result가 (`state.result`) 성공일수도 실패일수도 있지만 (`success || fail`) 공통되게 result라는 키를 가지고 있기 때문에 바로 접근이 가능한 것이다. (`state.result === ‘success’`)**

</aside>

> **이렇게 Discriminated Union 타입을 이용하면 좀 더 직관적으로 코드를 작성할 수 있다. 
✔︎`’response’ in state` 이 표현은 직관적지가 않고 부자연스러웠던 반면 `state.result === ‘success’` 은 직관적이며 자연스럽다.**
> 

> **이제 Union Type을 사용할 때 어떤 케이스든 공통적인 프로퍼티를 가지고 있음로써 조금 더 구분하기 쉽게 만든다라는 것을 알고 있어야겠다.**
> 

## Intersection 타입

---

### Intersection 타입

> `**AND**`로 생각할 수 있다.
**Union Type은 발생할 수 있는 모든 케이스 중에 한 가지만 선택하는 것이라면 Intersection Type은 그 모든 것을 합한 성격을 말한다.
즉, 다양한 타입들을 하나로 묶어서 선언할 수 있는 것이다.**
> 

```tsx
// Intersection Type
type Student = {
    name: string;
    score: number;
};

type Worker = {
    employeeId: number;
    work: () => void;
};

function internWork(person: Student & Worker) {
    console.log(person.name, person.employee, person.work());
    
}

internWork({
    name: 'seungyeon',
    score: 1,
    employeeId: 123,
    work(): void {},
})
```

<aside>
💡 **person이라는 타입은 Student & Worker 합친 타입이므로 두개의 정의된 타입의 모든 것들에 접근할 수 있다.**

</aside>

## Enum은 무엇이고 좋은건가?

---

### Enum

> **여러가지에 관련된 상수 값들을 한 곳에 모아서 정의할 수 있게 도와주는 타입이다.
JavaScript에는 이런 enum 타입이 존재하지 않으므로 TypeScript에서 자체적으로 제공하는 타입 중 하나이다.**
> 

> **JavaScript에서 상수를 정의할 때 보통 한번 정해지면 바뀌지 않는 어떤 특정한 고정 값을 나타낼 때 대문자 헝태로 사용한다. (ex> MAX_NUM, MAX_STUDENTS_PER_CLASS)
그런데, MONDAY, TUESDAY, ...와 같이 관련된 해당 상수를 정의하는 경우에 서로 연관되어 있지만 이것을 묶을 수 있는 타입이 따로 존재하지 않는다.
그래서 최대한 enum에 가깝게 표현할 수 있는 방법은 다음과 같다.**
> 

<aside>
💡 **즉, Enum Type은 여러가지 상수 값들을 한곳에 모아서 타입이 보장되고 이 타입의 값이 변화되지 않아 타입을 더 안전하게 사용할 수 있다.
그리고 이를 TypeScript에서는 다음과 같이 표현한다.**

</aside>

```tsx
const DAYS_ENUM = Object.freeze({"MONDAY": 0, "TUESDAY": 1, "WEDNESDAY": 2})
const dayOfToday = DAYS_ENUM.MONDAY; // 0 할당
```

```tsx
enum Days {
        Monday = 1,
        Tuesday,
        WednesDay,
        Thursday,
        Friday,
        Saturday,
        Sunday
    }
// console.log(Days.Saturday);
let day: Days = Days.Saturday;
day = Days.Tuesday;
day = 10; // 💩 no error
console.log(day); // 5
```

<aside>
💡 **따로 설정하지 않으면 0부터 시작한다. (따로 설정 가능)
문자열로 설정할 경우 수동적으로 하나씩 할당해줘야 한다.**

</aside>

<aside>
💡 **BUT, 타입스크립트에서 Enum 사용은 지양한다.
→ enum으로 타입이 지정된 변수에 다른 어떤 숫자도 할당할 수 있다는 것이 문제점이다.
예를 들어, 이미 `Days.Tuesday(2)`가 할당된 `day`라는 변수에 10이라는 숫자를 재할당해도 아무런 에러가 발생하지 않는 것이다. 컴파일 에러도 일어나지 않는다. 
대안책으로 TypeScript에서는 아래와 같이 Union Type을 활용할 수 있다.**

</aside>

 

```tsx
type DaysOfWeek = 'Monday' | 'Tuesday' | 'Wednesday';
    let dayOfweek: DaysOfWeek = 'Tuesday';
    dayOfweek = 'Monday';
```

<aside>
💡 Union Type으로 표현하면 이처럼 이미 ‘Tuesday’를 할당한 dayOfweek라는 변수에 재할당할 때 정해진 타입 중에서만 선택해서 사용할 수 가 있다.

</aside>

> **이처럼 enum은 충분히 union type으로 대체할 수 있으므로 enum을 굳이 사용하지 않고 union type을 이용해서 충분히 필요한 것들을 타입을 보장하면서 사용할 수 있다.**
> 

> **<예외>
enum을 사용할 수 밖에 없는 경우는 딱 하나 있다.
다른 모바일 클라이언트 (Android, IOS)는 Kotlin이나 Swift 같은 언어를 사용하므로 사용자의 데이터를 JSON으로 묶어서 다시 다른 클라이언트에게 보내야할 때 모바일 클라이언트에서 사용하는 Native Programming Language에서는 Union Type을 표현할 수 있는 방법이 없으므로 서로 이해할 수 있는 enum타입을 쓴다.**
> 

## 타입 추론이란?

---

### 타입 추론 (Type Inference)

> 타입이 뻔한 경우에는 타입을 생략하고 작성해도 된다. (Type Inference)
Parameter은 어떤 값이든 받을 수 있다. → 따라서 인자는 어떤 타입의 데이터를 받을건지 확실히 명시해줘야 한다.
> 

<aside>
💡 **타입 추론 좋은걸까...?

No!!!!
물론 간단하게 작성한 코드에서는 타입 추론이 되지만 보통 프로젝트를 진행할 때는 코드가 복잡하기 때문에 타입을 정확하게 명시해주는 것이 좋다.
다만, 원시 타입 같은 경우에는 뻔하기 때문에, 이런 경우는 생략이 가능하다. 그러나 함수 같은 경우에는 코드가 안에 들어있기 때문에 명시해줘야 한다. 
따로 리턴되는 타입이 정해져 있는 경우 타입을 명시해주는 습관을 들이는 것이 중요하다. 
예외로,void 타입인 경우 생략이 가능하다.**

</aside>

<aside>
💡 **타입 추론 같은 경우 팀 프로젝트 진행시 정확하게 스타일 가이드를 확정해서 ‘우리는 이런식으로 타입을 명시해야 하고 이런 경우에는 생략이 가능하다’는 부분을 명확하게 정한 후, 가독성을 생각해서 일관성있게 코딩하는게 중요하다.**

</aside>

## Type Assertion

---

> Type Assertion은 가급적 피하는 것이 좋지만 불가피하게 사용해야하는 경우가 있다.
100퍼센트 타입을 장담할 수 있을 때만 사용해야 한다.

코딩시에는 문제가 없는데 실행했을 때 문제가 생길 확률이 높다.
> 

```tsx
// Type Assertion
function jsStrFunc(): any {
    return 2;
}
//1. 
const result = jsStrFunc();
// result라는 변수가 string 타입인걸 확신한다 -> type assertion 사용 가능 
console.log((result as string).length);

//2.
const wrong: any = 5;
console.log((wrong as Array<number>).push(1)); // 💩

//3.
function findNumbers(): number[] | undefined {
    return undefined;
}
const numbers = findNumbers(); 
numbers?.push(2); // 이게 배열일거라 확신한다면 ?추가 -> 옵션이 아니라 절대적으로 값이 있다고 확신할 때 쓰는 것.

//4.
const button = document.querySelector('class')!; // 무조건 Null이 아니라 값이 있을거라고 확신할 때 ! 추가
if (button) {
    button.nodeValue;
}
```

<aside>
💡 **result가 string타입인게 확실할 때 다음과 같이 as string이라는 문구를 통해 length라는 문자열에 해당하는 메서드를 사용할 수 있다.

두번째 예제에서 `wrong`이 5라는 숫자가 할당됐는데도 `Array<number>`, 즉 배열로 설정 후 `push`라는 배열 메서드를 쓰는 경우 코드 자체에는 에러가 없지만 실행시 에러가 나올 수 밖에 없는 것이다.

세번째 예제에서 `findNumbers()`라는 함수가 `number[]` 타입으로 리턴할 것이 확실하다면 `?`를 추가하면 된다. (옵션이 아니라 절대적인 의미이다.)

네번째 예제의 경우 돔을 이용해서 코딩할 때 `class`가 `null`일 수도 있지만 코딩 작성자가 `null`이 아니라 확신할 경우 `!`을 추가하면 된다.**

</aside>

<aside>
💡 **위 네가지 경우가 Type Assertion을 사용하는 경우이다. 
코딩시 100프로 장담하는 경우가 아닌 이상 사용을 지양해야 한다.**

</aside>