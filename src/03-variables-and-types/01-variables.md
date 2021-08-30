# 变量

正如之前所说，Zinc 主要特点是安全。因此，默认情况下变量是不可变的。如果您要更改它们的值，您必须明确地将它们标记为可变的。 
它可以防止您的数据在编译器无法检查您的意图的情况下发生意外变化。

```rust,no_run,noplaypen
fn test() {
    let x = 0;    
    x = 42; // 编译错误：改变一个不可变的变量

    let mut y = 0;
    y = 42; // ok
}
```

> 如果您熟悉 Rust，那么理解这个概念不会有任何困难，
> 因为语法和语义几乎相同。但是，模式匹配和解构还没有实现。

不可变变量类似于常量。与常量一样，您不能更改不可变的变量值。但是，常量不能推断它们的类型，您必须明确指定它。

> 与 Rust 不同，变量只能在函数中声明。
> 如果你需要一个全局变量，你应该声明一个常量。
> 此限制旨在防止不必要的副作用、污染全局命名空间和糟糕的代码设计。

```rust,no_run,noplaypen
const VALUE: field = 0;

fn test() {
    let variable = VALUE;
}
```

变量遮蔽可能是一个方便的功能，但 Zinc 将强制执行 warning-as-error 的开发工作流程，禁止变量遮蔽作为潜在的不安全技巧。 
如果需要多个具有相似逻辑含义的相邻变量，则应使用可变变量或类型后缀。

```rust,no_run,noplaypen
fn test() {
    let mut x = 5;
    {        
        let x = 25; // compile error: redeclared variable 'x'
    };    
    let x = 25; // compile error: redeclared variable 'x'

    x = 25; // ok
}
```

### 元组解构

可以使用单个 `let` 语句声明多个变量：

```rust,no_run,noplaypen
fn main() {
    let (mut a, b) = (42, 25);

    let (c, (mut d, e)) = (42, (25, 16));
}
```

> 此功能与 Rust 的功能相同，但仅支持 `let` 语句。 函数参数不能被解构。
