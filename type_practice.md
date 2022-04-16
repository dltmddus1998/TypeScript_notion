## 💡 기본 타입 관련 예제 연습하기

### 계산기 만들기

```tsx
// 결과물
console.log(calculate('add', 1, 3)); // 4
console.log(calculate('substract', 3, 1)); // 2
console.log(calculate('multiply', 4, 2)); // 8
console.log(calculate('divide', 4, 2)); // 2
console.log(calculate('remainder', 5, 2)); //1
```

```tsx
// 나의 풀이
function calculate(symbol: string, num1: number, num2: number) {
    if (symbol === 'add') {
        return num1 + num2;
    } else if (symbol === 'substract') {
        return num1 - num2;
    } else if (symbol === 'multiply') {
        return num1 * num2;
    } else if (symbol === 'divide') {
        return num1 / num2;
    } else {
        return num1 % num2;
    }
}
```

```tsx
// 정답
type Command = 'add' | 'substract' | 'multiply' | 'divide' | 'remainder';
function calculate(command: Command, a: number, b: number): number {
    switch(command) {
        case 'add':
            return a + b;
        case 'substract':
            return a - b;
        case 'multiply':
            return a * b;
        case 'divide':
            return a / b;
        case 'remainder':
            return a % b;
        default:
            throw Error('unknown command');
    }
}
```

### 좌표 게임

```tsx
// 결과물
console.log(position); // { x: 0, y: 0 }
move('up');
console.log(position); // { x: 0, y: 1 }
move('down');
console.log(position); // { x: 0, y: 0 }
move('left');
console.log(position); // { x: -1, y: 0 }
move('right');
console.log(position); // { x: 0: y: 0 }
```

```tsx
// 나의 풀이 === 정답
const position = {
    x: 0,
    y: 0,
}

type Direction = 'up' | 'down' | 'left' | 'right';

function move(direction: Direction) {
    switch(direction) {
        case 'up':
            position.y++;
            break;
        case 'down':
            position.y--;
            break;
        case 'left':
            position.x--;
            break;
        case 'right':
            position.x++;
            break;
        default:
            throw Error('unknown direction');
    }
}
```

### 로딩 상태 표시

```tsx
// 문제
{
    type LoadingState = {
        state: 'loading';
    };

    type SuccessState = {
        state: 'success';
        response: {
            body: string;
        };
    };

    type FailState = {
        state: 'fail';
        reason: string;
    };

    type ResourceLoadState = LoadingState | SuccessState | FailState;

    // 코드 작성하기 (나의 풀이 === 정답)
		function printLoginState(state: ResourceLoadState) {
        switch(state.state) {
            case 'loading':
                console.log(`👀 ${state.state}`);
                break;
            case 'success':
                console.log(`😊 ${state.response.body}`);
                break;
            case 'fail':
                console.log(`😭 ${state.reason}`);
                break;
            default:
                throw Error(`${state} is unknown state`);
        }
    }

    printLoginState({ state: 'loading' }); // 👀 loading...
    printLoginState({ state: 'success', response: { body: 'loaded' } }); // 😊 loaded
    printLoginState({ state: 'fail', reason: 'no network' }); // 😭 no network
}
```