# 基础

## 数据类型

### 原始数据类型

#### boolean

```ts
let isDone: boolean = false;
```

#### number

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

#### 字符串

```ts
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

#### void

```ts
let unusable: void = undefined; // 只能赋值为null或者undefined

function alertName(): void {
    alert('My name is Tom');
}
```

#### Null和Undefined

是所有类型的子类型，可以被其他赋值

### 对象类型

#### 接口

定义对象的类型

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

#### 可选属性

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

#### 任意属性

一旦定义了任意属性，那么确定属性和可选属性的数据类型必须是任意属性的类型的子集

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

#### 只读属性

只能在第一次给对象赋值，之后都不能赋值

```ts
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};
```

### Any

允许被赋值成任意类型的值

声明时未指定其类型，且没有赋值，会被识别为Any

```ts
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
```

#### 属性和方法

返回的值都是Any

### 类型推论

在声明时赋值，会被自动赋予相应类型

```ts
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

等价于

```ts
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
```

### 联合类型

取值可以是多种类型中的一种

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

只能访问联合类型中共有的属性或方法

## 数组

### 定义方法

#### 类型 + 方括号

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];

let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

#### 泛型

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

#### 接口

```ts
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

## 函数

### 定义

#### 函数声明

```ts
function sum(x: number, y: number): number {
    return x + y;
}
```

#### 函数表达式

=> 在typescript中表示函数的定义

左边为输入类型

右边为输出类型

```ts
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

#### 接口定义

对等号左侧进行类型限制

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

### 参数

#### 可选参数

用`?`表示可选参数，可选参数后面不允许再出现必需参数了

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

#### 参数默认值

默认值参数会被识别为可选参数，但不需要一定在必需参数之后

```typescript
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

#### 剩余参数

`...rest`可用于获取函数中的剩余参数，用数组来定义它

```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

### 函数重载

使函数具有不同的功能

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

## 类型断言

手动指定一个值的类型，将其当作该类型进行处理

### tsx语法

```typescript
值 as 类型
```

### 将联合类型断言为其中一个类型

```typescript
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

### 将父类断言为更加具体的子类

```typescript
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

### 断言为any类型

any类型可以访问任何属性和方法

```typescript
(window as any).foo = 1;
```

### 将any断言为任何一个类型

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

### 双重断言

不使用

### 性质

- 仅在编译时起作用，不会真正影响到变量的类型

- 断言只需要相互兼容其一即可，而赋值需要向下兼容
  - `animal` 断言为 `Cat`，只需要满足 `Animal` 兼容 `Cat` 或 `Cat` 兼容 `Animal` 即可
  - `animal` 赋值给 `tom`，需要满足 `Cat` 兼容 `Animal` 才行

## 赋值断言

防止变量在赋值前被使用

```typescript
let x!: number;
initialize();
console.log(2 * x); // Ok

function initialize() {
  x = 10;
}
```

通过 `let x!: number;` 确定赋值断言，TypeScript 编译器就会知道该属性会被明确地赋值。

## 声明文件

存放声明语句的文件

声明文件必需以 `.d.ts` 为后缀。

## 内置对象

在全局作用域上存在的对象

### ECMAScript对象

`Boolean`、`Error`、`Date`、`RegExp` 等。

```typescript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

### DOM和BOM的内置对象

`Document`、`HTMLElement`、`Event`、`NodeList` 等。

```typescript
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

# 进阶

## 类型别名

给一个类型起新名字

使用type创建类型别名

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

## 字符串字面量类型

用于约束值为某几个字符串中的一个

使用type定义字符串字面量类型

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'
```

## 元组

```ts
let tom: [string, number] = ['Tom', 25];
```

长度内需要赋值为指定项的元素类型，越界元素会被限制为元组每个类型的联合类型

## 枚举

取值限定在一定场景

```typescript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

枚举成员赋值为从0开始递增的数字，同时会进行映射

```typescript
enum Days {Sun = 0, Mon, Tue, Wed, Thu, Fri, Sat}; // 手动赋值时不要出现覆盖的情况

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

### 枚举项

#### 常数项

前面的例子均是

#### 计算所得项

```typescript
enum Color {Red, Green, Blue = "blue".length};
```

紧接计算所得项之后是未手动赋值的项会出错

```typescript
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

### 常数枚举

编译阶段会被删除，并且不能包含计算成员

```typescript
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

### 外部枚举

使用`declare enum`定义的枚举类型

declare定义的类型只会用于编译时的检查，编译结果中会被删除

```typescript
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果

