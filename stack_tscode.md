## ๐ ์คํ ๊ตฌ์กฐ ๊ตฌํํด๋ณด๊ธฐ

```jsx
interface Stack {
    readonly size: number;
    push(value: string): void;
    pop(): string;
}

// ์ ๋ฌํ๋ value๋ฅผ ํ๋จ๊ณ ๊ฐ์ธ๋ node๋ผ๋ ๊ฒ์ ๋ง๋ค์ด์ 
// head๊ฐ ์๋ก๋ค์ด์จ ๊ฐ์ ๊ฐ๋ฆฌํค๊ณ , ์ด์ ์ ๊ฐ๋ฆฌํค๋ ๊ฒ์ด ์๋ค๋ฉด, ๊ทธ๊ฒ๋ค์ ์ฐพ์๊ฐ๋ ํ์. 
type StackNode = {
    // ํญ์ ์ฌ์ฉ์๊ฐ ๋ฐ์ดํฐ๋ฅผ ๋ฃ์ด์ ํ๋จ๊ณ ๊ฐ์ธ๋ ๋ฌด์ธ๊ฐ๋ฅผ ๋ง๋ ๋ค๋ฉด 
    // ๋ถ๋ณ์ฑ์ ์ ์งํ๋ ๊ฒ์ด ์ข๋ค. -> readonly
    readonly value: string;
    readonly next?: StackNode; 
		// ๋ค์ ์คํ ๋ธ๋๋ฅผ ๊ฐ๋ฆฌํค๊ฑฐ๋ ์๋ฌด๊ฒ๋ ๊ฐ๋ฆฌํค์ง ์๋๋ค. ?๋ก optional ์ค์ 
}

class StackImpl implements Stack {
    // ๋ด๋ถ์์ ๋์ผํ ์ด๋ฆ์ ์ฐ๋ ๊ฒฝ์ฐ ์์ _(์ธ๋๋ฐ)๋ฅผ ๋ถ์ฌ์ฃผ๋ฉด ๋๋ค.
    private _size: number = 0;
    
    // head๋ stacknode๋ฅผ ๊ฐ๋ฆฌํฌ์๋ ์๋์๋ ์์
    private head?: StackNode;

    // ๋ณดํต constructor์ ํตํด size๋ฅผ ์ค์ ํด๋๋๋ค.
    constructor(private capacity: number) {}

    // getter๋ง ์๊ณ  setter๋ ์๊ธฐ๋๋ฌธ์ ๋ด๋ถ์ ์ผ๋ก๋ง ๋ณ๊ฒฝํ  ์ ์๋ค.
    get size() {
        return this._size;
    }
    
    // node๋ฅผ ๋จผ์  ๋ง๋ค๊ณ  
    // 2. ์๋ก์ด node๋ head๊ฐ ๊ฐ๋ฆฌํค๋ ๊ฐ์ ๊ฐ๋ฆฌํค๋๋ก
    // 3. head๊ฐ ์๋ก์ด ๊ฐ์ ๊ฐ๋ฆฌํค๋๋ก
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
        // ๋ฐฉ๊ธ ๋ง๋  ๋ธ๋ ํ ๋น
        this.head = node;
        this._size++;
    }

    // head๊ฐ ๊ฐ๋ฆฌํค๊ณ  ์๋ ๊ฐ ๋ฆฌํด
    pop(): string {
        // ์คํ์ด ๋น์ด์๋๊ฒฝ์ฐ node๋ ์์์๋ ์๋ค
				// null == undefined, null !== undefined (null, undefined๋ชจ๋ ๊ฑฐ๋ฅด๊ธฐ ์ํด == ์ฌ์ฉ)
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
๐ก **Stack ๊ตฌ์กฐ๋ฅผ TypeScript๋ฅผ ์ด์ฉํด ๊ตฌํํด๋ณด์**

</aside>

1. **์ฐ์  ์ ๋ฌํ๋ ๊ฐ์ ํ ๋จ๊ณ ๊ฐ์ธ๋ ๋ธ๋๋ฅผ ๋ง๋ค์ด์, 
`head`๊ฐ ์๋ก์ด ๊ฐ(๋์ค์ ๋ค์ด์จ ๊ฐ)์ ๊ฐ๋ฆฌํค๋ ํ์์ผ๋ก ์ฝ๋๋ฅผ ์งค ๊ณํ์ด๋ค.**
2. `head`์ ํ์์ธ, `StackNode`๋ผ๋ ํ์์ ์ ์ํ๋ค.
    1. `value`: ๊ฐ
    2. `next`
        1. ๋ค์ ์คํ ๋ธ๋(`StackNode`)๋ฅผ ๊ฐ๋ฆฌํค๊ฑฐ๋ ์๋ฌด๊ฒ๋ ๊ฐ๋ฆฌํค์ง ์๋๋ค. (ํ์ฌ ์คํ์ 1๊ฐ์ ๊ฐ๋ง ์์ธ ๊ฒฝ์ฐ)
        2. ๋ฐ๋ผ์ `?` ๋ก optional ์ค์ ํ๋ค.
3. `StackImpl`์ด๋ผ๋ ํด๋์ค ์ ์
    1. `stack`์ ํฌ๊ธฐ ์ค์ 
        1. ์ธ๋ถ์์ ํฌ๊ธฐ๋ฅผ ์ค์ ํ  ์ ์๊ฒ ํ๋ค.
        โ ์ฌ๊ธฐ์ `readonly`๋ฅผ ์ฐ๋ฉด ๋ด๋ถ์์๋ ๊ฐ ์ค์ ์ด ๋ถ๊ฐํ๋ฏ๋ก, `private`์ ๋ถ์ฌ์ค๋ค.
        โ ๋ด๋ถ์ ์ผ๋ก๋ง ๋ณ๊ฒฝํ  ์ ์๊ฒ ํ๊ธฐ์ํด `getter`์ ์ฌ์ฉํ๋ค. 
        โ๏ธ ์ฌ๊ธฐ์ size๋ผ๋ ๋์ผํ ์ด๋ฆ์ด ๋ด๋ถ์์ ์ฌ์ฉ๋๋๋ฐ, ์ด๋ด ๊ฒฝ์ฐ, ์์ _(์ธ๋๋ฐ)๋ฅผ ๋ถ์ฌ์ฃผ๋ฉด ๋๋ค.
        โ๏ธ ๋ณดํต ์ด๋ฐ ์ฝ๋์์๋ ํฌ๊ธฐ๋ฅผ `constructor`์ ํตํด ์ค์ ํด๋๋๋ค
    2. push()
        1. `node`๋ฅผ ๋จผ์  ๋ง๋ ๋ค.
        2.  ์๋ก์ด `node`๋ `head`๊ฐ ๊ฐ๋ฆฌํค๋ ๊ฐ์ ๊ฐ๋ฆฌํค๋๋ก ํ๋ค.
        3.  head๊ฐ ์๋ก์ด ๊ฐ์ ๊ฐ๋ฆฌํค๋๋ก ํ๋ค.
        4.  pushํ์ผ๋ฏ๋ก ์ฌ์ด์ฆ๋ 1 ์ปค์ง๋ค.
    3. pop()
        1. `head`๊ฐ ๊ฐ๋ฆฌํค๋ ๊ฐ์ ๋ฆฌํดํ๋ค.
        2.  ์คํ์ด ๋น์ด์๋ ๊ฒฝ์ฐ `node`๋ ์์ ์๋ ์๋ค.
            1. ์ฌ๊ธฐ์ `this.head === undefined`๋ก ํํํ์ง ์๊ณ , `this.head == null`๋ก ํํํ๋ ์ด์ ๋ `undefined`, `null` ๋ชจ๋๋ฅผ ๊ฑฐ๋ฅด๊ธฐ ์ํจ์ด๋ค.
        3.  ์๋ก์ด `node`๋ ํ์ฌ head๋ฅผ ๊ฐ๋ฆฌํค๊ณ  ์์ผ๋ฏ๋ก, `node.value`๋ฅผ ํตํด pop()์ผ๋ก ์คํ์ ๊ฐ์ฅ ์์ ์์ฌ์๋ ๊ฐ์ ๋ฆฌํดํ๋ค.