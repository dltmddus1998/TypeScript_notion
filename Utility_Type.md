## Type Alias vs. Interface

---

### Interface

> **어떤 것의 규격 사항**
: 의사소통시 정해진 인터페이스를 토대로 서로 간 상호작용을 할 수 있도록 도와준다.
> 

### Type

> **데이터의 타입 결정**
: 어떠한 데이터를 담고 있는지 묘사한다.
> 

✔︎ **Interface는 merge가 되는데, Type은 안된다**

```tsx
interface PositionInterface {
	x: number;
	y: number;
}

interface PositionInterface {
	z: number;
}

const obj1: PositionInterface = {
	x: 1,
	y: 1,
	z: 1,
}
```

```tsx
type PositionType = {
  x: number;
  y: number;
}

// error
type PositionType = {
	z: number;
}

const obj2: PositionType = {
	x: 1,
	y: 1,
}
```

### ⭐️ Type Alias의 특징 - Utility Types

> **Index Type**
: 다른 타입에 있는 키에 접근해서 키의 값의 타입을 그대로 다시 선언할 수 있다.
> 

```tsx
{
    const obj = {
        name: 'ellie'
    }
    obj.name // ellie
    obj['name'] // ellie 

    // 인덱스를 기반으로 타입을 결정할 수 있다.
    type Animal = {
        name: string;
        age: number;
        gender: 'male' | 'female';
    };

    type Name = Animal['name']; // Name: String
    const text: Name = 'jhello';

    type Gender = Animal['gender']; // 'male' | 'female'

    type Keys = keyof Animal; // 'name' | 'age' | 'gender'
    const key: Keys = 'gender';

    type Person = {
        name: string;
        gender: Animal['gender'];
    };

    const person: Person = {
        name: 'ellie',
        gender: 'male',
    }
}
```

> **Mapped Type**
: 선언한 타입 수정시 이를 간편하게 하며 재사용성을 높여준다.
> 

```tsx
type Video = {
    title: string;
    author: string;
}

// type object 정의 안에서 '[]'를 이용하면 for...in을 사용한 것과 같다.
// ?를 통해 optional 설정해줬으므로, T의 키들을 써도 되고 안써도 된다.
type Optional<T> = {
    // P는 T에 있는 모든 키들 중 하나: T에서 P에 해당하는 값
    [P in keyof T]?: T[P] 
};

type VideoOptional = Optional<Video>;
// type VideoOptional = {
//     title?: string;
//     author?: string;
// }

// 재사용성...
type Animal = {
    name: string;
    age: number;
}
const animal: Optional<Animal> = {
    name: 'dog'
}
animal.name = 'cat';

// 타입 안에 있는 데이터들이 변경될 수 없게 설정
type ReadOnly<T> = {
    readonly [P in keyof T]: T[P];
}

const video: ReadOnly<Video> = {
    title: 'hi',
    author: 'lee'
};
video.title = 'ha'; // error

// 기존의 값의 타입을 쓰거나 null이 가능하도록 하는 타입
type Nullable<T> = { [P in keyof T]: T[P] | null };

const obj2: Nullable<Video> = {
    title: null,
    author: null,
}
```

> **Conditional Type**
: 타입을 조건적으로 결정할 수 있다.
> 

```tsx
type Check<T> = T extends string? boolean : number;
type Type = Check<string>; // boolean

type TypeName<T> = T extends string
  ? 'string'
  : T extends number
  ? 'number'
  : T extends boolean
  ? 'boolean'
  : T extends undefined
  ? 'undefined'
  : T extends Function
  ? 'function'
  : 'object';

type T0 = TypeName<string>; // string
type T1 = TypeName<'a'>; // string
type T2 = TypeName<() => void>; // function
```

> **ReadOnly**
: 가변성의 오브젝트를 여기저기 전달하는 것은 위험할 수 있다. 
→ 불변성 보장 중요!!
> 

```tsx
type ToDo = {
    title: string;
    description: string;
};

function display(todo: Readonly<ToDo>) {
    todo.title = 'jaaja'; //error
}
```

> **Partial Types**
: 기존의 타입 중 부분적으로 타입을 업데이트할 때 사용한다.
> 

```tsx
{
    type ToDo = {
        title: string;
        description: string;
        label: string;
        priority: 'high' | 'low';
    };
    
    // partial을 통해 todo 타입에 있는 것들 중 부분적인 것들만 받을 수 있다.
    function updateToDo(todo: ToDo, fieldsToUpdate: Partial<ToDo>): ToDo {
        return {...todo, ...fieldsToUpdate}
    }

    const todo: ToDo = {
        title: 'learn TypeScript',
        description: 'study hard',
        label: 'study',
        priority: 'high',
    };

    const updated = updateToDo(todo, {priority: 'low'});
    
    console.log(updated);
    // {
    //     title: 'learn TypeScript',
    //     description: 'study hard',
    //     label: 'study',
    //     priority: 'low'
    // }
}
```

> **Pick Type**
: 기존의 타입에서 원하는 속성만 골라서 더 제한적인 타입을 만들 수 있다.
> 

```tsx
	
type Video = {
    id: string;
    title: string;
    url: string;
    data: string;
};

type VideoMetadata = Pick<Video, 'id' | 'title'>;
// 공식문서
// type Pick<T, K extends keyof T> = {
//     [P in K]: T[P];
// };

function getVideo(id: string): Video {
    return {
        id,
        title: 'video',
        url: 'https://..',
        data: 'byte-data..',
    };
}

function getVideoMetadata(id: string): VideoMetadata{
    return {
        id,
        title: 'title',
    };
}
```

> **Omit Type**
: Pick Type과 반대로 원하는 것을 뺄 수 있다.
> 

```tsx
type Video = {
    id: string;
    title: string;
    url: string;
    data: string;
};

type VideoMetadata = Omit<Video, 'url' | 'title' | 'ab'>;
// 공식문서
// type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;

function getVideo(id: string): Video {
    return {
        id,
        title: 'video',
        url: 'https://..',
        data: 'byte-data..',
    };
}

function getVideoMetadata(id: string): VideoMetadata{
    return {
        id,
        data: 'data'
    };
}
```

> **Record Type**
: 타입 두개를 키와 값으로 활용하고 싶을 때 사용한다.
> 

```tsx
type PageInfo = {
    title: string;
}
type Page = 'home' | 'about' | 'contact';

// key: Page, Value: PageInfo
const nav: Record<Page, PageInfo> = {
    home: { title: 'Home' },
    about: { title: 'About' },
    contact: { title: 'Contact' },
}
```