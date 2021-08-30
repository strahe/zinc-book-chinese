# 声明语句

声明语句声明一个新项目，即类型、变量或模块。

## `let` 变量声明

`let [mut] {identifier}[: {type}] = {expression};`

`let` 声明的行为就像在 Rust 中一样，但它不允许未初始化的变量。

该类型是可选的，主要用于转换整数或仔细检查表达式结果类型，否则，它是推断的。

```rust,no_run,noplaypen
let mut variable: field = 0;
```

## `type` 别名声明

`type {identifier} = {type};`

`type` 语句声明了一个类型别名以避免重复复杂类型。

```rust,no_run,noplaypen
type Alias = (field, u8, [field; 8]);
```

## `struct` 类型声明

`struct` 语句声明了一个结构。

```rust,no_run,noplaypen
struct Data {
    a: field,
    b: u8,
    c: (),
}
```

## `enum` 类型声明

`enum` 语句声明一个枚举。

```rust,no_run,noplaypen
enum List {
    A = 1,
    B = 2,
    C = 3,
}
```

## `fn` 类型声明

`fn` 语句声明了一个函数。

```rust,no_run,noplaypen
fn sum(a: u8, b: u8) -> u8 {
    a + b
}
```

## `impl` 命名空间声明

`impl` 语句声明结构或枚举的命名空间。

```rust,no_run,noplaypen
struct Data {
    value: field,
}

impl Data {
    fn print(self) {
        dbg!("{}", data.value);
    }
}
```

## `mod` 模块声明

`mod {identifier};`

`mod` 语句声明一个新模块，并要求在声明模块目录中存在一个同名的模块文件。

也就是说，如果你在位于 `src` 目录下的文件 `main.zn` 中声明一个名为 `utils` 的模块，那么必须有一个文件 `src/utils.zn`。

Zinc 模块系统几乎完全模仿了 [Rust](https://doc.rust-lang.org/book/second-edition/ch07-00-modules.html),
但要求每个模块都驻留在一个单独的文件中，并暂时允许导入私有项目。

## `use` 模块导入

`use {path};`

`use` 语句将一个项目从另一个命名空间导入到当前命名空间。

使用上面的示例，您可以通过以下方式从您的 `utils` 模块导入项目：

```rust,no_run,noplaypen
mod utils;

use utils::UsefulUtility;

// some code using 'UsefulUtility'
```

## `contract` 声明

The `contract` 声明声明了一个智能合约。合约描述在
[这里](../07-smart-contracts/00-overview.md).
该语句是一个合并的 `struct` 和 `impl` 语句，但它只能在入口点文件中声明。

```rust,no_run,noplaypen
type Currency = u248;
type PairToken = u8;

contract Uniswap {
    // The contract storage fields     
    balance_1: Currency;
    balance_2: Currency;    
    rate: u248;
    
    // Public entries available from outside   
    
    pub fn deposit(self, amount: Currency, token: PairToken) {
        // ...
    }

    pub fn withdraw(self, amount: Currency) {
        // ...
    }
    
    pub fn buy(self, amount: Currency, from: PairToken) {
        // ...
    }
    
    // Private functions
    
    fn foo(self) {
        // ...
    }
}
```
