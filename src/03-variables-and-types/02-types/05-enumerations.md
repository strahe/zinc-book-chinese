# 枚举

这允许您通过枚举其可能的值来定义类型。 目前只支持简单的类 C 枚举，它们是一组常量：

```rust,no_run,noplaypen
enum Order {
    FIRST = 0,
    SECOND = 1,
}
```

枚举值可以与 `match` 表达式一起使用来定义每种可能情况下的行为：

```rust,no_run,noplaypen
let value = Order::FIRST;
let result = match value {
    Order::FIRST => do_this(),
    Order::SECOND => do_that(),
};
```

枚举值可以使用 `let` 语句隐式的或显式通过 `as` 运算符转换为整数：

```rust,no_run,noplaypen
let x = Order::FIRST; // the type is Order (inference)
let y: u8 = Order::SECOND; // the type is u8 (implicit casting)
let z = Order::SECOND as u8; // the type is u8 (explicit casting)
```

## 实现

可以实现枚举，即可以为其声明一些方法和相关项。 枚举实现类似于面向对象语言中类的行为部分。

```rust,no_run,noplaypen
enum List {
    First = 1,
    Second = 2,
    Third = 3,
}

impl List {
    pub fn first() -> Self {
        Self::First
    }

    pub fn second() -> Self {
        Self::Second
    }

    pub fn third() -> Self {
        Self::Third
    }
}

fn main(witness: field) -> field {
    (List::first() + List::second() + List::third()) as field * witness
}
```

有关方法的更多信息，请参阅此[章节](../03-functions.md).
