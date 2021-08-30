# 条件表达式

## `if`

`if` 条件表达式由条件、主块和可选的 `else` 块组成。每个块都开始一个新的可见范围。

```rust,no_run,noplaypen
let condition = true;
let c = if condition {
    let a = 5;
    a
} else {
    let b = 10;
    b
};
```

## `match`

匹配表达式是嵌套条件表达式的语法糖。每个分支块都开始一个新的可见范围。

```rust,no_run,noplaypen
enum MyEnum {
    ValueOne = 1,
    // ...
    ValueTen = 10,
}

fn main() {
    let value = MyEnum::ValueOne;

    match value {
        MyEnum::ValueOne => { /* ... */ },
        MyEnum::ValueTen => { /* ... */ },
    }
}
```

目前，仅支持以下匹配模式:
- 常数 (e.g. `42`)
- 路径 (e.g. `MyEnum::ValueOne`)
- 变量绑定 (e.g. `value`)
- 通配符 (`_`)

> 目前只能将简单类型用作 `match` 审查对象,
> 也就是说，您不能匹配数组、元组或结构体。
