## 知识框架

![](https://user-gold-cdn.xitu.io/2020/6/8/172916652ec072e3?imageslim)

## 基础类型

#### Boolean

> let isDone: boolean = false

#### Number 

> let count: number = 100

#### String

> let name: string = bob

#### Array 

> let list: number[] = [1,2,3]
>
> let list: Array<number> = [1,2,3,4]  //Array<number>泛型语法

#### Enum 枚举

1. 数字枚举

```js
enum week {
	Mon,
	Tue,
	Wed,
	Thu,
	Fri
}
let day: week = week.Mon
console.log(day)
//-------------编译结果
var week;
(function (week) {
    week[week["Mon"] = 0] = "Mon";
    week[week["Tue"] = 1] = "Tue";
    week[week["Wed"] = 2] = "Wed";
    week[week["Thu"] = 3] = "Thu";
    week[week["Fri"] = 4] = "Fri";
})(week || (week = {}));
var day = week.Mon;
console.log(day);  // 0
```

2. 字符串枚举

```js
enum week {
	Mon = 'first',
	Tue = 'second',
	Wed = 'third',
	Thu = 'fourth',
	Fri = 'fifth'
}
let day: week = week.Mon
console.log(day)
//------------编译结果
var week;
(function (week) {
    week["Mon"] = "first";
    week["Tue"] = "second";
    week["Wed"] = "third";
    week["Thu"] = "fourth";
    week["Fri"] = "fifth";
})(week || (week = {}));
var day = week.Mon;
console.log(day); // ‘’first
```

3. 异构枚举

```js
enum Enum {
  A,
  B,
  C = "C",
  D = "D",
  E = 8,
  F,
}
//------------编译结果
"use strict";
var Enum;
(function (Enum) {
    Enum[Enum["A"] = 0] = "A";
    Enum[Enum["B"] = 1] = "B";
    Enum["C"] = "C";
    Enum["D"] = "D";
    Enum[Enum["E"] = 8] = "E";
    Enum[Enum["F"] = 9] = "F";
})(Enum || (Enum = {}));
console.log(Enum.A) //输出：0
```

#### Any

> let flag: any = 10;
>
> flag = 1
>
> flag = false

#### Unknown 

```js
let value: unknown;
value = true; // OK
value = 42; // OK
value = "Hello World"; // OK
value = []; // OK
value = {}; // OK
value = Math.random; // OK
value = null; // OK
value = undefined; // OK
value = new TypeError(); // OK
value = Symbol("type"); // OK
```

`unknown`类型只能被赋给`any`类型和`unknown`类型本身

#### Tuple 

```js
let tupleType: [string, boolean];
tupleType = ["Semlinker", true];
console.log(tupleType[0]); // Semlinker
console.log(tupleType[1]); // true
```

#### Void

从某种程度上说，`void`和`any`相反，它表示没有任何类型，当一个函数没有返回值时，则其返回值类型应该为`void`

```js
function fn() : void {
	console.log('this is a normal function')
}
```

#### Null和undefined

```js
let u: undefined = undefined;
let n: null = null;
```

#### Never 

`Never`类型表示那些用不存在的值的类型，`Never`类型总是会抛出异常或根本不会有返回值的函数表达式或箭头函数的返回值类型。

```js
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}

```

#### object, Object 和 {} 类型

##### 1.object 类型

它用于表示非原始类型。

```typescript
// node_modules/typescript/lib/lib.es5.d.ts
interface ObjectConstructor {
  create(o: object | null): any;
  // ...
}

const proto = {};

Object.create(proto);     // OK
Object.create(null);      // OK
Object.create(undefined); // Error
Object.create(1337);      // Error
Object.create(true);      // Error
Object.create("oops");    // Error

```

##### 2.Object 类型

它是所有 Object 类的实例的类型，它由以下两个接口来定义

```ty
```

##### 3.{} 类型

{} 类型描述了一个没有成员的对象。当你试图访问这样一个对象的任意属性时，TypeScript 会产生一个编译时错误。

```typescript
// Type {}
const obj = {};

// Error: Property 'prop' does not exist on type '{}'.
obj.prop = "semlinker";
```

但是，你仍然可以使用在 Object 类型上定义的所有属性和方法，这些属性和方法可通过 JavaScript 的原型链隐式地使用

```typescript
// Type {}
const obj = {};

// "[object Object]"
obj.toString();

```



## 断言

有时候会遇到一种情况，已经确切的了解了某个值的详细信息，清楚的知道一个实体具有比它现有类型更确切的类型。

类型断言有两种形式：

1. "尖括号"语法

```js
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

1. as语法

```js
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

## 类型守卫

用于确保该类型再一定的范围内，l类型保护可以保证一个字符串是一个字符串，尽管他的值也可能是一个数值。其主要思想是尝试检测属性、方法或类型，以确定如何处理值。

#### in关键字

```ts
interface A {
  x: number;
}

interface B {
  y: string;
}

function doStuff(q: A | B) {
  if ('x' in q) {
    // q: A
  } else {
    // q: B
  }
}
```

#### typeof 关键字

```ts
interface Person {
  name: string;
  age: number;
}
const sem: Person = { name: "semlinker", age: 30 };
type Sem = typeof sem; // type Sem = Person
```

#### keyof

```ts
interface Person {
  name: string;
  age: number;
}

type K1 = keyof Person; // "name" | "age"
type K2 = keyof Person[]; // "length" | "toString" | "pop" | "push" | "concat" | "join" 
type K3 = keyof { [x: string]: Person };  // string | number
```

在 TypeScript 中支持两种索引签名，数字索引和字符串索引：

```typescript
interface StringArray {
  // 字符串索引 -> keyof StringArray => string | number
  [index: string]: string; 
}

interface StringArray1 {
  // 数字索引 -> keyof StringArray1 => number
  [index: number]: string;
}
```

#### instanceof  关键字

```ts
```

#### 自定义类型保护的类型谓词

```typescript
function isNumber(x: any): x is number {
  return typeof x === "number";
}

function isString(x: any): x is string {
  return typeof x === "string";
}
```

## 联合类型和类型别名

#### 联合类型

通常`null`和`undefined`同时使用

```js
const sayHello = (name: string | undefined) => {
  /* ... */
};
```

#### 可辨识联合

TypeScript 可辨识联合（Discriminated Unions）类型，也称为代数数据类型或标签联合类型。**它包含 3 个要点：可辨识、联合类型和类型守卫。**

**如果一个类型是多个类型的联合类型，且多个类型含有一个公共属性，那么就可以利用这个公共属性，来创建不同的类型保护区块。**

**可辨识**

//

**联合类型**

//

**类型守卫**

//

#### 类型别名

```js
type Message = string | string[];

let greet = (message: Message) => {
  // ...
};

```

### 交叉类型

//

## 内置工具方法

### Pick

Pick 的作用就是从一个对象中，挑选需要的字段出来，比如从 TODO 里面只取出 `title` 和 `completed`

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

### Partial

`Partial<T>` 将类型的属性变成可选

```ty
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

在以上代码中，首先通过 `keyof T` 拿到 `T` 的所有属性名，然后使用 `in` 进行遍历，将值赋给 `P`，最后通过 `T[P]` 取得相应的属性值的类。中间的 `?` 号，用于将所有属性变为可选。

```typescript
interface UserInfo {
    id: string;
    name: string;
}
// error：Property 'id' is missing in type '{ name: string; }' but required in type 'UserInfo'
const xiaoming: UserInfo = {
    name: 'xiaoming'
}

type NewUserInfo = Partial<UserInfo>;
const xiaoming: NewUserInfo = {
    name: 'xiaoming'
}

//相当于
interface NewUserInfo {
    id?: string;
    name?: string;
}
```

### Required

Required将类型的属性变成必选

```typescript
type Required<T> = { 
    [P in keyof T]-?: T[P] 
};
```

其中 `-?` 是代表移除 `?` 这个 modifier 的标识。再拓展一下，除了可以应用于 `?` 这个 modifiers ，还有应用在 `readonly` ，比如 `Readonly<T>` 这个类型

### Readonly

--

### Record

`Record<K extends keyof any, T>` 的作用是将 `K` 中所有的属性的值转化为 `T` 类型。

```typescript
interface PageInfo {
  title: string;
}

type Page = "home" | "about" | "contact";

const x: Record<Page, PageInfo> = {
  about: { title: "about" },
  contact: { title: "contact" },
  home: { title: "home" },
};
```

### Exclude

`Exclude<T, U>` 的作用是将某个类型中属于另一个的类型移除掉。

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

### Omit

`Omit<T, K extends keyof any>` 的作用是使用 `T` 类型中除了 `K` 类型的所有属性，来构造一个新的类型。

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

### NonNullable

`NonNullable<T>` 的作用是用来过滤类型中的 `null` 及 `undefined` 类型。

```typescript
type T0 = NonNullable<string | number | undefined>; // string | number
type T1 = NonNullable<string[] | null | undefined>; // string[]
```



## 函数

#### TypeScript 函数与 JavaScript 函数的区别

| TypeScript     | JavaScript         |
| -------------- | ------------------ |
| 含有类型       | 无类型             |
| 箭头函数       | 箭头函数（ES2015） |
| 函数类型       | 无函数类型         |
| 必填和可选参数 | 所有参数都是可选的 |
| 默认参数       | 默认参数           |
| 剩余参数       | 剩余参数           |
| 函数重载       | 无函数重载         |

#### 参数类型和返回类型

```js
function createFullName(firstName: string, lastName: string): string {
	return firstName + lastName
}
```

#### 函数类型

```js
let IdGenerator: (chars: string, nums: number) => string;

function createUserId(name: string, id: number): string {
  return name + id;
}

IdGenerator = createUserId;
```

#### 可选参数及默认参数

```js
// 可选参数
function createUserId(name: string, id: number, age?: number): string {
  return name + id;
}

// 默认参数
function createUserId(
  name: string = "bob",
  id: number,
  age?: number
): string {
  return name + id;
}
```

可以通过 `?` 号来定义可选参数，比如 `age?: number` 这种形式,需要注意的是可选参数要放在普通参数的后面，不然会导致编译错误。

#### 剩余参数

```js
function push(array, ...items) {
  items.forEach(function (item) {
    array.push(item);
  });
}

let a = [];
push(a, 1, 2, 3);
```

#### 函数重载

函数重载或方法重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。要解决前面遇到的问题，方法就是为同一个函数提供多个函数类型定义来进行函数重载，编译器会根据这个列表去处理函数的调用。

//

#### 数组

#### 解构

//

#### 扩展运算符

//

#### 数组遍历

//

### 对象

#### 解构

//

#### 扩展运算符

//



#### 为对象动态分配属性

在 JavaScript 中，我们可以很容易地为对象动态分配属性，比如：

```js
let developer = {};
developer.name = "semlinker";
```

但是在ts中会提示以下异常信息

> Property 'name' does not exist on type '{}'.(2339)

`{}` 类型表示一个没有包含成员的对象，所以该类型没有包含 `name` 属性。为了解决这个问题，我们可以声明一个 `LooseObject` 类型

```typescript
interface LooseObject {
  [key: string]: any
}
```

该类型使用 **索引签名** 的形式描述 `LooseObject` 类型可以接受 key 类型是字符串，值的类型是 any 类型的字段。有了 `LooseObject` 类型之后，我们就可以通过以下方式来解决上述问题：

```typescript
interface LooseObject {
  [key: string]: any
}

let developer: LooseObject = {};
developer.name = "semlinker";
```

另外一种场景

```typescript
interface Developer {
  name: string;
  age?: number;
  [key: string]: any
}

let developer: Developer = { name: "semlinker" };
developer.age = 30;
developer.city = "XiaMen";
```

除了使用`索引签名`之外，也可以使用ts内置的工具类型`Record`来定义以上接口 

```typescript
// type Record<K extends string | number | symbol, T> = { [P in K]: T; }
interface Developer extends Record<string, any> {
  name: string;
  age?: number;
}

let developer: Developer = { name: "semlinker" };
developer.age = 30;
developer.city = "XiaMen";
```



## 接口

#### 对象的type

```typescript
interface Person {
  name: string;
  age: number;
}

let Semlinker: Person = {
  name: "Semlinker",
  age: 33,
};
```

#### 可选 | 只读属性

```typescript
interface Person {
  readonly name: string;
  age?: number;
}
```

#### 任意属性

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

## 类

//

## 泛型

设计泛型的关键目的是在成员之间提供有意义的约束，这些成员可以是：类的实例成员、类的方法、函数参数和函数返回值。

泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

#### 泛型接口

```typescript
interface GenericIdentityFn<T> {
  (arg: T): T;
}
```

#### 泛型类

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

#### 泛型变量

- T（Type）：表示一个 TypeScript 类型
- K（Key）：表示对象中的键类型
- V（Value）：表示对象中的值类型
- E（Element）：表示元素类型

```typescript
function get<T extends object, K extends keyof T>(o: T, name: K): T[K] {
  return o[name]
}
```

**泛型应用**

Eg1

写一个函数，接受两个参数，一个为object，另一个为object中的一个key。函数返回类型指定为obj[key]的类型。

```js
interface Person{
	name:string,
	age:number
}

function demo<T extends object, K extends keyof T>(obj:T, key:K){
	return obj[key]
}

//测试
let obj:Person={
 	 name:"tea",
 	 age:23
}
let age = demo(obj, "age")   // number类型
let name = demo(obj, "name")   // string类型
```

Eg2

```ts
interface API {
    '/user': { name: string },
    '/menu': { foods: string[] }
}
const get = <URL extends keyof API>(url: URL): Promise<API[URL]> => {
    return fetch(url).then(res => res.json());
}

get('');
get('/menu').then(user => user.foods);
```

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/50d21166be6e4192af3508cfea2d045a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom:50%;" />

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3144b5a04054b0f87bff7f76e12cecc~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom: 50%;" />

#### 泛型工具类型

//





#### 泛型约束

```js
interface Length {
	langth:number
} 

function log<T extends Length>(value:T):T {
  console.log(value,value.length)
  return value
}

log('sshshs')
log([1,2,3])
log({length:1})
log(1) // 不行
```

## 装饰器



## 综合运用

**基础类型**

```js
type BasicProps = {
  message: string;
  count: number;
  disabled: boolean;
  /** 数组类型 */
  names: string[];
  /** 用「联合类型」限制为下面两种「字符串字面量」类型 */
  status: "waiting" | "success";
};
```

**对象类型**

```js
type ObjectOrArrayProps = {
  /** 如果你不需要用到具体的属性 可以这样模糊规定是个对象 ❌ 不推荐 */
  obj: object;
  obj2: {}; // 同上
  /** 拥有具体属性的对象类型 ✅ 推荐 */
  obj3: {
    id: string;
    title: string;
  };
  /** 对象数组 😁 常用 */
  objArr: {
    id: string;
    title: string;
  }[];
  /** key 可以为任意 string，值限制为 MyTypeHere 类型 */
  dict1: {
    [key: string]: MyTypeHere;
  };
  dict2: Record<string, MyTypeHere>; // 基本上和 dict1 相同，用了 TS 内置的 Record 类型。
}
```

**函数类型**

```js
type FunctionProps = {
  /** 任意的函数类型 ❌ 不推荐 不能规定参数以及返回值类型 */
  onSomething: Function;
  /** 没有参数的函数 不需要返回值 😁 常用 */
  onClick: () => void;
  /** 带参数的函数 😁 非常常用 */
  onChange: (id: number) => void;
  /** 另一种函数语法 参数是 React 的按钮事件 😁 非常常用 */
  onClick(event: React.MouseEvent<HTMLButtonElement>): void;
  /** 可选参数类型 😁 非常常用 */
  optional?: OptionalType;
}
```

**React相关类型**

```js
export declare interface AppProps {
  children1: JSX.Element; // ❌ 不推荐 没有考虑数组
  children2: JSX.Element | JSX.Element[]; // ❌ 不推荐 没有考虑字符串 children
  children4: React.ReactChild[]; // 稍微好点 但是没考虑 null
  children: React.ReactNode; // ✅ 包含所有 children 情况
  functionChildren: (name: string) => React.ReactNode; // ✅ 返回 React 节点的函数
  style?: React.CSSProperties; // ✅ 推荐 在内联 style 时使用
  // ✅ 推荐原生 button 标签自带的所有 props 类型
  // 也可以在泛型的位置传入组件 提取组件的 Props 类型
  props: React.ComponentProps<"button">;
  // ✅ 推荐 利用上一步的做法 再进一步的提取出原生的 onClick 函数类型 
  // 此时函数的第一个参数会自动推断为 React 的点击事件类型
  onClickButton：React.ComponentProps<"button">["onClick"]
}
```

## 外部类型继承或重写

不能在项目的`typings.d.ts`直接进行导入导出操作，因为当文件具有顶级`import`或`export`语句时，它被视为模块。

`typings.d.ts`中定义的type和interface不用手动导入，而去给它植入了`import`或`export`操作时会影响其他全局的类型声明。

所以可以将采用自定义全局type的方法对第三方类型进行调整,

```typescript
import { MicroAppStateActions } from 'qiankun';

// 给MicroAppStateActions添加方法
export interface _MicroAppStateActions extends MicroAppStateActions {
  getGlobalState?: (key: any) => any;
}

```

## 子类调用基类构造函数

```ts

class Animal {
  name: string;
  constructor(theName: string) {
    this.name = theName;
  }
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}
class Snake extends Animal {
  constructor(name: string) {
    super(name);
  }
  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}
```

### react event类型

```js
onChange={(e: React.ChangeEvent<HTMLInputElement>)}  // HTMLInputElement视情况而定
```

## 文章

- [TypeScript 入门教程](https://juejin.im/post/5edd8ad8f265da76fc45362c)
- [一份不可多得的 TS 学习指南-阿宝哥](https://juejin.cn/post/6872111128135073806#heading-16)
- [如何在 React 中完美运用？](https://juejin.cn/post/6910863689260204039)
- [ts高级开发技巧](https://www.nblogs.com/archives/518/)
- [TypeScript 错误property does not exist on type Object](https://www.cnblogs.com/limbobark/p/10043769.html)
- [TS错误代码大全](https://blog.csdn.net/u010785091/article/details/103123696/)
- [ts中泛型、泛型方法、泛型类、泛型接口](https://www.cnblogs.com/plBlog/p/12365627.html)
- [ts(7053)错误解决方法](https://blog.csdn.net/qq_41411483/article/details/111458367)
- [快速编写第三方包.d.ts](https://zhuanlan.zhihu.com/p/58123993)
- [你不知道的 TypeScript 泛型（万字长文，建议收藏）](https://segmentfault.com/a/1190000022993503)
- [漫谈 Typescript 研发体系建设](https://zhuanlan.zhihu.com/p/86276764)
- [ts文档](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tutorials/)
- [typescript 中的keyof、 in](https://blog.csdn.net/lhjuejiang/article/details/119038312)
- [配置详解和常见错误](https://juejin.cn/post/6985808225044004894#heading-42)
- [typescript 代码风格规范](https://www.jianshu.com/p/aae93fe0e84a)
- [错误信息列表](https://www.tslang.cn/docs/handbook/error.html)
- [never 和 unknown 的优雅之道](https://mp.weixin.qq.com/s/rZ96wy8xUrx4T1qG5OKS0w)
- [12个 Typescript 开发实用技巧清单](https://mp.weixin.qq.com/s/vsA0PJWkcAgaoisTDiLmfA)









