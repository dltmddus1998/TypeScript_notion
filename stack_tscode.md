## 👀 스택 구조 구현해보기

```jsx
interface Stack {
    readonly size: number;
    push(value: string): void;
    pop(): string;
}

// 전달하는 value를 한단계 감싸는 node라는 것을 만들어서 
// head가 새로들어온 값을 가리키고, 이전에 가리키던 것이 있다면, 그것들을 찾아가는 형식. 
type StackNode = {
    // 항상 사용자가 데이터를 넣어서 한단계 감싸는 무언가를 만든다면 
    // 불변성을 유지하는 것이 좋다. -> readonly
    readonly value: string;
    readonly next?: StackNode; 
		// 다음 스택 노드를 가리키거나 아무것도 가리키지 않는다. ?로 optional 설정
}

class StackImpl implements Stack {
    // 내부에서 동일한 이름을 쓰는 경우 앞에 _(언더바)를 붙여주면 된다.
    private _size: number = 0;
    
    // head는 stacknode를 가리킬수도 아닐수도 있음
    private head?: StackNode;

    // 보통 constructor을 통해 size를 설정해놓는다.
    constructor(private capacity: number) {}

    // getter만 있고 setter는 없기때문에 내부적으로만 변경할 수 있다.
    get size() {
        return this._size;
    }
    
    // node를 먼저 만들고 
    // 2. 새로운 node는 head가 가리키는 값을 가리키도록
    // 3. head가 새로운 값을 가리키도록
    push(value: string) {
        if(this._size === this.capacity) {
            throw new Error('Stack is full!');
        }
        const node: StackNode = {
            value,
            // 2. 
            next: this.head
        };
        // 3. 
        // 방금 만든 노드 할당
        this.head = node;
        this._size++;
    }

    // head가 가리키고 있는 값 리턴
    pop(): string {
        // 스택이 비어있는경우 node는 없을수도 있다
				// null == undefined, null !== undefined (null, undefined모두 거르기 위해 == 사용)
        if (this.head == null) { 
            throw new Error("Stack is Empty");
        }
        const node = this.head;
        this.head = node.next;
        this._size--;
        return node.value;
    }
}

const stack = new StackImpl(10);
stack.push('sy 1');
stack.push('bob 2');
stack.push('steve 3');
while(stack.size !== 0) {
    console.log(stack.pop());
}
```

<aside>
💡 **Stack 구조를 TypeScript를 이용해 구현해보자**

</aside>

1. **우선 전달하는 값을 한 단계 감싸는 노드를 만들어서, 
`head`가 새로운 값(나중에 들어온 값)을 가리키는 형식으로 코드를 짤 계획이다.**
2. `head`의 타입인, `StackNode`라는 타입을 정의한다.
    1. `value`: 값
    2. `next`
        1. 다음 스택 노드(`StackNode`)를 가리키거나 아무것도 가리키지 않는다. (현재 스택에 1개의 값만 쌓인 경우)
        2. 따라서 `?` 로 optional 설정한다.
3. `StackImpl`이라는 클래스 정의
    1. `stack`의 크기 설정
        1. 외부에서 크기를 설정할 수 없게 한다.
        → 여기서 `readonly`를 쓰면 내부에서도 값 설정이 불가하므로, `private`을 붙여준다.
        → 내부적으로만 변경할 수 있게 하기위해 `getter`을 사용한다. 
        ✔︎ 여기서 size라는 동일한 이름이 내부에서 사용되는데, 이럴 경우, 앞에 _(언더바)를 붙여주면 된다.
        ✔︎ 보통 이런 코드에서는 크기를 `constructor`을 통해 설정해놓는다
    2. push()
        1. `node`를 먼저 만든다.
        2.  새로운 `node`는 `head`가 가리키는 값을 가리키도록 한다.
        3.  head가 새로운 값을 가리키도록 한다.
        4.  push했으므로 사이즈는 1 커진다.
    3. pop()
        1. `head`가 가리키는 값을 리턴한다.
        2.  스택이 비어있는 경우 `node`는 없을 수도 있다.
            1. 여기서 `this.head === undefined`로 표현하지 않고, `this.head == null`로 표현하는 이유는 `undefined`, `null` 모두를 거르기 위함이다.
        3.  새로운 `node`는 현재 head를 가리키고 있으므로, `node.value`를 통해 pop()으로 스택의 가장 위에 쌓여있는 값을 리턴한다.