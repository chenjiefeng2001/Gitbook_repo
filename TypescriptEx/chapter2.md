# 函数类型

## 函数类型表达式

```ts
function greeter(fn:(a:string) => void){
	fn("Hello World");
}
function printToConsole(s:string){
console.log(s);
}
greeter(printToConsole);//log "Hello World"
```

语法表示“具有一个名为参数的函数，其类型为，没有返回值”。

当然，我们也可以使用类型别名来命名函数类型：

```ts
type GreetFunction = (a：string) => void;
function greeter(fn:GreetFunction){
	//write code here ...
}
```

## 呼叫签名

在JS中，函数除了可以调用之外还可以具有属性。但是，函数类型表达式语法不允许声明属性。如果我们想要用属性描述我们可以调用的东西，我们可以在对象类型中编写一个调用签名。

```ts
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};
function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}
 
function myFunc(someArg: number) {
  return someArg > 3;
}
myFunc.description = "default description";
 
doSomething(myFunc);
```

> 请注意，与函数类型表达式相比，语法略有不同 - 在参数列表和返回类型之间使用，而不是在 .`:``=>`

## 构造签名

JS函数也可以使用运算符调用。TS将它们称之为*构造函数*，因为它们通常会创建一个新对象。您可以通过在调用签名前添加关键字来编写构造签名。

```ts
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}
```

> 某些对象(JS的对象)可以调用，可以在任意组合的同一类型的调用和构造签名

```ts
interface CallOrConstruct {
  (n?: number): string;
  new (s: string): Date;
}
```

## 泛型函数

编写一个函数，其中输入类型与输出的类型相关，或者两个输入的类型以某种方式相关

```ts
function firstElement(arr:any[]){
  return arr[0];
}
```

此函数可以完成其工作，但不幸的是具有返回类型。如果函数返回数组元素的类型

### 推理

我们不必在此示例中指定。类型是由TS自动选择的。`Type`我们也可以使用多个类型参数。

```ts
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
  return arr.map(func);
}
 
// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(["1", "2", "3"], (n) => parseInt(n));
```

>  请注意，在此示例中，TS既可以推断类型参数，也可以根据函数表达式的返回值推断参数

### 约束

如果我们想
