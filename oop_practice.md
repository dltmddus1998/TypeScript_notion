## ？객체지향적으로 커피기계 만들기

---

### 절차지향적으로 커피기계 만들기

```tsx
{
    type CoffeeCup = {
        shots: number;
        hasMilk: boolean;
    }

    const BEANS_GRAMM_PER_SHOT: number = 7;
    let coffeeBeans: number = 0;
    function makeCoffee(shots: number): CoffeeCup {
        if (coffeeBeans < shots * BEANS_GRAMM_PER_SHOT) {
            throw new Error('Not enough coffee beans!!');
        }
        coffeeBeans -= shots * BEANS_GRAMM_PER_SHOT;
        return {
            shots,
            hasMilk: false
        };
    }

    coffeeBeans += 3 * BEANS_GRAMM_PER_SHOT;
    const coffee = makeCoffee(2);
    console.log(coffee);
    
}

// 출력
// { shots: 2, hasMilk: false }
```

### 1️⃣ 객체지향적으로 커피기계 만들기 (static 사용)

```tsx
{
    type CoffeeCup = {
        shots: number;
        hasMilk: boolean;
    }

    class CoffeeMaker {
        static BEANS_GRAMM_PER_SHOT: number = 7;
        coffeeBeans: number = 0;

        constructor(coffeeBenas: number) {
            this.coffeeBeans = coffeeBenas; // 해당 클래스 안에 있는 coffeeBeans를 전달된 coffeeBeans만큼 할당
        }

        makeCoffee(shots: number): CoffeeCup {
            if (this.coffeeBeans < shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT) {
                throw new Error('Not enough coffee beans!!');
            }
            this.coffeeBeans -= shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT;
            return {
                shots,
                hasMilk: false
            };
        }
    }

    const maker = new CoffeeMaker(32);
    console.log(maker);

    const maker2 = new CoffeeMaker(34);
    console.log(maker2);
    
    
}

// 출력
// CoffeeMaker { BEANS_GRAMM_PER_SHOT: 7, coffeeBeans: 32 }
```

<aside>
💡 **내 클래스 안에 있는 멤버변수에 접근할 때는 this를 사용해야 한다.**

</aside>

<aside>
💡 **constructor : 클래스로 오브젝트 인스턴스를 만들 때 항상 호출되는 함수**

</aside>

<aside>
💡 **`BEANS_GRAMM_PER_SHOT`의 경우 클래스 내부에서 연결된 정보이고 변하지 않는 상수이다. 그런데 이렇게 멤버변수로 작성하게 되면 클래스로 만드는 오브젝트마다 해당 상수 (7)이 들어있게된다. 
이처럼 클래스에서 한번 정의되고 클래스를 이용한 오브젝트 사이에서 모두 공유될 수 있는 것들은 멤버변수로 두면 오브젝트 생성할 때마다 중복되어 나타나므로 메모리 낭비 가능성이 높다. 
해결책으로는 `static`이라는 키워드를 붙이면 class level로 분류된다.
class level은 class와 연결되므로 오브젝트 생성할 때마다 나타나지 않는다.여기서 class level은 클래스 안에 있는게 아닌 클래스 자체에 있는 것이므로 this를 사용하지 않는다.
대신, 클래스 이름을 지정해야 한다. (`this` → `CoffeeMaker`)
(붙이지 않은 것은 instance|object level이라 불린다.)**

</aside>

<aside>
💡 **클래스는 관련된 속성과 함수들을 묶어서 어떤 모양의 데이터가 될거라는 걸 정의하는 것이고, 이 클래스에 실제 데이터를 넣어서 오브젝트를 만들 수 있다.**

</aside>

### 만약에 constructor을 사용하고 싶지 않다?

```tsx
{
    type CoffeeCup = {
        shots: number;
        hasMilk: boolean;
    }

    class CoffeeMaker {
        static BEANS_GRAMM_PER_SHOT: number = 7;
        coffeeBeans: number = 0;

        constructor(coffeeBenas: number) {
            this.coffeeBeans = coffeeBenas; // 해당 클래스 안에 있는 coffeeBeans를 전달된 coffeeBeans만큼 할당
        }

        static makeMachine(coffeeBeans: number): CoffeeMaker {
            return new CoffeeMaker(coffeeBeans);
        }

        makeCoffee(shots: number): CoffeeCup {
            if (this.coffeeBeans < shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT) {
                throw new Error('Not enough coffee beans!!');
            }
            this.coffeeBeans -= shots * CoffeeMaker.BEANS_GRAMM_PER_SHOT;
            return {
                shots,
                hasMilk: false
            };
        }
    }

    const maker = new CoffeeMaker(32);
    console.log(maker);

    const maker2 = new CoffeeMaker(34);
    console.log(maker2);
    
    const maker3 = CoffeeMaker.makeMachine(3);
}
```

<aside>
💡 **클래스 내부의 어떠한 속성값도 필요로 하지 않으므로 앞에 static을 붙여주면 된다.
이를 통해, 외부에서도 클래스 만들지않고 `CoffeeMaker` 클래스에 있는 `makeMachine` 함수를 통해 간단하게 커피기계를 만들 수 있다.
만약 static을 붙이지 않는다면, `CoffeeMaker.makeMachine()`은 사용불가하고, 위에 만들어진 오브젝트를 통해 (`maker.makeMachine()`) 함수를 호출할 수 있다.**

</aside>