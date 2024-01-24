# 第一章 Typescript基本知识

## 类型推断

TS支持类型推断

```ts
let value1:string="Hello world";
```

通过感知JS的工作原理，TS可以构建一个接受JS代码但具有类型系统。就可以不需要直接

## 定义类型

你可以在JS中使用各种各样的设计模式。然而，使用某些设计模式难以进行自动推断。为了避免无法推断而引发错误，可以在TS中使用定义类型：

```ts
const user={
	name:string
	id:number,
}
```

在定义了该类型之后，还可以使用声明接口的方式声明此对象的内部数据类型：

```ts
interface User {
	name:string;
	id:number;
}
const uer:User={
	name:"Hello";
	id:0;
}
```

JS支持类和面向对象编程，TS也支持。也可以将接口声明与类一起使用：

```ts
interface User{
	name:string;
	id:number;
}
class UserAccount {
	name:string;
	id:number;
	construtor(name:string,id:number){
		this.name=name;
		this.number=number;
		};
}
```

## 组合类型

使用TS可以创建许多的组合类型（类型体操预定）

联合类型：可以声明类型可以是众多类型的一种。

```ts
type MyBool = true| false;
```

> 注意：如果将鼠标悬停在上面的 `MyBool`上，可以看到被归类为什么基础类型

另一个用法是描述 `string`或者 `number`的字面量的合法值

```ts
type WindowStates = "open" | "closed" | "minimized";
type LockStates ="locked" | "unlocked";
type PositiveoddnNumberUnderten = 1 |2 | 3 | 4 | 5 | 6 | 7 | 8 | 9; 
```

## 泛型

泛型为类型提供变量。

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectwithNumberArray = Array<{name:string}> 
```

# 结构化的类型系统

TS的一个核心原则是类型检查基于对象的属性和行为。有时被叫做“鸭子类型”或者“结构类型”

在结构化的类型系统中，如果两个对象具有相同的结构，则认为它们是相同类型的。

```ts
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// 打印 "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```
