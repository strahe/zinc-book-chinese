# Casting and conversions

该语言强制执行静态强显式类型语义。 
它是最严格的类型系统，因为可靠性高于一切。 
但是，某些推理能力不会造成任何伤害，因此您不必在非常明显的地方指定类型。

## 显式

只能使用强制转换运算符对整数和枚举类型执行类型转换。
[这一章节](../../04-operators/05-casting.md) 解释了行为细节。

## 隐式

如果类型指定在赋值符号的左侧，`let` 语句可以执行整数的隐式类型转换。让我们检查一下声明：

```rust,no_run,noplaypen
let a: field = 42 as u32;
```

1. `42` 被推断为类型 `u8` 的值`.
2. `42` 从 `u8` 转换为 `u32`.
3. 表达式 `42 as u32` 结果被转换为 `field`.
4. 字段值分配给变量`a`.

隐式转换的第二种情况是否定运算符，无论输入参数如何，它始终返回相同位长的有符号整数类型值。

```rust,no_run,noplaypen
let positive = 100; // u8
let negative = -positive; // i8
```

[这一章节](../../04-operators/01-arithmetic.md) 更详细地描述了否定运算符。

## 推断

目前，Zinc 在两种情况下推断类型：整数字面量和 `let` 绑定。

整数字面量总是被推断为最小可能大小的值。
也就是说，`255` 是一个 `u8` 值，而 `256` 是一个 `u16` 值。

`let` 语句可以在未指定类型的情况下推断类型。

```rust,no_run,noplaypen
let value = 0xffffffff_ffffffff_ffffffff_ffffffff;
```

在上面的例子中，`value` 变量的类型为 `u128`，因为 128 个字节足以表示值 `0xffffffff_ffffffff_ffffffff_ffffffff`；
