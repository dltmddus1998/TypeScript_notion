## ⁇ 제네릭 함수란?

---

> **제네릭을 이용하면 어떠한 타입이든 받을 수 있고, 코딩할 때 타입이 결정되므로 타입을 보장받을 수 있다.**
> 

> **사용자가 타입에 숫자를 넣으면 숫자 타입, 문자열을 넣으면 문자 타입, 이런식으로 리턴되는게 제네릭 함수라고 생각하면된다.**
> 

> **제네릭의 타입은 보통 대문자 하나만 쓴다.**
> 

```tsx
// 나쁜 예시
{
    function checkNotNullBad(arg: number | null): number {
        if (arg == null) {
            throw new Error('not valid number');
        }
        return arg;
    }

    function checkNotNullAnyBad(arg: any | null): any {
        if (arg == null) {
            throw new Error('not valid number');
        }
        return arg;
    }    
}
```

```tsx
// 좋은 예시 -> Generic 사용 (<>)
{
	function checkNotNull<T>(arg: T | null): T {
        if (arg == null) {
            throw new Error('not valid number');
        }
        return arg;
    }

    const number = checkNotNull(123);
    console.log(number);
    const boal: boolean = checkNotNull(true);
    console.log(boal);
}
```

## ⁇ 제네릭 클래스란?

---

> 제네릭을 잘 활용하면, 활용성 높은 클래스를 만들 수 있다.
> 

```tsx
// either: a or b
interface Either<L, R> {
    left: () => L;
    right: () => R;
}

class SimpleEither<L, R> implements Either<L, R> {
    constructor(private leftValue: L, private rightValue: R) {}
    left(): L {
        return this.leftValue;
    }
    right(): R {
        return this.rightValue;
    }
}

const either: Either<number, number> = new SimpleEither(4, 5);
either.left(); // 4
either.right(); // 5
const best = new SimpleEither({ name: 'seungyeon' }, 'hello');
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac05afbb-ccf8-40b0-a45d-9d504ed9a7a9/Untitled.png)

<aside>
💡 **best 변수에 마우스를 올리면 다음과 같이 제네릭의 특성에 의해 유동적으로 타입을 결정할 수 있음을 알 수 있다.**

</aside>

## ⭐️ 제네릭 조건

---

```tsx
interface Employee {
    pay(): void;
}

class FullTimeEmployee implements Employee {
    pay() {
        console.log(`full time!!`);
    }

    workFullTime() {

    }
}

class PartTimeEmployee implements Employee {
    pay() {
        console.log(`part time!!`);
    }

    workPartTime() {

    }
}

// 세부적인 타입을 인자로 받아서 추상적인 타입으로 다시 리턴하는 함수 -> 👿
function payBad(employee: Employee): Employee {
    employee.pay();
    return employee;
}

// Employee를 확장한 타입만 가능하다는 뜻
function pay<T extends Employee>(employee: T): T {
    employee.pay();
    return employee;
}

const ellie = new FullTimeEmployee();
const bob = new PartTimeEmployee();
ellie.workFullTime();
bob.workPartTime();

const ellieAfterPay = pay(ellie);
const bobAfterPay = pay(bob);
```

> **✔︎ payBad 함수는 Employee라는 세부적인 타입을 인자로 받은 경우인데, 이런 추상적인 타입으로 다시 리턴하는 함수는 지양해야 한다.

✔︎ pay 함수에서 `T extends Employee`라는 제네릭 표현을 통해 잘못된 함수를 수정했다.
→ 여기서 `T extends Employee`는 *“Employee를 확장한 타입만 가능하다”*는 의미이다.**
> 

```tsx
// 👀 추가 예제

// K extends keyof T : 객체 T 안에 있는 키의 타입
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const obj = {
    name: 'ellie',
    age: 20,
};
console.log(getValue(obj, 'name')); // ellie
```

> **✔︎ 단, 위에서 getValue의 key에 name외에 다른 것은 쓸 수 없다.**
>