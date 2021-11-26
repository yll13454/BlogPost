[掌握 TS 这些工具类型，让你开发事半功倍](https://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484142&idx=1&sn=946ba90d10e2625513f09e60a462b3a7&chksm=ea47a3b6dd302aa05af716d0bd70d8d7c682c9f4527a9a0c03cd107635711c394ab155127f9e&scene=21#wechat_redirect)

### 一、类型别名 

TypeScript 提供了为类型注解设置别名的便捷语法，你可以使用 `type SomeName = someValidTypeAnnotation` 来创建别名，比如：

```tsx
type Pet = 'cat' | 'dog';
let pet: Pet;

pet = 'cat'; // Ok
pet = 'dog'; // Ok
pet = 'zebra'; // Compiler error
```

### 二、基础知识

#### 2.1 typeof

在 TypeScript 中，`typeof` 操作符可以用来获取一个变量声明或对象的类型。

```tsx
interface Person {
  name: string;
  age: number;
}

const sem: Person = { name: 'semlinker', age: 30 };
type Sem= typeof sem; // -> Person

function toArray(x: number): Array<number> {
  return [x];
}

type Func = typeof toArray; // -> (x: number) => number[]
```

#### [继续阅读：TypeScript typeof 操作符](http://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484084&idx=1&sn=da6c267d8dd3f6981d1b10cb3cf37254&chksm=ea47a3ecdd302afa38fb56060d2a3e81513b51197cdfaca15b9db1a5dd95cb2a85c06cf8652f&scene=21#wechat_redirect)

在上面代码中，我们通过 `typeof` 操作符获取 sem 变量的类型并赋值给 Sem 类型变量，之后我们就可以使用 Sem 类型：

```tsx
const lolo: Sem  = { name: "lolo", age: 5 }
```

你也可以对嵌套对象执行相同的操作：

```tsx
const kakuqo = {
    name: "kakuqo",
    age: 30,
    address: {
      province: '福建',
      city: '厦门'
    }
}

type Kakuqo = typeof kakuqo;
/*
 type Kakuqo = {
    name: string;
    age: number;
    address: {
        province: string;
        city: string;
    };
}
*/
```

此外，`typeof` 操作符除了可以获取对象的结构类型之外，它也可以用来获取函数对象的类型，比如：

```tsx
function toArray(x: number): Array<number> {
  return [x];
}

type Func = typeof toArray; // -> (x: number) => number[]
```

### const 断言

 const 断言。当我们使用 const 断言构造新的字面量表达式时，我们可以向编程语言发出以下信号：

- 表达式中的任何字面量类型都不应该被扩展；
- 对象字面量的属性，将使用 `readonly` 修饰；
- 数组字面量将变成 `readonly` 元组。

```tsx
let x = "hello"asconst;
type X = typeof x; // type X = "hello"

let y = [10, 20] asconst;
type Y = typeof y; // type Y = readonly [10, 20]

let z = { text: "hello" } asconst;
type Z = typeof z; // let z: { readonly text: "hello"; }
```

数组字面量应用 const 断言后，它将变成 `readonly` 元组，之后我们还可以通过 `typeof` 操作符获取元组中元素值的联合类型，具体如下：

```
type Data = typeof y[number]; // type Data = 10 | 20
```

这同样适用于包含引用类型的数组，比如包含普通的对象的数组。这里我们也来举一个具体的例子：

```tsx
const locales = [
  {
    locale: "zh-CN",
    language: "中文"
  },
  {
    locale: "en",
    language: "English"
  }
] asconst;

// type Locale = "zh-CN" | "en"
type Locale = typeof locales[number]["locale"];
```

另外在使用 const 断言的时候，我们还需要注意以下两个注意事项：

1. `const` 断言只适用于简单的字面量表达式

```tsx
// A 'const' assertions can only be applied to references to enum members,
// or string, number, boolean, array, or object literals.
let a = (Math.random() < 0.5 ? 0 : 1) asconst; // error

let b = Math.random() < 0.5 ? 0asconst :
    1asconst;
```

1. `const` 上下文不会立即将表达式转换为完全不可变

```tsx
let arr = [1, 2, 3, 4];

let foo = {
    name: "foo",
    contents: arr,
} asconst;

foo.name = "bar";   // error!
foo.contents = [];  // error!

foo.contents.push(5); // ...works!
```

### typeof 和 keyof 操作符

`typeof` 操作符可以用来获取一个变量或对象的类型。而 `keyof` 操作符可以用于获取某种类型的所有键，其返回类型是联合类型。

```tsx
const COLORS = {
  red: 'red',
  blue: 'blue'
}

// 首先通过typeof操作符获取Colors变量的类型，然后通过keyof操作符获取该类型的所有键，
// 即字符串字面量联合类型 'red' | 'blue'
type Colors = keyof typeof COLORS
let color: Colors;
color = 'red'// Ok
color = 'blue'// Ok

// Type '"yellow"' is not assignable to type '"red" | "blue"'.
color = 'yellow'// Error
```

#### 2.2 keyof

`keyof` 操作符可以用来一个对象中的所有 key 值：

```
interface Person {
    name: string;
    age: number;
}

type K1 = keyof Person; // "name" | "age"
type K2 = keyof Person[]; // "length" | "toString" | "pop" | "push" | "concat" | "join"
type K3 = keyof { [x: string]: Person };  // string | number
```

#### [继续阅读：TypeScript keyof 操作符](http://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484077&idx=1&sn=1215e14604232f1da0031dc3ee4f0b82&chksm=ea47a3f5dd302ae3c89633513fb8c0de72458a1915bb2a079e7961a2f4ea198afc4dc8d4c62e&scene=21#wechat_redirect)

**例子：**

`prop` 函数的作用，该函数用于获取某个对象中指定属性的属性值。因此我们期望用户输入的属性是对象上已存在的属性，那么如何限制属性名的范围呢？这时我们可以利用本文的主角 `keyof` 操作符：

```tsx
function prop<T extends object, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
```

在以上代码中，我们使用了 TypeScript 的泛型和泛型约束。**首先定义了 T 类型并使用 `extends` 关键字约束该类型必须是 object 类型的子类型，然后使用 `keyof` 操作符获取 T 类型的所有键，其返回类型是联合类型，最后利用 `extends` 关键字约束 K 类型必须为 `keyof T` 联合类型的子类型。**

```tsx
type Todo = {
  id: number;
  text: string;
  done: boolean;
}

const todo: Todo = {
  id: 1,
  text: "Learn TypeScript keyof",
  done: false
}

function prop<T extends object, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

const id = prop(todo, "id"); // const id: number
const text = prop(todo, "text"); // const text: string
const done = prop(todo, "done"); // const done: boolean
```

#### 2.3 in

`in` 用来遍历枚举类型：

```tsx
type Keys = "a" | "b" | "c"

type Obj =  {
  [p in Keys]: any
} // -> { a: any, b: any, c: any }
```

#### [继续阅读：在 TS 中如何实现类型保护？类型谓词了解一下](http://mp.weixin.qq.com/s?__biz=MzI2MjcxNTQ0Nw==&mid=2247484114&idx=1&sn=af33c36580d21c2ffe4e8204f71c10b8&chksm=ea47a38add302a9c01c9bea63f5974554e2a9ab856f1cb3b66620a02452d12d1967fb676dc9f&scene=21#wechat_redirect)

