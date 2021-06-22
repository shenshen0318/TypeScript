### 类型断言

你清楚地知道一个实体具有比它现有类型更确切的类型

类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

类型断言有两种形式：

- “尖括号” 语法

```ts
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

- as 语法

```ts
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```


### 非空断言

在上下文中当类型检查器无法断定类型时，一个新的后缀表达式操作符 `!` 可以用于断言操作对象是非 null 和非 undefined 类型。**具体而言，`x!` 将从 `x` 值域中排除 `null` 和 `undefined`**。

- 忽略 undefined 和 null 类型

  ```ts
  function myFunc(maybeString: string | undefined | null) {
    // Type 'string | null | undefined' is not assignable to type 'string'.
    // Type 'undefined' is not assignable to type 'string'. 
    const onlyString: string = maybeString; // Error
    const ignoreUndefinedAndNull: string = maybeString!; // Ok
  }
  ```

- 调用函数时忽略 undefined 类型
  
  ```ts
  type NumGenerator = () => number;

  function myFunc(numGenerator: NumGenerator | undefined) {
    // Object is possibly 'undefined'.(2532)
    // Cannot invoke an object which is possibly 'undefined'.(2722)
    const num1 = numGenerator(); // Error
    const num2 = numGenerator!(); //OK
  }
  ```

`!` 非空断言操作符会从编译生成的 JavaScript 代码中移除，所以在实际使用的过程中，要特别注意。


### 确定赋值断言

在 TypeScript 2.7 版本中引入了确定赋值断言，即允许在实例属性和变量声明后面放置一个 `!` 号，从而告诉 TypeScript 该属性会被明确地赋值。

```ts
let x: number;
// initialize();
// Variable 'x' is used before being assigned.(2454)
console.log(2 * x); // Error

function initialize() {
  x = 10;
}
```

很明显该异常信息是说变量 x 在赋值前被使用了，要解决该问题，我们可以使用确定赋值断言：

```ts
let x!: number;
initialize();
console.log(2 * x); // Ok

function initialize() {
  x = 10;
}
```

通过 `let x!: number;` 确定赋值断言，TypeScript 编译器就会知道该属性会被明确地赋值。