```typescript
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

## 类

### 修饰符

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问，子类和对象都不行

  - 当构造函数修饰为 `private` 时，该类不允许被继承或者实例化

    ```typescript
    class Animal {
      public name;
      private constructor(name) {
        this.name = name;
      }
    }
    class Cat extends Animal {
      constructor(name) {
        super(name);
      }
    }
    
    let a = new Animal('Jack');
    
    // index.ts(7,19): TS2675: Cannot extend a class 'Animal'. Class constructor is marked as private.
    // index.ts(13,9): TS2673: Constructor of class 'Animal' is private and only accessible within the class declaration.
    ```

- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

  - 当构造函数修饰为 `protected` 时，该类只允许被继承

    ```typescript
    class Animal {
      public name;
      protected constructor(name) {
        this.name = name;
      }
    }
    class Cat extends Animal {
      constructor(name) {
        super(name);
      }
    }
    
    let a = new Animal('Jack');
    
    // index.ts(13,9): TS2674: Constructor of class 'Animal' is protected and only accessible within the class declaration.
    ```


### 参数属性 readonly

只读属性，不允许进行修改

```typescript
class Animal {
  readonly name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';

// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.
```

### 抽象类

`abstract` 用于定义抽象类和其中的抽象方法。

不允许被实例化，抽象方法需要被子类实现

```typescript
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');
```

### 类与接口

#### 类实现接口

```typescript
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

#### 接口继承接口

```typescript
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
```

#### 接口继承类

```typescript
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

## 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型

### 函数合并

相当于函数重载

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

### 接口合并

接口中的属性在合并时会简单的合并到一个接口中

```typescript
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

重复的属性类型需要保持一直才可进行合并

### 类的合并

同接口合并

## 泛型

指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```typescript
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

### 多个类型参数

一次定义多个类型参数

```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

### 泛型约束

由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法

使用泛型约束令泛型具有属性和方法

使用extend

```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

loggingIdentity(7);

// index.ts(10,17): error TS2345: Argument of type '7' is not assignable to parameter of type 'Lengthwise'.
```

### 泛型接口

```typescript
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

### 泛型类

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

# 声明文件

## 全局类型

### 变量

比如现在有一个全局变量，那对应的d.ts文件里面这样写。

```
declare var aaa:number
```

其中关键字`declare`表示声明的意思。**在d.ts文件里面，在最外层声明变量或者函数或者类要在前面加上这个关键字。在typescript的规则里面，如果一个`.ts`、`.d.ts`文件如果没有用到import或者export语法的话，那么最顶层声明的变量就是全局变量。**

所以我们在这里声明了一个全局变量aaa,类型是数字类型（number）。当然了也可以是string类型或者其他或者：

```
declare var aaa:number|string //注意这里用的是一个竖线表示"或"的意思
```

如果是常量的话用关键字const表示：

```
declare const max:200
```

### 函数

由上面的全局变量的写法我们很自然的推断出一个全局函数的写法如下：

```js
/** id是用户的id，可以是number或者string */
decalre function getName(id:number|string):string
```

最后的那个string表示的是函数的返回值的类型。如果函数没有返回值可以用void表示。

如果一个函数有若干中写法

那么对应d.ts对应的写法为

```js
declare function get(id: string | number): string
declare function get(name:string,age:number): string
```

如果有些参数可有可无，可以加个`?`表示非必须。

```js
declare function render(callback?:()=>void): string
```

### 用interface声明函数

```js
declare interface Get{
    (id: string): string
    (name:string,age:number):string
}

//get是Get类型的
declare var get:Get
```

![clipboard.png](https://segmentfault.com/img/bVbxExy?w=432&h=136)

### class

```js
declare class Person {

    static maxAge: number //静态变量
    static getMaxAge(): number //静态方法

    constructor(name: string, age: number)  //构造函数
    getName(id: number): string 
}
```

### 对象

```js
declare namespace OOO{
    var aaa: number | string
    function getName(id: number | string): string
    class Person {

        static maxAge: number //静态变量
        static getMaxAge(): number //静态方法

        constructor(name: string, age: number) //构造函数
        getName(id: number): string //实例方法
    }
}
```

## 混合类型

### 既是函数又是对象

```js
declare function $2(s:string): void

declare namespace $2{
    let aaa:number
}
```

### 既是函数，又是类（可以new出来），又是对象

```js
/** 作为函数使用 */
declare function People(w: number): number
declare function People(w: string): number

declare class People {
    /** 构造函数 */
    constructor(name: string, age: number)
    constructor(id: number)

    // 实例属性和实例方法
    name: string
    age: number
    getName(): string
    getAge(): number

    /** 作为对象，调用对象上的方法或者变量 */
    static staticA(): number
    static aaa: string
}

/** 作为对象，调用对象上的方法或者变量 */
declare namespace People {
    export var abc: number
}
```

## 模块化全局变量

有时候我们定义全局变量的时候需要引入(别人写的)文件，比如这样的，我想声明个全局变量req：

![clipboard.png](https://segmentfault.com/img/bVbxEAj?w=526&h=124)

由于我们当前的d.ts文件使用了import/export语法，那么ts编译器就不把我们通过`declare var xxx:yyy`当成了全局变量了，那么我们就需要通过以下的方式声明全局变量：

```js
import { Request,Response} from 'express'

declare global {
    var req: Request
    var res: Response

    namespace OOO {
        var a:number
    }
}
```

![clipboard.png](https://segmentfault.com/img/bVbxEBp?w=430&h=214)

