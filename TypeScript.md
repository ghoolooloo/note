*TypeScript 2.7*

# 简介

## TypeScript与ECMAScript

TypeScript是ECMAScript的超集，JavaScript是ECMAScript的实现。

ECMAScript的标准发布在 <https://tc39.github.io/ecma262/> 。

[ES6 Fiddle](http://www.es6fiddler.net)：在线运行ES6代码的网站。

[Babel](http://babeljs.io/repl)：在线将ES6脚本转化为ES5脚本。

## TypeScript vs Dart

Dart与第三方JavaScript库的互操作性不是很好；

Dart需要Dart VM才能运行，并不是所有浏览器能带有Dart VM；

Dart生成的JavaScript不易于阅读。

# 入门

## 安装

```bash
$ npm install -g typescript
```

查看版本：

```bash
$ tsc -v
```

## 编程

hello.ts：

```typescript
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

## 编译

```bash
$ tsc hello.ts
```

将`hello.ts`编译为`hello.js`（ES5）

`-t`或`--target`：指定生成的目标代码，取值有：`ES3`、`ES5`、`ES2015`、`ES2016`、`ES2017`

`--noResolve`：编译器只将命令行上显式列出的.ts文件编译成目标代码，所有未显式列出的文件不会包含在目标代码中。

`--sourcemap`：生成source map文件，将TypeScript源码映射到JavaScript中，这样可以在TypeScript代码中设置断点，即使执行的是JavaScript。

### 配置——tsconfig.json



## 运行

### 在命令行上运行

```bash
$ node hello.js
```

如果要直接运行typescript脚本，可以使用`ts-node`模块：

```bash
$ npm install -g ts-node
$ ts-node hello.ts
```

### 在浏览器上运行

首先，加上一个页面——hello.html：

```html
<!DOCTYPE html>
<html>
  <head><title>TypeScript Greeter</title></head>
  <body>
    <script src="hello.js"></script>
  </body>
</html>：
```
然后，在浏览器中打开`hello.html`。

### 在线运行TypeScript脚本

[TypeScript Playground](http://www.typescriptlang.org/play/index.html)

# 基本类型

## 布尔型——boolean

布尔型只有两个取值：`true`（真）和`false`（假）。

## 数值型——number

在TypeScript中，数值型都是浮点型。

十进制：`6`；

十六进制：`0xf00d`；

二进制：`0b1010`；

八进制：`0o744`。

## 字符串型——string

和JavaScript一样，字符串可以使用双引号`"`或单引号`'`包围。

### 模版字符串

模版字符串可以定义多行文本和内嵌表达式。 这种字符串是被反引号包围，并且以`${ expr }`这种形式嵌入**插值**：

```typescript
let sentence: string = `Hello, my name is ${ name }.
                        I'll be ${ age + 1 } years old next month.`;
```

这与下面定义`sentence`的方式效果相同：

```javascript
let sentence: string = "Hello, my name is " + name + ".\n\n" +
                       "I'll be " + (age + 1) + " years old next month.";
```

### 带标签的模板字符串

如果一个模板字符串紧跟在一个函数名称引用的后面，那么这个字符串首先会被计算，之后被传入到函数中做进一步处理。模板中的字符串部分会被组成一个数组作为函数第一个参数，而每个插值会被当成单独的参数依次传入函数的第二个参数、……。

```javascript
function currencyAdjustment(stringParts, region, amount) {
  console.log(stringParts);
  console.log(region);
  console.log(amount);
  
  var sign;
  if (region == 1) {
    sign = "$";
  } else {
    sign = "\u20AC"  //欧元货币符号
    amount = 0.9 * amount;
  }
  
  return `${stringParts[0]}${sign}${amount}${stringParts[2]}`;
}

var amount = 100;
var region = 2;

var message = currencyAdjustment`You've earned ${region} ${amount}!`;
console.log(message);
```

上面的代码将输出：

```bash
["You've earned ", " ", "!"]
2
100
You've earned €90! 
```
## 数组

有两种方式可以定义数组。 

第一种：

```typescript
let list: number[] = [1, 2, 3];
```

第二种，使用泛型数组`Array<元素类型>`：

```typescript
let list: Array<number> = [1, 2, 3];
```

## 元组

元组类型用于表示一个元素数量固定和元素类型确定的数组，各元素的类型不必相同。 

```typescript
// Declare a tuple type
let x: [string, number];

// Initialize it
x = ['hello', 10]; // OK

// Initialize it incorrectly
x = [10, 'hello']; // Error
```

可通过索引来访问数组的元素：

```typescript
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

允许访问越界的元素，即允许动态增加元组的长度，而且新增元素的索引不必与原有元素索引连续。甚至，可以使用负数索引。这些新增加的元素的类型是元组所有原有元素的类型组成的联合类型：

```typescript
x[6] = 'world'; // OK, 字符串可以赋值给(string | number)类型
console.log(x[-10].toString()); // OK, 'string' 和 'number' 都有 toString
x[20] = true; // Error, 布尔不是(string | number)类型
```

## 枚举

```typescript
enum Color {Red, Green, Blue};

let c: Color = Color.Green;
```

默认情况下，从`0`开始依次为枚举量编号。 也可以手动的指定枚举量的编号（可以使用表达式）。 例如，我们将上面的例子改成从`1`开始编号：

```typescript
enum Color {Red = 1, Green, Blue};
```

或者，全部都采用手动编号：

```typescript
enum Color {Red = 1, Green = 2, Blue = 4};
```

你可以由枚举量的编号得到它的名字：

```typescript
let colorName: string = Color[2];
```

枚举量的编号最好不要重复：

```typescript
enum Color { Red = 1, Green = 2, Blue = 2 };
let colorName: string = Color[2];
alert(colorName);  //“Blue”
```

### 常量枚举

常量枚举的值只能使用常数枚举表达式并且不同于常规的枚举的是它们在编译阶段会被删除。

```typescript
const enum Directions {
  Up,
  Down,
  Left,
  Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right]
```

生成后的代码为：（常量枚举被移除）

```typescript
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

### 外部枚举

外部枚举用来描述已经存在的枚举类型的形状。

```typescript
declare enum Enum {
  A = 1,
  B,
  C = 2
}
```

外部枚举和非外部枚举之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常数成员。 对于非常数的外部枚举而言，没有初始化方法时被当做需要经过计算的。

## any

TypeScript中的所有类型都是`any`类型的子类型。声明成`any`类型的变量可以持有任何类型的值：

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

将变量声明成`any`类型，基本上就是在编写动态类型，这样做会失去TypeScript编译器带来的类型检查优点。所以，要慎用`any`。

`any`与`Object`有点类似，但`Object`类型的变量只是允许你给它赋任意值，但是却不能够在它上面调用任意的方法，即便它真的有这些方法：

```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

## void

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是`void`：

```typescript
function warnUser(): void {
  alert("This is my warning message");
}
```

声明一个`void`类型的变量没有什么大用，因为你只能为它赋予`undefined`和`null`：

```typescript
let unusable: void = undefined;
```

## undefined

`undefined`类型的变量只能赋给`undefined`值。

```typescript
let u: undefined = undefined;
```



## null

`null`类型的变量只能赋给`null`值。

默认情况下`null`和`undefined`是所有类型的子类型。例如可以把`null`和`undefined`赋值给`number`类型的变量。

然而，当你指定了`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`类型和它们各自类型的变量。 这能避免很多常见的问题。 在这种情况下，如果你想在某处传入一个`string`或`null`或`undefined`，你就要使用联合类型`string | null | undefined`。

## never

`never`类型表示的是那些永不存在的值的类型。 例如，`never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是`never`类型，当它们被永不为`true`的类型守卫（Guard）所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使`any`也不可以赋值给`never`。

下面是一些返回never类型的函数：

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
  return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {
  }
}

```

## 字符串字面量类型

字符串字面量类型，它的类型名是一个字符串，它的实例就是它本身。

字符串字面量类型可以与联合类型，类型检查和类型别名很好的配合。 通过结合使用这些特性，你可以实现类似枚举类型的字符串。

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";  // Easing是联合类型的别名
class UIElement {
  animate(dx: number, dy: number, easing: Easing) {
    if (easing === "ease-in") {
      // ...
    }
    else if (easing === "ease-out") {
    }
    else if (easing === "ease-in-out") {
    }
    else {
      // error! should not pass null or undefined.
    }
  }
}

let button = new UIElement();
button.animate(0, 0, "ease-in"); // 只能从三种允许的字符串中选择其一来做为参数传递
button.animate(0, 0, "uneasy"); // error: "uneasy" is not allowed here
```

字符串字面量类型用于函数重载：

```typescript
function createElement(tagName: "img"): HTMLImageElement;
function createElement(tagName: "input"): HTMLInputElement;
// ... more overloads ...
function createElement(tagName: string): Element {
  // ... code goes here ...
}
```

## 符号类型

符号类型的值是通过`Symbol`构造函数创建的。

```typescript
let sym1 = Symbol();
let sym2 = Symbol("key"); // 可选的字符串key
```

符号是不可改变且唯一的。

```typescript
let sym2 = Symbol("key");
let sym3 = Symbol("key");
sym2 === sym3; // false, symbols是唯一的
```

像字符串一样，符号也可以被用做对象属性的键：

```typescript
let sym = Symbol();
let obj = {
	[sym]: &quot;value&quot;
};
console.log(obj[sym]); // "value"
```

作为类成员：

```typescript
const getClassNameSymbol = Symbol();

class C {
  [getClassNameSymbol](){
    return "C";
  }
}

let c = new C();
let className = c[getClassNameSymbol](); // "C"
```

### Symbol

Symbol.hasInstance：

方法，会被`instanceof`运算符调用。构造器对象用来识别一个对象是否是其实例。

Symbol.isConcatSpreadable：

布尔值，表示当在一个对象上调用`Array.prototype.concat`时，这个对象的数组元素是否可展开。

Symbol.iterator：

方法，被*for-of*语句调用。返回对象的默认迭代器。

Symbol.match：

方法，被`String.prototype.match`调用。正则表达式用来匹配字符串。

Symbol.replace：

方法，被`String.prototype.replace`调用。正则表达式用来替换字符串中匹配的子串。

Symbol.search：

方法，被`String.prototype.search`调用。正则表达式返回被匹配部分在字符串中的索引。

Symbol.species：

函数值，为一个构造函数。用来创建派生对象。

Symbol.split：

方法，被`String.prototype.split`调用。正则表达式来用分割字符串。

Symbol.toPrimitive：

方法，被`ToPrimitive`抽象操作调用。把对象转换为相应的原始值。

Symbol.toStringTag：

方法，被内置方法`Object.prototype.toString`调用。返回创建对象时默认的字符串描述。

Symbol.unscopables：

对象，它自己拥有的属性会被`with`作用域排除在外。

## this

`this`类型表示的是某个包含类或接口的子类型。它是多态的，即**F-bounded多态性**。

```typescript
class BasicCalculator {
  public constructor(protected value: number = 0) { }
  public currentValue(): number {
  	return this.value;
  }
  public add(operand: number): this {
    this.value += operand;
    return this;
  }
  public multiply(operand: number): this {
    this.value *= operand;
    return this;
  }
  // ... other operations go here ...
}

let v = new BasicCalculator(2)
              .multiply(5)
              .add(1)
              .currentValue();

class ScientificCalculator extends BasicCalculator {
  public constructor(value = 0) {
    super(value);
  }

  public sin() {
    this.value = Math.sin(this.value);
    return this;
  }
  // ... other operations go here ...
}

let v = new ScientificCalculator(2)
              .multiply(5)
              .sin()  // 这是ScientificCalculator新增的方法
              .add(1)
              .currentValue();
```

# 声明

## 变量

### var

通过`var`声明的变量总是自动提升为全局的，与它声明的位置无关，这称为**变量提升**。在声明语句之前，提升的变量不会被初始化。在声明语句之后，才会被初始化。

```typescript
function f(shouldInitialize: boolean) {
  if (shouldInitialize) {
  	var x = 10;
  }

  return x;
}

f(true);  // returns '10'
f(false); // returns 'undefined'
```

> 在ES5中，函数定义也会被提升，但ES6中则使用块级作用域。

这种作用域规则有时又称为**函数作用域**，函数参数也是函数作用域。

函数作用域允许多次声明同一个变量，后声明的覆盖之前声明的。但如果先用`let`声明，则不能再用`var`声明同一变量。反之也一样。

```typescript
function sumMatrix(matrix: number) {
  var sum = 0;
  for (var i = 0; i < matrix.length; i++) {
    var currentRow = matrix[i];
    for (var i = 0; i < currentRow.length; i++) {
      sum += currentRow[i];
    }
  }
  return sum;
}
```

函数作用域的怪异行为：

例1：

```typescript
var fns = [];

for (var i = 0; i < 5; i += 1) {
  fns.push(function() {
    console.log(i);
  })
}

fns.forEach(fn => fn());
```

输出：

```bash
5
5
5
5
5
```

上例的问题在于`console.log(i)`是在for循环结束之后，在`fns.forEach()`调用中才执行的。

例2：

```typescript
for (var i = 0; i < 10; i++) {
  setTimeout(function() { console.log(i); }, 100 * i);
}
```

输出：

```bash
10
10
10
…
```

setTimeout在若干毫秒后执行一个函数，并且是在for循环结束后。 for循环结束后，i的值为10。 所以当函数被调用的时候，它会打印出10！

一个通常的解决方法是使用立即执行的函数表达式（IIFE）来捕获每次迭代时i的值：

for (var i = 0; i < 10; i++) {

​    // capture the current state of 'i'

​    // by invoking a function with its current value

​    (function(i) {  //参数i会覆盖for循环里的i

​        setTimeout(function() { console.log(i); }, 100 * i);

​    })(i);

}

### let

# 语句

# 函数

## 可选参数

JavaScript里，每个参数都是可选的，可传也可不传。 没传参的时候，它的值就是`undefined`。但在TypeScript里，默认情况形参与实参的个数必须一致。

```typescript
function buildName(firstName: string, lastName: string) {
  return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // ah, just right
```

在TypeScript里我们可以在参数名旁使用`?`实现可选参数的功能：

```typescript
function buildName(firstName: string, lastName?: string) {
  if (lastName)
    return firstName + " " + lastName;
  else
    return firstName;
}

let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```

可选参数必须跟在必须参数后面。

## 参数默认值

```typescript
function buildName(firstName: string, lastName = "Smith") {
  return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // works correctly now, returns "Bob Smith"
let result2 = buildName("Bob", undefined);       // still works, also returns "Bob Smith"
let result3 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result4 = buildName("Bob", "Adams");         // ah, just right
```

只有当用户没有传递这个参数或传递的值是`undefined`时，才会使用默认值。

参数默认值既可以是一个常量，也可以是函数调用：

```javascript
function calcTaxES6(income, state = getDefaultState()) {
  console.log("ES6. Calculating tax for the resident of " + state + " with the income " + income);
}

function getDefaultState() {
  return "Florida";
}

calcTaxES6(50000);
```
参数默认值与解构：

```typescript
function f({a, b} = {a: "", b: 0}): void {
  // ...
}

f(); // ok, default to {a: "", b: 0}

function f({a, b = 0} = {a: ""}): void {
  // ...
}

f({a: "yes"}) // ok, default b = 0
f() // ok, default to {a: ""}, which then defaults b = 0
f({}) // error, 'a' is required if you supply an argument
```

在所有必须参数后面的带默认值的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。 也就是说可选参数与末尾的默认参数共享参数类型。

```typescript
function buildName(firstName: string, lastName?: string) {
  // ...
}
```

和

```typescript
function buildName(firstName: string, lastName = "Smith") {
  // ...
}
```

共享同样的类型`(firstName: string, lastName?: string) => …`。 

与普通可选参数不同的是，带默认值的参数不需要放在必须参数的后面。 如果带默认值的参数出现在必须参数前面，用户必须明确的传入`undefined`值来获得默认值。

```typescript
function buildName(firstName = "Will", lastName: string) {
  return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // error, too few parameters
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");         // okay and returns "Bob Adams"
let result4 = buildName(undefined, "Adams");     // okay and returns "Will Adams"
```



# 类型系统

## 类型断言（即类型转换）

类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你——程序员，已经进行了必要的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

另一个为`as`断言：

```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在TypeScript里使用JSX时，只有`as`断言是被允许的。